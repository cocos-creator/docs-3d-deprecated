# 扩展已有的功能

Cocos Creator 3D 支持各个功能间的互相扩展。

我们在编写一个扩展插件的时候，可以查询编辑器内已有功能是否提供了对外扩展的能力。如果对应功能提供了扩展能力，则能够在编写扩展插件的时候使用这些能力。

## 扩展数据定义

扩展功能，统一在 package.json 里的 contributions 字段定义。

```json5
{
    "name": "hello-world",
    "contributions": {}
}
```

contributions 定义规范:

```typescript
interface contributions {
    [name: string]: any;
}
```

name 是被扩展的功能或者另一个插件的名字， value 则是 any 类型，由被扩展的那个功能插件的作者自行定义。

现阶段只开放了对编辑器内部功能的扩展，未来我们将提供扩展插件的扩展能力。
