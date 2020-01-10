# 原生平台 JavaScript 调试

游戏发布到原生平台后，由于运行环境不同，可能会出现在浏览器预览时无法重现的 Bug，这时我们就必须直接在原生平台下进行调试。Cocos Creator 3D 可以很方便地对原生平台中的 JavaScript 进行远程调试。

## 真机调试

如果游戏只有在真机上才能运行，或者模拟器重现不了问题，那就必须用真机对打包后的游戏进行调试。调试步骤如下：

- 确保 Android/iOS 设备与 Windows 或者 Mac 在同一个局域网中。注意在调试过程中请勿使用 VPN ，否则可能导致无法正常调试。
- 在 Creator 3D 的 **构建发布** 面板选择 Android/iOS 平台、Debug 模式，构建编译运行工程（iOS 平台建议通过 Xcode 连接真机进行编译运行）。
- 用 Chrome 浏览器打开地址：`chrome-devtools://devtools/bundled/inspector.html?v8only=true&ws={IP}:6086/00010002-0003-4004-8005-000600070008` 即可进行调试。其中 `{IP}` 为 Android/iOS 设备的本地 IP。

  ![](debug-jsb/v8-android-debug.png)

## 其它平台调试

如果需要在 Release 模式下调试，或者需要发布到 Windows 或者 Mac 平台上调试，或者需要调试定制后的原生引擎，可参考更详细的 [JSB 2.0 使用指南：远程调试与 Profile](https://docs.cocos.com/creator/manual/zh/advanced-topics/JSB2.0-learning.html)。
