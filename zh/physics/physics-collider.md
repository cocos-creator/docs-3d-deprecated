# ColliderComponent

## 获取碰撞体组件

Cocos Creator 3D 目前支持两种语言进行开发，分别为`JavaScript`和`TypeScript`。

**注：`TypeScript`具有良好的语法分析和类型提示，推荐使用**。

以获取`BoxColliderComponent`为例，在`JavaScript`中获取`Collider`组件：

1. `this.getComponent('cc.BoxColliderComponent')`
2. `this.getComponent(cc.BoxColliderComponent)`

在`TypeScript`中获取`Collider`组件：

1. 上述`JavaScript`使用的方式
2. **`this.getComponent(BoxColliderComponent)`** (**推荐使用，提示导入时，注意导入位置为`cc`**)

**注：若无智能导入提示，请检查工作目录是不是在工程的顶层，以及是否使用较新的`VSCode`编辑器**。

## 碰撞器和触发器

`Collider`组件具有`isTrigger`属性，当`isTrigger`为`true`时，表示为触发器，反之为碰撞器。

**注：关于碰撞器和触发器的区别将在 [物理事件](physics-event.md) 中介绍**。

## `Collider`和`RigidBody`的关系

首先，`Collider`和`RigidBody`组件都是为了服务于物理元素，分别操控着物理元素上的一部分属性。这也意味着要了解它们之间的关系，需要先了解 Cocos Creator 3D 中的物理元素是如何构成的。

### 元素如何构成

在[**物理简介**](physics.md)中，介绍了一个物理元素是由`Collider`和`RigidBody`组件相互组合而成的，其中指出了物理元素只能有一个或零个`RigidBody`组件，并且可以有多个`Collider`组件。

单个节点是很容易看出是否有物理元素的，但如果我们以节点链为单位，这样将会很难看出物理元素是由哪些节点以及哪些组件组成的。

对于节点链的情况，目前有两个思路：

1. 每个节点只要有物理组件，就是一个元素，也就是说父子节点的组件无依赖关系，需要多个形状，往该节点上添加相应的`Collider`组件。

2. 从自身节点开始往父链节点上搜索，如果找到了`RigidBody`组件，则将自身的`Collider`组件绑定到该节点上，否则整条链上的`Collider`组件将共享一个`RigidBody`组件，元素对应的节点是最顶层的`Collider`组件所对应的节点。

这两个思路各有利弊：

- 思路一不够直观，多个形状只能往一个节点上加，显示形状需要增加子节点模型。
- 思路一调整参数时，需要调整两个地方，分别为子节点的位置信息和父节点上对应`Collider`组件的数据信息。
- 思路二增加了节点耦合，节点更新时，需要更新相应的依赖节点。
- 思路二在节点链被破坏时，需要维护内容更多，节点链在反复被破坏时需要处理复杂的逻辑。

**注：Cocos Creator 3D 的物理目前使用的是思路一，后续可能会进行调整，请留意版本更新公告**。

### `Collider`的`attachedRigidbody`属性

在`Collider`组件中具有`attachedRigidbody`属性，此属性可获得当前`Collider`组件所绑定的`RigidBody`组件，但是请注意以下几点：

- 在自身节点无`RigidBody`组件时，该属性返回为`null`。
- `attachedRigidbody`是一个只读的属性。

## 物理材质

碰撞体拥有物理材质属性，相关内容在[物理材质](physics-material.md)中有详细介绍，这里主要介绍共享和非共享的接口区别。

目前`Collider`组件提供了两个属性去访问和设置，分别为`material`和`sharedMaterial`，它们的区别主要如下：

1. 设置`sharedMaterial`或者`material`是一样的效果，在没有调用这些接口之前是共享状态，当发现设置的与当前引用不是同一实例时，后面获取`material`将不会生成新的材质实例，此时是非共享状态。

2. 在共享状态前提下，获取`material`将会生成新的材质实例，以确保只有当前碰撞体引用了该材质，这样修改时不会影响到其他的碰撞体，之后就是非共享状态了。

3. 获取`sharedMaterial`不会生成新的，而是直接返回引用。

---

继续前往 [物理材质](physics-material.md) 说明文档。
