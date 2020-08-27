# Project Settings

The **Project Settings** windows are available from Cocos Creator 3D’s main menu (**Project > Project Settings**) which includes all the settings related to your project. These settings will be saved in the project's `settings / packages` folder. If you need to synchronize project settings between different developers, this folder should be added in your source control system.

## General

![general](./index/general.jpg)

### Default Canvas settings

The default Canvas settings include design resolution and adapted screen width/height, which are used to specify the default design resolution value in Canvas when creating a new scene or Canvas component, as well as the `Fit Height, Fit Width` options.
different
For more information, please refer to [Multi-resolution adaptation scheme](../ui-system/components/engine/multi-resolution.md) documentation.

## Engine Modules

![modules](./index/modules.jpg)

The setting here is to crop the modules used in the engine to reduce the size of the released engine. Modules not selected in the panel will be cropped when they are packaged and previewed. It is recommended to perform a complete test after released to avoid using cropped modules in scenes and scripts.

## Macro Config

For more information and code of the engine macro module, please refer to [Engine macro](https://github.com/cocos-creator/engine/blob/3d/cocos/core/platform/macro.ts#L824) documentation.

This panel here provide the convenience to modify the macro configuration. The **Macro Config** will take effect during preview and build. At the same time, the default value of the current macro configuration will be updated with the configuration of the custom engine.

![macro](./index/macro.png)

## Texture Compress

> As of v1.2, the editor has modified the use of compressed texture configuration to configure presets in project settings and select presets for image asset's inspector. After the old version of the project is upgraded, the editor will automatically scan all the compressed texture configurations in the project and sort out as several presets.

Used to add compressed texture preset configuration, you can directly select the compressed texture preset to quickly add in the inspector of image asset. At the same time, after adding presets, you can also directly modify the presets to update batch texture compress configuration. Project settings allow users to add multiple compressed texture configurations, and each compressed texture configuration allows to add different format for different platform categories.

Platform is rough devised as following:

1. Web: refers to the two platforms Web-Mobile and Web-Desktop
2. Mac & Windows
3. iOS
4. Mini Game: Refers to the mini-games of various manufacturers' platforms, such as WeChat Mini Games and Huawei Quick Game Waiting;
5. Android

For the detail of the texture compression support of the platforms, please refer to [Compressed Texture Chapter](../../asset/compress-texture.md) documentation.

### Add / remove texture compression presets

Enter the name of the compressed texture preset in the input box and press **Enter** or the plus button on the left to add it.

![Add texture compression preset](./texture-compress/add.jpg)

After adding the compressed texture preset, if you need to delete it, you can directly move the mouse to the preset name and click the delete button on the right.

![Delete texture compression preset](./texture-compress/delete.jpg)

### Add / remove texture compression format

Click the `Add Format` button, select the desired texture format, and configure the corresponding quality level. Currently, only one image format of the same type can be added at the same time.

![Add texture compression format](./texture-compress/add-format.png)

To delete, move the mouse over the texture format and click the red delete button.

### Modify compressed texture preset name

The name of the compressed texture is only used for display. When adding a compressed texture preset, uuid will be randomly generated as the ID of the preset, so directly modifying the preset name will not affect the reference to the preset in the image asset.

![Modify the name of the texture compression preset](./texture-compress/edit.jpg)

If you want to replace all the preset options currently used for image asset, you can move the mouse to the preset name, click the button to copy the ID to and search and replace in the project by yourself.

## Layers

![Layers](./index/layers.png)

- Layers allows the camera to render part of the scene and let the light illuminate part of the scene. It is also possible to deal with whether objects collide through Layers during ray inspection.
- You can customize 0 to 19 Layers, and the original settings will be deleted when you clear the input box.
- The last 12 Layers are built-in in the engine and cannot be modified.
- The positions currently used are: when editing the node node, the Layer property on the inspector panel; when editing the Camera node, the Visibility property.

![Layers-node](./index/layers-node.png)

![Layers-camera](./index/layers-camera.png)

## Bone map layout settings

Explicitly specify the bone texture layout to assist the instancing of the skinning models. For details, please refer to [here](joints-texture-layout.md).
