
## cc 类

将装饰器 [ccclass]() 应用在类上时，此类称为 cc 类。
cc 类注入了额外的信息以控制 Cocos Creator 3D 对该类对象的序列化、编辑器对该类对象的展示等。

### ccclass

cc 类的各种特性是通过 `ccclass(name)` 的 cc 类选项参数来指定的。

#### cc 类名

选项 `name` 指定了 cc 类的名称。cc 类名应该是**独一无二**的。

当需要相应的 cc 类时，可以通过其 cc 类名来查找，例如：

- 序列化。
若对象是 cc 类对象，
则在序列化时将记录该对象的 cc 类名，
反序列化时将根据此名称找到相应的 cc 类进行序列化。

- 当 cc 类是组件类时，`Node` 通过可以组件类的 cc 类名查找该组件；

## cc 属性

当装饰器 [property]() 应用在 cc 类的属性或访问器上时，此属性称为 cc 属性。
与 cc 类类似，cc 属性注入了额外的信息以控制 Cocos Creator 3D 对该属性的序列化、编辑器对该属性的展示等。

### property

cc 属性的各种特性是通过 `property()` 的 cc 属性选项参数来指定的。

#### cc 类型

选项 `type` 指定了属性的 cc 类型。

可以通过以下几种形式的参数指定类型：

- 构造函数。
构造函数所指定的类型就直接作为属性的 cc 类型。
注意，当 Javascript 构造函数 `Number`、`String`、`Boolean`
用作 cc 类型时将给出警告，并且将
分别视为 cc 类型 `CCFloat`、`CCString`、`CCBoolean`。

- Cocos Creator 3D 内置属性类型标识。
`CCInteger`、`CCFloat`、`CCBoolean`、`CCString` 是内置属性类型标识。
`CCInteger` 声明类型为 **Cocos Creator 3D 整数**；
`CCFloat` 声明类型为 **Cocos Creator 3D 浮点数**；
`CCString` 声明类型为 **Cocos Creator 3D 字符串**；
`CCBoolean` 声明类型为 **Cocos Creator 3D 布尔值**。

- 数组。
通过将构造函数、Cocos Creator 3D 内置属性类型标识或数组作为数组元素时，
属性被指定为 **Cocos Creator 3D 数组**。
例如 `[CCInteger]` 就将类型声明为元素为Cocos Creator 3D 整数的 Cocos Creator 3D 数组。

若属性未指定 cc 类型，Cocos Creator 3D 将从属性的默认值或初始化式的求值结果推导其 cc 类型：
- 若值的类型是 Javascript 原始类型 `number`、`string`、`boolean`，
则其 cc 类型分别为 Cocos Creator 3D 浮点数、Cocos Creator 3D 字符串、Cocos Creator 3D 布尔值。
- 否则，若值是对象类型，则相当于使用对象的构造函数指定了 cc 类型；
- 否则，属性的 cc 类型是**未定义**的。

关于 cc 类型如何影响 cc 属性以及对未定义 cc 类型的属性的处理，见：
- [编辑器和属性类型]()
- [序列化]()

下列代码演示了不同 cc 类型 的 cc 属性的声明：

```ts
import { _decorator, CCInteger, Node } from "cc";
const { ccclass, property } = _decorator;
@ccclass
class MyClass {
    @property(CCInteger) // 声明属性 _id 的 cc 类型为 Cocos 整数
    private _id = 0;

    @property(Node) // 声明属性 _targetNode 的 cc 类型为 Node
    private _targetNode: Node | null = null;

    @property([Node]) // 声明属性 _children 的 cc 类型为 Node 数组
    private _children: Node[] = [];

    @property
    private _count = 0; // 未声明 cc 类型，从初始化式的求值结果推断为 Cocos 浮点数

    @property(String) // 警告：不应该使用构造函数 String
                      // 等价于 CCString
    private _name: string = '';

    @property
    private _children2 = []; // 未声明 cc 类型，从初始化式的求值结果推断为：元素为未定义的 Cocos 数组
}
```

#### 默认值

选项 `default` 指定了 cc 属性的默认值。

