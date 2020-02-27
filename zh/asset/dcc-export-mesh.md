# 导入从DCC工具导出的模型
目前大多数数字内容制作(Digital Content Creation, DCC)工具(3ds Max, Maya, Blender)都能导出FBX和glTF两种格式的模型文件，所以这些工具导出的内容都能在Cocos Creator 3D得到良好的展示。
## 导出FBX
因为DCC工具的坐标系和游戏引擎的坐标系不一定一致，所以在导出模型时需要进行一些变换才能在引擎中得到想要的结果。例如Blender的坐标系为X轴向右，Y轴向外，Z轴向上的右手坐标系，而Cocos Creator 3D为X轴向右，Y轴向上，Z轴向外的右手坐标系，所以需要做旋转才能使得轴向一致。

以下以Blender 2.8作为例子，介绍模型的导入流程，首先我们在Blender中创建一个模型。

![blender model](./mesh/blender_model.png)

在[Blender的FBX导出选项](https://docs.blender.org/manual/en/2.80/addons/io_scene_fbx.html)中，我们选择Up为Y Up，Forward为-Z Forward。

![blender export](./mesh/blender_export_fbx_1.png)

导入到Cocos Creator 3D中，可以看到节点在X轴做了-90的旋转，以便将轴和Cocos Creator 3D
的轴对齐。

![blender export c3d](./mesh/blender_model_c3d.png)

如果不想要这个旋转值，Blender的FBX导出插件提供了一个实验性功能（Apply Transform），可以将旋转数据直接变换到模型的顶点数据中。

![blender export bake](./mesh/blender_export_bake.png)

可以看到在Cocos Creator 3D中旋转数据没有了。

![blender export bake c3d](./mesh/blender_model_bake_c3d.png)

## 导出glTF
[glTF使用的也是右手坐标系](https://github.com/KhronosGroup/glTF/tree/master/specification/2.0#coordinate-system-and-units)，Blender的[导出glTF的选项](https://docs.blender.org/manual/en/2.80/addons/io_scene_gltf2.html)比较简单，只要把+Y Up选项勾上就可以了，导出的数据中也没有旋转值。

![blender export glTF](./mesh/blender_export_gltf.png)

## 朝向问题
游戏开发过程中可能会需要用到模型的朝向，例如想要一些物体面向玩家(使用了LookAt方法)，这时就需要考虑模型的初始朝向，这里提供两种方法来调整模型的初始朝向。

1. Cocos Creator 3D中是以-Z轴做为正前方的朝向，而在Blender中正前方朝向为+Y轴，所以在制作模型时需要以Y轴正方向做为物体的朝向，经过导出的变换后，在Cocos Creator 3D就会是以-Z轴做为正前方的朝向。
2. 如果不想在DCC工具中改变朝向，可以在场景中尝试为导入的模型增加一个父节点，然后旋转模型以使得模型的初始朝向为-Z轴，之后的各种旋转相关的操作都以父节点为操作对象。