# Access Nodes and Components

You can modify __Nodes__ and __Components__ in the **Property Inspector**, and also dynamically using scripts. The advantage of dynamic modification is that it can continuously modify attributes and transition attributes within a period of time to achieve gradual effects. Scripts can also respond to player input, modify, create and destroy __Nodes__ or __Components__, and implement various game logic. To achieve these effects, developers need to obtain the __Node__ or __Component__ that needs to be modified in the script.

## Document topics

- Obtain the node where the component is located.
- Obtain other components.
- Use **Property Inspector** to set up nodes and components.
- Find child nodes.
- Global node search.
- Access values in existing variables.

## Obtain the node where the component is located

It's easy to get the node where the component is. Just access the `this.node` variable in the component method:
```ts
    start(){
        let node = this.node;
        node.setPosition(0.0,0.0,0.0);
    }
```

## Obtaining other components

It will often be need to get other components on the same node. Use the `getComponent` API, which will help to find the component that is needed.

```ts
import { _decorator, Component, LabelComponent } from "cc";
const { ccclass, property } = _decorator;

@ccclass("test")
export class test extends Component {
    private label: any = null

    start(){
        this.label = this.getComponent(LabelComponent);
        let text = this.name + 'started';
        // Change the text in Label Component
        this.label.string = text;
    }
}
```

It is also possible to pass in a class name for `getComponent`. For user-defined components, the class name is the file name of the script and is **case sensitive**. For example, the component declared in `SinRotate.ts`, the class name is `SinRotate`.

```ts
    let rotate = this.getComponent("SinRotate");
```

There is also ther `getComponent` method on the node, and their functions are the same:

```ts
    start() {
        console.log( this.node.getComponent(LabelComponent) === this.getComponent(LabelComponent) );  // true
    }
```

If the component that is needed is not found on the node, `getComponent` will return `null`. If you try to access the value of `null`, a __TypeError__ error will be thrown at runtime. If you are not sure whether the component exists, please remember to check:

```ts
import { _decorator, Component, LabelComponent } from "cc";
const { ccclass, property } = _decorator;

@ccclass("test")
export class test extends Component {
    private label: any =null;

    start() {
        this.label = this.getComponent(LabelComponent);
        if (this.label) {
            this.label.string = "Hello";
        }
        else {
            console.error("Something wrong?");
        }
    }
}
```

## Get other nodes and their components

It is usually not enough to only have access to the node's own components, and scripts usually require interaction between multiple nodes. For example, a cannon that automatically aims at the player needs to constantly obtain the latest position of the player. __Cocos Creator 3D__ provides some different methods to obtain other nodes or components.

### Use the Property Inspector to set the node

The most straightforward way is to set the objects you need in the **Property Inspector**. Take node as an example, this only needs to declare an attribute with type `Node` in the script:

```ts
// Cannon.ts

import { _decorator, Component, Node } from "cc";
const { ccclass, property } = _decorator;

@ccclass("Cannon")
export class Cannon extends Component {
    // Declare Player properties
    @property({type:Node})
    private player = null;
}
```

This code declares a `player` property in `properties`, the default value is `null`, and its object type is specified as `Node`. This is equivalent to declaring `public Node player = null;` in other languages. After the script is compiled, this component looks like this in the **Property Inspector**:

![player-in-inspector-null](access-node-component/player-in-inspector-null.png)

Then you can drag any node on the __Hierarchy Manager__ to the `Player` control:

![player-in-inspector](access-node-component/player-in-inspector.png)

这样一来它的 player 属性就会被设置成功，你可以直接在脚本里访问 player：

```ts
// Cannon.ts

import { _decorator, Component, Node } from "cc";
const { ccclass, property } = _decorator;

@ccclass("Cannon")
export class Cannon extends Component {

    @property({type:Node})
    private player = null;

    start() {
        console.log("The player is " + this.player.name);
    }
}
```

### 利用属性检查器设置组件

在上面的例子中，如果你将属性的 type 声明为 Player 组件，当你拖动节点 "Player Node" 到 **属性检查器**，player 属性就会被设置为这个节点里面的 Player 组件。这样你就不需要再自己调用 `getComponent` 啦。

```ts
// Cannon.ts

import { _decorator, Component } from "cc";
const { ccclass, property } = _decorator;
import { Player } from "Player";

@ccclass("Cannon")
export class Cannon extends Component {
    @property({type:Player})
    private player = null;

    start(){
        let PlayerComp = this.player;
    }
}
```

你还可以将属性的默认值由 `null` 改为数组`[]`，这样你就能在 **属性检查器** 中同时设置多个对象。<br>
不过如果需要在运行时动态获取其它对象，还需要用到下面介绍的查找方法。

### 查找子节点

有时候，游戏场景中会有很多个相同类型的对象，像是炮塔、敌人和特效，它们通常都有一个全局的脚本来统一管理。如果用 **属性检查器** 来一个一个将它们关联到这个脚本上，那工作就会很繁琐。为了更好地统一管理这些对象，我们可以把它们放到一个统一的父物体下，然后通过父物体来获得所有的子物体：

```ts
// CannonManager.ts

import { _decorator, Component, Node } from "cc";
const { ccclass, property } = _decorator;

@ccclass("CannonManager")
export class CannonManager extends Component {

    start() {
        let cannons = this.node.children;
        //...
    }

}
```

你还可以使用 `getChildByName`：

```ts
this.node.getChildByName("Cannon 01");
```

如果子节点的层次较深，你还可以使用 `find`，`find` 将根据传入的路径进行逐级查找：

```ts
find("Cannon 01/Barrel/SFX", this.node);
```

### 全局名字查找

当 `find` 只传入第一个参数时，将从场景根节点开始逐级查找：

```ts
this.backNode = find("Canvas/Menu/Back");
```

## 访问已有变量里的值

如果你已经在一个地方保存了节点或组件的引用，你也可以直接访问它们

### 通过模块访问

你可以使用 `import` 来实现脚本的跨文件操作，让我们看个示例：

```ts
// Global.ts, now the filename matters
import { _decorator, Component, Node } from "cc";
const { ccclass, property } = _decorator;

@ccclass("Global")
export class Global extends Component {

    public static backNode:any=null;
    public static backLabel:any=null;
}
```

每个脚本都能用 `import{ } from` + 文件名(不含路径) 来获取到对方 exports 的对象。

```ts
// Back.ts
import { _decorator, Component, Node, LabelComponent } from "cc";
const { ccclass, property } = _decorator;
// this feels more safe since you know where the object comes from
import{Global}from "./Global";

@ccclass("Back")
export class Back extends Component {
    onLoad(){
        Global.backNode=this.node;
        Global.backLabel=this.getComponent(LabelComponent);
    }
}
```

```ts
// AnyScript.ts
import { _decorator, Component, Node } from "cc";
const { ccclass, property } = _decorator;
// this feels more safe since you know where the object comes from
import{Global}from "./Global";

@ccclass("AnyScript")
export class AnyScript extends Component {
    start () {
        var text = "Back";
        Global.backLabel.string=text;
    }
}
```
---

继续前往 [常用节点和组件接口](basic-node-api.md)。
