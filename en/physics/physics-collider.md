# Collision Component

## Getting the Collider Component
The following code works in both __JavaScript__ and __TypeScript__:

```javascript
this.getComponent('cc.BoxCollider')

- or -

this.getComponent(cc.BoxCollider)
```

## Collider and Trigger

The `Collider` component has an `isTrigger` attribute. When `isTrigger` is `true`, it is represented as a trigger, otherwise it is a collider.

> **Note**: to learn more about he differences between colliders and triggers please review the [Physics Event](physics-event.md) documentation.

## The relationship between `Collider` and `RigidBody`

The `Collider` and `RigidBody` components are meant to serve physical elements, and control a part of the attributes on the physical elements respectively. This also means that to understand the relationship between them, you need to first understand how the physical elements in __Cocos Creator 3D__ are composed.

### How Elements Are Composed

In the [Introduction to Physics](physics.md) documentation, it is introduced that a physical element is composed of a `Collider` and `RigidBody` components. This indicates that there can only be one or zero physical elements `RigidBody` components, and there can be multiple `Collider` components.

It is easy to see if there is a physical element for a single node, but if we consider the node chain as a single unit, it will be difficult to see which nodes and which components the physical element is composed of.

There are two ways to consider viewing the the node chain:

1. As long as each node has a physics component, it is an element, which means that the components of the parent and child nodes have no dependencies and require multiple shapes. Add the corresponding `Collider` component to the node.

2. Start searching from the own node to the parent chain node. If the `RigidBody` component is found, bind its own `Collider` component to the node, otherwise the `Collider` component on the entire chain will share a ` RigidBody` component, the node corresponding to the element is the node corresponding to the topmost `Collider` component.

These two ideas have their pros and cons:

- Idea 1 is not intuitive enough. Multiple shapes can only be added to one node. To display the shape, you need to add a child node model.
- When adjusting the parameters in Idea 1, two places need to be adjusted, namely the position information of the child node and the data information of the corresponding `Collider` component on the parent node.
- Idea 2 adds the coupling of node. When a node is updated, the corresponding dependent node needs to be updated.
- When the node chain is destroyed, more content needs to be maintained, and the node chain needs to deal with complex logic when it is repeatedly destroyed in Idea 2.

> **Note**: the physics engine in __Cocos Creator 3D__ is currently using idea 1, which may be adjusted in the future.

### `attachedRigidbody` property of `Collider`

The `Collider` component has an `attachedRigidbody` property. This property can get the `RigidBody` component bound to the current `Collider` component, but please note the following points:

- When there is no `RigidBody` component in its own node, this property is returned as `null`.
- `attachedRigidbody` is a read-only property.

## Auto-Scaling

Each component is bound to a node, some components dynamically update their data based on the bound node, collider components update their data based on the node The information automatically updates the corresponding shape data, allowing collision bodies to more easily fit the rendered model.

Updating data to model components as an example.

The model component automatically updates the model's world matrix according to the bound nodes, thus changing the node's position, scaling, rotation, etc. , which allows the rendered model to have a corresponding affine transformation.

However, some properties of collider shape lead to a different treatment of scaling.

- Collider shape are generally described by their geometry structure.
- Most of the collider shape are of the convex hull type.

These properties restrict transformations such as tangential, non-uniform scaling, etc., taking the sphere collider as an example.

Assume that the scaling information of the bound nodes is `(1,2,1)` (non-uniform scaling), since the structure described by the model and the collider is not  same, the sphere model is represented using multiple primitives (e.g., triangles) that, when scaled, model shape into a pebble-like shape; however, the sphere collider is described by the size of the radius, which is scaled in the dimension with the largest value (to ensure that the collision body can be as large as possible of the enclosing model), but scaled it is still a sphere.

![non-uniform-scale](img/collider-non-uniform-scale.jpg)

## PhysicsMaterial

The collision body has physics material properties. Related content is described in detail in the [physics material](physics-material.md)documentation. The difference between shared and non-shared interfaces is mainly introduced.

The `Collider` component provides two properties to `get` and `set`. These are `material` and `sharedMaterial`, and their differences are as follows:

1. Setting `sharedMaterial` or `material` has the same effect. It is in a shared state before calling these interfaces. When it is found that the set and the current reference are not the same instance, obtaining a material later will not generate a new material instance. At this time, it is a non-shared state.

2. Under the premise of shared state, obtaining `material` will generate a new material instance to ensure that only the current collision body references the material, so that the modification will not affect other collision bodies, and then it will be in the non-shared state.

3. Obtaining `sharedMaterial` does not generate a new one, but directly returns a reference.

---

Continue to the [physics material](physics-material.md) documentation.
