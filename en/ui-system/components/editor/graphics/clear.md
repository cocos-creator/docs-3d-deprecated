# clear

Use `clear()` to clear all paths.

## Example

```javascript
update: function (dt) {
    var ctx = node.getComponent(cc.GraphicsComponent);
    ctx.clear();
    ctx.circle(200,200, 200);
    ctx.stroke();
}

```

<hr>

Return to [Graphics Component Reference](../graphics.md).
