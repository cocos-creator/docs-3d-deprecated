# Graphics Component Reference

The Graphics component provides a series of drawing interfaces that reference the canvas's drawing interface.

![](graphics/graphics.png)

Select a node in the **Hierarchy** panel, then click the **Add Component** button at the bottom of the **Inspector** panel and select **Graphics** from **UI -> Render**. Then you can add the Graphics component to the node.

## Graphic Properties

| Properties | Function Explanation |
| -------------- | ----------- |
| [FillColor](graphics/fillColor.md)     | Sets or returns the color used for the fill. |
| [LineCap](graphics/lineCap.md) | LineCap determines how the end points of every line are drawn. |
| [LineJoin](graphics/lineJoin.md)       | LineJoin determines how two connecting segments (of lines, arcs or curves) with non-zero lengths in a shape are joined together. |
| [MiterLimit](graphics/miterLimit.md)   | Sets or returns the maximum miter length.  |
| [StrokeColor](graphics/strokeColor.md) | Stroke color. Sets or returns the color used for the stroke. |

## Graphics API

### Path

| Function | Function Explanation |
| -------------- | ----------- |
| [moveTo](graphics/moveTo.md) (x, y) | Move the path to a specified point in the canvas without creating lines. |
| [lineTo](graphics/lineTo.md) (x, y) | Adds a straight line to the path. |
| [bezierCurveTo](graphics/bezierCurveTo.md) (c1x, c1y, c2x, c2y, x, y) | Adds a cubic Bézier curve to the path. |
| [quadraticCurveTo](graphics/quadraticCurveTo.md) (cx, cy, x, y) | Adds a quadratic Bézier curve to the path. |
| [arc](graphics/arc.md) (cx, cy, r, a0, a1, counterclockwise) | Adds an arc to the path which is centered at (cx, cy) position with radius r starting at startAngle and ending at endAngle going in the given direction by counterclockwise (defaulting to false). |
| [ellipse](graphics/ellipse.md) (cx, cy, rx, ry) | Adds an ellipse to the path. |
| [circle](graphics/circle.md) (cx, cy, r) | Adds an circle to the path. |
| [rect](graphics/rect.md) (x, y, w, h) | Adds an rectangle to the path. |
| [close](graphics/close.md) () | Adds an round corner rectangle to the path. |
| [stroke](graphics/stroke.md) () | Draws a filled rectangle. |
| [fill](graphics/fill.md) () | Erasing any previously drawn content. |
| [clear](graphics/clear.md) () | Causes the point of the pen to move back to the start of the current path. It tries to add a straight line from the current point to the start. |
