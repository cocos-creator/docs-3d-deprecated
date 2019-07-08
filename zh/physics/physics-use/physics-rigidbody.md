# RigidBodyComponent

## 获取刚体组件

与 ColliderComponent 中获取组件的方式类似，以下为 TypeScript 的代码示例：
`const rigidBody = this.getComponent(RigidBodyComponent);`

## 休眠和唤醒刚体

代码示例：

```
if (rigidBody.isAwake) {
    rigidBody.sleep(); // 休眠
}
if (rigidBody.isSleeping) {
    rigidBody.wakeUp(); // 唤醒
}
```

## 让刚体运动起来

让刚体运动，需要改变刚体的速度，目前改变刚体的速度有以下几种方式：

### 通过重力

刚体组件提供了`useGravity`属性，设置为`true`将受到重力的作用。

### 通过力

刚体组件提供了`applyForce`接口，签名为：`applyForce (force: Vec3, position?: Vec3)`。
根据牛顿第二定律`F = m * a`，对刚体某点上施加力，这样就有了加速度，随着时间变化，速度会随加速度变化，就会使得刚体运动起来。

代码示例：`rigidBody.applyForce(new Vec3(200, 0, 0));`

### 通过冲量

刚体组件提供了`applyImpulse`接口，签名为：`applyImpulse (impulse: Vec3, position?: Vec3)`。
根据动量守恒的方程式 `F * Δt = m * Δv`，对刚体某点施加冲量，随着时间增加，但物体的质量是恒定的，速度就会产生变化，刚体就会运动起来。

代码示例：`rigidBody.applyImpulse(new Vec3(5, 0, 0));`

### 通过直接改变速度

- 线性速度
刚体组件提供了`setLinearVelocity`接口，可用于改变线性速度，签名为：`setLinearVelocity (value: Vec3)`。
- 旋转速度
刚体组件提供了`setAngularVelocity`接口，可用于改变旋转速度，签名为：`setAngularVelocity (value: Vec3)`。

代码示例：

```
rigidBody.setLinearVelocity(new Vec3(5, 0, 0)); // 改变线性速度
rigidBody.setAngularVelocity(new Vec3(5, 0, 0)); // 改变旋转速度
```

## 限制刚体的运动

## 通过休眠

休眠刚体时，会将刚体所有的力和速度清空，这将使刚体停下来。

**注：目前，施加力或冲量，以及改变速度会重新唤醒刚体，后续可能会进行调整，请留意版本更新公告**。

## 通过阻尼

刚体组件提供了`linearDamping`和`angularDamping`属性，分别用于设置线性和旋转的阻尼。
阻尼参数的范围可以在 0 到无穷之间，0 意味着没有阻尼，无穷意味着满阻尼，通常来说，阻尼的值应在 0 到 0.1 之间。

## 通过固定旋转

刚体组件提供了`fixedRotation`属性，默认为 false，设置为 true 可以用于固定刚体，使其不会产生旋转。

## 通过因子

刚体组件提供了`linearFactor`和`angularFactor`属性，分别用于设置线性和旋转的因子。
因子是`Vec3`的类型，相应分量的数值用于缩放相应轴向的速度变化，默认值都为 1，代表着缩放为 1 倍，即无影响。

**注：将因子某分量值设置为 0，可以固定某个轴向的移动或旋转，如果要完全固定旋转，请用 fixedRotation**。

### [**继续下一篇** 物理事件](physics-event.md)

### [**或者回到** 物理使用](./../physics-use.md)
