# lineWidth

Use `lineWidth()` method to add a new point, and then create a line from that point to the last specified point in the canvas.

| Parameter | Description
| -------------- | ----------- |
| number | The width of the current line, in pixels |

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.lineWidth = 20;
ctx.rect(20,20,80,100);
ctx.stroke();
```

<a href="lineWidth.png"><img src="lineWidth.png"></a>

<hr>

Return to [Graphics Component Reference](../graphics.md).
