# ellipse

Use `ellipse()` method to create an ecllipse.

| Parameter | Description |
| --------- | ----------- |
| cx | The x coordinate of the center of the circle. |
| cy | The y coordinate of the center of the circle. |
| rx | The x radius of the circle. |
| ry | The y radius of the circle. |

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.ellipse(200,100, 200,100);
ctx.stroke();
```

<a href="ellipse.png"><img src="ellipse.png"></a>

<hr>

Return to [Graphics Component Reference](../graphics.md).
