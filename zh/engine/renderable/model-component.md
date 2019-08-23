# 模型组件

模型组件用于显示一个静态的3D模型。通过mesh设置模型网格，通过material改变模型外观。

属性| 功能
---|---
**mesh** | 用于渲染的3D模型资源。
**materials**|用于渲染模型的材质，一个材质对应mesh中的一个submesh。
**shadowCastingMode**|如果场景启用了平面阴影，则启用后会渲染平面阴影。
**visibility**|用于模型会被哪个摄像机渲染，只有visibility与模型相同的摄像机才会渲染该模型。

## 模型分组渲染

分组渲染功能是通过相机组件([CameraComponent](../../editor/components/camera-component.md)) 的 Visibility 属性配合 模型组件([ModelComponent](../../engine/renderable/model-component.md)) 的 Visibility 属性共同决定。用户可通过代码设置 Visibility 的值来完成分组渲染，需要注意的是，Visibility 的值是 **按位比较** 的，用户可通过 **位运算** 操作 Visibility 的 **前 20 位** 来完成分组。<br>
我们默认提供的摄像机和模型都为不分组全部渲染，用户没有特殊需求的情况下不需更改此值。

---