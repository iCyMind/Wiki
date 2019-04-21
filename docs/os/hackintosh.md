# Hackintosh

## lilu 黑果平台

- 是现代的黑苹果方法, 在 mojave 上推荐使用
- 结合插件完成一些功能, 如: 配置显卡/网卡/声卡, 主要的插件是:
  - whateverGreen
  - AppleALC
  - AirportBrcmFixup
- 使用 lilu 及其插件的话, 推荐删除所有老方法的各种 kext

## kext 安装位置

- EFI 分区的 Clovers/kexts/other 只放 FakeSMC, 以便可以启动macOS 安装盘, 或者引导 MacOS recovery
- 其他 kext 只放 /Library/Extensions (放到 EFI 分区的话, 没有缓存)

## 一些观点

- Device Properties 字段是一个新的方法来定义设备
- 可以使用 hackintools 来确定自己的 platformID
- 手动复制 kext 到 /L/E 文件夹后, 需要修复权限(软体: Kext Utility)

## 安装 mojave 踩的坑

- 安装盘无法启动.

  原因: u盘不是 gpt 格式.
  解决方法: 需要将 u盘 格式化为 gpt 分区. 而 macos 的高级版本里, disk utils 默认只显示分区不显示设备, 所以我只格式化了分区, 没把 u盘整个格式化为 gpt. 在左边栏的下拉框按钮"view" 中, 把"Show all device"选中即可显示所有分区/设备.

- 安装的过程中卡住, 屏幕中间出现一个白色的禁止符号🚫

  原因: BootLoader 无法用 USB 3.0 接口读取数据
  解决方法: 将U盘插到  USB 2.0 接口上

- 无法重启, 表现为点击重启按钮后, 显示器无信号, 主机的灯一直亮着, 无法强制关机, 也无法进入 bios. 一段时间后(10+分钟以上)启动成功. 

  原因: 未知.
  解决方法: 未知. 莫名其妙就好了. 可能跟这几个因素有关:

    1. 将 energy saver 里将 computer sleep 设置为 nerver. 取消 put hard disk to sleep when possible / wake for ethernet network access 的勾选. 但后来将这些设置勾选回来, 重启也没什么问题.
    2. 使用 clover configarator 开启启动的 verbose 模式. 后来取消了 verbose 模式, 重启也没问题.

- 启动很慢, 跟 windows 比慢, 跟淘宝帮我装的黑苹果比, 也慢.

  原因: 可能是因为升级到了 10.14.3, 之前淘宝装的黑苹果是 10.14.2
  解决方法: 无找到.

- 内置显卡型号识别正确, 但是现存只识别到 7MB. 动画卡顿

  原因: 未知.
  解决方法: 对比了淘宝小哥给我安装的黑苹果配置文件, 发现他的配置里没有设置 ig-platform-id. 于是删除了我的配置里的这个字段, 搞定.

- 休眠后无法唤醒, 表现为重启

  原因: 未知
  解决方法:

- SMBIOS ProductName 不恰当导致的花屏

  原因: 不同的 SMBIOS 会导致 macOS 有不同的行为. iMac18,2 是有独显的 iMac 设备, 而我的黑苹果只有集显, 应该用 iMac18,1 这个 ProductName
  解决方法: 改为 iMac18,1.

- 使用 hackintools:

  - 根据它的建议改为 imac18,2 之后出现花屏(改为 imac18,1 后花屏消失)
  - 使用 nram 2048, 屏幕颜色变紫色. 取消后正常
  - 默认分辨率不正确, 显示模糊. 手动改为正确的分辨率

- framebuffer
  - HDMI 口识别为 port 5, 在 busID 为6时, 显示正常. 改为4的话, 插上显示器, 则
  - DVI 口识别为 port 6, 在 busID 为4时, 显示正常

- 升级 clover 后无法引导
  - 缺少了内存补丁, 导致卡在 randomseed
  - 缺少了afs补丁, 导致找不到分区


log:
- 添加了 ig-platform-id 后, 可以双屏(后来证实是偶然)
- 双屏开机. 到登录界面 DVI 信号丢失. 拔出再插入, 直到DVI的显示器能识别到输入(黑屏也没关系). 识别到 DVI 输入后, 把 HDMI 拔出的情况下, 反复尝试DVI接口, 直到屏幕亮起. 这时候插入 HDMI 接口, HDMI 显示器会变绿闪一下, 这时候双屏就成功了.
- 或者使用 hackintoll, 当DVI接口那一栏变红时, 把 HDMI 拔出. 插拔DVI直到点亮, 再插入HDMI
