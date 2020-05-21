## Particle Renderer
粒子渲染部分由 ParticleSystemRenderer 控制，ParticleSystemRenderer 分为 CPU 渲染器和 GPU 渲染器，CPU 渲染器通过一个对象池来维护所有粒子，根据粒子当前状态来生成对应的 vb、ib 数据，持有粒子需要渲染的材质，并且保存相关渲染状态。GPU 渲染器目前版本是在 CPU 端生成粒子，只提交初始参数的 vb, ib 数据，但是模块相关的计算则是通过预采样数据的形式，初始化时提交一次数据，后续的模块系统则是在 GPU 端对数据进行提取模拟运算，减少 CPU 端的计算压力，目前版本不支持 TrailModule 和 LimitVelocityOvertimeModule, 后续版本仍将持续对粒子系统进行优化改进。

![](particle-system/renderer.png)


**属性** | **作用**
---|---
**renderMode** | 设置一个粒子面片的生成方式，**billboard** 粒子始终面向摄像机，**stretchedBillboard** 粒子始终面向摄像机,但会根据相关参数进行拉伸，**horizontalBillboard** 粒子面片始终与 xz 平面平行,**verticalBillboard** 粒子面片始终与Y轴平行，但会朝向摄像机，**mesh** 粒子为一个模型。
**velocityScale** | 在 **stretchedBillboard** 模式下,对粒子在运动方向上按速度大小进行拉伸。
**lengthScale** | 在 **stretchedBillboard** 模式下,对粒子在运动方向上按粒子大小进行拉伸。
**mesh**| 在 renderMode 为 mesh 时，指定要渲染的粒子的模型。
**particleMaterial**| 用于粒子渲染的材质，当选用 CPU 渲染器时，也就是不勾选 useGPU 的情况下，材质使用的 effect 只能是 builtin-particle，不支持其它的 effect。当选用 GPU 渲染器时，也就是勾选 useGPU 的情况下，材质使用的 effect 只能是 builtin-particle-gpu, 不支持其它的 effect。
**trailMaterial**|用于拖尾渲染的材质，材质的 effect 只支持 builtin-particle-trail，不支持其它的 effect。
**useGPU**|是否使用 GPU 渲染器进行粒子的渲染，默认不勾选。当不勾选该选项的情况下，使用 CPU 渲染器 ParticleSystemRendererCPU 进行粒子的渲染。当勾选该选项的情况下，使用 GPU 渲染器 ParticleSystemRendererGPU 进行粒子的渲染。