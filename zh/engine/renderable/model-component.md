# 模型组件

模型组件用于显示一个静态的3D模型。通过mesh设置模型网格，通过material改变模型外观。

属性| 功能
---|---
**mesh** | 用于渲染的3D模型资源。
**materials**|用于渲染模型的材质，一个材质对应mesh中的一个submesh。
**shadowCastingMode**|如果场景启用了平面阴影，则启用后会渲染平面阴影。
**visibility**|用于模型会被哪个摄像机渲染，只有visibility与模型相同的摄像机才会渲染该模型。