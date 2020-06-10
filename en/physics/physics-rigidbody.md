#  RigidBodyComponent

## Get RigidBodyComponent

Code example of `TypeScript`: `const rigidBody = this.getComponent(RigidBodyComponent);`

## Rigid Body Type

Rigid bodies are generally divided into three types, `static`, `dynamic`, and `kinematic`.

- **static**, which means a static rigid body, like a stone with a huge mass, specifically a mass with a mass of `0`, or only physical elements with collision components.
- **dynamic**, which means that a dynamic rigid body can **be subjected to forces**, specifically those with a mass greater than `0` and `isKinematic` being `false`.
- **kinematic**, which means kinematic rigid body, the user controls the movement of the rigid body, specifically the mass is greater than `0` and `isKinematic` is `true`.

## Sleep And Wake Rigid Body

Code example:

```ts
if (rigidBody.isAwake) {
    rigidBody.sleep();
}
if (rigidBody.isSleeping) {
    rigidBody.wakeUp();
}
```

## Let The Rigid Body Move

To move a rigid body, you need to change the speed of the rigid body. Currently, there are several ways to change the speed of the rigid body:

### By Gravity

The rigid body component provides the `useGravity` property, set it to `true` and the rigid body will be affected by gravity.

### By Applying Force

The rigid body component provides an `applyForce` interface with the signature: `applyForce (force: Vec3, relativePoint?: Vec3)`.
According to Newton's second law `F = m * a`, a force is applied to a certain point of the rigid body, so that there is acceleration, and the speed will change with the acceleration with time, which will cause the rigid body to move.

Code example: `rigidBody.applyForce(new Vec3(200, 0, 0));`

### By Applying Torsional Force

The rigid body component provides the `applyTorque` interface with the signature: `applyTorque (torque: Vec3)`.
Through this interface, you can apply torque to the rigid body, because it only affects the rotation axis, so no longer need to specify the point of action.

### By Applying Impulse

The rigid body component provides the `applyImpulse` interface, with the signature: `applyImpulse (impulse: Vec3, relativePoint?: Vec3)`.
According to the equation of conservation of momentum `F * Δt = m * Δv`, impulse is applied to a certain point of the rigid body. Since the mass of the object is constant, the speed will change immediately and the rigid body will move.

Code example: `rigidBody.applyImpulse(new Vec3(5, 0, 0));`

### By Directly Changing The Speed

-Linear speed
The rigid body component provides the `setLinearVelocity` interface, which can be used to change the linear velocity. The signature is: `setLinearVelocity (value: Vec3)`.
- spinning speed
The rigid body component provides the `setAngularVelocity` interface, which can be used to change the rotation speed. The signature is: `setAngularVelocity (value: Vec3)`.

Code example:

```ts
rigidBody.setLinearVelocity(new Vec3(5, 0, 0));
rigidBody.setAngularVelocity(new Vec3(5, 0, 0));
```

## Limit Movement Of Rigid Body

## By Sleeping

When Sleeping the rigid body, all the force and speed of the rigid body will be emptied, which will stop the rigid body.

**Note: Currently application of force or impulse, and changing the speed will wake up the rigid body again, and subsequent adjustments may be made, please pay attention to the version update announcement**.

## By Damping

The rigid body component provides `linearDamping` and `angularDamping` properties, which are used to set linear and rotational damping, respectively.
The damping parameter can range from `0` to infinity, `0` means no damping, and infinite means full damping.

## By Fixed Rotation

The rigid body component provides the `fixedRotation` property. The default is `false`. Setting it to `true` can be used to fix the rigid body so that it does not rotate.

## By Factor

The rigid body component provides the `linearFactor` and `angularFactor` properties, which are used to set the linear and rotation factors, respectively.
The factor is the type of `Vec3`. The value of the corresponding component is used to scale the speed change of the corresponding axis. The default value is `1`, which means that the scaling is `1` times, that is, no effect.

**Note: Set a certain component value of the factor to `0`, you can fix a certain axis of movement or rotation, if you want to completely fix the rotation, please use `fixedRotation`**.

---

Continue to the [physics event](physics-event.md) documentation.
