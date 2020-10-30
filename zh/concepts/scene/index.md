# 场景制作工作流程

场景是游戏中的环境因素的抽象集合，是创建游戏环境的局部单位，我们可以理解为游戏开发设计人员通过在编辑器中制作一个场景，来表现游戏中的一部分世界内容。

![scene world](./scene/world01.jpg)

## 场景结构

Cocos Creator 在组件系统上实现了 3D 场景结构。场景通过 RenderScene 表现，Component 引用了 RenderScene 中维护的模型（Model）、相机（Camera）、灯光（Light）等元素。这些元素通过 Node 关联在一起，RenderScene 中更新元素的各种 Transform，也是通过 Node 来实现。

## 场景制作相关工作流程

- [节点和组件](node-component.md)
- [坐标系和节点属性变换](coord.md)
- [节点层级和显示顺序](node-tree.md)
- [使用场景编辑器搭建场景](scene-editing.md)
- [天空盒](skybox.md)
- [全局雾](fog.md)
- [阴影](shadow.md)
