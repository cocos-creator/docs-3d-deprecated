# Canvas（画布）组件参考

**Canvas（画布）** 组件能够随时获得设备屏幕的实际分辨率并对场景中所有渲染元素进行适当的缩放。场景中的 Canvas 可以有多个，所有 UI 元素都必须在 Canvas 及其子节点下才能被渲染，Canvas 的设计分辨率和适配方案统一通过项目配置获取。在 Canvas 内部会自带一个相机，默认照射 z 轴方向是从 -1000 - 998，所以针对 UI 上的 z 轴设计必须在这个范围内才能正常显示（不取临界值）。

在过去的设计里 Canvas 是最后渲染的，意味着他可以遮盖 3D 的所有内容渲染，但这远远不能满足项目开发需求（例如：一个 2D 地图配合 3D 角色的功能）。因此，我们加入了 RenderMode 属性，用户可以在原有基础上决定是否切换 UI 的相机的行为为 3D 相机和 UI 相机的排序渲染。当然，这个功能是要配合 ClearFlag 来调控。

Canvas 组件不仅是 UI 渲染的根节点，同时在游戏制作时面对多分辨率适配也起到关键作用，具体请参考[多分辨率适配方案](../engine/multi-resolution.md)。

## Canvas 属性

| 属性           | 功能说明                                                 |
| -------------- | -----------                                            |
| RenderMode    | Canvas 渲染模式，**Intersperse** 下可以指定 Canvas 与场景中的相机的渲染顺序，**Overlay** 下 Canvas 会在所有场景相机渲染完成后渲染。注意：启用 **Intersperse** 模式，如果 3D 场景的相机内容显示上要在 Canvas 前面，相机的 ClearFlags 也要为 None。
| Priority       | 当 RenderMode 为 **Intersperse** 时，指定与其它相机的渲染顺序，当 RenderMode 为 **Overlay** 时，指定跟其余 Canvas 做排序使用。
| ClearFlag     | Canvas 清理屏幕缓冲区的标记，None 不清理，Depth_Stencil 清理深度缓冲，Solid_Color 清理颜色深度缓冲。
| Color     | 清理颜色缓冲区后的颜色。

## 注意事项

ClearFlag 的选择，**一个场景里必须有一个相机做 Solid_Color**，否则会导致渲染过程中的花屏或闪屏等现象。特此说明一下，如何选择做 Solid_Color 的相机，如果你的场景里只有一个 2d 或者 3d 相机的话，模式选择 Solid_Color；如果出现 2d 背景、3d 场景、2d UI 的情况，2d 背景 Solid_Color、3d 场景（Depth_Stencil）、2d UI（有模型 Depth_Stencil 避免模型闪屏，无模型 None / Depth_Stencil 皆可）。

---

### [**其他基础模块参考**](base-component.md)

### [**渲染模块参考**](render-component.md)
