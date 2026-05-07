# 洞察报告

## 基本信息

- **链接**: https://aniket.foo/posts/20260505-netboot/
- **标题**: Diskless Linux boot using ZFS, iSCSI & PXE
- **作者**: Aniket
- **发布日期**: 2026年5月5日
- **标签**: ZFS, iSCSI, Debian, Netboot

---

## 内容总结

### 动机

作者希望在游戏PC上测试新的Unsloth模型（Qwen3.6和Gemma4），但不想在Windows上编译llama.cpp，也不想破坏现有的Windows系统和GRUB引导。同时，作者不想重新分区已有的NVMe硬盘，也不想依赖容易丢失的USB驱动器。因此决定使用NAS进行远程启动。

### 局限性

在网络驱动器上安装Debian会比本地安装明显慢，但由于作者会用本地NVMe存储和加载模型，且有足够的RAM，运行操作系统本身不是问题。

### 架构概述

整个方案基于一台运行Proxmox的Debian 13服务器，提供以下服务：
- Netboot.xyz
- TFTPD
- iSCSI Target
- ZFS ZVol

路由器使用Asus Merlin固件的DNSMasq进行DHCP配置。

### 主要步骤

1. **安装配置Netboot.xyz**：安装apache2、git、ansible、tftpd-hpa和targetcli-fb，克隆netboot.xyz仓库，编辑配置文件设置site_name和boot_domain，然后使用ansible部署到/var/www/html。

2. **创建自定义iPXE菜单**：创建debian13-iscsi.ipxe文件，配置iSCSI服务器地址、IQN、 initiator IQN和认证信息，使主机能够从网络启动 Debian 安装程序或从 iSCSI 磁盘启动。

3. **配置TFTP**：编辑/etc/default/tftpd-hpa，将netboot.xyz编译的二进制文件复制到/srv/tftp/ipxe/目录。

4. **配置DNSMasq**：在路由器上配置dnsmasq.conf.add，针对BIOS客户端、UEFI x86-64客户端和iPXE客户端设置不同的dhcp-boot选项，将它们重定向到TFTP服务器。

5. **创建ZFS ZVol**：使用zfs create -V 32G tank/debian-disk-12700k创建32GB的ZFS卷作为iSCSI目标磁盘。

6. **配置iSCSI**：使用targetcli工具创建iSCSI backstore、target、TPG，设置访问控制列表（ACL）和双向CHAP认证，创建LUN映射。

7. **安装Debian**：通过PXE启动进入netboot菜单，选择自定义菜单加载iSCSI配置，按步骤完成Debian安装。安装过程中需要手动配置iSCSI连接，包括输入InitiatorName、用户名、密码等信息。安装完成后重启即可从iSCSI磁盘远程启动Debian系统。

### 结论

该方案实现了通过网络PXE引导和iSCSI存储启动Linux系统，避免了对本地硬盘的依赖，同时保持了GRUB引导在远程存储上，便于统一管理和维护。