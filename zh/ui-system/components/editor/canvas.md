# Canvas（画布）组件参考

**Canvas（画布）** 组件能够随时获得设备屏幕的实际分辨率并对场景中所有渲染元素进行适当的缩放。场景中的 Canvas 可以有多个，所有 UI 元素都必须在 Canvas 及其子节点下才能被渲染，Canvas 的设计分辨率和适配方案统一通过项目配置获取。在 Canvas 内部会自带一个相机，默认照射 z 轴方向是从 0 - 1000，所以针对 UI 上的 z 轴设计必须在这个范围内才能正常显示。

Canvas 组件不仅是 UI 渲染的根节点，同时在游戏制作时还有一个很重要的功能在多分辨率适配方案，具体请参考[多分辨率适配方案](../engine/multi-resolution.md)。


<!-- 画布的脚本接口请参考[Canvas API](../../../api/zh/classes/Canvas.html)。 -->

## Canvas 属性

| 属性           | 功能说明                                                 |
| -------------- | -----------                                            |
| RenderMode    | canvas渲染模式，intersperse下可以指定canvas与场景中的camera的渲染顺序，overlay下canvas会在所有场景camera渲染完成后渲染。
| priority       | 当RenderMode为intersperse时，指定与其它camera的渲染顺序，当RenderMode为overlay时，指定跟其余 Canvas 做排序使用。      |
| clearFlag     | canvas清理屏幕缓冲区的标记，none不清理，depth_stencil清理深度缓冲，solid_color清理颜色深度缓冲。
| color     | 清理颜色缓冲区后的颜色。
---

### [**其他基础模块参考**](base-component.md)

### [**渲染模块参考**](render-component.md)
