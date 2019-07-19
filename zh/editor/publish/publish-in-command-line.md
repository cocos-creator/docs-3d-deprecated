# 命令行发布项目

通过命令行发布项目可以帮助大家构建自己的自动化构建流程，大家可以修改命令行的参数来达到不同的构建需求。

## 命令行发布参考

**例如**：构建 web-desktop 平台、Debug 模式

- Mac

  ```bash
  /Applications/CocosCreator.app/Contents/MacOS/CocosCreator --path projectPath
  --build "platform=web-desktop;debug=true"
  ```

- Windows

  ```bash
  CocosCreator/CocosCreator.exe --path projectPath --build "platform=web-desktop;debug=true"
  ```

## 构建参数

 - `--project`：指定项目路径
 - `--build`：指定构建项目使用的参数
在 `--build`  后如果没有指定参数，则会使用 Cocos Creator 3D 中构建面板当前的平台、模板等设置来作为默认参数。如果指定了其他参数设置，则会使用指定的参数来覆盖默认参数。可选择的参数有：
- `configPath` - 参数文件路径。如果定义了这个字段，那么构建时将会按照 `json` 文件格式来加载这个数据，并作为构建参数。这个参数可以自己修改也可以直接从构建面板导出。
- `includedModules` - 定制打包的引擎模块数组，有需要打包部分模块而不是全部模块的，可以传递此参数。具体模块可以从 [这里](https://github.com/cocos-creator/engine/blob/3d-v1.0.0/scripts/module-division/division-config.json) 查找到，注意传递的是模块 entry 字段组成的数组。
- `title` - 项目名
- `platform` - 构建的平台 [web-mobile、web-desktop、wechatgame、wechatgame-subcontext]
- `buildPath` - 构建目录
- `startScene` - 主场景的 uuid 值（参与构建的场景将使用上一次的编辑器中的构建设置）
- `debug` - 是否为 debug 模式
- `previewWidth` - web desktop 窗口宽度
- `previewHeight` - web desktop 窗口高度
- `sourceMaps` - 是否需要加入 source maps
- `webOrientation` - web mobile 平台（不含微信小游戏）下的旋转选项 [landscape、portrait、auto]
- `inlineSpriteFrames` - 是否内联所有 SpriteFrame
- `mergeStartScene` - 是否合并初始场景依赖的所有 JSON
- `optimizeHotUpdate` - 是否将图集中的全部 SpriteFrame 合并到同一个包中
- `template` - native 平台下的模板选项 [default、link]
- `embedWebDebugger` - 是否在 web 平台下插入 vConsole 调试插件
- `md5Cache` - 是否开启 md5 缓存
- `wechatgame` - 微信小游戏发布选项
  - `appid`- 发布微信小游戏时需要的 id
  - `orientation` - 微信小游戏屏幕方向 [landscape、portrait]

## 在 Jenkins 上部署

Cocos Creator 3D 命令行运行的时候也是需要 GUI 环境的。如果你的 Jenkins 无法使用 Cocos Creator 3D 命令行运行，一个解决办法是：确保 Jenkins 运行在 agent 模式下，这样才能访问到 WindowServer。详见：<https://stackoverflow.com/questions/13966595/build-unity-project-with-jenkins-failed>

如果你的 Jenkins 在 Windows 下无法编译，请在 Windows 的 **控制面板 -> 管理工具 -> 服务** 中为 Jenkins 的服务指定一个本地用户，然后重启电脑就可以了。不必单独设置一个 master-slave 模式。