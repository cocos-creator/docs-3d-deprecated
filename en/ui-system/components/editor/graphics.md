# Graphics 组件参考

Graphics 组件提供了一系列绘画接口，这些接口参考了 canvas 的绘画接口来进行实现。

![](graphics/graphics.png)

新建一个空节点，然后点击 **属性检查器** 下方的 **添加组件** 按钮，从 **添加渲染组件** 中选择 **Graphics**，即可添加 Graphics 组件到节点上。

## 路径

| 方法 |   功能说明
| -------------- | ----------- |
| [moveTo](graphics/moveTo.md) (x, y) | 把路径移动到画布中的指定点，不创建线条
| [lineTo](graphics/lineTo.md) (x, y) | 添加一个新点，然后在画布中创建从该点到最后指定点的线条
| [bezierCurveTo](graphics/bezierCurveTo.md) (c1x, c1y, c2x, c2y, x, y) | 创建三次方贝塞尔曲线
| [quadraticCurveTo](graphics/quadraticCurveTo.md) (cx, cy, x, y) | 创建二次贝塞尔曲线
| [arc](graphics/arc.md) (cx, cy, r, a0, a1, counterclockwise) | 创建弧/曲线（用于创建圆形或部分圆）
| [ellipse](graphics/ellipse.md) (cx, cy, rx, ry) | 创建椭圆
| [circle](graphics/circle.md) (cx, cy, r) | 创建圆形
| [rect](graphics/rect.md) (x, y, w, h) | 创建矩形
| [close](graphics/close.md) () | 创建从当前点回到起始点的路径
| [stroke](graphics/stroke.md) () | 绘制已定义的路径
| [fill](graphics/fill.md) () | 填充当前绘图（路径）
| [clear](graphics/clear.md) () | 清楚所有路径

## 颜色，样式

| 属性 |   功能说明
| -------------- | ----------- |
| [lineCap](graphics/lineCap.md) | 设置或返回线条的结束端点样式
| [lineJoin](graphics/lineJoin.md) | 设置或返回两条线相交时，所创建的拐角类型
| [lineWidth](graphics/lineWidth.md) | 设置或返回当前的线条宽度
| [miterLimit](graphics/miterLimit.md) | 设置或返回最大斜接长度
| [strokeColor](graphics/strokeColor.md) | 设置或返回笔触的颜色
| [fillColor](graphics/fillColor.md) | 设置或返回填充绘画的颜色

更多关于 **Graphics** 的信息请前往 [绘画](graphics/index.md)

---

### [**其他渲染模块参考**](render-component.md)

### [**基础模块参考**](base-component.md)
