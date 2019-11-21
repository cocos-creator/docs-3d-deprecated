## 事件

事件是用于引擎内对象交互的消息传递机制。

#### 事件监听

```ts
// 该事件监听每次都会触发，需要手动取消注册
xxx.on(type, func, target);
```

的方式来监听，其中 `type` 为事件注册字符串，`func` 为执行事件监听的回调，`target` 为事件接收对象。

#### 事件取消

```ts
// 取消对象身上所有注册的该类型的事件
xxx.off(type);
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

| 事件名            | 事件类型说明                                           |
| --------------  | -----------                                       |
| TRANSFORM_CHANGED       | 节点改变位置、旋转或缩放事件。                                       |
| POSITION_PART         | 节点位置改变事件。统一由 TRANSFORM_CHANGED 监听，派发后判断类型。|
| ROTATION_PART | 节点旋转事件。统一由 TRANSFORM_CHANGED 监听，派发后判断类型。       |
| SCALE_PART         | 节点缩放事件。统一由 TRANSFORM_CHANGED 监听，派发后判断类型。 |
| SIZE_CHANGED | 当节点尺寸改变时触发的事件。       |
| ANCHOR_CHANGED      | 当节点锚点改变时触发的事件。                                  |
| CHILD_ADDED         | 节点子类添加。 |
| CHILD_REMOVED | 节点子类移除。       |
