# Unciv · iPhone 16 Pro 侧载构建

[![IPA build](https://github.com/basgoji-tech/Unciv-iOS-build/actions/workflows/build-ipa.yml/badge.svg)](https://github.com/basgoji-tech/Unciv-iOS-build/actions/workflows/build-ipa.yml)

此仓库使用 GitHub macOS 构建机器生成未签名 arm64 IPA，供自己的侧载工具签名安装。不需要在仓库中填写 Apple ID、证书或密码。

## 下载 IPA

1. 打开 [Actions](https://github.com/basgoji-tech/Unciv-iOS-build/actions)，选择成功（绿色）的构建。
2. 到运行页面底部的 Artifacts，下载 **Unciv-iPhone16Pro-unsigned**。
3. 解压 ZIP，取出 **Unciv-iPhone16Pro-unsigned.ipa**，导入自己的侧载工具签名后安装。

黄色表示仍在构建，红色表示失败；失败运行不会提供 IPA。未签名 IPA 不能直接点击安装。需要重新构建时，在 Actions → Build unsigned iPhone IPA 中点击 Run workflow。

## 灵动岛适配

从 UIKit 动态读取安全区域，操作控件避让灵动岛和底部手势条。开启“延伸至全屏”时只让地图、背景延伸，控件继续使用安全区域。支持左右横屏和竖屏，包含旋转及触摸坐标回归测试。没有加入灵动岛实时活动卡片。

本地 8 项安全区域测试已通过。云端会再次测试、检查 iOS API 兼容性、编译并检查 ZIP 完整性及 arm64 真机可执行程序。尚未在 iPhone 16 Pro 真机验证，安装后仍需检查按钮位置、弹窗、地图拖动、存档和后台恢复。

## 公开源码与版本

原游戏：[yairm210/Unciv](https://github.com/yairm210/Unciv)。iOS 移植：[jerry8870/Unciv4iOS](https://github.com/jerry8870/Unciv4iOS/tree/ios)。固定基线提交为 **47bfb558646ef451cb175753c2c3bc8d0d638f2a**，默认 iOS 模块版本号 **4.21.14**，不是 Android 最新版的直接转换。

源码修改完整保存在 [工作流文件](.github/workflows/build-ipa.yml) 中，并随 IPA 产物附上 source-changes.patch。代码沿用上游 MPL-2.0；资源许可和署名见上游 LICENSE 与 docs/Credits.md。此公开默认构建不包含 TestFlight 单独维护的 iCloud 扩展。
