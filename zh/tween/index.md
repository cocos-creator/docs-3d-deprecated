# 缓动系统

为了更友好和高效的开发，并且保持 Cocos Creator 2D 缓动系统的使用体验，Cocos Creator 3D 在 tween.js 的基础上封装了一层类似于 Cocos Creator 2D 的缓动系统的 API 使用方式，并且未来将兼容 [tween.js](https://github.com/cocos-creator/tween.js) 的使用方式（目前暂未开放）。

## 简单示例

```
import { _decorator, Component, Vec3, tween } from "cc";

@ccclass("tween-test")
export class tweentest extends Component {

    private _pos: Vec3 = new Vec3(0, 0, 0);

    start () {
        Vec3.copy(this._pos, this.node.position);
        tweenUtil(this._pos)
            .to(3, new Vec3(10, 10, 10), { easing: 'Bounce-InOut' })
            .to(3, new Vec3(0, 0, 0), { easing: 'Elastic-Out' })
            .union()
            .repeat(1)
            .start();
    }

    update (deltaTime: number) {
        this.node.position = this._pos;
    }
}
```

## 注意事项

### API 差异

由于底层的实现不同，以及 3D 与 2D 的差异较大，所以目前 API 上存在以下区别：

- tween 方法改名为了 tweenUtil。
- repeat 方法语义为重复几次，即repeat(1) 代表重复一次，执行两次，在 2D 中语义为执行次数，即执行一次。
- repeat 方法多次调用为重写，在 2D 中是累加。
- repeat 方法中暂不支持插入缓动，后面会考虑支持。
- 暂不支持对单个属性进行 easing 、progress 等
- easing 的可选值有变化，并且形式与 2D 的不同。
- progress 用 easing 代替，可以输入自定义函数到 easing 参数。
- 部分 API 暂不支持：clone 、 removeSelf 、 reverseTime 、 sequnence 、 target 、 then 。
- 部分 API 尚不稳定，行为与 2D 可能有所出入。

### 特殊机制产生的差异

Cocos Creator 3D 中 Node 的 dirty 机制

为了降低更新 Node Transform 信息的频率，Node 内部维护了一个 dirty 状态，只有在调用了可能会改变 Node Transform 信息的接口，才会将 dirty 置为需要更新的状态。

但是目前的接口存在一定的局限性，例如：通过 `this.node.position` 获取到的 `position` 是一个通用的 Vec3，当执行`this.node.position.x = 1`这段代码的时候，只执行 Node position 的 getter，并没有执行postion 的 setter，由于 dirty 并没有更新，就会导致渲染时使用的节点的 Transform 信息没有更新。

目前，我们也不支持这样的调用，而是鼓励使用 setPostion 这样的接口，或者通过 position 的 setter，即以下代码方式：

```
let _pos = new Vec3(0, 1, 0);
this.node.position = _pos;      // 这里将通过 position 的 setter
this.node.setPosition(_pos);    // 这里将通过接口 setPosition
```

因此，缓动系统目前暂不支持直接为 Node Transform 相关的数据进行插值，后续变化请留意更新公告。
**注：简单示例中介绍了如何通过缓动系统改变 Node 的 Transform**。
