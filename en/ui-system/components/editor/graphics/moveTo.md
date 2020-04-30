# moveTo

Use `moveTo` method set the starting point of a path.

Parameter | Description |
| -------------- | ----------- |
| X | The x coordinate of the target location of the path. |
| Y | The y coordinate of the target position of the path. |

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.moveTo(0,0);
ctx.lineTo(300,150);
ctx.stroke();
```

<a href="moveTo.png"><img src="moveTo.png"></a>

<hr>

Return to [Graphics Component Reference](../graphics.md).
