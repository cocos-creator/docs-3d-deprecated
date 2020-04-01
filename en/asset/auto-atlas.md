# Auto Atlas

**Auto Atlas Assets** is the picture-combining method that comes as part of __Cocos Creator 3D__. You can pack a specified series of images into a __sprite sheet__. This capability is very similar to the function of __Texture Packer__.

## Creating Auto Atlas Assets

__Right-click__ in the **Explorer** panel, select **New-> Auto Atlas Configuration** in the menu. Selecting this option will create a new asset similar to **AutoAtlas.pac**.

![create auto atlas](auto-atlas/create-auto-atlas.jpg)

**AutoAtlas** will pack all **SpriteFrame** assets in the same folder into a big **Sprite Atlas** asset during the build process. We might add other ways to choose assets for packing in the future. If the original **SpriteFrame** asset have been configured, then all configurations will be preserved.

## Configuring Auto Atlas Assets

After selecting an **Auto Atlas Resource** in the __Explorer__ panel, the **Attributes Inspector** panel will display all of the configurable items for the **Auto Atlas Resource**.

| Properties | Functional Description
| -------------- | ----------- |
| Maximum Width | Single Atlas Maximum Width |
| Maximum Height | Maximum Height of a Single Atlas |
| Spacing | Spacing between shreds in the atlas |
| Allow Rotation | Whether Rotate Fragments |
| Output size is square | Whether to force the size of the atlas to be square |
| The output size is a power of two | Whether to set the size of the atlas to a multiple of a square |
| Algorithm | Atlas packaging strategy, currently only one option |
| Output format | Atlas image generation format, the available formats are [png, jpg, webp ...] |
| Expand the edge | Expand a pixel outer frame outside the border of the broken image, and copy the adjacent broken image pixels to the outer frame. This feature is also called "Extrude". |
| Does not include unreferenced assets | In preview, this option will not take effect, this option will take effect after building |

After the configuration is complete, you can click the **Preview** button to preview the packaged results. The related results generated according to the current automatic atlas configuration will be displayed in the area below the **Properties Inspector**. Please note that after each configuration, you need to re-click **Preview** to regenerate the preview image.

The results are divided into:
   - __Packed Textures:__ Display the packaged atlas pictures and picture-related information. If there are multiple pictures to be generated, they will be listed below in the **Attributes Inspector**.
   - __Unpacked Textures:__ Display the broken image assets that cannot be packed into the atlas. The cause may be that the size of these broken image assets is larger than the size of the atlas assets. At this time, the configuration or fragmentation of the following atlas may need to be adjusted. The size of the figure is increased.

## Generating an Atlas

When inside the editor or previewing the project __Cocos Creator 3D__ is directly using the split **SpriteFrame** assets, only after you build the project with the option **AutoAtlas** enabled, the Atlas asset will be generated and be used instead of all split assets.