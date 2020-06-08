# UI 变换组件

定义了 UI 上的矩形信息，包括矩形的尺寸和锚点位置。开发者可以通过该组件任意地操作矩形的大小、位置。一般用于渲染、点击事件的计算、界面布局以及屏幕适配等。

点击 **属性检查器** 下面的 **添加组件** 按钮，然后选择 **UI/UITransform** 即可添加 UITransform 组件到节点上。

## UITransform 属性介绍

| 属性 |   功能说明
| -------------- | ----------- |
| ContentSize | UI 矩形内容尺寸
| AnchorPoint | UI 矩形锚点位置
| Priority | UI 节点优先级，在当前父节点下排序，Canvas 节点顺序不受此属性影响。

---

- [其他基础模块参考](base-component.md)

- [渲染模块参考](render-component.md)
