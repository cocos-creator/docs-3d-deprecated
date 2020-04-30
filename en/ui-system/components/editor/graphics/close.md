# close

Use `close()` to create a path from current point to beginning point.

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.moveTo(20,20);
ctx.lineTo(20,100);
ctx.lineTo(70,100);
ctx.close();
ctx.stroke();
```

<a href="close.png"><img src="close.png"></a>

<hr>

Return to [Graphics Component Reference](../graphics.md).
