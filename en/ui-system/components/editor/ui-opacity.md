# UIOpacity Component References

The UIOpacity Component records a transparency modification flag for the node, which is used to influence all the render nodes inside its sub tree. Normally it's used on a non render nodes, otherwise its opacity will be multiplied with the render component's opacity. The render nodes can set transparency individually by setting the alpha channel of `color`.

## UIOpacity Properties

| Properties | Function Description |
| -------- | ----------- |
| Opacity        | transparency |

- [UI Basic Components](base-component.md)

- [UI Renderer Components](render-component.md)
