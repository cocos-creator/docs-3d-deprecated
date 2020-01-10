# 命令行测试文档

## 基本测试方法介绍
（以 windows 为例）
1. 在任意文件夹内点击空白处，按下 `shift + 右键`，选择菜单里面的`打开此处的 Powershell 窗口`或者自行打开 cmd 窗口
![cmd](./index/cmd.jpg)
2. 先执行一遍 `chcp 65001` 避免中文输出乱码
3. 输入命令行构建参数，具体可以参考 [命令行构建文档](https://docs.cocos.com/creator3d/manual/en/editor/publish/publish-in-command-line.html)

```
// 以 windows 为例
CocosCreator3D 路径 --dev --project 项目路径 --build "platform=web-desktop;debug=true"

// 以我本地电脑上实际执行命令为例
 C:\software\CocosCreator3D\CocosCreator3D.exe --dev --project C:\code\NewProjectTest_002 --build "platform=wechatgame; debug=true;"
```

其中 `CocosCreator3D 路径` 和 `项目路径` 可以通过直接拖拽 exe 或者文件夹到命令行工具中自动生成路径

4. 执行命令，正常结果会提示构建成功，如下图

![build-success](./index/build-success.jpg)

5. configPath 的使用
在实际测试中，不需要每次去手动输入构建配置参数，可以在面板中调好配置，在配置面板的右上方点击导出即可生成对应平台的测试参数。之后执行下面的命令，注意`--dev`是用来打开类似于构建调试工具的，参数顺序上要放在最前面
```
CocosCreator3D 路径  --dev --project 项目路径 --build "configPath=构建参数路径"

// 实际示例
C:\software\CocosCreator3D\CocosCreator3D.exe  --dev --project C:\code\NewProjectTest_002 --build "configPath=C:\code\NewProjectTest_002\build\buildConfig_web-desktop.json"
```

## 基本测试参数组合
1. 不使用 configPath 简单按照上面的参数 `platform=wechatgame; debug=true;` 这样的，更改平台参数即可
2. 使用 configPath，可以在测试构建之后顺手导出几个平台的参数配置用于测试即可，目的是测试能正常构建；
