# Label Component Reference

Label component is used to display a piece of text, the text can be **System Font**, **TrueType Font**, or **BMFont**. In addition, Label also has typesetting function.

![label-property](./label/label-property.png)

Select a node in the **Hierarchy** panel, then click the **Add Component** button at the bottom of the **Inspector** panel and select **Label** from **UI -> Render** to add the Label component to the node.

## Label Properties

| Properties | Function Explanation |
| -------------- | ----------- |
| Color | Label color. |
| String | Text content character string. |
| HorizontalAlign | Horizontal alignment of the text, including `LEFT`, `CENTER` and `RIGHT`. |
| VerticalAlign | Vertical alignment of the text, including `LEFT`, `CENTER` and `RIGHT`. |
| FontSize | Font size of the text. |
| FontFamily | Font family name (Takes effect when using **System Font**). |
| LineHeight | Line height of the text. |
| Overflow | Layout of the text. Currently supports `CLAMP`, `SHRINK` and `RESIZE_HEIGHT`. For details, see [Label Layout](#label-layout) below or [Label Layout](../engine/label-layout.md) document. |
| EnableWrapText | Enable or disable the text line feed (which takes effect when the `Overflow` is set to `CLAMP` or `SHRINK`). |
| Font | Specify the font file needed for text rendering. If the System Font is used, then this property can be set to `null`. |
| UseSystemFont | If enabled, **System Font** will be used. |
| CacheMode | Text cache mode, including `NONE`, `BITMAP` and `CHAR`. Takes effect only for **System Font** or **TTF Font**, BMFont does not require this optimization. See [Cache Mode](#cache-mode) below for details. |
| IsBold | If enabled, bold effect will be added to the text. (Takes effect when using System Font or some TTF font). |
| IsItalic | If enabled, italic effect will be added to the text. (Takes effect when using System Font or TTF font). |
| IsUnderline | If enabled, underline effect will be added to the text. (Takes effect when using System Font or TTF font). |
| SrcBlendFactor | The source image blend mode. |
| DstBlendFactor | The destination image blend mode. Together with the above properties, you can mix the foreground and background Sprite in different ways to render, and the effect preview can refer to [glBlendFunc Tool](http://www.andersriggelsen.dk/glblendfunc.php). |

## Label Layout

| Properties | Function Explanation |
| -------------- | ----------- |
| CLAMP | The text size won't zoom in/out as the Content size changes.<br>When Wrap Text is disabled, parts exceeding the Content size won't be shown according to the normal character layout.<br>When Wrap Text is enabled, it will try to wrap the text exceeding the boundaries to the next line. If the vertical space is not enough, any not completely visible text will also be hidden. |
| SHRINK | The text size will zoom in or out as the Content size changes (it won't zoom out automatically, the maximum size that will show is specified by Font Size) as the Content Size size changes.<br>When Wrap Text is enabled, if the width is not enough, it will try to wrap the text to the next line before automatically adapting the Content Size's size to make the text show completely.<br>If Wrap Text is disabled, then it will compose according to the current text and zoom automatically if it exceeds the boundaries.<br>**Note**: This mode may takes up more CPU resources when the label is refreshed. |
| RESIZE_HEIGHT | The text Content Size will adapt to the layout of the text. The developer cannot manually change the height of text in this status, it is automatically calculated by the internal algorithm. |

## Cache Mode

| Properties | Function Explanation |
| -------------- | ----------- |
| NONE | Defaults, the entire text in label will generate a bitmap. |
|BITMAP| After selection, the entire text in the Label will still generate a bitmap. As long as the requirements of Dynamic Atlas are met, the Draw Call will be merged with the other Sprite or Label in the Dynamic Atlas. Because Dynamic Atlas consume more memory, **this mode can only be used for Label with infrequently updated text**. **Note**: Similar to NONE, BITMAP will force a bitmap to be generated for each Label component, regardless of whether the text content is equivalent. If there are a lot of Labels with the same text in the scene, it is recommended to use CHAR to reuse the memory space. |
| CHAR | The principle of CHAR is similar to BMFont, Label will cache text to the global shared bitmap in "word" units, each character of the same font style and font size will share a cache globally. Can support frequent modification of text, the most friendly to performance and memory. However, there are currently restrictions on this model, which we will optimize in subsequent releases:<br>1. **This mode can only be used for font style and fixed font size (by recording the fontSize, fontFamily, color, and outline of the font as key information for repetitive use of characters, other users who use special custom text formats need to be aware). And will not frequently appear with a huge amount of unused characters of Label.** This is to save the cache, because the global shared bitmap size is **2048 * 2048**, it will only be cleared when the scene is switched. Once the bitmap is full, the newly appearing characters will not be rendered. <br>2. Overflow does not support SHRINK.<br>3. Cannot participate in dynamic mapping (multiple labels with CHAR mode enabled can still merge Draw Call in the case of without interrupting the rendering sequence). |

**Note**: Cache Mode has an optimized effect for all platforms.

## Detailed Explanation

By dragging the TTF font file and BMFont font file into the `Font` property in the **Inspector** panel, the Label component can alter the rendering font type. If you want to stop using a font file, you can use the system font again by checking `UseSystemFont`.
