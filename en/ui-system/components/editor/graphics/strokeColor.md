# StrokeColor

The `strokeColor` property sets or returns the color used for the stroke.

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.lineWidth = 2;
ctx.strokeColor = cc.hexToColor('#0000ff');
ctx.rect(20,20,250,200);
ctx.stroke();
```

<a href="strokeColor.png"><img src="strokeColor.png"></a>

<hr>

Return to [Graphics Component Reference](../graphics.md).
