# 烘焙系统
烘焙系统是通过计算引擎场景中所有光源对物体的影响最终生成光照贴图并在场景中应用的过程，该系统的目的是减少实时光源的运算，从而提高场景运行效率。

![lightmap](./images/bake.png)

## 创建
开启烘焙系统需要三个步骤:

1. 在顶部菜单栏 `Project(项目)` 中单击该菜单按钮，在弹出菜单栏中单击`Lightmap(光照贴图)`按钮，弹出lightmap面板。
   
   ![create lightmap](./images/bake_menu.png)

2. 在烘焙前需要在编辑器中设置光源组件的`Bakeable`属性为`true`(想启用哪个光源就开启对应光源即可)。
   
   ![enable lightbake](./images/bakeable.png)

3. 在弹出的lightmap面板中，设置好对应参数后可点击`Lightmap Generate(生成光照贴图)`按钮，选择对应存储文件夹即可生成光照贴图(注意:存储文件夹一定要存储在assets文件夹下才生效)。
   
   ![create lightmap asset](./images/lightmap_generate.png)

## 使用
生成烘焙贴图过程中会有生成的进度提示，生成后可在`lightmap`面板中的->`Baked` tab中查看。

![bake result](./images/lightmap_result.png)

其中`Baked result(烘焙结果显示面板)`显示了烘焙后的lightmap贴图, `Lightmap clear(清空按钮)`可以删除烘焙的生成结果，`信息输出面板`显示了每张烘焙图片的信息(文件名,大小等)。

## 编辑
烘焙过程是根据面板设置的各个参数来计算生成的结果，如下图：

![bake result](./images/bake_param.png)

下表描述了各个参数的具体含义。
参数| 描述
---|---
MSAA | 多重采样: 有以下值(1, 2, 4, 8)
Resolution | 烘培贴图大小: 有以下值(128, 256, 512, 1024, 2048)
Gamma | Gamma 矫正值
GIScale | 全局光照缩放系数
GISamples | 全局光照采样系数
AOLevel | AO级别
AOStrength | AO强度
AORadius | AO半径
AOColor | AO颜色