### 构造函数

#### 通过 constructor 定义

CCClass 的构造函数使用 `constructor` 定义，为了保证反序列化能始终正确运行，`constructor` **不允许**定义**构造参数**。

> 开发者如果确实需要使用构造参数，可以通过 `arguments` 获取，但要记得如果这个类会被序列化，必须保证构造参数都缺省的情况下仍然能 new 出对象。

## 判断类型

### 判断实例

需要做类型判断时，可以用 JavaScript 原生的 `instanceof`：

```typescript
class Sub extends Base {
       
}
let sub = new Sub();
console.log(sub instanceof Sub);  //true
console.log(sub instanceof Base);  //true

let base = new Base();
console.log(base instanceof Sub);  // false
```

## 成员

### 实例变量

在构造函数中定义的实例变量不能被序列化，也不能在 **属性检查器** 中查看。

```typescript
class Sprite{
    //声明变量
    url: string;
    id: number;
    constructor() {
        //赋值
        this.url = "";
        this.id = 0;
    }
}
```

> 如果是私有的变量，建议在变量名前面添加下划线 `_` 以示区分。

### 实例方法

实例方法请在原型对象中声明：

```typescript
class Sprite{
    text: string;
    constructor() {
        this.text = "this is sprite"
    }
    // 声明一个名叫 "print" 的实例方法
    print(){
        console.log(this.text);
    }
}
let obj = new Sprite();
// 调用实例方法
obj.print();
```

### 静态变量和静态方法

静态变量或静态方法可以用 `statics` 声明：

```typescript
class Sprite{
    static count=0;
    static getBounds(){
        
    }
}
```

静态成员会被子类继承，继承时会将父类的静态变量**浅拷贝**给子类，因此：

```typescript
class Object{ 
    static count= 11;
    static range: { w: 100, h: 100 }
}
class Sprite extends Object{
    
}
console.log(Sprite.count);    // 结果是 11，因为 count 继承自 Object 类

Sprite.range.w = 200;
console.log(Object.range.w);  // 结果是 200，因为 Sprite.range 和 Object.range 指向同一个对象
```

如果你不需要考虑继承，私有的静态成员也可以直接定义在类的外面：

```typescript
// 局部方法
doLoad(sprite){
    // ...
};
// 局部变量
let url = "foo.png";

class Sprite{
    load() {
        this.url = url;
        doLoad(this);
    };
};
```

## 继承

### 父构造函数

请注意，不论子类是否有定义构造函数，子类实例化前父类的构造函数都会被自动调用。

```typescript
class Node {
    name: string;
    constructor(){
        this.name = "node";
    }
}
class Sprite extends Node{
    constructor() {
        super();
        // 子构造函数被调用前，父构造函数已经被调用过，所以 this.name 已经被初始化过了
        console.log(this.name);    // "node"
        // 重新设置 this.name
        this.name = "sprite";
    }
}
let obj = new Sprite();
console.log(obj.name);    // "sprite"
```


### 重写

所有成员方法都是虚方法，子类方法可以直接重写父类方法：

```typescript
class Shape{
    getName() {
        return "shape";
    }
};
class Rect extends Shape{
    getName () {
        return "rect";
    }
};
let obj = new Rect();
console.log(obj.getName());    // "rect"
```

## 属性

属性是特殊的实例变量，能够显示在 **属性检查器** 中，也能被序列化。

### 属性和构造函数

属性**不用**在构造函数里定义，在构造函数被调用前，属性已经被赋为默认值了，可以在构造函数内访问到。如果属性的默认值无法在定义 CCClass 时提供，需要在运行时才能获得，你也可以在构造函数中重新给属性赋**默认**值。

```typescript
class Sprite {
    constructor() {
        this.num = 1;
    }
    @property({type:CCInteger})
    private num = 0;
}
```

不过要注意的是，属性被反序列化的过程紧接着发生在构造函数执行**之后**，因此构造函数中只能获得和修改属性的默认值，还无法获得和修改之前保存（序列化）的值。

### 属性参数

#### <a name="default"></a>default 参数

