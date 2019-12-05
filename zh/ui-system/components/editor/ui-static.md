# UIStaticBatchComponent UI 静态合批组件

UI 静态合批组件是一个复用 IA 的组件，当点击 collect 属性时候，会通过动态合批的流程将该节点下所有子节点（除模型以及 mask 和 graphices）的数据缓存在一个单独的 IA 上，之后复用此 IA。此后的坐标变换将不再生效，当你需要修改静态数据的时候，可以再次点击 collect 来重新收集。

## UIStaticComponent 属性

| 属性                 | 功能说明             |
| --------------       | -----------        |
| Collect               | 收集当前节点下的所有子节点数据成静态数据。
