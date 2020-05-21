# ScrollBar Component Reference

The ScrollBar allows the user to scroll a image by dragging a sliding block. It's a bit similar to the __Slider__ component, but it is mostly used for scrolling while Slider is used to set values.

![scrollbar.png](scroll/scrollbar.png)

Click the __Add Component__ button at the bottom of the __Inspector__ panel and select __UI/ScrollBar__ to add the ScrollBar component to the node.

## ScrollBar Properties

| Properties | Function Explanation |
| -------------- | ----------- |
| AutoHide Time | Auto hide time, need to set `Enable Auto Hide` along with it |
| Direction | Scroll direction, including __HORIZONTAL__ and __VERTICAL__
| EnableAutoHide | Enable or disable auto hide. If this property is enabled, it will automatically disappear for `Auto Hide Time` times after the ScrollBar is displayed |
| Handle | ScrollBar foreground picture. Its length/width will be calculated according to the content size of ScrollView and the dimensions of the actual display area |

## Detailed Explanation

The ScrollBar normally is used together with `ScrollView` instead of being used alone. Also, ScrollBar needs to assign a `Sprite` component, i.e. `Handle` in the Inspector panel.

Normally we will also designate a background image to ScrollBar. This can be used to indicate the length or width of the whole ScrollBar.
