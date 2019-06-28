# 物理事件

Cocos3D 的物理事件有触发事件和碰撞事件，分别由触发器和碰撞器产生。

## 触发器和碰撞器

当碰撞时，触发器不会会产生物理行为，而碰撞器会产生物理行为，所以触发器是只进行碰撞检测的 Collider，而碰撞器是既进行碰撞检测，又进行物理模拟的 Collider。

两者的区别

- 触发器不会与其它触发器或者碰撞器做更精细的检测。
- 碰撞器与碰撞器会做更精细的检测，并会提供因碰撞产生的一些额外的数据，如碰撞点、法线等。

**注：设置一个 Collider 组件为触发器，可以通过设置 Collider 组件的 isTrigger 属性**。

## 触发事件和碰撞事件

### 触发事件

Cocos3D 中的触发事件由触发器生成，目前分为三种 onTriggerEnter、onTriggerStay、onTriggerExit，分别代表着触发开始，触发保持，触发结束。

监听触发事件，可以通过注册事件的方式来添加触发后的回调，以下步骤可以完成触发事件的监听：

1. 通过 `this.getComponent(ColliderComponent)` 获取到 `ColliderComponent`
2. 通过 `ColliderComponent` 的 `on` 或者 `once` 方法注册相应事件的回调

代码示例：

```
public start () {
    let Collider = this.getComponent(ColliderComponent);
    Collider.on('onTriggerStay', this.onTrigger, this);
}

private onTrigger (event: ITriggerEvent) {
    console.log(event.type, event);
}
```

### 碰撞事件

Cocos3D 中的碰撞事件由碰撞器生成，目前分为三种 onCollisionEnter、onCollisionStay、onCollisionExit，分别代表着碰撞开始，碰撞保持，碰撞结束。

监听碰撞事件，可以通过注册事件的方式来添加碰撞后的回调，以下步骤可以完成碰撞事件的监听：

1. 通过 `this.getComponent(ColliderComponent)` 获取到 `ColliderComponent`
2. 通过 `ColliderComponent` 的 `on` 或者 `once` 方法注册相应事件的回调

代码示例：

```
public start () {
    let Collider = this.getComponent(ColliderComponent);
    Collider.on('onCollisionStay', this.onCollision, this);
}

private onCollision (event: ICollisionEvent) {
    console.log(event.type, event);
}
```

**注：ColliderComponent 是所有碰撞组件的父类**。

两者的区别

- 触发事件由触发器生成，碰撞事件由碰撞器生成。
- 触发事件可以由一个触发器和另一个触发器或者另一个碰撞器产生，而碰撞事件需要由两个碰撞器产生。

## [**继续下一篇** 分组和掩码](physics-group-mask.md)

## [**或者回到** 物理使用](./../physics-use.md)
