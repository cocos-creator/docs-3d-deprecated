
## cc 类

将装饰器 [ccclass](#ccclass) 应用在类上时，此类称为 cc 类。
cc 类注入了额外的信息以控制 Cocos Creator 对该类对象的序列化、编辑器对该类对象的展示等。

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

当装饰器 [property](#property) 应用在 cc 类的属性或访问器上时，此属性称为 cc 属性。
与 cc 类类似，cc 属性注入了额外的信息以控制 Cocos Creator 对该属性的序列化、编辑器对该属性的展示等。

### property

cc 属性的各种特性是通过 `property()` 的 cc 属性选项参数来指定的。

#### cc 类型

选项 `type` 指定了属性的 cc 类型。

可以通过以下几种形式的参数指定类型：

- 构造函数。
构造函数所指定的类型就直接作为属性的 cc 类型。
注意，当 Javascript 内置构造函数 `Number`、`String`、`Boolean`
用作 cc 类型时将给出警告，并且将
分别视为 cc 类型 `CCFloat`、`CCString`、`CCBoolean`。

- Cocos Creator 内置属性类型标识。
`CCInteger`、`CCFloat`、`CCBoolean`、`CCString` 是内置属性类型标识。
`CCInteger` 声明类型为 **Cocos Creator 整数**；
`CCFloat` 声明类型为 **Cocos Creator 浮点数**；
`CCString` 声明类型为 **Cocos Creator 字符串**；
`CCBoolean` 声明类型为 **Cocos Creator 布尔值**。

- 数组。
通过将构造函数、Cocos Creator 内置属性类型标识或数组作为数组元素时，
属性被指定为 **Cocos Creator 数组**。
例如 `[CCInteger]` 就将类型声明为元素为Cocos Creator 整数的 Cocos Creator 数组。

若属性未指定 cc 类型，Cocos Creator 将从属性的默认值或初始化式的求值结果推导其 cc 类型：
- 若值的类型是 Javascript 原始类型 `number`、`string`、`boolean`，
则其 cc 类型分别为 Cocos Creator 浮点数、Cocos Creator 字符串、Cocos Creator 布尔值。
- 否则，若值是对象类型，则相当于使用对象的构造函数指定了 cc 类型；
- 否则，属性的 cc 类型是**未定义**的。

一般地，仅需要在以下情况中需要显式地声明 cc 类型：
- 当需要将属性显示为整数时；
- 当属性的实际值可能是多个类型时。

关于 cc 类型如何影响 cc 属性以及对未定义 cc 类型的属性的处理，见：

- [属性类型](#属性参数)
- [序列化参数](#serializable参数)

为了方便，额外提供了以下装饰器以快速声明 cc 类型：

|  	| 等价于 	|
|----------	|----------------------	|
| @type(t) 	| @property(t) 	|
| @integer 	| @property(CCInteger) 	|
| @float 	| @property(CCFloat) 	|
| @string 	| @property(CCString) 	|
| @boolean 	| @property(CCBoolean) 	|

下列代码演示了不同 cc 类型 的 cc 属性的声明：

```ts
import { _decorator, CCInteger, Node } from "cc";
const { ccclass, property, integer, float, boolean, string, type } = _decorator;
@ccclass
class MyClass {
    @integer // 声明属性 _id 的 cc 类型为 Cocos 整数
    private _id = 0;

    @type(Node) // 声明属性 _targetNode 的 cc 类型为 Node
    private _targetNode: Node | null = null;

    @type([Node]) // 声明属性 _children 的 cc 类型为 Node 数组
    private _children: Node[] = [];

    @property
    private _count = 0; // 未声明 cc 类型，从初始化式的求值结果推断为 Cocos 浮点数

    @type(String) // 警告：不应该使用构造函数 String
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

需要做类型判断时，可以用 TypeScript 原生的 `instanceof`：

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

#### default参数

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

#### visible参数

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

#### serializable参数

指定了 `default` 默认值的属性默认情况下都会被序列化，序列化后就会将编辑器中设置好的值保存到场景等资源文件中，并且在加载场景时自动还原之前设置好的值。如果不想序列化，可以设置`serializable: false`。

```typescript
@property({serializable:false})
    private num = 0;
```

#### type参数

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

    ```typescript
    enum A{
        c,
        d
    }
    Enum(A);
    @ccclass("test")
    export class test extends Component {

        @property({type:A})
        accx:A=A.c;

    }
    ```

#### override参数

所有属性都将被子类继承，如果子类要覆盖父类同名属性，需要显式设置 override 参数，否则会有重名警告：

```typescript
@property({type:CCString,tooltip:"my id",override:true})
private _id = "";

@property({displayName:"Name",override:true})
private _name = null;
private get name(){
    return this._name;
}
```

更多参数内容请查阅 [属性参数](./reference/attributes.md)。

## GetSet 方法

在属性中设置了 get 或 set 以后，访问属性的时候，就能触发预定义的 get 或 set 方法。

### get

在属性中设置 get 方法：

```typescript
@property({type:CCInteger})
private _num = 0;
private get num(){
    return this._num;
}
```

get 方法可以返回任意类型的值。<br>
这个属性同样能显示在 **属性检查器** 中，并且可以在包括构造函数内的所有代码里直接访问。

```typescript
class Sprite{
    _width: number;
    constructor() {
        this._width = 128;
        console.log(this.width);    // 128
    }
    @property({type:CCInteger})
    private width = 0;
    private get width(){
        return this._width;
    }
};
```

请注意：

- 设定了 get 以后，这个属性就不能被序列化，也不能指定默认值，但仍然可附带除了 `default`, `serializable` 外的大部分参数。

    ```typescript
    @property({type:CCInteger,tooltip: "The width of sprite"})
    private _width = 0;
    private get width(){
        return this._width;
    }
    ```

- get 属性本身是只读的，但返回的对象并不是只读的。用户使用代码依然可以修改对象内部的属性，例如：

    ```typescript
    @property
    _num=0;

    private get num(){
        return this._num;
    }

    start(){
        consolo.log(this.num);
    }
    ```

### set

在属性中设置 set 方法：

```typescript
@property({type:CCInteger})
    private _width = 0;
    set(value){
        this._width = value
    }
```

set 方法接收一个传入参数，这个参数可以是任意类型。

set 一般和 get 一起使用：

```typescript
@property
_width=0;
private get width(){
    return this._width;
}
set(value){
    this._width = value;
}

```

> 如果没有和 get 一起定义，则 set 自身不能附带任何参数。<br>
> 和 get 一样，设定了 set 以后，这个属性就不能被序列化，也不能指定默认值。
