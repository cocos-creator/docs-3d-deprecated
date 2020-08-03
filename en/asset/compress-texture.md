# Compress texture

__Cocos Creator 3D__ can set the compression method required for textures directly in the editor, and then automatically compress the textures when the project is released. For the web platform, multiple image formats can be exported at the same time, and the engine will automatically download the appropriate format according to different browsers.

## Configure compressed texture

__Cocos Creator 3D__ supports importing images in multiple formats (see the table below for details), but in an actual running game, we do not recommend using the original images as assets to load. For example, on a mobile platform, only 80% or less of the original image quality may be required, or a `.png` without the transparent channel can be converted into a `.jpg`, which can reduce the storage space required.

| Image format | Android | iOS | WeChat game | Web |
| ------------ | ------------- | --------- | -------- | -------- |
| **PNG** | Supported | Supported | Supported | Supported |
| **JPG** | Supported | Supported | Supported | Supported |
| **WEBP** | Native Supported for Android 4.0+<br>Other versions can use [this library](https://github.com/alexey-pelykh/webp-android-backport) | can use [this library](https://github.com/carsonmcdonald/WebP-iOS-example) | Not Supported | [partially supported](https://caniuse.com/#feat=webp) |
| **PVR** | Not Supported | Supported | Supported iOS devices | Supported iOS devices |
| **ETC1** | Supported | Not Supported | Supported Android devices | Supported Android devices |
| **ETC2** | Supported with __WebGL2__ or __WebGL__ extension if available |

By default, __Cocos Creator 3D__ outputs the original image during construction. If you need to compress an image during the build process, you can select this image in the **Assets Panel** and then manage it in the **Property Inspector** to edit the texture format of the image.

![compress-texture](compress-texture/compress-texture.png)

## Detailed compression textures

If you want to use compressed textures, you need to turn on the __compressed texture option__ when you build the project:

![compress-texture-build](compress-texture/compress-build.png)

When __Cocos Creator 3D__ builds the image, it will find whether the current image has been already configured to use compressed textures. If not, it will continue to find whether the default configuration has been made. If not, it will output the original image.

If the configuration of the compressed texture is found, the image will be compressed according to the found configuration. Multiple texture formats can be specified on one platform, and each texture format is compressed to generate an image of the specified format when it is constructed.

These generated images will not all be loaded into the engine during runtime, the engine will choose to load the appropriate image according to the configuration in `macro.SUPPORT_TEXTURE_FORMATS`. `macro.SUPPORT_TEXTURE_FORMATS` enumerates all the image formats supported by the current platform. When the engine loads the images, it will find, from the generated images in this list, **the format with the highest priority** (that is, the order is higher) to load.

The user can customize the supported image assets for a platform and the priority of the loading order, by modifying `macro.SUPPORT_TEXTURE_FORMATS`.

## Example

![1](compress-texture/compress-1.jpg)
![2](compress-texture/compress-2.jpg)

In the example images above, the default platform is configured with compressed textures in `png` format, the web platform is configured with compressed textures in `pvr` and `png` formats, and no configuration is added on other platforms. When building a web platform, this image will be compressed into two formats: `pvr` and `png`. When building other platforms, it will only generate images in the `png` format.

The default settings of `macro.SUPPORT_TEXTURE_FORMATS` is only supported by `pvr` on the iOS platform. This means `pvr` images will only be loaded on iOS browsers, and browsers on other platforms will load `png` images.