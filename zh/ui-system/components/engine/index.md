# UI 结构说明

UI 采用的是基于树状的渲染结构，整个 UI 的渲染都是基于 Canvas 节点（带有 CanvasComponent 的节点）作为根节点来进行，也就是 UI 节点的最终根节点必须是 Canvas 节点才可以被该 Canvas 渲染。每一个 UI 节点必须带有 UITransformComponent 组件以作为点击或者对齐策略等生效的必要条件。

在整体渲染方面，UI 采用了一套独立的渲染管线且优先级是最高的，整个渲染管线会先渲染完 3D 部分再渲染 UI。同时，UI 又通过 Canvas 节点的 CanvasComponent 组件上的 priority 来决定渲染的先后。

UI 还支持对模型进行渲染，唯一的条件是必须对带有模型组件（例如：ModelComponent / SkinningModelComponent）的节点添加 **UI/Model** 组件才可以和 UI 在相同的管线上进行渲染。

渲染数据组织以及流程如下：

![render](render.png)

# UI 规则介绍

- [渲染排序规则](priority.md)
- [多分辨率适配方案](multi-resolution.md)
- [对齐策略](widget-align.md)
- [文字排版](label-layout.md)
- [自动布局容器](auto-layout.md)
- [制作动态生成内容的列表](list-with-data.md)
- [制作可任意拉伸的 UI 图像](sliced-sprite.md)
