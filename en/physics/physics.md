# Physics Introduction

__Cocos Creator 3D__ currently supports the lightweight collision detection system `builtin` and the physics engine `cannon.js` with physical simulation, and the `asm.js`/`wasm` version `ammo.js` of the powerful physics engine `bullet`, also we provides users with efficient component-based workflow and convenient methods of use.

## Physics World And Elements

Elements in the physics world can be divided into **rigid body**. We can adding physics elements by adding a collider (`Collider`) or rigid body (`RigidBody`) component to the game object. The physics system will perform calculations on these elements to make their behaviors the same with the real world.

> **Note**: The __rigid body__ here doesn't refer to the `RigidBody` component. The `RigidBody` component is used to control the properties related to the physical behavior of the rigid body.

### Adding a Physical Element

Adding a physical element to the world can be divided int the following steps：

1. Create a new shape `Cube`；
2. Click `Add Component` on the `Inspector` panel witch is on the right of editor；
3. Select `BoxColliderComponent` under the `Physics` menu, and adjust the parameters；
4. add a `RigidBodyComponent` component in order to make it have physical behavior。

In this way we get a physical element that has **both a collider and a physical behavior**.

### Perfecting The Physics World

We can add a ground to the world. Following the steps 1,2,and 3, you can add another `Plane` with **collider only**.

Then, adjust the view of the camera (select the camera and press the shortcut `Ctrl + Shift + F` to align the camera view to screen).

Finally, click the run button, you can see the changes of physical elements in the scene. The final scene is shown in the following figure:

![physics world](img/physics.jpg)

> **Note**: You can see the new preview result on the browser you just ran by clicking the refresh button directly after adjusting the property value of the component.

### Composition Of Physical Elements

A physical element can be composed of the following ways:

- A `RigidBody` component
- One or more `Collider` components
- One `RigidBody` component plus one or more collider components

## More Detailed Modules

Additional physics system will be introduced in more detail through the following modules:

Module | Description
---|---
[**Physics Options**](physics-item.md) | Introduces the optional options of low-level physics engine in **Cocos Creator 3D**
[**Physics System**](physics-system.md) | Introduces the physics system and a series of properties and interfaces of the physics system.
[**Physics Component**](physics-component.md) | Introduces some physics components and a series of properties on the panel.
[**Physics Usage**](physics-use.md) | Further introduces the use of physics, events, group masks, etc.

---

Continue to the [Physics Options](physics-item.md) documentation。
