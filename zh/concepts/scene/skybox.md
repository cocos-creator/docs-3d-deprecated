# 天空盒

游戏中的天空盒是一个包裹整个场景的立方体，天空盒可以很好的渲染整个环境的气氛，并表达整个场景的环境，在基于 PBR 的工作流中天空盒也是非常重要的部分。

 Cocos Creator 3D 中的天空盒，如下图所示：
![skybox](skybox/Skybox.jpg)

## 开启天空盒

在 Cocos Creator 3D 中开启天空盒的效果，只需要以下一步：

- Skybox 的面板处于 Scene 节点的属性面板上，将 Enabled 属性勾选上便开启了天空盒
![开启 skybox](skybox/SkyboxPanel.jpg)

**注： Skybox 的 Envmap 属性为空时，使用和显示的将是像素贴图**。

## 修改天空盒的环境贴图

在 Cocos Creator 3D 中修改天空盒的环境贴图，是通过设置 TextureCube 类型的资源。
而从资源导入到设置为 TextureCube ，并且设置到 Skybox 中，可以分为以下几步：

1. 导入图片资源。（**注：此处以全景图为示例，下面有制作 CubeMap 的介绍方法**。）
2. 选中导入的全景图，在右侧的属性面板上，将其设置为 TextureCube 类型，如下图所示。
![设置为 TextureCube](skybox/TextureCube.jpg)
3. 将以上 TextureCube 资源拖入到 Skybox 面板上的 Envmap 属性上。
![设置天空盒的环境贴图](skybox/EnvmapSet.jpg)

完成以上步骤后，就可以在场景中看到最新替换的环境图。

## Skybox 面板

![ skybox 面板](skybox/SkyboxDetail.jpg)

以下介绍了面板的所有属性：

属性 | 解释
---|---
**enabled** | 是否开启 Skybox
**envmap** | 环境贴图，类型为 TextureCube
**isRGBE** | 环境贴图的像素格式是否为 RGBE
**useIBL** | 是否使用环境光照

## CubeMap

CubeMap （立方体贴图）是天空盒的一种环境贴图资源，它由立方体上六个面的贴图资源组合而成，它可以当作 TextureCube 资源来使用。

### 制作与应用 CubeMap

在 Cocos Creator 3D 中制作一张 CubeMap 并且设置到 Skybox 中，只需要以下步骤：

1. 新建 CubeMap 资源，并且导入预先准备好的六张贴图资源，并将这些贴图资源设置为 Texture 类型。
2. 将导入的贴图资源拖入到相应的输入框中，完成后点击绿色勾选按钮，这样就完成了一张 CubeMap。
![设置为 CubeMap](skybox/CubeMap.jpg)
3. 最后，将完成的 CubeMap 资源拖入到 Skybox 的 Envmap 属性框中，这样就完成了 CubeMap 的应用。

**注： CubeMap 中未设置贴图的面将用默认的资源进行填充**。
