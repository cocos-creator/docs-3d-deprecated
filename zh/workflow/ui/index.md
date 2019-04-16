## UI 结构

Cocos 3D UI 系统复用了 Creator 的整体设计结构，每帧对场景节点下的每一个渲染组件进行数据组装，然后存入 MeshBuffer 中并同时生成 Model，最后在当前帧结束后取 Model 数据进行渲染，如下图：

![index.png](index.png)
