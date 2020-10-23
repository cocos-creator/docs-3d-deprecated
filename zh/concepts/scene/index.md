# 场景制作工作流程

场景是游戏中的环境因素的抽象集合，是创建游戏环境的局部单位，我们可以理解为游戏开发设计人员通过在编辑器中制作一个场景，来表现游戏中的一部分世界内容。

![scene world](./scene/world01.jpg)

## 场景结构

Cocos Creator 3D 在 Creator 的 ECS 实体组件系统框架上增加了 3D 场景结构。3D 场景通过 RenderScene 表现，ECS 结构中相应的 Component 引用了 RenderScene 中维护的模型（Model）、相机（Camera）、灯光（Light）等元素。这些元素通过 Node 关联在一起，RenderScene 中更新元素的各种 Transform，也是通过 Node 来实现。

ECS 结构中的 Scene 与 3D 场景结构中的 RenderScene 区别在于：
- ECS 中的 Scene 是作为 Node 的逻辑组织结构，且 ECS Scene 中包含了 RenderScene 成员变量，两者一一对应。
- RenderScene 是作为场景渲染元素的组织结构。

ECS 与 3D 场景结构的关系，如下图所示：

![ecs & scene](./scene/ecs-scene.jpg)

可以看到整个 3D 场景结构是被封装在 Component 之下的，然后通过 Node 建立起组织关系。这样对于 ECS 的上层用户来说是完全透明的，而用户层面对于 3D 场景的结构对象是无感知的。

## 场景制作相关工作流程

- [节点和组件](node-component.md)
- [坐标系和节点属性变换](coord.md)
- [节点层级和显示顺序](node-tree.md)
- [使用场景编辑器搭建场景](scene-editing.md)
- [天空盒](skybox.md)
- [全局雾](fog.md)
- [阴影](shadow.md)

---

继续前往 [节点和组件](node-component.md) 说明文档。
