# 漏洞洞察报告：io_uring ZCRX freelist 本地提权漏洞

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://ze3tar.github.io/post-zcrx.html |
| **漏洞标题** | You gave me a u32. I gave you root. (io_uring ZCRX freelist LPE) |
| **漏洞类型** | OOB (越界) 堆写入漏洞 → 本地提权 (LPE) |
| **目标系统** | Linux 6.15 – 6.19 (未包含修复补丁 770594e) |
| **漏洞日期** | 2026-05-06 |
| **漏洞状态** | 已公开披露 |
| **标签** | linux, kernel, io_uring, lpe, heap |

---

## 漏洞概述

Linux 6.15 引入了 io_uring 的新型零拷贝接收子系统 ZCRX (Zero-Copy Receive)。该子系统使用一个栈式结构来管理空闲的网络 I/O 向量 (niovs)：`freelist[]` 存储可用的槽位索引，`free_count` 表示栈深度。**关键问题在于 `free_count` 没有任何上限检查。**

两条独立的内核清理路径都会将 niov 返回到同一个 freelist，当它们重叠时，`free_count` 会超过分配的数组长度，导致向相邻的 slab 内存写入 4 字节的越界数据。

---

## 漏洞技术细节

### 根本原因

`io_zcrx_return_niov_freelist` 函数执行以下操作：

```c
area->freelist[area->free_count++] = net_iov_idx(niov);
```

**无任何边界检查**。当 `free_count == num_niovs` 时，写入会访问 `freelist[num_niovs]`，即数组末尾之后的第一个位置。

### 双重返回问题

存在两条将 niov 返回到 freelist 的代码路径：

1. **路径A：正常接收完成** - 网络栈释放数据包时调用 `io_pp_zc_release_netmem`，将 niov 推回 freelist
2. **路径B：页面池清理** - NIC 关闭时调用 `io_pp_zc_destroy`，遍历并强制返回所有仍有引用的 niov

当 `ptr_ring` 排空和清理循环不是原子操作时，某些 niov 会被两条路径重复计数，导致 `free_count` 超过数组边界。

### 触发条件

关闭 io_uring 文件描述符**不会**触发此漏洞。必须满足以下条件：

- 真实的 ZCRX 网卡 (mlx5/ice/nfp)，而非测试模块
- 需要 `CAP_NET_ADMIN` 权限
- 通过 `SIOCSIFFLAGS IFF_DOWN` 触发 NIC 关闭，进而触发 `page_pool_destroy()`

---

## 利用原语

### 越界写入特点

- 写入值：niov 索引，小整数 (0 到 N-1)
- 写入大小：4 字节 (u32)
- 通过选择注册时的区域大小，可以选择 freelist 所在的 slab 缓存

| num_niovs | freelist 大小 | slab 缓存 | 写入值范围 |
|-----------|--------------|-----------|-----------|
| 8         | 32 B         | kmalloc-32 | 0 – 7     |
| 16        | 64 B         | kmalloc-64 | 0 – 15    |
| 32        | 128 B        | kmalloc-128| 0 – 31    |
| 64        | 256 B        | kmalloc-256| 0 – 63    |

### 利用链

1. **堆喷洒**：通过 `msgsnd()` 喷洒 512 个 kmalloc-128 的 `msg_msg` 对象
2. **注册 ZCRX**：freelist (128 字节) 落在 kmalloc-128，与 msg_msg 相邻
3. **触发 OOB**：NIC 关闭时，写入 `freelist[32] = niov_idx`，损坏相邻 msg_msg 的 `m_list.next` 指针低 32 位
4. **利用损坏的指针**：由于高 32 位保持不变，结果仍是内核 physmap 地址，通过大量喷洒使某个 msg_msg 恰好占据该地址
5. **读取泄漏**：使用 `msgrcv` 的 `MSG_COPY` 标志读取损坏消息，扫描内核指针以绕过 KASLR
6. **覆盖 modprobe_path**：通过 `/proc/sys/kernel/modprobe` 写入恶意脚本路径
7. **触发 root 执行**：创建未知 socket 协议触发 `call_usermodehelper`，以 uid=0 运行脚本

---

## KASLR 绕过

漏洞利用按顺序尝试三种 KASLR 绕过方法：

1. **/proc/kallsyms** - 当 `kptr_restrict=0` 时直接读取
2. **dmesg** - 当 `dmesg_restrict=0` 时从内核消息中获取指针
3. **msgrcv 越界读取** - 扫描堆喷洒消息中的内核文本指针

---

## 影响范围

| 要求 | 详情 |
|------|------|
| 内核版本 | 6.15 – 6.19 (未包含 commit 770594e) |
| 配置 | `CONFIG_IO_URING_ZCRX=y` (大多数发行版内核尚未启用) |
| 硬件 | 真实的 ZCRX 网卡：Mellanox ConnectX-6+、Intel E800 系列、Netronome NFP、Broadcom BCM5750x、Google GVE |
| 权限 | `CAP_NET_ADMIN` (IORING_REGISTER_ZCRX_IFQ 和 SIOCSIFFLAGS) |

---

## 修复方案

修复代码在 `io_zcrx_return_niov_freelist` 中添加边界检查：

```c
if (WARN_ON_ONCE(area->free_count >= area->nia.num_niovs))
    return;
```

修复提交：`770594e` (2026年4月21日)，截至报告时尚未进入任何稳定分支。

---

## 结论

这是一个高质量的本地提权漏洞：
- **利用条件明确**：需要真实的 ZCRX 网卡和 CAP_NET_ADMIN 权限
- **利用链完整**：从越界写入到 root 权限的完整攻击链
- **修复状态**：漏洞已公开披露，官方已发布修复补丁

此漏洞主要影响场景：
- Kubernetes 网络 sidecar 容器
- 拥有网络管理权限的 VM 客户机
- 直接被授予 CAP_NET_ADMIN 能力的本地用户