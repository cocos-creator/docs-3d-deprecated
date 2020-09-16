# Extension panel

While implementing a function, it is likely to require UI interaction on the interface. Cocos Creator 3D also provides this ability for extension.

## Declare the panel in the extension

The panels field can be defined in package.json.

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

This field is an object, defined as follows:

```typescript
// panels definition
interface PanelMap {
    [name: string]: PanelInfo;
}

// The definition of each panel
interface PanelInfo {
    // Panel title, supports i18n:key format
    title: string;
    // Panel entry, a relative path
    main: string;
    // Panel icon, a relative path
    icon?: string;
    // Panel type, default dockable
    type?:'dockable' |'simple';

    flags?: PanelFlags;
    size?: PanelSize;
}

// Some tags in the panel
interface PanelFlags {
    // Whether to allow zoom, default true
    resizable?: boolean;
    // Need to save, default false
    save?: boolean;
}

// Some size limitations of panel
interface PanelSize {
    width?: number;
    height?: number;
    'min-width'?: number;
    'min-height'?: number;
}
```