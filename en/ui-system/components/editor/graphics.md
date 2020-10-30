# Graphics Component Reference

The __Graphics__ component provides a series of drawing functions that reference the Web canvas's drawing APIs.

![](graphics/graphics.png)

Select a node in the __Hierarchy__ panel, then click the __Add Component__ button at the bottom of the __Inspector__ panel and select __Graphics__ from __UI -> Render__. Then you can add the __Graphics__ component to the node.

## Graphic Properties

| Properties | Function Explanation |
| -------------- | ----------- |
| [*FillColor*](graphics/fillColor.md)     | Sets or returns the color used for the `fill` function. |
| [*LineCap*](graphics/lineCap.md) | LineCap determines how the end points of every line are drawn. |
| [*LineJoin*](graphics/lineJoin.md)       | LineJoin determines how two connecting segments (of lines, arcs or curves) with non-zero lengths in a shape are joined together. |
| [*MiterLimit*](graphics/miterLimit.md)   | Sets or returns the maximum miter length.  |
| [*StrokeColor*](graphics/strokeColor.md) | Stroke color. Sets or returns the color used for the stroke. |
<!-- [LineWidth](graphics/lineWidth.md): Current line width. -->

## Graphics API

### Path

| Function | Function Explanation |
| -------------- | ----------- |
| [*moveTo*](graphics/moveTo.md) (x, y) | Move the render cursor to a specified point in the canvas without creating lines. |
| [*lineTo*](graphics/lineTo.md) (x, y) | Adds a straight line to the path. |
| [*bezierCurveTo*](graphics/bezierCurveTo.md) (c1x, c1y, c2x, c2y, x, y) | Adds a cubic Bézier curve to the path. |
| [*quadraticCurveTo*](graphics/quadraticCurveTo.md) (cx, cy, x, y) | Adds a quadratic Bézier curve to the path. |
| [*arc*](graphics/arc.md) (cx, cy, r, a0, a1, counterclockwise) | Adds an arc to the path which is centered at (cx, cy) position with radius r starting at startAngle and ending at endAngle going in the given direction by counterclockwise (defaulting to false). |
| [*ellipse*](graphics/ellipse.md) (cx, cy, rx, ry) | Adds an ellipse to the path. |
| [*circle*](graphics/circle.md) (cx, cy, r) | Adds a circle to the path. |
| [*rect*](graphics/rect.md) (x, y, w, h) | Adds a rectangle to the path. |
| [*close*](graphics/close.md) () | Close the previous path by connecting the last point and the beginning point. |
| [*stroke*](graphics/stroke.md) () | Stroke the path as lines |
| [*fill*](graphics/fill.md) () | Close and fill a path's inner surface |
| [*clear*](graphics/clear.md) () | Erase any previously drawn content. |

- [UI Basic Components](base-component.md)

- [UI Renderer Components](render-component.md)
