# RichText Component Reference

RichText component could be used for displaying a string with multiple styles. You could customize the text style of each text segment with a few simple BBCode.

The currently supported tags are: `color`, `size`, `outline`, `b`, `i`, `u`, `br`, `img` and `on`, these tags could also be nested with each other.

For more information about BBCode, please refer to the **BBCode format** section below.

![richtext](richtext/richtext.png)

Click the **Add Component** button at the bottom of the **Inspector** panel and select **RichText** from **UI -> Render** to add the RichText component to the node.

## RichText Properties

| Properties       | Function Explanation  |
| --------------   | -----------   |
| Font             | Custom TTF font of RichText, all the label segment will use the same custom TTF font.  |
| FontSize         | Font size, in points (**Note**: This field does not affect the font size set in BBCode.) |
| HandleTouchEvent | Once checked, the RichText will block all input events (mouse and touch) within the bounding box of the node, preventing the input from penetrating into the underlying node. |
| Horizontal Align | Horizontal alignment   |
| ImageAtlas      | The image atlas for the img tag. For each `src` value in the `img` tag, there should be a valid `spriteFrame` in the imageAtlas. |
| LineHeight      | Line height, in points.    |
| MaxWidth        | The maximize width of RichText, pass 0 means not limit the maximize width. |
| String           | Text of the RichText, you could use BBcode in the string. |

## BBCode format

### Basics

Currently the supported tag list is: `size`, `color`, `b`, `i`, `u`, `img` and `on`. There are used for customizing the font size, font color, bold, italic, underline, image and click event.
Every tag should has a start tag and an end tag. The name and attribute format of the start tag must meet the requirements and be all lowercase.

It will check the start tag name, but the end tag name restrict is loose, it only requires a `</>` tag, the end tags name doesn't matter.

Here is an example of the `size` and `color` tag:

`<color=green>Hello</color>，<size=50>Creator3D</>`

### Supported tags

**Note**: All tag names should be lowercase and the attribute assignment should use `=` sign.

| Name | Description | Example | Note |
| -------|------- | -----|------ |
| color | Specify the font rendering color, the color value could be a built-in value or a hex value. eg, use `#ff0000` for red. | `<color=#ff0000>Red Text</color>` |  |
| size | Specify the font rendering size, the size should be a integer. | `<size=30>enlarge me</size>` | The size assignment should use `=` sign. |
| outline | Specify the font outline, you could customize the outline color and width by using the `color` and `width` attribute. | `<outline color=red width=4>A label with outline</outline>` | If you don't specify the color or width of outline, the default color value is `#ffffff` and the default width is `1`. |
| b | Render text as bold font | `<b>This text will be rendered as bold</b>`| The tag name must be lowercase and cannot be written in `bold`. |
| i | Render text as italic font | `<i>This text will be rendered as italic</i>`| The tag name must be lowercase and cannot be written in `italic`. |
| u | Add a underline to the text |`<u>This text will have a underline</u>`| The tag name must be lowercase and cannot be written in `underline`. |
| on | Specify a event callback to a text node, when you click the node, the callback will be triggered. | `<on click="handler"> click me! </on>` | Except for `on` tag, `color` and `size` tags can also add `click` event parameter. eg. `<size=10 click="handler2">click me</size>` |
| param | When the click event is triggered, the value can be obtained in the second attribute of the callback function. | `<on click="handler" param="test"> click me! </on>` | Depends on the click event. |
| br | Insert a empty line | `<br/>` | `<br></br>` and `<br>` are both invalid tags. |
| img | Add image and emoji support to your RichText. The emoji name should be a valid spriteframe name in the ImageAtlas. | `<img src='emoji1' click='handler' />` | Only `<img src='foo' click='bar' />` is a valid img tag. If you specify a large emoji image, it will scale the sprite height to the line height of the RichText together with the sprite width. |

### Nested Tags

Tags could be nested, the rules is the same as normal HTML tags. For example, the following settings will render a label with font size 30 and color value green.

`<size=30><color=green>I'm green</color></size>`

is equal to:

`<color=green><size=30>I'm green</size></color>`

<!--
There are two ways to set the color of RichText:
1. Selected the node and set the overall color of RichText in **RichTextComponent -> String** of the **Inspector**.
2. Use BBCode to set colors on the inside of RichText separately.
Use BBCode to set the colors separately inside RichText.

**Note**: The two cannot be mixed. If mixed, the color set in the **second** way will prevail at runtime.
-->

## Detailed explanation

The RichText component is implemented in the Javascript layer and uses the Label node as the rendering part. All the layout logic goes also in Javascript layer. This means if you create a very complex RichText which will end up with many label node created under the hook. And all these label node are using system font for rendering.

So, you should avoid modifying the RichText content frequently in your game's main loop, which can lead to lower performance. Also, try to use the normal Label component instead of the RichText component if you can, and BMFont is the most efficient.
