# 物理事件

Cocos Creator 的物理事件有触发事件和碰撞事件。

## 触发器和碰撞器

当碰撞时，触发器不会产生物理行为，而碰撞器会产生物理行为。

因此触发器是只进行碰撞检测的`Collider`，**犹如幽灵一般**。而碰撞器是既进行碰撞检测，又进行物理模拟的`Collider`。

两者的区别

- 触发器不会与其它触发器或者碰撞器做更精细的检测。
- 碰撞器与碰撞器会做更精细的检测，并会产生碰撞数据，如碰撞点、法线等。

> **注**：`isTrigger`属性决定`Collider`组件是否为触发器。

## 触发事件和碰撞事件

### 触发事件

触发事件由触发器生成，目前分为三种 `onTriggerEnter`、`onTriggerStay`、`onTriggerExit`，分别代表着触发开始，触发保持，触发结束。

监听触发事件，需要通过注册事件来添加相应的回调：

1. 通过`this.getComponent(Collider)`获取到`Collider`
2. 通过`Collider`的`on`或者`once`方法注册相应事件的回调

代码示例：

```ts
public start () {
    let Collider = this.getComponent(Collider);
    Collider.on('onTriggerStay', this.onTrigger, this);
}

private onTrigger (event: ITriggerEvent) {
    console.log(event.type, event);
}
```

### 碰撞事件

碰撞事件根据碰撞数据产生，碰撞数据只对动力学刚体产生作用，因此必须要有一个动力学刚体才能产生碰撞事件。

目前碰撞事件分为三种类型 `onCollisionEnter`、`onCollisionStay`、`onCollisionExit`，分别代表着碰撞开始，碰撞保持，碰撞结束。

监听碰撞事件，需要通过注册事件来添加相应的回调：

1. 通过 `this.getComponent(Collider)` 获取到 `Collider`
2. 通过 `Collider` 的 `on` 或者 `once` 方法注册相应事件的回调

代码示例：

```ts
public start () {
    let Collider = this.getComponent(Collider);
    Collider.on('onCollisionStay', this.onCollision, this);
}

private onCollision (event: ICollisionEvent) {
    console.log(event.type, event);
}
```

> **注**：`Collider`是所有碰撞组件的父类。
> **注**：目前碰撞事件以物理元素为单位，所有该元素上的碰撞器组件都会接受到碰撞事件。

两者的区别

- 触发事件由触发器生成，碰撞事件根据碰撞数据生成。
- 触发事件可以由触发器和另一个触发器或者另一个碰撞器产生。
- 碰撞事件需要由两个碰撞器产生，并且至少有一个动力学刚体。

---

继续前往 [分组和掩码](physics-group-mask.md) 说明文档。
