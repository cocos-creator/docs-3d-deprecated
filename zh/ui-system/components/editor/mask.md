# Mask（遮罩）组件参考

Mask 用于规定子节点可渲染的范围，带有 Mask 组件的节点会使用该节点的约束框（也就是 **属性检查器** 中 Node 组件的 **Size** 规定的范围）创建一个矩形渲染遮罩，该节点的所有子节点都会依据这个遮罩进行裁剪，遮罩范围外的将不会渲染。

<!-- ![](mask/mask.png) -->

点击 **属性检查器** 下面的 **添加组件** 按钮，然后从 **UI** 中选择 **Mask**，即可添加 Mask 组件到节点上。注意该组件不能添加到有其他渲染组件（如 **Sprite**、**Label** 等）的节点上。

<!-- 遮罩的脚本接口请参考 [Mask API](../../../api/zh/classes/Mask.html)。 -->

## Mask 属性

| 属性  |   功能说明           |
| -------------- | ----------- |
| Type           | 遮罩类型。包括 **RECT**、**ELLIPSE**、**GRAPHICS_STENCIL** <!--、**IMAGE_STENCIL** 三种-->类型。|
| Segments      | 椭圆遮罩的曲线细分数，只在遮罩类型设为 **ELLIPSE** 时生效 |
| Inverted       | 反向遮罩

**注意**：节点添加了 Mask 组件之后，所有在该节点下的子节点，在渲染的时候都会受 Mask 影响，**GRAPHICS_STENCIL** 类型在这里没有做任何引擎需要的事，只是放开了对 graphics 操控，用户可以使用这个 graphics 来绘制自定义图形。

---

### [**其他渲染模块参考**](render-component.md)

### [**基础模块参考**](base-component.md)
