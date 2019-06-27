
# 动画组件

动画组件控制动画的播放。

像其他组件一样为结点添加动画组件：

```ts
import { AnimationComponent, Node } from "Cocos3D";

function (node: Node) {
    const animationComponent = node.addComponent(AnimationComponent);
}
```

动画组件管理了一组动画剪辑。
动画组件开始运作前，它为每一个动画剪辑都创建了相应的 **动画状态** 对象。
可以通过 `getState()` 获取动画剪辑对应的状态，
并通过动画状态对象的各种接口完成动画的播放、停止、变速、循环能功能。

```ts
const animationComponent = node.getComponent(AnimationComponent);
animationComponent.clips = [ idleClip, runClip ];

// 获取 `idleClip` 的状态
const idleState = animationComponent.getState(idleClip.name);
```

## 动画状态

动画状态控制某个动画剪辑在结点的播放过程。
一个动画剪辑可以同时为多个动画状态所用。

### 播放控制

动画状态提供了相应的接口以实现动画的播放控制：

接口 | 功能 |
---|---
`play()` | 播放。
`pause()` | 暂停。
`resume()` | 恢复。
`stop()` | 停止。
`speed` | 获取或设置播放速度。

*注意，你应该尽量使用动画组件提供的 `play()` 实现动画的播放而不是直接调用动画状态的 `play()`，因为这关系到动画切换*。

## 动画切换

每个动画组件都有一个“当前动画”。动画组件提供了 `play()` 方法以切换当前动画，旧的当前动画将立即停止。
在某些情况下，应用需要这种切换是“淡入淡出”的，此时应当使用 `crossFade()` 方法。
`crossFade()` 会在指定的周期内逐渐完成切换，使得切换看起来不那么突兀。

## 默认动画

当动画组件的 `playOnLoad` 为 `true` 时，
动画组件将在第一次运行时自动播放默认动画剪辑 `defaultClip`。

## 帧事件

你可以为动画的每一时间点添加事件。

`AnimationClip` 的 `events` 包含了此动画所有的事件描述，每个事件描述都具有以下属性：
```ts
{
    frame: number;
    func: string;
    params: any[];
}
```
其中 `frame` 表示事件触发的时间点，单位为秒，
例如 `0.618` 就表示当动画到达第 0.618 秒时将触发事件。

`func` 表示事件触发时回调的方法名称，事件触发时，
会在当前结点的所有组件上搜索名为 `func` 的方法，一旦找到，将 `params` 传递给它并调用。

以下代码演示了这一过程。

```ts
import { AnimationComponent, Component } from "Cocos3D";
class MyScript extends Component {
    constructor() {

    }

    public start() {
        const animationComponent = this.node.getComponent(AnimationComponent);
        if (animationComponent && animationComponent.defaultClip) {
            const { defaultClip } = animationComponent;
            defaultClip.events.push({
                frame: 0.5, // 第 0.5 秒时触发事件
                func: 'onTriggered', // 事件触发时调用的函数名称
                params: [ 0 ], // 向 `func` 传递的参数
            });
            defaultClip.updateEventDatas();
        }
    }

    public onTriggered(arg: number) {
        console.log(`I'm triggered!`);
    }
}
```

以上代码表示，`MyScript` 组件所在结点的动画组件的默认动画剪辑
在进行到第 0.5 秒将调用 `MyScript` 组件的 `test()` 方法并传递参数 `0`。