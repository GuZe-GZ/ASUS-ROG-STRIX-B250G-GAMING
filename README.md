# ASUS-ROG-STRIX-B250G-GAMING
Hackintosh-EFI

可正常工作

 声卡 (板载) / 网卡 (板载)
 
 显卡 (核显 + 独显) / 硬解 4K (HEVC + H.264)
 
 WiFi (PCI-E 设备) / 蓝牙 (PEI-E 载 USB 设备)
 
 隔空投送 / 接力 / 随航
 
 FaceTime / iMessage
 
 Apple Music / Apple TV Plus
 
 原生电源管理 / HWP 变频
 
 睡眠 / 键盘、鼠标唤醒
 
 其他白果功能 (99%)
 
![截屏2023-06-02 23 53 11](https://github.com/GuZe-GZ/ASUS-ROG-STRIX-B250G-GAMING/assets/70998332/8878815b-dc74-494f-9df0-6e71f698df3e)
![截屏2023-06-02 23 53 38](https://github.com/GuZe-GZ/ASUS-ROG-STRIX-B250G-GAMING/assets/70998332/18059100-1003-4727-bb17-43a1c55ffebd)
![截屏2023-06-02 23 54 49](https://github.com/GuZe-GZ/ASUS-ROG-STRIX-B250G-GAMING/assets/70998332/be14cd8d-b0c8-4fdd-bfcd-e285d9d9932b)
![截屏2023-06-02 23 54 49](https://github.com/GuZe-GZ/ASUS-ROG-STRIX-B250G-GAMING/assets/70998332/d33d705f-0626-44f1-87c1-55eabf498655)

直接使用

适合同时使用核显 + 独显的用户。
下载整包后，如果之前在 Clover 时就使用iMac19,1机型，可直接使用之前的三码，或使用 Hackintool (其他工具亦可) 选择iMac19,1机型生成新的三码 + ROM，用 ProperTree 打开/EFI/OC/Config.plist配置文件，填入到 PlatformInfo > Generic 位置中 (如下图)。
 
保存后，先通过 USB 测试引导，无问题后再将 EFI 文件夹放置到启动磁盘 EFI 分区，重启电脑。

无核显使用

适合只使用独显的用户。

填入iMacPro1,1机型的三码 + ROM 信息到/EFI/OC/Config.plist配置文件 PlatformInfo > Generic 处，并将机型修改为iMacPro1,1。
删除/EFI/OC/Config.plist配置文件 DeviceProperties > Add > PciRoot(0x0)/Pci(0x2,0x0) 这一行参数 (如下图)。


保存后，先通过 USB 测试引导，无问题后再将 EFI 文件夹放置到启动磁盘 EFI 分区，重启电脑。
无独显使用

暂时只适合只使用核显的用户。

填入Macmini8,1机型的三码 + ROM 信息到/EFI/OC/Config.plist配置文件 PlatformInfo > Generic 处，并将机型修改为Macmini8,1。
修改/EFI/OC/Config.plist配置文件 DeviceProperties > Add > PciRoot(0x0)/Pci(0x2,0x0) 下 AAPL,ig-platform-id 参数为07009b3e，并新增 device-id 参数为9b3e0000，新增 framebuffer-patch-enable 参数为01000000，新增 framebuffer-unifiedmem 参数为00000080 (如下图)。


保存后，先通过 USB 测试引导，无问题后再将 EFI 文件夹放置到启动磁盘 EFI 分区，重启电脑。
对于 RX 5000 \ RX 6000 系列显卡

目前 RX 5000 系列 Navi 10 核心显卡、RX 6000 系列 Navi 21 核心显卡和 RX 6000 系列 Navi 23 核心显卡均应该手动添加 agdpmod=pikera 启动参数来防止开机黑屏，暂无其他解决方法。

进阶使用

参考 OpenCore Post-Install 或 黑苹果星球 (包含 Windows 版 [推荐在 Windows 进行定制]，需付费查看，无相关利益) 使用 USBToolBox 生成 USBMap.kext 定制 USB 映射文件，放入 /EFI/OC/Kexts/ 替换同名文件，打开 /EFI/OC/Config.plist，关闭 Kernel > Add > 7，打开 8。
 目录内有我的 USB 定制文件，可在备份好自己 EFI 的情况下尝试使用。SMBIOS 为 iMac19,1，若你使用其他机型需自行打开 USBMap.kext/Contents/Info.plist 修改 IOKitPersonalities > XHC > model 参数的 iMac19,1 为你的机型；若你需要取消 HS07 内建，打开 USBMap.kext/Contents/Info.plist 修改 IOKitPersonalities > XHC > IOProviderMergeProperties > ports > HS07 > UsbConnector 参数为 0 即可。端口具体定制情况如下：

参考 xjn 博客 的进阶部分「4.1 CPU 的变频优化」或 xjn 大佬发表于 PCbeta 的帖子 FCPX 核显独显全程满速指南 中「HWP 变频」部分，根据个人需求定制CPUFriendDataProvider.kextHWP 变频文件，放入/EFI/OC/Kexts/替换同名文件，启用/EFI/OC/Config.plist文件 Kernel > Add > 10 和 11。
 iMac19,2 和 iMacPro1,1 均不支持 HWP 变频，无需尝试。对于有核显的用户建议选择 iMac19,1 机型，对于只使用核显的用户推荐 Macmini8,1 机型。

1. 开机时苹果 logo 显示不正常怎么办？

有两个方法可以解决这个问题。
方法一：在/EFI/OC/Config.plist配置文件 UEFI > Output > Resolution 处填写正确的显示器分辨率；
方法二：将 BIOS「STTINGS\启动\全荧幕商标」设置为 [允许]，并确保配置文件 UEFI > Output > Resolution 的参数为 Max。 
两种方法选择其一即可，如果同时使用的话开机 logo 的显示依旧会不正常，原本更推荐方法二 (会比方法一进入系统登陆界面略快一些)，但反复测试后发现，如果在 BIOS 打开「Windows 10 WHQL支持」，使用方法二可能会导致关机再开机时丢失苹果 logo，请测试后选择适合自己喜欢的方法。
P.S. 目前 OC 已支持自动检测 HiDPI，如果你使用 2K 及以下分辨率无法开启 HiDPI 的显示器且开机时显示不正常，请尝试将配置文件 UEFI > Output > UIScale 设置为01。

2. 无法正常进入睡眠状态怎么办？

已知的情况是 bugOSmacOS 10.15 至 10.15.7 (包括补充更新版本) 都存在睡眠相关 bugs，需要在节能中打开 “启用电能小憩” 选项才可正常进入睡眠。
一般而言，系统更新到 macOS 11 Big Sur 和 macOS 12 Monterey 以后可在关闭 “启用电能小憩” 选项的情况下正常进入睡眠。

3. 为什么推荐拥有核显的 CPU？

首先，macOS Catalina 原生支持 4K 双硬解的独显最低为 RX VEGA⁵⁶，而第七代及以后的酷睿处理器核显可以和低于 RX VEGA⁵⁶ 的独显协同工作，完成 4K 双硬解；
其次，因为黑果没有 T2 芯片，所以没有核显的黑果无法使用随航 (Sidecar) 功能。

4. 引导过程触发原生快捷键怎么这么难？

我也被这个问题困扰了许久，在 OC 0.5.5 之前尝试过各种配置组合，均无法触发，但 OC 更新 0.5.5 后，通过设置 TakeoffDelay 参数可在引导过程中触发原生快捷键，建议在启动时按住组合键，或键盘灯亮起时不断重按组合键，可自行调整 TakeoffDelay 参数。

5. NVMe 硬盘温度过高怎么办？

一般来说读写速度越快的硬盘温度往往越高，无需太过担心，但待机情况下超过 50℃ 或你认为硬盘的温度不正常，可尝试加载 NVMeFix 解决。
将 NVMeFix.kext 放入/EFI/OC/Kexts/目录，打开 OC 配置文件，在 Kernel > Add 处添加 NVMeFix.kext（参考其他 kext 的添加方式），当前已默认添加并启用。

6. 可以观看 Apple TV+ / Netflix 等 DRM 媒体吗？更新！

得益于 WhateverGreen 的功能，添加 shikigva=80 启动参数后，拥有独立显卡的机器都可以直接使用 tv 应用，并观看 Apple TV+，也支持 Safari 硬解观看 Netflix / Amazon Prime 等流媒体。
macOS 10.15.4 之前版本，RX 4XX/5XX 大部分显卡不可使用 Safari 硬解 DRM (表现为冻屏)，但这一问题在 10.15.4 中已经被修复，直接升级系统即可。
WhateverGreen 的 DRM 补丁目前不支持 Safari 14 和 macOS 11 及以后版本，但可以在终端中依次执行下列代码后正常使用 tv 应用观看 Apple TV+ (Safari 仍然不可以正常解码 DRM)。

defaults write com.apple.AppleGVA gvaForceAMDKE -bool YES
defaults write com.apple.AppleGVA gvaForceAMDAVCEncode -bool YES
defaults write com.apple.AppleGVA gvaForceAMDAVCDecode -bool YES
defaults write com.apple.AppleGVA gvaForceAMDHEVCDecode -bool YES
注意：因为缺少 Apple Firmware，导致 iGPU 无法硬解 DRM，所以没有独显的机器无法观看 DRM 媒体。
