# PageviewIndicator Component Reference

The PageViewIndicator component is used to display the current page number of the PageView and the page where the tag is currently located.

![pageviewindicator.png](./pageviewindicator/pageviewindicator.png)

Click the **Add Component** button at the bottom of the **Inspector** panel and select **UI/PageViewIndicator** to add the PageViewIndicator component to the node.

## PageviewIndicator Properties

| Properties | Function Description |
| ----------- | ----------- |
| CellSize    | The cellsize for each element |
| Direction   | The location direction of PageViewIndicator, including **HORIZONTAL** and **VERTICAL** |
| Spacing     | The distance between each element |
| SpriteFrame | The spriteFrame for each element |

## Detailed Explanation

PageviewIndicator is not used alone, it needs to be used with `PageView` to create a tag of the number of pages corresponding to the page. When you slide to a page, PageviewIndicator will highlight its corresponding mark.
