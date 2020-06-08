# lineJoin

`lineJoin` 属性设置或返回线条末端线帽的样式。

| 参数 |   描述
| -------------- | ----------- |
|cc.GraphicsComponent.LineJoin.BEVEL   | 创建斜角。
|cc.GraphicsComponent.LineJoin.ROUND  | 创建圆角。
|cc.GraphicsComponent.LineJoin.MITER | 默认。创建尖角。

## 实例

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

返回 [Graphics 组件参考](../graphics.md)

