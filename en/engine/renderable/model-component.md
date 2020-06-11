# Model component

The __ModelComponent__ is used to display a static 3D model. Set the model grid through a mesh, and change the appearance of the model through material.

Properties | Functions
--- | ---
*mesh* | 3D model resources for rendering.
*materials* | The material used to render the model, one material corresponds to one submesh in the mesh.
*shadowCastingMode* | If flat shadows are enabled in the scene, flat shadows will be rendered when enabled.
*visibility* | Used for which camera the model will be rendered, only the camera with the same visibility as the model will render the model.

## Model group rendering

The group rendering function is determined by the `Visibility` property of the camera component and the `Layer` property of the node. Users can set the `Visibility` value through code to complete the group rendering. All nodes belong to the `DEFAULT` layer by default and are visible on all cameras.

  > **Note**: Please review the [CameraComponent](../../editor/components/camera-component.md) documentation before proceeding.

## Static batch

The current static batching scheme is static batching at run time. Static batching can be performed by calling `BatchingUtility.batchStaticModel`. This function receives a node, and then merges all `Mesh` in `ModelComponent` under that node into one, and hangs it under another node.

After batching, the original transform of `ModelComponent` cannot be changed, but the transform of the root node after batching can be changed. Only nodes that meet the following conditions can be statically batched:

  * The child node can only contain `ModelComponent`.
  * The vertex data structure of `Mesh` of `ModelComponent` under child nodes must be consistent.
  * The material of `ModelComponent` under child nodes must be the same.

## About dynamic batching

The engine currently provides two sets of dynamic batching systems, *instancing batching* and *dynamic merging VB mode batching*. The two methods cannot coexist, and the priority of instancing is greater than that of dynamic merging VB. To enable batching, simply check the `USE_INSTANCING` or `USE_BATCHING` switch in the material used in the model.

  > **Note**: Batching can participate in the frustum culling process normally, but the transparent model cannot be sorted, which will cause the mixing effect to be incorrect. The engine does not explicitly prohibit the approval of transparent objects, and developers can control the trade-offs.

### Instancing batching

The batch through **instancing** is suitable for drawing a large number of dynamic models with the same vertex data. When enabled, drawing will be grouped according to the material and vertex data, and the instanced attributes information will be organized in each group, and then complete the drawing at one time.

For the support and related settings of the skin model, refer to the [Bone Animation Component](../animation/skeletal-animation.md#AboutDynamic-Instancing) documentation.

In addition, instancing also supports custom additional instanced attributes, which can pass more differential data between different instances (such as the difference in appearance of a diffuse color between different characters, or the influence of wind in a large grass field). This requires the support of custom effects. For more detailed instructions, please refer to the [Syntax Guide](../../material-system/effect-syntax.md#Custom-Instanced-Properties) documentation.

### Merge VB batching

Dynamic batching is suitable for drawing a large number of non-skinned dynamic models with low number of faces and different vertex data. When enabled, drawing will be grouped according to the material, and then the vertex and world transformation information will be merged in each frame of each group, and then completed in batches. <sup id="a1">[1](#f1)</sup>.

Operations such as merging vertices per frame introduce a portion of CPU overhead, which is particularly expensive in JavaScript. In addition, it is necessary to remind that the number of __draw calls__ is not as low as possible. <sup id="a2">[2](#f2)</sup>, most good performance is often the result of CPU and GPU load balancing, so when using batch functions, be sure to do more tests to identify performance bottlenecks and do targeted optimization.

## Batch best practices

Generally speaking, the priority of the batch system is: static batching> dynamic instancing> dynamic batching. First of all, we must ensure that the material is unified, under this premise:
If you are certain that certain models will remain completely unchanged during the game cycle, you can use static batching. If there are a large number of the same model repeated drawing, there is only a relatively controllable small difference between each other, you can use dynamic instancing. If there are a large number of models with very low number of faces but different vertex data, you can consider trying dynamic batching.

---

<b id="f1">[1]</b> Note that currently using uniform to upload the batch-transformed world transformation matrix, taking into account the WebGL standard uniform quantity limit, the current batch draws up to 10 models, so for a large number of same For the material model, the number of drawcalls is expected to be reduced by up to 10 times after opening the batch. [↩](#a1)

<b id="f2">[2]</b> There have been many discussions in the industry on the topic of batching and performance, you can refer to this [nVidia slide](https://www.nvidia.com/docs/IO/8228/BatchBatchBatch.pdf) slide [↩](#a2)