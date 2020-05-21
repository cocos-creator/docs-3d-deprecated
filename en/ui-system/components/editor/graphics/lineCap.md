# Line Cap

The `lineCap()` method sets or returns the style of the line end cap.

| Parameter | Description |
| -------------- | ----------- |
| *cc.GraphicsComponent.LineCap.BUTT*   | Default. Add a straight edge to each end of the line. |
| *cc.GraphicsComponent.LineCap.ROUND*  | Add a circular cap to each end of the line. |
| *cc.GraphicsComponent.LineCap.SQUARE* | Add a square line cap to each end of the line. |

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.lineCap = cc.GraphicsComponent.LineCap.ROUND;
ctx.lineWidth = 10;
ctx.moveTo(100, 100);
ctx.lineTo(300, 100);
ctx.stroke();
```

<a href="lineCap.png"><img src="lineCap.png"></a>

<hr>

Return to the [Graphics Component Reference](../graphics.md) documentation.
