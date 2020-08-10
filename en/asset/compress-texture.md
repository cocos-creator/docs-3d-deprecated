# Compress texture

**Cocos Creator 3D** can set the compression method required for textures directly in the editor, and then automatically compress the textures when the project is released. For the web platform, multiple image formats can be exported at the same time, and the engine will automatically download the appropriate format according to different browsers.

## Configure compressed texture

**Cocos Creator 3D** supports importing images in multiple formats (see the table below for details), but in an actual running game, we do not recommend using the original images as assets to load. For example, on a mobile platform, only 80% or less of the original image quality may be required, or a `.png` without the transparent channel can be converted into a `.jpg`, which can reduce the storage space required.

| Image format | Android                                                                                                                            | iOS                                                                        | MIni Game                 | Web                                                   | Mac & Windows         |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ------------------------- | ----------------------------------------------------- | --------------------- |
| PNG          | Supported                                                                                                                          | Supported                                                                  | Supported                 | Supported                                             | Supported             | Supported |
| JPG          | Supported                                                                                                                          | Supported                                                                  | Supported                 | Supported                                             | Supported             | Supported |
| WEBP         | Native Supported for Android 4.0+<br>Other versions can use [this library](https://github.com/alexey-pelykh/webp-android-backport) | can use [this library](https://github.com/carsonmcdonald/WebP-iOS-example) | Supported                 | [partially supported](https://caniuse.com/#feat=webp) | Supported             | Supported |
| PVR          | Not Supported                                                                                                                      | Supported                                                                  | Supported iOS devices     | Supported iOS devices                                 | Supported Mac devices |
| ETC1         | Supported                                                                                                                          | Not Supported                                                              | Supported Android devices | Supported Android devices                             | Not Supported         |
| ETC2         | Supported with **WebGL2** or **WebGL** extension if available                                                                      | Not Supported                                                              | Not Supported             | Supported Android devices                             | Not Supported         |

By default, **Cocos Creator 3D** outputs the original image during build. If you need to compress an image during the build process, you can select this image in the **Assets Panel** and then manage it in the **Inspector** to edit the compress texture format of the image.

![compress-texture](compress-texture/compress-texture.png)

The editor will provide a preset by default. If you need to add more presets, you can click the `Edit preset` button to open `Project Setting -> Compress Texture` to add and edit presets. The compression format here is readonly. For how to add texture compression presets, please refer to [Project Settings](./editor/project/index.md).

![meta](compress-texture/meta.png)

The compress-texture options on the image asset will be stored in the asset's meta file.`PresetId` is the ID of the selected compressed texture preset.

## Detailed compression textures

If you want to use compressed textures, you need to turn on the **compressed texture option** when you build the project:

![compress-texture-build](compress-texture/compress-build.png)

When **Cocos Creator 3D** builds the image, it will find whether the current image has been already configured to use compressed textures. If not, it will output the original image.

If the configuration of the compressed texture is founded, the image will be compressed according to the configuration.The compress texture configuration in the project settings is divided into different platforms, and the support of in the actual platform is also difference. **builder** will make certain elimination and priority selection of the configured texture format according to the **actual build platform**and the current **image texture transparency channel**. You can refer to the following example to understand this rule.

Multiple texture formats can be specified on one platform, and each texture format is compressed to generate an image of the specified format when it is constructed.

These generated images will not all be loaded into the engine during runtime, the engine will choose to load the appropriate image according to the configuration in `macro.SUPPORT_TEXTURE_FORMATS`. `macro.SUPPORT_TEXTURE_FORMATS` enumerates all the image formats supported by the current platform. When the engine loads the images, it will find, from the generated images in this list, **the format with the highest priority** (that is, the order is higher) to load.

The user can customize the supported image assets for a platform and the priority of the loading order, by modifying `macro.SUPPORT_TEXTURE_FORMATS`.

## Example

![1](compress-texture/compress-1.jpg)

**Example (1):**
As the compress presets of the MiniGame platform shown in the figure, if the build target is **Huawei Quick Game**, **Builder** will not package the **PVR** texture format. For more details about the support of platforms, please refer to [Details of compressed texture support for platforms](##Details of compressed texture support for platforms)

![2](compress-texture/compress-2.jpg)

**Example (2):**
In the example picture above, both **ETC1** and **PVR** types are configured with RGB and RGBA two types of texture formats. In this case, **Builder** will be according to whether the current picture has a transparent channel to choose one of the same types of formats. The image asset in the example is with a transparent channel, then **Builder** will only pack a compressed texture format with REGA type. Of course, if there is only RGB picture format in the configuration, even if the current picture is with a transparent channel, it will be packaged normally.

## Details of compressed texture support for platforms

| Platform          | TextureCompressTypes |
| ----------------- | -------------------- |
| Web Desktop       | ETC2 / ETC1 / PVR    |
| Web Mobile        | ETC2 / ETC1 / PVR    |
| WeChat Game       | ETC1 / PVR           |
| AliPay Mini Game  | ETC1 / PVR           |
| Baidu Mini Game   | ETC1 / PVR           |
| OPPO Mini Game    | ETC1                 |
| vivo Mini Game    | ETC1                 |
| Huawei Quick Game | ETC1                 |
| Cocos Play        | ETC1                 |
| Xiaomi Quick Game | ETC1                 |
| iOS / Mac         | ASTC / PVR           |
| Android           | ASTC / ETC2 / ETC1   |
| Windows           | ASTC                 |
