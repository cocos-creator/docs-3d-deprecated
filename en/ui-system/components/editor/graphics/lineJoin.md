# Line Join

The `lineJoin` property represents the style of the joint between two line segments.

| Possible line join options | Description |
| -------------- | ----------- |
| *cc.GraphicsComponent.LineJoin.BEVEL* | Creates a bevel |
| *cc.GraphicsComponent.LineJoin.ROUND* | Create a fillet |
| *cc.GraphicsComponent.LineJoin.MITER* | Default. Create sharp corners |

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.lineJoin = cc.GraphicsComponent.LineJoin.ROUND;
ctx.moveTo(20,20);
ctx.lineTo(100,50);
ctx.lineTo(20,100);
ctx.stroke();
```

<a href="lineJoin.png"><img src="lineJoin.png"></a>

<hr>

Return to the [Graphics Component Reference](../graphics.md) documentation.
