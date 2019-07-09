# 发布到 Web 平台

Cocos Creator 3D 提供了两种 Web 平台的页面模板，可以通过 **发布平台** 的下拉菜单选择 `Web Mobile` 或 `Web Desktop`，他们的区别主要在于 `Web Mobile` 会默认将游戏视图撑满整个浏览器窗口，而 `Web Desktop` 允许在发布时指定一个游戏视图的分辨率，而且之后游戏视图也不会随着浏览器窗口大小变化而变化。

打开主菜单的 `项目 -> 构建发布`，打开构建发布面板。
## 参数说明
一些通用的构建通用参数介绍，请参考[通用构建参数介绍](build-options.md)

### resolution
指定的游戏视图分辨率，仅在 web-desktop 下可以配置。

## 构建和预览
配置好构建参数后，点击 **构建** 按钮，开始 Web 平台版本构建。面板上会出现一个进度条，当进度条达到 100% 时，构建就完成了。

接下来可以点击 **运行** 按钮，在浏览器中打开构建后的游戏版本进行预览和调试。

![web mobile](publish-web/web-mobile.png)

上图所示就是 Web Mobile 模式的预览，可以看到游戏视图会占满整个窗口, 而 Web Desktop 则不会撑满屏幕，如下图。

![web mobile](publish-web/web-desktop.gif)
### 浏览器兼容性

Cocos Creator 3D 开发过程中测试的桌面浏览器包括： Chrome，Firefox（火狐），IE11
其他浏览器只要内核版本够高也可以正常使用，对部分浏览器来说请勿开启 IE6 兼容模式。

移动设备上测试的浏览器包括：Safari (iOS)，Chrome，QQ 浏览器，UC 浏览器，百度浏览器，微信内置 Webview。

## Retina 设置

可以在脚本中通过 `cc.view.enableRetina(true)` 设置是否使用高分辨率，构建到 Web 平台时默认会开启 Retina 显示。

## 发布到 Web 服务器

要在互联网上发布或分享您的游戏，只要点击 **发布路径** 旁边的 **打开** 按钮，打开发布路径之后，按照当前构建任务名称，将构建出的对应文件夹里的内容整个复制到您的 Web 服务器上就可以通过相应的地址访问了。

关于 Web 服务器的架设，可以自行搜索 Apache、Nginx、IIS、Express 等相关解决方案。