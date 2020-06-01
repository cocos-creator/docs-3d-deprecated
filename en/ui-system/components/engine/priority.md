# Rendering Order

The rendering order of the UI uses the Breadth-First Sorting scheme, and each renderer component (Such as SpriteComponent) has a `priority` property.

Sorting starts from the child nodes under the root node, and determines the overall rendering structure according to the priority of the child nodes, that is, the rendering order of the child nodes under the root node has determined the final rendering order. The `priority` property of all child nodes under each node is used to determine the rendering order under the current node.

For example:

![priority.png](priority/priority.png)

It can be seen from the figure that although some nodes have no renderer components, their children can participate in sorting, so these nodes also need to participate in sorting. The overall rendering order is __B -> b1 -> C -> A -> a1 -> a2__, and the rendering state on the screen is __a2 -> a1 -> A -> C -> b1 -> B__. Since we will eventually sort the nodes based on the `priority`, the order of the nodes that are ultimately presented to you is the final rendering order.

For nodes that don't have a renderer component but also need to be sorted, you can use the [UIReorderComponent](../editor/ui-reorder-component.md).
