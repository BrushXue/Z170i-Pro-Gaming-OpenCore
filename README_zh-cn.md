# 华硕Z170i Pro Gaming黑苹果Opencore

我不是专业码农。我只是一名爱好者，按照官方文档制作并分享本EFI。相似的硬件配置（Z170+Skylake）理论上稍加修改也可以使用本EFI，但是我不做任何保证！有关调试的问题请去问kext原作者！

本EFI可在macOS Big Sur (11.0.1)上驱动。FCPX显卡渲染正常工作。HDR可正常开启。支持睡眠，隔空投送和接力。
![image](Screenshot_en-us.png)

Big Sur需要OpenCore 0.6.3，系统更新前请先更新EFI。

## 注意事项
1. 在Clover和OpenCore之间切换时请清除NVRAM。
2. 强烈建议使用Xcode或其他plist编辑器。不推荐使用OpenCore Configurator，原因是每次OpenCore更新文件结构都在改变。
3. 如果你没有独立显卡（核显输出），参考[这里](https://dortania.github.io/OpenCore-Desktop-Guide/config.plist/skylake.html#add_1)修改`/EFI/OC/config.plist` -> `DeviceProperties/Add/PciRoot(0x0)/Pci(0x2,0x0)`。
4. OpenCanopy和HiDPI默认开启。如果你没有4K显示器，请修改`/EFI/OC/config.plist` -> `NVRAM/Add/4D1EDE05-38C7-4A6A-9CC6-4BCCA8B38C14/UIScale`。
5. 我在USB映射里面加满了15个端口。2个USB 3.0(前置) + 4个USB 3.0(后置) + 2个USB 2.0(后置) + 蓝牙(内部M.2) = 15个端口。一般情况下ITX机箱没有前置USB 2.0接口所以我没有添加板载USB 2.0(HS11/HS12)。这项设定应该符合绝大多数人的需求。但如果你选购了一些奇奇怪怪的需要单独连USB 2.0的无线网卡, 参考[这里](https://dortania.github.io/USB-Map-Guide/)创建自己的USB映射。
6. 板载声卡3.5mm输出需要接在绿色（LINE OUT）插孔上面。如果从Windows系统重新启动会没有声音，这是华硕声卡驱动的问题。每次从Windows重启进入macOS时请先关机再开机。

![image](Z170iProGaming.jpg)

## 硬件
| 类别 | 品牌 | 型号 | 驱动 | 备注 |
|-----|-----|-----|-----|-----|
| 主板 | 华硕 | Z170i Pro Gaming | | |
| 处理器 | 英特尔 | i7-6700K |  | |
| 内存 | 芝奇 | TridentZ 2x16GB DDR4 3000 |  | 超频至 3200 |
| 核芯显卡 | 英特尔 | HD Graphics 530 | 免驱 | Headless模式 |
| 独立显卡 | 讯景 | RX 580 GTS XXX Edition 8GB | 免驱 | 2304流处理器 |
| 固态硬盘 | 三星 | SM961 1TB NVMe | [NVMeFix](https://github.com/acidanthera/NVMeFix) | |
| 无线 | 博通 | BCM94360NG M.2 | 免驱 | 替换QCA61x4A* |
| 以太网 | 英特尔 | I219-V | [IntelMausi](https://github.com/acidanthera/IntelMausi) | |
| 声卡 | 瑞昱 | ALC1150 | [AppleALC](https://github.com/acidanthera/AppleALC) | |
| 电源 | 海盗船 | SF600 白金版 | | |
| 机箱 | Dan | A4-SFX | | |
| 显示器 | 戴尔 | U2720Q | | |

*高通的QCA61x4A无解，请参见[此教程](https://www.tonymacx86.com/threads/bcm94352z-installed-on-asus-z170i-pro-gaming-wifi-and-bt.191274)替换原厂无线网卡。理论上BCM94352Z或BCM94360CS2加转接卡都可以免驱使用。
## BIOS设置
| 名称 | 选项 |
| --- | --- |
| SW Guard Extensions (SGX) | 关闭 |
| CFG Lock | 关闭 |
| VT-d | 关闭 |
| 大于4G地址空间解码 | 开启 |
| 首选显卡 | PCIE |
| 初始化iGPU | 开启 |
| DVMT Pre-Allocated | 128M |
| IOAPIC 24-119 Entries | 关闭 |
| 网络堆栈 | 关闭 |
| Legacy USB 支持| 开启 |
| 快速启动 | 关闭 |
| 操作系统类型 | 其他操作系统 |
| 开启CSM | 关闭 |

## 已知问题
显卡风扇在启动画面时有概率满速几秒，进入桌面后正常。确定是讯景BIOS的bug，刷其他品牌BIOS后恢复正常。
