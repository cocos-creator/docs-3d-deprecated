# stroke

The `stroke()` method actually draws a path defined by a path method such as `moveTo()` and `lineTo()`. The default color is black.

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.moveTo(20,100);
ctx.stroke(20,20);
ctx.stroke(70,20);
ctx.stroke();
```

<a href="stroke.png"><img src="stroke.png"></a>

<hr>

Return to [Graphics Component Reference](../graphics.md).
