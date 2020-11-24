## 全局雾类型

引擎内部提供了四种全局雾类型:

1. Linear Fog
2. Exponential Fog
3. Exponential Squared Fog
4. Layered Fog

所有雾化类型的求取是通过相机与模型顶点求得雾化的混合因子，最终得出不同的全局雾效果。

## 开启方式

![image](./fog/fogInspector.png)

点击场景的Scene节点,在Inspector面板中展开fog选项,勾选`Enabled`属性即可开启全局雾功能，不论全局雾的类型如何，都可以通过`FogColor`设置全局雾的颜色。

## 不同类型的使用

### Linear Fog


![image](./fog/linear_fog.png)

线性雾的混合因子的算法通过:

`` f = (FogEnd - Cam_dis) / (FogEnd - FogStart)；``

也就是:

当Cam_dis = `FogEnd`时，混合因子为0，此时物体为全雾化，

当Cam_dis = `FogStart`时，混合因子为1，此时物体不受任何雾化影响。

如果要增加Linear Fog在固定camera distance下的浓度，有两种方式:

1. 固定`FogStart`数值，减少`FogEnd`数值。
2. 减少`FogStart`数值, 固定`FogEnd`数值。

如果要调整合适浓度,最好同时对`FogStart`与`FogEnd`适当调整。

### Exponential Fog 与 Exponential Squared Fog

![image](./fog/expfog.png)

Exponential Fog的混合因子f的求取通过如下的方式：

`` f = e^(-distance * fogDensity) ``

Exponential Squared Fog的混合因子求取如下：

`` f = e^(-distance * fogDensity)² ``

除了可以通过`FogDensity`调整全局雾浓度外，还新增了`FogAtten`属性，它是雾化的衰减系数，用户可以调整该参数以适应在合适位置的不同浓度。

### Layered Fog

![image](./fog/layerfog.png)

引擎对Layered Fog的计算相对前面三种雾化类型的计算稍显复杂，引入了`FogTop`的概念，同时需要在x-z平面进行距离计算。

以下是它的参数解释:

`FogAtten`: 雾化衰减系数。

`FogTop`: 模型顶点的世界坐标垂直方向上的位置，小于该位置时的所有顶点都会受到雾化效果的影响。

`FogRange`: 雾化效果影响的范围。

Layered Fog在现实中还是比较常见的,高耸入云的山脉与建筑物经常有它的身影,如果能合理利用，相信对效果有不错的提升，但是同时计算量也会有一定增大，具体取舍取决于开发者。
