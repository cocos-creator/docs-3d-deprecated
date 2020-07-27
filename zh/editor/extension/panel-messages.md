# 面板与扩展插件的通信

一些实用工具或者是简单的功能可以直接写在面板上，但是面板毕竟不是可靠的数据存储位置，窗口随时可能被关闭，面板也会被关闭。

最常见的例子就是面板被拖拽停靠到主窗口里。这时候面板会先关闭，然后重新打开，而数据如果不进行存储和备份，则会丢失。

这时候就需要与扩展插件主体进行一定程度的数据交互。

在看这章节前，需要对 [消息系统](./messages.md) 有一定程度的了解。

## 定义扩展插件上和面板的方法

首先我们定义一份 package.json

```json5
{
    "name": "hello-world",
    "main": "./browser.js",
    "panels": {
        "default": {
            "title": "hw",
            "main": "./panel.js"
        }
    },
    "contributions": {
        "messages": {
            "upload": {
                "methods": ["saveData"]
            },
            "push": {
                "methods": ["default.receiveData"]
            }
        }
    }
}
```

然后定义扩展插件的 main 文件 - browser.js：

```javascript
exports.methods = {
    saveData(path, data) {
        // 收到数据后打印到控制台
        console.log(path, data);
    }
};

exports.load = function() {};
exports.unload = function() {};
```

然后定义面板的 main 文件：

 ```javascript
 exports.methods = {
    async receiveData(data) {
        // 收到数据后上传到扩展插件进程
        await Editor.Message.request('hello-world', 'upload', 'abc', data);
    }
 };

exports.ready = function() {};
exports.close = function() {};
 ```

 ## 发送消息

 当我们定义好插件和插件面板后，就可以尝试触发这些消息。

 按下 ctrl(cmd) + shift + i 打开控制台。在控制台打开面板:

 ```javascript
 // default 可以省略，如果面板名字是非 default，则需要填写 'hello-world.xxx'
 Editor.Panel.open('hello-world');
 ```

 打开后，发送消息指令：

 ```javascript
 Editor.Message.send('hello.world', 'push', { test: 1 });
 ```

 执行完这条命令后，Message 系统首先根据 messages 里的 upload 定义（"methods": ["default.receiveData"]），将消息发给刚刚打开的 default 面板，触发面板上的 receiveData 方法。

 而这个方法里，又发送了一个消息，发送给 hello-world 插件的 upload 消息。
 根据 messages 里的 upload 定义（"methods": ["saveData"]），这条消息将触发扩展插件进程里的 saveData 方法。所以在控制台将打印出一条消息。

 至此，我们完成了一次和外部与面板，面板与扩展插件进程的交互动作。
 