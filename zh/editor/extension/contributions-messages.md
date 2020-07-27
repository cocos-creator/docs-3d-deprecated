# 消息通信

Cocos Creator 3D 编辑器内，所有的功能交互都是通过 [消息系统](./messages.md) 进行交互。

而消息也需要在 contributions 里定义后才能使用。

## 查看已有功能的公开消息

编辑器在顶部菜单 "开发者" - "消息列表" 里，预置了一个消息管理面板，面板里可以显示每个功能定义的消息以及说明。

## 定义一条消息

```json5
{
    "name": "hello-world",
    "contributions": {
        "messages": {
            // name 是消息的名称
            "name": {
                "public": false,
                "description": "",
                "doc": "",
                "methods": []
            }
        }
    }
}
```

### public 

类型 {string}

是否对外显示这条消息，如果为 true，则会在消息列表界面显示这条消息的基本信息。

### description

类型 {string}

如果 public 为 true，则会在消息列表显示一些简单的描述，支持 i18n:key 语法

### doc

类型 {string}

如果 public 为 true，则会显示这条消息的一些文档，支持 i18n:key 语法。

这个文档使用 markdown 格式撰写并渲染。

### methods

类型 {string[]}

该条消息触发的方法数组。

这是一个字符串数组，字符串为扩展插件或者面板上的方法（methods）。
如果是扩展插件上的方法，则直接定义 "methodName"，如果要触发面板上的方法，则要填写 "panelName.methodName"（panel.methods）

## 定义广播消息

开发一个扩展插件的额时候，完成一个动作后需要向其他功能发送一些通知，这些通知也需要显示在 "消息列表" 界面的话，可以这样定义消息：

```json5
{
    "name": "hello-world",
    "contributions": {
        "messages": {
            "hello-world:ready": {
                "public": true,
                "description": "hello-world 插件准备就绪"
            }
        }
    }
}
```