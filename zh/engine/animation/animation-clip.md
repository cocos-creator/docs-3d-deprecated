
# 动画剪辑

动画剪辑是一组动画曲线，包含了所有动画数据。

## 动画曲线

动画曲线描述了某一对象上某一属性值随着时间的变化。
在内部，动画曲线存储了一系列时间点，每个时间点都对应着一个（曲线）值，称为一帧，或关键帧。
当动画系统运作时，动画组件根据当前动画状态计算出指定时间点应有的（结果）值并赋值给对象，完成属性变化；这一计算过程称为采样。

下图是一条示例曲线，它包含 6 个关键帧：

<canvas id="curve-example-canvas" width="400" height="300"></canvas>
<script src="./curve-example.js">
</script>
<script>
drawCurve(document.getElementById("curve-example-canvas"), 6, {xAxisText: "帧时间（秒）", yAxisText: "曲线值"});
</script>

以下代码片段演示了如何程序化地创建动画剪辑。
```ts
import { AnimationClip, color, v3 } from "Cocos Creator 3D";
const animationClip = new AnimationClip();
animationClip.duration = 1.0; // 整个动画剪辑的周期。任何帧时间都不应该大于此属性。
const headCurveKeys = [ 0.3, 0.6, 0.9 ];
const headCurveValues = [ v3(0.0), v3(0.5), v3(0.0) ];
const bodyCurveKeys = [ 0.0, 0.2, 0.4, 0.6, 0.8, 1.0 ];
const bodyCurveValues = [ color(0), color(51), color(102), color(153), color(204), color(255) ];
animationClip.keys = [ headCurveKeys, bodyCurveKeys ]; // 该动画剪辑所有曲线共享的帧时间
animationClip.curveDatas = {
    "/Head": {
        "position": { // `Head` 子结点的 `position` 属性的曲线
            keys: 0, // 引用的帧时间，它是 `Animation.keys` 的索引，对于此处来说，即引用 `headCurveKeys`
            values: headCurveValues,
        },
    },
    "/Body": {
        comps: {
            "cc.Sprite": { // `Body` 子结点上，`cc.Sprite` 组件的 `color` 属性的曲线
                keys: 1, // 即 `bodyCruveKeys`
                values: bodyCurveValues,
            },
        },
    },
};
```

以上创建的动画剪辑包含两条曲线：
- 一条曲线控制子结点 `Head` 的位置变化，包含 3 帧，使得 `Head` 的 x 坐标由 0 变化为 0.5 再变化为 0。
- 另一条曲线控制子结点 `Body` 上 `cc.Sprite` 组件的颜色变化，包含 6 帧，
使得 `Body` 上的 `cc.Sprite` 组件的颜色从黑逐渐变化为白。

注意，曲线的帧时间是以引用方式索引到 `AnimationClip.keys` 数组中的。
如此一来，多条曲线可以共享帧时间。这将带来额外的性能优化。

### 目标对象

