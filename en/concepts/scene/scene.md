# Scene structure

__Cocos Creator 3D__ adds a 3D scene structure to __Creator’s__ __ECS (entity component system) framework__. The 3D scene is represented by __RenderScene__. The corresponding __Component__ in the ECS structure references the *Model*, *Camera*, and *Light* are maintained in `RenderScene`. Other elements are also linked together through __Node__, including the various *Transformations* that update elements in __RenderScene__.

> __Note__: the differences between __Scene__ in __ECS structure__ and __RenderScene__ in a __3D__ scene structure. __Scene__ in __ECS__ is the logical organization structure of __Node__. __RenderScene__ in __3D__ is the organization structure of scene rendering elements. __ECS scene__'s contain __RenderScene__ member variables, corresponding one by one.

The relationship between __ECS__ and __3D__ scene structure is shown in the following figure:

![ecs & scene](scene/ecs-scene.jpg)

The entire __3D__ scene structure is encapsulated under __Component__, and the organizational relationship is established through __Node__. This is completely transparent relationship between __ECS__ and a __3D__ scene.

---

Continue to the [Node](node.md) documentation.