# 洞察报告

## 基本信息

| 项目 | 内容 |
|------|------|
| **洞察链接** | https://www.xda-developers.com/linux-gaming-is-getting-faster-because-windows-apis-are-becoming-linux-kernel-features/ |
| **来源** | XDA Developers |
| **作者** | Ty Sherback |
| **发布日期** | 2026年5月10日 |
| **语言** | 中文 |

---

## 核心洞察

### Linux游戏市场现状

2026年3月，Linux在Steam用户中的占比首次突破5%，创下历史新高。这一突破主要得益于两个因素：

1. **Windows 10停止支持**：微软于2025年10月终止Windows 10支持，推动用户寻求替代方案
2. **Steam Deck的普及**：Valve的掌机将数百万用户转变为Linux游戏玩家，而他们可能并未意识到这一点

### NTSYNC：内核级性能提升

**NTSYNC是什么**

NTSYNC是直接添加到Linux内核的驱动程序，为Linux提供Windows特定工具的原生实现。现代游戏需要同时协调渲染管线、资源加载、物理模拟、音频处理、AI逻辑和输入处理等众多任务。NTSYNC使Linux内核能够原生理解这些Windows API调用，无需Wine进行模拟。

**技术演进路径**

- **过去**：Wine使用esync和fsync等变通方案来模拟Windows的线程同步机制
- **现在**：NTSYNC将这些机制直接内置于Linux内核，Wine无需再模拟任何内容
- **优势**：性能提升显著，修复了之前变通方案导致的偶发卡顿、死锁等边缘案例问题

### 性能表现说明

文章提醒需理性看待性能数据：

- NTSYNC原始基准测试显示40%-200%的FPS提升，但这是与未修改的上游Wine对比
- 实际使用中（尤其是Steam Deck），用户使用的是已集成fsync的Proton
- 与fsync相比，NTSYNC的性能提升更为温和
- **最大受益者**：之前运行困难的那些游戏，而非本就流畅的游戏

### Valve的战略意义

Valve工程师Pierre-Loup Griffais承认fsync已经足够快，但Valve仍选择在2026年3月将NTSYNC纳入稳定版SteamOS。原因在于：

- fsync本质上是变通方案，在特定游戏中会产生微妙的行为问题
- NTSYNC从源头匹配Windows行为，消除潜在的兼容性问题
- 这表明Linux游戏生态系统正从社区补丁向内核级原生支持转型

### 生态发展趋势

NTSYNC并非孤例。Linux此前已添加了对多事件等待等Windows原生功能的支持。这种"因Windows游戏需求而将Windows功能重建到Linux内核"的模式正在成为趋势。

**主要推动力量**：

- Valve（通过Steam Deck和Proton）
- CodeWeavers（NTSYNC作者Elizabeth Figura的雇主）
- 社区贡献者

### 未来展望

Linux游戏正在经历从"Wine补丁和社区变通方案"向"游戏巨头推动内核级变更"的根本性转变。随着Steam用户中Linux占比超过5%，这一趋势只会加速。Bazzite、CachyOS、Fedora、Ubuntu等主流发行版都将从这一技术进步中受益。

---

## 关键要点

1. **NTSYNC**是将Windows API原生实现集成到Linux内核的驱动程序，消除了Wine模拟层
2. **5%的Steam用户占比**标志着Linux游戏的重要里程碑
3. **性能提升**主要惠及之前存在兼容问题的游戏，而非所有游戏
4. **Valve的推动**使Linux内核级支持成为可能，这是开源社区与传统游戏产业的重要融合
5. **NTSYNC不是终点**，未来还会有更多Windows功能被原生集成到Linux内核

---

*报告生成时间：2026-05-14*
