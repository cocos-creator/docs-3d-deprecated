# Rendering Order

The rendering order of the UI uses the Breadth-First Sorting scheme, and each renderer component (Such as SpriteComponent) has a `priority` property.


Sorting starts from the child nodes under the root node, and determines the overall rendering structure according to the priority of the child nodes, that is, the rendering order of the child nodes under the root node has determined the final rendering order. The `priority` property of all child nodes under each node is used to determine the rendering order under the current node.

For example:

![priority.png](priority/priority.png)

从图中可以看出有一些节点是没有渲染组件的，但是它的子节点是有权利继续排序，因此，也是需要参与到排序的规则。整体的渲染顺序则是：**B -> b1 -> C -> A -> a1 -> a2**，在屏幕上的呈现状态为：**a2 -> a1 -> A -> C -> b1 -> B**。由于我们最终会根据 priority 进行节点排序，所以最终呈现给用户的节点顺序就是最终的渲染排序。

It can be seen from the figure that although some nodes have no renderer components, their children can participate in sorting, so these nodes also need to participate in sorting. The overall rendering order is **B -> b1 -> C -> A -> a1 -> a2**, and the rendering state on the screen is **a2 -> a1 -> A -> C -> b1 -> B**. Since we will eventually sort the nodes based on the `priority`, the order of the nodes that are ultimately presented to you is the final rendering order.

For nodes that don't have a renderer component but also need to be sorted, you can use the [UIReorderComponent](../editor/ui-reorder-component.md) component.
