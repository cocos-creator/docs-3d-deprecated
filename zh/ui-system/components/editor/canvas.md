# Canvas（画布）组件参考

**Canvas（画布）** 组件能够随时获得设备屏幕的实际分辨率并对场景中所有渲染元素进行适当的缩放。场景中的 Canvas 可以有多个，所有 UI 元素都必须在 Canvas 及其子节点下才能被渲染，Canvas 的设计分辨率和适配方案统一通过项目配置获取。在 Canvas 内部会自带一个相机，默认照射 z 轴方向是从 0 - 1000，所以针对 UI 上的 z 轴设计必须在这个范围内才能正常显示。

Canvas 组件不仅是 UI 渲染的根节点，同时在游戏制作时还有一个很重要的功能在多分辨率适配方案，具体请参考[多分辨率适配方案](../engine/multi-resolution.md)。


<!-- 画布的脚本接口请参考[Canvas API](../../../api/zh/classes/Canvas.html)。 -->

## Canvas 属性

| 属性           | 功能说明                                                 |
| -------------- | -----------                                            |
| RenderMode    | Canvas 渲染模式，***intersperse*** 下可以指定 Canvas 与场景中的相机的渲染顺序，***overlay*** 下 Canvas 会在所有场景相机渲染完成后渲染。注意：启用 ***intersperse*** 模式，如果 3D 场景的相机内容显示上要在 Canvas 前面，相机的 clearFlags 也要为 none。
| priority       | 当 RenderMode 为 ***intersperse*** 时，指定与其它相机的渲染顺序，当 RenderMode 为 ***overlay*** 时，指定跟其余 Canvas 做排序使用。
| clearFlag     | Canvas 清理屏幕缓冲区的标记，none 不清理，depth_stencil 清理深度缓冲，solid_color 清理颜色深度缓冲。
| color     | 清理颜色缓冲区后的颜色。
---

### [**其他基础模块参考**](base-component.md)

### [**渲染模块参考**](render-component.md)
