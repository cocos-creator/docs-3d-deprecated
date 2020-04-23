# UI 变换组件

Cocos Creator 3D 大多数的节点变换操作已经在节点在节点上完成，对于 UI 部分由于涉及到 UI 尺寸以及 UI 锚点问题，所以衍生出了 UI 变换组件。一般用于点击事件的计算，以及适配的作用。

点击 **属性检查器** 下面的 **添加组件** 按钮，然后选择 **UI/UITransform** 即可添加 UITransform 组件到节点上。

## UITransform 属性介绍

| 属性 |   功能说明
| -------------- | ----------- |
| ContentSize | UI 矩形内容尺寸
| AnchorPoint | UI 矩形锚点位置
| Priority | UI 节点优先级，在当前父节点下排序，Canvas 节点顺序不受此属性影响。

---

### [**其他基础模块参考**](base-component.md)

### [**渲染模块参考**](render-component.md)
