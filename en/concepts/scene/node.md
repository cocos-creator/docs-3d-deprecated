# Node

__Nodes__ are the basic building blocks of a scene. __Nodes__ are organized in a __tree-like__ relationship. Each __Node__ can have multiple __child nodes__ and correspond to a __parent node__ at the same time.

![nodes](scene/nodes.jpg)

__Nodes__ have the following characteristics:
  - __Nodes__ contain a set of basic information (*displacement*, *rotation*, *scaling*), and __Nodes__ are organized together through a set of relative transformation relationships.
  - The update order between __Nodes__ is updated step by step. The update of child nodes depends on the parent node, and the child nodes change with the parent node.
  - Components can be added to __Nodes__ to associate multiple components with __Nodes__

In short, __Nodes__ are the basic means of organizing the structure of any game. We can classify multiple elements through __Nodes__, and perform hierarchical operations on __Nodes__, or perform batch operations on a group of __Nodes__, such as: *transform*, *delete*, or hiding and showing them as needed, etc.

---

Continue to the [Skybox](skybox.md) documentation.