`default` 用于声明属性的默认值，声明了默认值的属性会被 CCClass 实现为成员变量。默认值只有在**第一次创建**对象的时候才会用到，也就是说修改默认值时，并不会改变已添加到场景里的组件的当前值。

> 当你在编辑器中添加了一个组件以后，再回到脚本中修改一个默认值的话，**属性检查器** 里面是看不到变化的。因为属性的当前值已经序列化到了场景中，不再是第一次创建时用到的默认值了。如果要强制把所有属性设回默认值，可以在 **属性检查器** 的组件菜单中选择 Reset。

`default` 允许设置为以下几种值类型：

1. 任意 number, string 或 boolean 类型的值
2. `null` 或 `undefined`
3. 继承自 `ValueType` 的子类，如 `Vec3`, `Color` 或 `Rect` 的实例化对象：
    ```typescript
    @property({type:Vec3})
    private pos = null;
    ```
4. 空数组 `[]` 或空对象 `{}`
5. 一个允许返回任意类型值的 function，这个 function 会在每次实例化该类时重新调用，并且以返回值作为新的默认值：
    ```typescript
    properties: {
        pos: {
            default: function () {
                return [1, 2, 3];
            },
        }
    }
    ```

#### <a name="visible"></a>visible 参数

默认情况下，是否显示在 **属性检查器** 取决于属性名是否以下划线 `_` 开头。如果以下划线开头，则默认不显示在 **属性检查器**，否则默认显示。

如果要强制显示在 **属性检查器**，可以设置 `visible` 参数为 true:

```typescript
@property({visible:true})
    private _num = 0;
```

如果要强制隐藏，可以设置 `visible` 参数为 false:

```typescript
@property({visible:false})
    private num = 0;
```

#### <a name="serializable"></a>serializable 参数

指定了 `default` 默认值的属性默认情况下都会被序列化，序列化后就会将编辑器中设置好的值保存到场景等资源文件中，并且在加载场景时自动还原之前设置好的值。如果不想序列化，可以设置`serializable: false`。

```typescript
@property({serializable:false})
    private num = 0;
```

#### <a name="type"></a>type 参数

当 `default` 不能提供足够详细的类型信息时，为了能在 **属性检查器** 显示正确的输入控件，就要用 `type` 显式声明具体的类型：

- 当默认值为 null 时，将 type 设置为指定类型的构造函数，这样 **属性检查器** 才知道应该显示一个 Node 控件。

    ```typescript
    @property({type:Node})
    private enemy = null;
    ```

- 当默认值为数值（number）类型时，将 type 设置为 `cc.Integer`，用来表示这是一个整数，这样属性在 **属性检查器** 里就不能输入小数点。

    ```typescript
    @property({type:CCInteger})
    private num = 0;
    ```

- 当默认值是一个枚举（`Enum`）时，由于枚举值本身其实也是一个数字（number），所以要将 type 设置为枚举类型，才能在 **属性检查器** 中显示为枚举下拉框。

    ```javascript
    wrap: {
        default: Texture.WrapMode.Clamp,
        type: Texture.WrapMode
    }
    ```

#### <a name="override"></a>override 参数

所有属性都将被子类继承，如果子类要覆盖父类同名属性，需要显式设置 override 参数，否则会有重名警告：

```javascript
_id: {
    default: "",
    tooltip: "my id",
    override: true
},
name: {
    get: function () {
        return this._name;
    },
    displayName: "Name",
    override: true
}
```

更多参数内容请查阅 [属性参数](attributes.md)。

### <a name="deferred-definition"></a> 属性延迟定义

如果两个类相互引用，脚本加载阶段就会出现循环引用，循环引用将导致脚本加载出错：

 - Game.ts
 
    ```typescript
    import { _decorator, Component, Node } from "cc";
    const { ccclass, property } = _decorator;
    import{ Item } from "Item";

    @ccclass("Game")
    export class Game extends Component {
        @property({type:Item})
        private item = null;
    }
    ```

 - Item.ts

    ```typescript
    import { _decorator, Component, Node } from "cc";
    const { ccclass, property } = _decorator;
    import { Game } from "Game";

    @ccclass("Item")
    export class Item extends Component {
        @property({type:Game})
        private game = null;
    }
    ```

