# 射线检测

目前可以分为两大类，分别为物理和 render-scene。

**注：目前所有的返回结果都是复用的**。

## 物理提供的射线检测

### 基于物理碰撞器的射线检测

Cocos Creator 3D 在 v 1.0.1 版本上提供了一套基于物理引擎的射线检测功能。

需要注意的是，这套射线检测针对的是物理世界中的碰撞器，在场景面板上与之对应的是碰撞器组件，例如 `BoxColliderComponent`。

目前检测对象是所有的物理碰撞体，由物理引擎底层提供支持，由 PhysicsSystem 提供对外接口：

- raycastAll : 根据给定的参数，检测场景中所有的碰撞体，返回布尔值, 表示是否检测成功。
- raycastClosest ：根据给定的参数，检测场景中所有的碰撞体，同样返回布尔值。

参数解释：

- worldRay 世界空间下的射线
- mask 掩码，标记可检测的层（目前暂时失效）
- maxDistance 最大检测距离，目前请勿传入 Infinity 或 Number.MAX_VALUE
- queryTrigger 是否检测触发器

通过 PhysicsSystem 获取结果：

- 获取 raycastAll 的检测结果：`PhysicsSystem.instance.raycastResults`
- 获取 raycastClosest 的检测结果：`PhysicsSystem.instance.raycastClosestResult`

## render-scene 提供的射线检测

共有两类，分别基于模型和 UITransform 组件。

### 基于模型的射线检测

目前检测对象是图元类型为三角类的模型，由 render-scene 提供接口：

- raycastAllModels: 根据给定的参数，检测场景中所有的模型，返回布尔值
- raycastSingleModel: 根据给定的参数，检测参数中的模型，返回布尔值

参数解释：

- worldRay 世界空间下的射线
- model 模型 （只有 raycastSingleModel 有这个参数）
- mask 掩码，标记可检测的层，用于筛选实例
- distance 最大检测距离

通过 render-scene 获取结果：

```
// 获取 render-scene
const renderScene = director.getScene().renderScene;

// 获取 raycastAllModels 的结果
renderScene.rayResultModels

// 获取 raycastSingleModel 的结果
renderScene.rayResultSingleModel
```

### 基于 UITransform 组件的射线检测

目前检测对象是带 ui-transform 组件的节点，底层调用的是 intersect 提供的 ray-aabb 相交性检测功能，由 render-scene 提供接口（接口的参数与模型的一样）：

- raycastAllCanvas: 根据给定的参数，检测场景中所有符合条件的 canvas 以及它的节点树，返回布尔值。

通过 render-scene 获取结果：

```
// 获取 raycastAllCanvas 的结果
renderScene.rayResultCanvas
```

**注：Canvas 拥有自己的相机实体，具体可以查看[test-case-3d](https://github.com/cocos-creator/test-cases-3d)中的示例**。

---

回到 [物理使用](physics-use.md) 说明文档。
