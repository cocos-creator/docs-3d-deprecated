# ColliderComponent

## 获取 Collider 组件

Cocos Creator 3D 目前支持两种语言进行开发，分别为 JavaScript 和 TypeScript。

**注：TypeScript 具有良好的语法分析和类型提示，推荐使用 TypeScript 进行开发**。

以获取 BoxColliderComponent 为例，在 JavaScript 中可以用以下方式获取相应的Collider组件：

1. `this.getComponent('cc.BoxColliderComponent')`
2. `this.getComponent(cc.BoxColliderComponent)`

在 TypeScript 中可以用以下方式获取相应的Collider组件：

1. 上述 javascript 使用的方式
2. **`this.getComponent(BoxColliderComponent)`** (**推荐使用，提示导入时，注意导入位置为“ cc ”**)

**注：若无智能导入提示，请检查工作目录是不是在工程的顶层，以及是否使用较新的 Vs Code 编辑器**。

## 碰撞器和触发器

Collider 组件具有 isTrigger 属性，当 isTrigger 为 true 时，表示为碰撞器，反之为触发器。

**注：关于碰撞器和触发器的区别将在 [物理事件](physics-event.md) 中介绍**。

## Collider 和 RigidBody 的关系

Collider 和 RigidBody 组件都是为了服务于物理元素，分别操控着物理元素上的一部分属性。这也意味着要了解它们之间的关系，需要先了解 Cocos Creator 3D 中的物理元素是如何构成的。

### 元素如何构成

在[**物理简介**](./../physics.md)中，介绍了一个物理元素是由 Collider 和 RigidBody 组件相互组合而成的，其中指出了一个元素只能有一个或零个 RigidBody 组件，但可以有多个 Collider 组件。

这便也意味着一个问题：父子节点链上的 Collider 如何代表物理元素？

目前有两个思路：

1. 每个节点只要有物理组件，就是一个元素，也就是说父子节点的组件无依赖关系，需要多个形状，往该节点上添加相应的 Collider 组件。

2. 从自身节点开始往父链节点上搜索，如果找到了 RigidBody 组件，则将自身的 Collider 组件绑定到该节点上，否则，整条链上的 Collider 组件共享一个 RigidBody ，元素对应的节点是最顶层的 Collider 所对应的节点。

这两个思路各有利弊：

- 思路 1 不够直观，当需要多个形状在一起的时候，只能往一个节点上加，需要显示出形状时，需要增加子节点模型。
- 思路 1 调整参数时，需要调整两个地方，分别为子节点的位置信息和父节点上对应 Collider 组件的数据信息。
- 思路 2 增加了节点耦合，节点更新时，需要更新相应的依赖节点。
- 思路 2 在节点链被破坏时，需要维护内容更多，节点链在反复被破坏时需要处理更复杂的逻辑。

**注：Cocos Creator 3D 的物理目前使用的是第一个思路，后续可能会进行调整，请留意版本更新公告**。

### Collider 的 attachedRigidbody 属性

在 Collider 组件中具有一个 attachedRigidbody 属性，这个属性可以获得当前 Collider 组件所绑定的 RigidBody 组件，但是请注意以下几点：

- 在自身节点无 RigidBody 组件时，该属性返回为 null
- attachedRigidbody 是一个只能获取并且只读的属性

---

继续前往 [Rigidbody 组件](physics-rigidbody.md) 说明文档。
