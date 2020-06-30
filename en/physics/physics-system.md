# Physics System

The physics system is used to manage all physics related functions. Currently, it is responsible for synchronizing physical elements, triggering physics events and scheduling iterations of the physical world.

## Physics World

When the physics world iterates, physical calculations will be made on physical elements, such as calculating whether each object collides and the force of the object. When the calculation is completed, the physics system will update the physics world to the scene world, so that the game objects will generate corresponding physical behaviors.

The current physics execution flow of __Coccos Creator 3D__: **trigger physics events** -> **sync scene data to physics** -> **physics world iteration** -> **sync physics data to scene**.

> **Note**: There is only a single physical world, and the functional support of the multi-physics world will be discussed later.

Scene World And Physics World:

![Scene World and Physics World](img/physics-world.jpg)

## Physics System Properties

The properties of the physics system can only be set through the code for the time being. A setting panel will be added in the future, please pay attention to the update announcement.

Properties | Description
---|---
**enable** | Whether to enable the physics system, the default is `true`
**allowSleep** | Whether to allow the physics system to automatically sleep, the default is `true`
**useFixedTime** | Whether the physical simulations used a fixed time(the number of steps will be fixed to `1`), the default is `true`
**maxSubStep** | The maximum number of physical simulation sub-steps per frame, the default is `2`
**deltaTime** | The time spent in each step of physical simulation, the default is `1/60`, note that is not every frame
**gravity** | The gravity value of the physical world, the default is `(0, -10, 0)`
**defaultMaterial** | Get the default physics material (read only)

Obtain the instance of physics system using: `PhysicsSystem.instance`

---

Continue to the [physics component](physics-component.md) documentation.
