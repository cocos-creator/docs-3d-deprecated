# Ellipse

The `ellipse()` method is used to create an ellipse.

| Parameter | Description |
| --------- | ----------- |
| *cx* | The x coordinate of the center of the circle. |
| *cy* | The y coordinate of the center of the circle. |
| *rx* | The x radius of the circle. |
| *ry* | The y radius of the circle. |

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.ellipse(200,100, 200,100);
ctx.stroke();
```

<a href="ellipse.png"><img src="ellipse.png"></a>

<hr>

Return to the [Graphics Component Reference](../graphics.md) documentation.
