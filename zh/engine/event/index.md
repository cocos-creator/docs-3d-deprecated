## 事件

事件是用于引擎内对象交互的消息传递机制。

#### 事件监听

```ts
// 该事件监听每次都会触发，需要手动取消注册
xxx.on(type, func, target);
// 该事件监听只执行一次，无需取消注册
xxx.once(type, func, target);
```

的方式来监听，其中 `type` 为事件注册字符串，`func` 为执行事件监听的回调，`target` 为事件接收对象。

#### 事件取消

```ts
// 取消对象身上所有注册的该类型的事件
xxx.off(type);
// 取消对象身上该类型指定回调的事件
xxx.off(type, func);
// 取消对象身上该类型指定回调指定目标的事件
xxx.off(type, func, target);
```

#### 事件派发

```ts
// 事件派发的时候可以指定派发参数
xxx.emit(type, ...arg);
```

需要说明的是，出于底层事件派发的性能考虑，这里最多只支持传递 5 个事件参数。所以在传参时需要注意控制参数的传递个数。

## 事件说明

### 系统事件

系统事件指的是全局事件，直接从浏览器进行事件监听和派发。

```ts
cc.systemEvent.on(type, func, target);
```

当前支持的系统事件有：触摸事件，鼠标事件，重力事件，按键事件。可以通过 `cc.SystemEventType` 获取全局事件类型。

| 事件名            | 事件类型说明                                           |
| --------------  | -----------                                       |
| TOUCH_START      | 手指开始触摸事件。                                  |
| TOUCH_MOVE       | 当手指在屏幕上移动时。                                       |
| TOUCH_END         | 手指结束触摸事件。 |
| TOUCH_CANCEL | 当手指在目标节点区域外离开屏幕时。       |
| MOUSE_DOWN      | 当鼠标按下时触发一次。                                  |
| MOUSE_MOVE       | 当鼠标在目标节点在目标节点区域中移动时，不论是否按下。                     |
| MOUSE_UP         | 当鼠标从按下状态松开时触发一次。 |
| MOUSE_WHEEL | 鼠标滚动事件。       |
| MOUSE_ENTER      | 当鼠标移入目标节点区域时，不论是否按下。                                  |
| MOUSE_LEAVE       | 当鼠标移出目标节点区域时，不论是否按下。                                       |
| KEY_DOWN         | 当按下按键时触发的事件。 |
| KEY_UP | 当松开按键时触发的事件。       |
| DEVICEMOTION      | 重力感应。                                  |
| TRANSFORM_CHANGED       | 节点改变位置、旋转或缩放事件.。                                       |
| POSITION_PART         | 节点位置改变事件。统一由 TRANSFORM_CHANGED 监听，派发后判断类型。|
| ROTATION_PART | 节点旋转事件。统一由 TRANSFORM_CHANGED 监听，派发后判断类型。       |
| SCALE_PART         | 节点缩放事件。统一由 TRANSFORM_CHANGED 监听，派发后判断类型。 |
| SIZE_CHANGED | 当节点尺寸改变时触发的事件。       |
| ANCHOR_CHANGED      | 当节点锚点改变时触发的事件。                                  |
| CHILD_ADDED         | 节点子类添加。 |
| CHILD_REMOVED | 节点子类移除。       |

`KEY` 事件获取的键值列表具体可参照 API 的 `Macro` 来使用。

### UI 事件

事件处理是在节点（cc.Node）中完成的。对于组件，可以通过访问节点 this.node 来注册和监听事件。监听事件可以 通过 this.node.on() 函数来注册，方法如下：

```ts
import { _decorator, Component, Node } from "Cocos3D";
const { ccclass } = _decorator;

@ccclass("example")
export class example extends Component {
    onLoad() {
      this.node.on(Node.EventType.TOUCH_CANCEL, this.callback, this);
    }

    callback(){

    }

    onDestroy(){
      // 一般为了数据回收把控，我们会指定 func，并且在组件 destroy 的时候注销事件
      this.node.off(Node.EventType.TOUCH_CANCEL);
    }
```

节点的事件派发还支持 `dispatchEvent`，与 `emit` 的差别在于 `dispatchEvent` 可以做事件传递。通过该方法发射的事件，会进入事件派送阶段。在 Cocos Creator 的事件派送系统中，我们采用冒泡派送的方式。冒泡派送会将事件从事件发起节点，不断地向上传递给他的父级节点，直到到达根节点或者在某个节点的响应函数中做了中断处理 `event.propagationStopped = true`

![bubble-event](bubble-event.png)

如上图所示，当我们从节点 c 发送事件 “foobar”，倘若节点 a，b 均做了 “foobar” 事件的监听，则 事件会经由 c 依次传递给 b，a 节点。如：

```ts
// 节点 c 的组件脚本中
this.node.dispatchEvent( new cc.Event.EventCustom('foobar', true) );
```

如果我们希望在 b 节点截获事件后就不再将事件传递，我们可以通过调用 event.stopPropagation() 函数来完成。具体方法如下：

```ts
// 节点 b 的组件脚本中
this.node.on('foobar', (event) => {
  event.propagationStopped = true;
});
```

请注意，在发送用户自定义事件的时候，请不要直接创建 cc.Event 对象，因为它是一个抽象类，请创建 cc.Event.EventCustom 对象来进行派发。

### 事件对象

在事件监听回调中，开发者会接收到一个 `cc.Event` 类型的事件对象 `event`，`propagationStopped` 就是 `cc.Event` 的标准 API，其它重要的 API 包含：

| API 名 | 类型 | 意义 |
| ------ |:---:|:---:|
| `type` | `String` | 事件的类型（事件名） |
| `target` | `cc.Node` | 接收到事件的原始对象 |
| `currentTarget` | `cc.Node` | 接收到事件的当前对象，UI 事件在冒泡阶段当前对象可能与原始对象不同， |
| `getType` | `Function` | 获取事件的类型 |
| `propagationStopped` | `Function` | 停止冒泡阶段，事件将不会继续向父节点传递，当前节点的剩余监听器仍然会接收到事件|
| `propagationImmediateStopped` | `Function` | 立即停止事件的传递，事件将不会传给父节点以及当前节点的剩余监听器 |
| `getCurrentTarget` | `Function` | 获取当前接收到事件的目标节点 |
| `detail` | `Function` | 自定义事件的信息（属于 `cc.Event.EventCustom`） |
| `setUserData` | `Function` | 设置自定义事件的信息（属于 `cc.Event.EventCustom`） |
| `getUserData` | `Function` | 获取自定义事件的信息（属于 `cc.Event.EventCustom`） |

完整的 API 列表可以参考 `cc.Event` 及其子类的 API 文档。

### 注意事项

Cocos Creator 3D 和 Cocos Creator 的事件系统带有屏幕坐标的轻微差别。由于 3D 世界的屏幕就是 Canvas 的大小，而 UI 世界是带有[屏幕适配](../../ui-system/components/engine/multi-resolution.md)计算，所以实际的尺寸并不是屏幕的尺寸，所以在获取到的屏幕坐标两者是完全不一样的。如果你想要获取真实的屏幕大小，可以通过 `cc.view.getCanvasSize()` 来获取，如果你要的是 UI 屏幕坐标，可以通过 `cc.view.getVisibleSize()` 来获取。系统事件所得到的位置就是屏幕位置，节点得到的位置就是 UI 世界点击位置。
