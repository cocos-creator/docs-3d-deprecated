# Layout Component Reference

__Layout__ is a container component. The container can unlock the auto-layout function to automatically arrange all the sub-objects according to the specifications, so that the developer can use it to make list, page turning and other functions conveniently.

- Horizontal Layout

  ![horizontal-layout.png](layout/horizontal-layout.png)

- Vertical Layout

  ![vertical-layout.png](layout/vertical-layout.png)

- Grid Layout

  ![grid-layout.png](layout/grid-layout.png)

Click the __Add Component__ button at the bottom of the __Inspector__ panel and select __UI/Layout__ to add the __Layout__ component to the node.

## Layout Properties

| Properties           | Function Explanation      |
| --------------       | -----------   |
| *Type*                 | Layout type, including __NONE__, __HORIZONTAL__, __VERTICAL__ and __GRID__. See [Auto Layout](../engine/auto-layout.md) for details. |
| *ResizeMode*           | Resize mode, including __NONE__, __CHILDREN__ and __CONTAINER__. |
| *PaddingLeft*          | The left padding between the sub-object and the container frame in the layout. |
| *PaddingRight*         | The right padding between the sub-object and the container frame in the layout. |
| *PaddingTop*           | The top padding between the sub-object and the container frame in the layout. |
| *PaddingBottom*        | The bottom padding between the sub-object and the container frame in the layout. |
| *SpacingX*             | The spacing between sub-objects in the horizontal layout. __NONE__ mode __doesn't__ have this property. |
| *SpacingY*             | The spacing between sub-objects in the vertical layout. __NONE__ mode __doesn't__ have this property. |
| *HorizontalDirection* | When it is designated as horizontal layout, which side does the first child node start in the layout? The container left or the right? When the __Layout__ type is set to __Grid__, this property with `Start Axis` property determines the starting horizontal alignment of __Grid__ layout elements. |
| *VerticalDirection*   | When it is designated as vertical layout, which side does the first child node start in the layout? The container upside or the downside? When the __Layout__ type is set to __Grid__, this property with `Start Axis` property determines the starting vertical alignment of __Grid__ layout elements. |
| *CellSize*            | This property is only available in __Grid__ layout, __CHILDREN__ resize mode. The size of each child elements. |
| *StartAxis*           | This property is only available in __Grid__ layout, the arrangement direction of child elements. |
| *AffectedByScale*    | Whether the scaling of the child node affects the layout.  |

## Detailed Explanation

The default layout type is __NONE__ after adding the __Layout__ component. It indicates that the container won't change size and position of the sub-object. When the user places sub-object manually, the container will take the minimum rectangular region that can contain all the sub-objects as its own size.

You can switch the layout container type by altering `Type` property in __Inspector__ panel, all the layout types support `Resize Mode`.

- When __Resize Mode__ is __NONE__, the size of the container and sub-object is independent of each other.

- When __Resize Mode__ is __CHILDREN__, the size of the sub-object will change with the size of the container.

- When __Resize Mode__ is __CONTAINER__, the size of the container will change with the size of the sub-object.

When using __Grid__ layout, the __Start Axis__ is very important.

- When choosing __HORIZONTAL__, it will fill an entire row before a new row is started.

- When choosing __VERTICAL__, it will fill an entire column before a new column is started.

> __Node__: After setting the __Layout__, the results need to be updated until the next frame, unless you manually call `updateLayout` API.
