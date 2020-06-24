# Skeletal animation

__Skeletal animation__ is a common but special type of animation. Two different systems are provided and each is optimized for different purposes. Switching between these two systems uses the `useBakedAnimation` switch on `SkeletalAnimationComponent`. The corresponding pre-baked skeletal animation system is enabled, and when disabled, it corresponds to the real-time calculation skeletal animation system, which can also be seamlessly switched at runtime.

## Pre-baked skeleton animation system

The purpose of this system is performance, and some sacrifices in expressiveness are also considered acceptable. __Cocos Creator 3D__ has made many low-level optimizations in a targeted manner. The current runtime process is roughly as follows:
  * All animation data will be pre-sampled in advance according to the specified frame rate and baked onto the global multiplexed skeleton animation texture collection;
  * Depending on whether the operating platform supports floating-point textures, the corresponding fallback in `RGBA32F` or `RGBA8` format will be used; (users at this layer do not need to care, and do not affect the final performance, but only the final guarantee strategy for low-end platforms)
  * Each Skeletal Animation Component (`SkeletalAnimationComponent`) is responsible for maintaining the current playback progress, stored in the form of UBO (a `vec4`);
  * Each skinning model component (`SkinningModelComponent`) holds a pre-baked skinning model class (`BakedSkinningModel`). Calculating the culling based on the same pre-baked box information, update the UBO, and get the current data from the texture collection on the GPU to complete the skinning.

## Real-time calculation of skeletal animation system

The overwhelming purpose of this system is expressiveness, ensuring the correct display of all details, and complete program control capabilities.

The current runtime process is roughly as follows:
  * All animation data are calculated dynamically according to the current global time;
  * Animation data will be output to the skeleton node tree of the scene;
  * Users and any other system can affect the skin effect by manipulating this bone node tree;
  * Each skinning model component (`SkinningModelComponent`) holds a common skinning model class (`SkinningModel`). Extract the information of the number of bone nodes in each frame to calculate culling, upload the complete bone transformation information of the current frame to UBO, and complete the skinning in the GPU.

This provides the most basic support for all the following functions:
  * blendshape support
  * Mixing and masking of any number of animation clips
  * IK, secondary physical influence
  * Pure program control joint position

## Selection and best practice of two systems

After importing all model resources, all prefabs use the pre-baking system by default to achieve the best performance. It is recommend that you only use the real-time computing system if you clearly feel that the performance of the pre-baking system cannot reach the standard. Although the two systems can be switched seamlessly at runtime, try not to perform high frequency, because each switch involves the reconstruction of the underlying rendering data.

## Skinning algorithm

We have built-in two common standard skinning algorithms, which have similar performance and only affect the final performance:

1. LBS (Linear Mixed Skinning): bone information is stored in the form of a 3x4 matrix, and the matrix is ​​interpolated directly to achieve skinning, and there are typical known problems such as volume loss.
2. DQS (Dual Quaternion Skinning): The bone information is interpolated in the form of double quaternion, which is more accurate and natural for the skeleton animation without scaling transformation, but for performance reasons, there is an approximate simplified process for all scaling animations.

The engine uses LBS by default. You can switch the skinning algorithm by modifying the `updateJointData` function reference of the engine skeletal-animation-utils.ts and the header file reference in cc-skinning.chunk.

It is recommended that projects with higher pursuit of skin animation quality can try to enable DQS, but since there is no `fma` instruction before GLSL 400, operations such as `cross` cannot bypass floating-point cancellation on some GPUs, and the error is relatively high. Large, may introduce some visible defects.

## Hanging point system

If you need to hang some external nodes to the specified bone joints, you need to use the Socket system of the skeleton animation component:
  * Create a new child node under the bone animation component to be docked (the parent directly should be the node where the animation component is located).
  * Add an array element to the sockets property of the skeleton animation component, select the path of the bone to be connected from the drop-down list, and specify the target as the child node just created.
  * This child node becomes the target hanging point, you can put any external node under this child node, it will follow the specified bone transformation.

The hanging point model in the `FBX` or `glTF` resource will automatically connect to the hanging point system without any manual operation.

## About News Instancing

Based on the frame design of the pre-baking system, the instancing of the skin model has also become a function within reach, but to ensure correctness, you need to collect some relatively low-level information.

The fundamental problem here is that the bone maps used by each model in the same drawcall must be the same. If they are not the same, the display effect will be completely messy.

So how to assign animation data to each bone map becomes a user-defined information, corresponding to the [skeletal map layout panel] set in the editor project (../../editor/project/joints- texture-layout.md) to configure.

> **Note**: Instancing is only supported under the pre-baking system. Although we do not strictly prohibit instancing under the real-time computing framework (only the warning in the editor), there will be problems with the animation effect, which depends on the actual material allocation of the model. The situation is that completely consistent animations are displayed between different instances, and in the worst case, the model will be completely disorganized.

> **Note**: For models with instancing turned on in the material, the flat shading system will also automatically draw using instancing. In particular, the shadow of the skin model has a higher requirement for the layout of the bone map, because the pipeline state of the shadow is unified, ** all the animation of the skin model with the shadow turned on ** needs to be guaranteed on the same texture (Compared to when drawing the model itself, only the instances in the same drawcall need to keep the bone texture consistent).

## Batch skin model components

At present, the bone texture uploaded by the GPU on the bottom layer has been globally automatically batched and reused. The upper layer data can currently be combined with all the sub-skin models controlled by the same bone animation component by using the BatchedSkinningModelComponent:

![](batched-skinning-model-component.png)

The batch version of the effect is relatively complicated to write, but it can basically be based on the common effects used by the sub-materials, adding some relatively direct preprocessing and interface changes. The built-in resources in the editor (util/batched-unlit) provide a The integrated version of builtin-unlit can be referenced.

> **Note**: Only using the batch skin model component under the pre-baking system can guarantee the correctness. Although it can also be used under the real-time computing framework, there will be rendering problems when the number of bones after the merger exceeds 30 (the maximum number of Uniform arrays).