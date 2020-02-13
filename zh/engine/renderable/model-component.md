# 模型组件

模型组件用于显示一个静态的3D模型。通过mesh设置模型网格，通过material改变模型外观。

属性 | 功能
--- | ---
**mesh** | 用于渲染的3D模型资源。
**materials** | 用于渲染模型的材质，一个材质对应mesh中的一个submesh。
**shadowCastingMode** | 如果场景启用了平面阴影，则启用后会渲染平面阴影。
**visibility** | 用于模型会被哪个摄像机渲染，只有visibility与模型相同的摄像机才会渲染该模型。
**enableDynamicBatching** | 是否启用动态合批，细节见下节

## 模型分组渲染

分组渲染功能是通过相机组件([CameraComponent](../../editor/components/camera-component.md)) 的 Visibility 属性配合 模型组件([ModelComponent](../../engine/renderable/model-component.md)) 的 Visibility 属性共同决定。用户可通过代码设置 Visibility 的值来完成分组渲染，需要注意的是，Visibility 的值是 **按位比较** 的，用户可通过 **位运算** 操作 Visibility 的 **前 20 位** 来完成分组。<br>
我们默认提供的摄像机和模型都为不分组全部渲染，用户没有特殊需求的情况下不需更改此值。

## 关于动态合批

动态合批适用于绘制大量低面数、同材质的非透明动态模型的需求，目前要开启动态合批，只需在所使用的材质中勾选 USE_BATCHING 开关，无需任何其他操作。<br>
运行时会根据这些模型使用的材质分组，然后对每个材质组每帧合并顶点和世界变换信息，然后一次性完成绘制<sup id="a1">[1](#f1)</sup>。

注意因为合批会影响透明模型的排序，对透明物体即使开启开关也不会实际参与合批。

合批不会影响正常的 frustum culling 流程，但每帧合并顶点等操作还是会引入一部分 CPU 开销（在 JS 中尤其昂贵）；另外需要提醒 drawcall 数量并不是越少越好<sup id="a2">[2](#f2)</sup>，最佳性能往往是 CPU 与 GPU 负载均衡的结果，所以在尝试使用合批功能时，请一定多做测试，明确性能瓶颈，做有针对性的优化。

---

<b id="f1">[1]</b> 注意目前使用 uniform 上传合批后的世界变换矩阵，考虑到 WebGL 标准的 uniform 数量限制，目前一批最多绘制 10 个模型，所以对大量同材质的模型，开启合批后 drawcall 数量预期最多会减少 10 倍。 [↩](#a1)<br>
<b id="f2">[2]</b> 关于合批与性能的话题业界一直有不少探讨，可以参考比如 [这里](http://www.ce.u-sys.org/Veranstaltungen/Interaktive%20Computergraphik%20(Stamminger)/papers/BatchBatchBatch.pdf) 的 slide [↩](#a2)<br>

## 静态合批
目前静态合批方案仅支持运行时静态合批，通过调用BatchingUtility.batchStaticModel(staticModelRoot: Node, batchedRoot: Node)可进行静态合批。该函数接收一个节点，然后将该节点下的所有ModelComponent里的mesh合并成一个mesh，并将其挂到另一个节点下。在合批后，将无法改变原有的ModelComponent的transform，但可以改变合批后的根节点的transform。只有满足以下条件的结点才能进行静态合批：

- 子节点中只能包含ModelComponent
- 子节点下的ModelComponent的mesh的结构必须一致
- 子节点下的ModelComponent的材质必须相同
