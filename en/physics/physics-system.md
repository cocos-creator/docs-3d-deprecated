# Physics System

The physics system is used to manage all physics related functions. Currently, it is responsible for synchronizing physical elements, triggering physics events and scheduling iterations of the physical world.

## Physics World

When the physics world iterates, physical calculations will be made on physical elements, such as calculating whether each object collides and the force of the object. When the calculation is completed, the physics system will update the physics world to the scene world, so that the game objects will generate corresponding physical behaviors.

The current physics execution flow of __Coccos Creator__: **trigger physics events** -> **sync scene data to physics** -> **physics world iteration** -> **sync physics data to scene**.

> **Note**: There is only a single physical world, and the functional support of the multi-physics world will be discussed later.

Scene World And Physics World:

![Scene World and Physics World](img/physics-world.jpg)

## Properties

The properties of the physics system can only be set through the code for the time being. A setting panel will be added in the future, please pay attention to the update announcement.

> **Note**: Gets the instance of physics system using: `PhysicsSystem.instance`

Properties | Description
---|---
**enable** | Whether to enable the physics system, the default is `true`
**gravity** | The gravity value of the physics world, the default is `(0, -10, 0)`
**allowSleep** | Whether to allow the physics system to automatically sleep, the default is `true`
**maxSubSteps** | The maximum number of physics simulation sub-steps per frame, the default is `2`
**fixedTimeStep** | The time spent in each step of physics simulation, the default is `1/60`, note that is not every frame
**sleepThreshold** | The default speed threshold for going to sleep, the default is `0.1`
**autoSimulation** | Automatic simulation, the default is `true`
**defaultMaterial** | Get the default physics material (read only)
**raycastResults** | Gets the raycast test results (read only)
**raycastClosestResult** | Gets the raycastClosest test result (read only)
**useCollisionMatrix** | Whether to use a collision matrix, the default is `false`
**collisionMatrix** | Gets the collision matrix

## Interfaces

Property | Signature | Description
---|---|---
**isCollisionGroup** | `(g1:number, g2:number)=>boolean` | Are collisions between `g1` and `g2`?
**setCollisionGroup** | `(g1:number, g2:number, collision=true)=>void` | Sets whether collisions occur between `g1` and `g2`.
**resetCollisionMatrix** | `(mask=0xffffffff)=>void` | Reset the mask corresponding to all groups of the collision matrix to the given value
**resetAccumulator** | `(time=0)=>void` | Reset the accumulator of time to given value
