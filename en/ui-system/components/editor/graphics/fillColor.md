# Fill Color

The `fillColor` property represents the color used for the [`fill`](./fill.md) function.

## Example

```javascript
var ctx = node.getComponent(cc.GraphicsComponent);
ctx.fillColor = new cc.Color().fromHEX('#0000ff');
ctx.rect(20,20,250,200);
ctx.stroke();
```

![](fillColor.png)

<hr>

Return to the [Graphics Component Reference](../graphics.md) documentation.
