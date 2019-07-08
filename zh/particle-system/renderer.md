## Particle Renderer
粒子渲染部分由ParticleSystemRenderer控制，ParticleSystemRenderer通过一个对象池来维护所有粒子，根据粒子当前状态来生成对应的vb、ib数据，持有粒子需要渲染的材质，并且保存相关渲染状态。
![](particle-system/renderer.png)


**属性** | **作用**
---|---
**renderMode** | 设置一个粒子面片的生成方式，**billboard**粒子始终面向摄像机，**stretchedBillboard**粒子始终面向摄像机,但会根据相关参数进行拉伸，**horizontalBillboard**粒子面片始终与xz平面平行,**verticalBillboard**粒子面片始终与Y轴平行，但会朝向摄像机，**mesh**粒子为一个模型。
**velocityScale** | 在**stretchedBillboard**模式下,对粒子在运动方向上按速度大小进行拉伸。
**lengthScale** | 在**stretchedBillboard**模式下,对粒子在运动方向上按粒子大小进行拉伸。
**mesh**|在renderMode为mesh时，指定要渲染的粒子的模型。
**ParticleMaterial**|用于粒子渲染的材质，材质使用的effect只能是builtin-particle，不支持其它的effect。
**TrailMaterial**|用于拖尾渲染的材质，材质的effect只支持builtin-particle-trail，不支持其它的effect。