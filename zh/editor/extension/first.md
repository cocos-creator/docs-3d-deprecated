# 第一个扩展包

我们将通过本文，学会创建一个 Creator3D 扩展包，并通过扩展包执行一段自定义脚本。

## 创建并安装扩展包

创建一个空的文件夹，并命名为 "hello-world"，并在文件夹内创建 browser.js 和 package.json 两个文件。扩展包的结构大致如下：

```
hello-world
  |--main.js
  |--package.json
```

将该文件夹放入到 ~/.CocosEditor3D/packages（Windows 用户为 C:\Users\${你的用户名}\.CocosEditor3D\packages），或者放入到 ${你的项目路径}/packages 文件夹下。然后通过编辑器顶部菜单的 "扩展" - "插件管理器" 进行开关管理。

## 定义包描述文件 package.json

每个扩展包都需要有一份 package.json 文件，用于描述改扩展包的用途。只有完整定义了包描述文件 package.json 后，Cocos Creator 3D 编辑器才能知道这个包具体的功能，加载入口等。

虽然 package.json 在很多字段上的定义和 node.js 的 npm-package 相似，它们仍然是为不同的产品服务的，所以从 npm 社区中下载的包，并不能直接放入到 Cocos Creator 中变成插件，但是可以使用它。

```json
{
    "name": "hello-world",
    "version": "1.0.0",
    "main": "./browser.js",
    "description": "一份简单的扩展包",
    "contributions": {
        "menu": [{
            "path": "Develop",
            "label": "test",
            "message": "log"
        }],
        "messages": {
            "log": {
                "methods": ["log"]
            }
        }
    }
}
```

我们需要在 contributions 内定义一个 messages 对象，这里是向编辑器的消息系统注册对应的消息，这个消息绑定一个或多个的扩展内定义的方法。更多定义数据请参考 [消息通信](contributions-messages.md)。

然后需要在 contributions 内再定义一个 menu 数组，向 menu 组件提供一个菜单的基础信息，并绑定到一条的消息。更详细的请参考：[扩展主菜单](./contributions-menu.md)

关于详细的 package.json 格式定义，请参考 [扩展包定义](./define.md)。

## 入口程序 browser.js

定义好描述文件以后，接下来就要书写入口程序 browser.js 了。

内容如下:

```javascript
'use strict';

// 扩展内定义的方法
exports.methods = {
    log() {
        console.log('Hello World');
    },
};

// 当扩展被启动的时候执行
exports.load = {};

// 当扩展被关闭的时候执行
exports.unload = {};
```

这份入口程序会在 Cocos Creator 的启动过程中被加载。methods 内定义的方法，将会作为操作的接口，通过 [消息系统](messages.md) 跨扩展调用，或者是和面板通信。

## 运行扩展包

现在，我们可以打开 Cocos Creator 3D，找到并打开顶部的 "扩展" - "插件管理器"，在面板上选择扩展位置（全局或者项目），然后在列表里启动，关闭，或者重启对应的扩展。

如果插件已经启动，在顶部菜单区域会出现一个 Develop 菜单，里面有一个 test 菜单项。点击后，会根据定义触发消息发送，并根据消息定义的触发方法，执行到插件里的对应方法，然后在控制台打印出  "Hello World" 的日志信息。

恭喜你已经编写了第一个简单的编辑器扩展插件。