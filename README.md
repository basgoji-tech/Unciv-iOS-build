# Unciv · iPhone 16 Pro 侧载构建

[![IPA build](https://github.com/basgoji-tech/Unciv-iOS-build/actions/workflows/build-ipa.yml/badge.svg)](https://github.com/basgoji-tech/Unciv-iOS-build/actions/workflows/build-ipa.yml)

此仓库使用 GitHub macOS 构建机器生成未签名 arm64 IPA，供自己的侧载工具签名安装。不需要在仓库中填写 Apple ID、证书或密码。

## 下载 IPA（构建 2）

1. 打开[构建 2 的运行记录](https://github.com/basgoji-tech/Unciv-iOS-build/actions/runs/38092484789)，确认状态为绿色 Success。
2. 到页面底部 Artifacts，下载 **Unciv-iPhone16Pro-build2-unsigned**。
3. 解压 ZIP，取出 **Unciv-iPhone16Pro-build2-unsigned.ipa**，导入自己的侧载工具签名后安装。

黄色表示仍在构建，红色表示验证失败。正常交付产物只在验证通过后上传。未签名 IPA 不能直接点击安装。需要重新构建时，在 Actions → Build unsigned iPhone IPA 中点击 Run workflow。

构建 1 在 iPhone 16 Pro / iOS 18.6.2 可进入世界地图，但首次自动存档报 `AchievementMountainParty` 缺失。请使用文件名包含 `build2` 的修复版。

## 自动存档修复

RoboVM 原生编译会裁剪没有静态引用的类；libGDX JSON 通过反射解析成就模型的泛型元素类型，旧构建缺少这些类。构建 2 在 `forceLinkClasses` 中保留 `com.unciv.**`，覆盖整个应用的反射类型，同时增加构建配置检查。

本地 10 项回归测试已通过，覆盖空成就历史克隆、非空山地小队/撤退/俘获历史、完整 GameInfo 克隆、JSON 保存读取、独立副本及安全区域。云端再次测试、执行 iOS 检查并编译真机 IPA，再在原生 iOS 模拟器运行同一序列化验证程序，通过后上传交付产物。没有关闭自动存档或成就，也没有修改存档格式。

安装构建 2 后，请新建游戏、结束一个回合，再退出并读取存档，确认真机上可正常继续游戏。

## 灵动岛适配

从 UIKit 动态读取安全区域，操作控件避让灵动岛和底部手势条。开启“延伸至全屏”时只让地图、背景延伸，控件继续使用安全区域。支持左右横屏和竖屏，包含旋转及触摸坐标回归测试。没有加入灵动岛实时活动卡片。构建 2 保留这些修改；真实设备的显示与完整游戏过程仍需实际检查。

## 公开源码与版本

原游戏：[yairm210/Unciv](https://github.com/yairm210/Unciv)。iOS 移植：[jerry8870/Unciv4iOS](https://github.com/jerry8870/Unciv4iOS/tree/ios)。固定源码提交为 **47bfb558646ef451cb175753c2c3bc8d0d638f2a**。

核心游戏实际版本为 **4.22.5**。构建 1 的 iOS 包元数据仍写 **4.21.14**，构建 2 已同步包版本为 **4.22.5**，构建号为 **2**，包 ID 保持 `com.aishuati.unciv`。

完整源码变换及验证程序在[工作流文件](.github/workflows/build-ipa.yml)中，IPA 产物附带运行时代码补丁、来源说明、许可及 SHA-256。代码沿用上游 MPL-2.0；资源许可和署名见上游 LICENSE 与 docs/Credits.md。此公开默认构建不包含 TestFlight 单独维护的 iCloud 扩展。