动画曲线的目标可以是任意 Cocos Creator 3D 结点以及其上附加的组件。
曲线记录了目标结点的相对路径，
运行时，由动画组件根据此路径动态确定目标对象。
例如，若曲线的路径为 `/Spline/Leg` ，而动画剪辑的所在结点<sup id="a1">[1](#f1)</sup>为 `Human`，
则在运行时，曲线的目标结点为 `Human` 结点的 `Spline` 子结点的 `Leg` 子结点；
而当曲线的路径为 `/` 或空字符串时，曲线的目标结点即为 `Human` 结点本身。

动画曲线的这种动态绑定特性使得动画剪辑可以复用到多个对象上。

### 采样

若采样时间点恰好就等于某一关键帧的时间点，则使用该关键帧上的动画数据。
否则——当采样时间点居于两帧之间时，结果值应同时受两帧数据的影响，
采样时间点在两处关键帧的时刻区间上的比例（`[0,1]`）反应了影响的程度。
Cocos Creator 3D 允许将该比例映射为另一个比例，以实现不同的“渐变”效果。
这些映射方式，在 Cocos Creator 3D中称为**渐变方式**。
在比例确定之后，根据指定的**插值方式**计算出最终的结果值。
渐变方式和插值方式都影响着动画的平滑度。

#### 渐变方式

可以为每一帧指定渐变方式，也可以为所有帧指定统一的渐变方式。
渐变方式可以是内置渐变方式的名称或贝塞尔控制点。

以下列出了几种常用的渐变方式。
- `linear` 保持原有比例，即线性渐变；当未指定渐变方式时默认使用这种方式；
- `constant` 始终使用比例 0，即不进行渐变；与插值方式 `Step` 类似；
- `quadIn` 渐变由慢到快。
- `quadOut` 渐变由快到慢。
- `quadInOut` 渐变由慢到快再到慢。
- `quadOutIn` 渐变由快到慢再到快。
- `IBezierControlPoints`

<script src="./easing-method-example.js"> </script>
<button onclick="onEasingMethodExampleButtonClicked()">展开对比</button>
<div id="easing-method-example-panel"> </div>

#### 曲线值与插值方式

有些插值算法需要每一帧的曲线值中存储额外的数据，因此，
曲线值与目标属性的值类型不一定相同。
对于数值类型或值类型，Cocos Creator 3D 提供了几种通用的插值方式；
同时，也可以定义自己的插值方式。

当曲线数据的 `interpolate` 属性为 `true` 时，曲线将尝试使用插值函数：
- 若曲线值的类型为 `number`、`Number`，将应用线性插值；
- 否则，若曲线值继承自 `ValueType`，将调用 `ValueType` 的 `lerp` 函数完成插值，
Cocos Creator 3D 内置的大多数值类型都将其 `lerp` 实现为线性插值；
- 否则，若曲线值是[可插值的]()，将调用曲线值的 `lerp` 函数完成插值<sup id="a2">[2](#f2)</sup>。

若曲线值不满足上述任何条件，或当曲线数据的 `interpolate` 属性为 `false`时，
将不会进行插值操作 --- 永远使用前一帧的曲线值作为结果。

```ts
import { AnimationClip, color, IPropertyCurveData, SpriteFrame, v3 } from "Cocos3D";

const animationClip = new AnimationClip();

const keys = [ 0, 0.5, 1.0, 2.0 ];
animationClip.duration = keys.length === 0 ? 0 : keys[keys.length - 1];
animationClip.keys = [ keys ]; // 所有曲线共享一列帧时间

// 使用数值的线性插值
const numberCurve: IPropertyCurveData = {
    keys: 0,
    values: [ 0, 1, 2, 3 ],
    /* interpolate: true, */ // interpolate 属性默认打开
};

// 使用值类型 Vec3 的 lerp()
const vec3Curve: IPropertyCurveData = {
    keys: 0,
    values: [ v3(0), v3(2), v3(4), v4(6) ],
    interpolate: true,
};

// 不插值（因为显式禁用了插值）
const colorCuve: IPropertyCurveData = {
    keys: 0,
    values: [ color(255), color(128), color(61), color(0) ],
    interpolate: false, // 不进行插值
};

// 不插值（因为 SpriteFrame 无法进行插值）
const spriteCurve: IPropertyCurveData = {
    keys: 0,
    values: [
        new SpriteFrame(),
        new SpriteFrame(),
        new SpriteFrame(),
        new SpriteFrame()
    ],
};
```

下列代码展示了如何自定义插值算法：

```ts
import { ILerpable, IPropertyCurveData, Quat, quat, Vec3, v3, vmath } from "Cocos3D";

class MyCurveValue implements ILerpable {
    public position: Vec3;
    public rotation: Quat;

    constructor(position: Vec3, rotation: Quat) {
        this.position = position;
        this.rotation = rotation;
    }

    /** 将调用此方法进行插值。
     * @param this 起始曲线值
     * @param to 目标曲线值
     * @param t 插值比率，取值范围为 [0, 1]
     * @param dt 起始曲线值和目标曲线值之间的帧时间间隔
     */
    lerp (to: MyCurveValue, t: number, dt: number) {
        return new MyCurveValue(
            // 位置属性不插值
            this.position.clone(),
            // 旋转属性使用 Quat 的 lerp() 方法
            this.rotation.lerp(to.rotation, t), //
        );
    }

    /** 此方法在不插值时调用。
      * 它是可选的，若未定义此方法，则使用曲线值本身（即 `this`）作为结果值。
      */
    getNoLerp () {
        return this;
    }
}

/**
 * 创建了一条曲线，它实现了在整个周期内平滑地旋转但是骤然地变换位置。
 */
function createMyCurve (): IPropertyCurveData {
    const rotation1 = quat();
    const rotation2 = quat();
    const rotation3 = quat();

    vmath.quat.rotateY(rotation1, rotation1, 0);
    vmath.quat.rotateY(rotation2, rotation2, Math.PI);
    vmath.quat.rotateY(rotation3, rotation3, 0);

    return {
        keys: 0 /* 帧时间 */,
        values: [
            new MyCurveValue(v3(0), rotation1),
            new MyCurveValue(v3(10), rotation2),
            new MyCurveValue(v3(0), rotation3),
        ],
    };
}
```

渐变方式和插值方式都影响着动画的平滑度。


## 循环模式

可以通过设置 `AnimationClip.wrapMode` 为动画剪辑设置不同的循环模式。

以下列出出了几种常用的循环模式：

| AnimationClip.wrapMode | 效果 |
|---|---|
| WrapMode.Normal | 播放到结尾后停止。 |
| WrapMode.Loop | 循环播放。 |
| WrapMode.PingPng | 从动画开头播放到结尾后，从结尾开始反向播放到开头，如此循环 |

对于更多的循环模式，见 [WrapMode]()。

<b id="f1">1</b> 动画剪辑的所在结点是指引用该动画剪辑的动画状态对象所在动画组件所附加的结点。 [↩](#a1)

<b id="f2">2</b> 对于数值、四元数以及各种向量，Cocos 提供了相应的可插值类以实现[三次样条插值](https://en.wikipedia.org/wiki/Spline_interpolation)。 [↩](#a2)