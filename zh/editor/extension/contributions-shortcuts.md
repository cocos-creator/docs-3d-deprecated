# 快捷键

编辑器内的快捷键由 "快捷键管理器" 统一管理。每一个快捷键都需要绑定一个消息，当快捷键按下的时候，触发绑定的消息。

## 定义一个快捷键

```json5
{
    "name": "hello-world",
    "contributions": {
        "messages": {
            "undo": {
                "title": "i18n:hello.messages.undo.title",
                "methods": ["say-undo"]
            }
        },
        "shortcuts": [
            {
                "message": "undo",
                "win": "ctrl+z",
                "mac": "cmd+z",
            }
        ]
    }
}
```

contributions.messages 参看 [消息通信](./contributions-messages.md)

contributions.shortcuts 参数说明:

### message

快捷键绑定的消息，当这个快捷键被触发的时候，发送这条消息

### win

在 windows 平台上，监听的按键

### mac

在 MacOS 上，监听的按键