上面两个脚本加载时，由于它们在 require 的过程中形成了闭环，因此加载会出现循环引用的错误，循环引用时 type 就会变为 undefined。<br>
因此我们提倡使用以下的属性定义方式：

 - Game.js
 
    ```javascript
    var Game = cc.Class({
        properties: () => ({
            item: {
                default: null,
                type: require("Item")
            }
        })
    });
    
    module.exports = Game;
    ```

 - Item.js

    ```javascript
    var Item = cc.Class({
        properties: () => ({
            game: {
                default: null,
                type: require("Game")
            }
        })
    });
    
    module.exports = Item;
    ```

这种方式就是将 properties 指定为一个 ES6 的箭头函数（lambda 表达式），箭头函数的内容在脚本加载过程中并不会同步执行，而是会被 CCClass 以异步的形式在所有脚本加载成功后才调用。因此加载过程中并不会出现循环引用，属性都可以正常初始化。

> 箭头函数的用法符合 TypeScript 的 ES6 标准，并且 Creator 3D会自动将 ES6 转义为 ES5，用户不用担心浏览器的兼容问题。

你可以这样来理解箭头函数：

```
// 箭头函数支持省略掉 `return` 语句，我们推荐的是这种省略后的写法：

properties: () => ({    // <- 箭头右边的括号 "(" 不可省略
    game: {
        default: null,
        type: require("Game")
    }
})

// 如果要完整写出 `return`，那么上面的写法等价于：

properties: () => {
    return {
        game: {
            default: null,
            type: require("Game")
        }
    };      // <- 这里 return 的内容，就是原先箭头右边括号里的部分
}

// 我们也可以不用箭头函数，而是用普通的匿名函数：

properties: function () {
    return {
        game: {
            default: null,
            type: require("Game")
        }
    };
}
```


## GetSet 方法

在属性中设置了 get 或 set 以后，访问属性的时候，就能触发预定义的 get 或 set 方法。

### get

在属性中设置 get 方法：

```javascript
properties: {
    width: {
        get: function () {
            return this.__width;
        }
    }
}
```

get 方法可以返回任意类型的值。<br>
这个属性同样能显示在 **属性检查器** 中，并且可以在包括构造函数内的所有代码里直接访问。

```javascript
var Sprite = cc.Class({
    ctor: function () {
        this.__width = 128;
        cc.log(this.width);    // 128
    },
    properties: {
        width: {
            get: function () {
                return this.__width;
            }
        }
    }
});
```

请注意：

- 设定了 get 以后，这个属性就不能被序列化，也不能指定默认值，但仍然可附带除了 `default`, `serializable` 外的大部分参数。

    ```javascript
    width: {
        get: function () {
            return this.__width;
        },
        type: cc.Integer,
        tooltip: "The width of sprite"
    }
    ```

- get 属性本身是只读的，但返回的对象并不是只读的。用户使用代码依然可以修改对象内部的属性，例如：

    ```javascript
    var Sprite = cc.Class({
        ...
        position: {
            get: function () {
                return this._position;
            },
        }
        ...
    });
    var obj = new Sprite();
    obj.position = new cc.Vec2(10, 20);   // 失败！position 是只读的！
    obj.position.x = 100;                 // 允许！position 返回的 _position 对象本身可以修改！
    ```

### set

在属性中设置 set 方法：

```javascript
width: {
    set: function (value) {
        this._width = value;
    }
}
```

set 方法接收一个传入参数，这个参数可以是任意类型。

set 一般和 get 一起使用：

```javascript
width: {
    get: function () {
        return this._width;
    },
    set: function (value) {
        this._width = value;
    },
    type: cc.Integer,
    tooltip: "The width of sprite"
}
```
    
> 如果没有和 get 一起定义，则 set 自身不能附带任何参数。<br>
> 和 get 一样，设定了 set 以后，这个属性就不能被序列化，也不能指定默认值。

