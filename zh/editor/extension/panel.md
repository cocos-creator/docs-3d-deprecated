# 扩展面板

在实现一个功能的同时，很可能需要界面上的 UI 交互，Cocos Creator 3D 也为扩展插件提供了这种能力。

## 在扩展插件里声明面板

在 package.json 里可以定义 panels 字段。

```json
{
    "name": "hello-world",
    "panels": {
        "default": {
            "title": "world panel",
            "type": "dockable",
            "main": "./panels/default.js",
            "icon": "./static/default.png"
        },
        "list": {
            "title": "world list",
            "type": "simple",
            "main": "./panels/default.js",
            "icon": "./static/list.png",

            "flags": {},
            "size": {}
        }
    }
}
```

这个字段是个 object，定义如下：

```typescript
// panels 定义
interface PanelMap {
    [name: string]: PanelInfo;
}

// 每个 panel 的定义
interface PanelInfo {
    // 面板标题，支持 i18n:key 格式
    title: string;
    // 面板入口，一个相对路径
    main: string;
    // 面板图标，一个相对路径
    icon?: string;
    // 面板类型，默认 dockable
    type?: 'dockable' | 'simple';

    flags?: PanelFlags;
    size?: PanelSize;
}

// panel 里的一些标记
interface PanelFlags {
    // 是否允许缩放，默认 true
    resizable?: boolean;
    // 是否需要保存，默认 false
    save?: boolean;
}

// panel 的一些尺寸限制
interface PanelSize {
    width?: number;
    height?: number;
    'min-width'?: number;
    'min-height'?: number;
}
```