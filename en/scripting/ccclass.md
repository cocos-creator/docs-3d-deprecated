## ccclass

When the decorator [ccclass](#ccclass) is applied to a class, the class is called `ccclass`. The `ccclass` injects additional information to control __Cocos Creator 3D__'s serialization of this kind of object along with the editor's display of this kind of object, etc.

### ccclass

Various characteristics of the `ccclass` are specified by the `ccclass` option parameter of `ccclass(name)`.

#### ccclass name

The option `name` specifies the name of the `ccclass`. The `ccclass` name should be **unique**.

When you need the corresponding `ccclass`, you can find it by its `ccclass` name, for example:

- Serialization.
  If the object is a cc object, the `ccclass` name of the object will be recorded during serialization. During deserialization, the corresponding `ccclass` will be found for serialization based on this name.

- When the `ccclass` is a component class, `Node` can find the component by its `ccclass` name.

## ccattributes

When the decorator [property](#property) is applied to a property or accessor of the `ccclass`, this property is called a `ccproperty`.
Similar to the `ccclass`, the `ccattribute` injects additional information to control the serialization of the attribute in **Cocos Creator 3D** and the display of the attribute in the editor.

### property

Various characteristics of the `ccattribute` are specified by the `ccattribute` option parameter of `property()`.

#### cctype

The option `type` specifies the `cctype` of the attribute.

The type can be specified by the following parameters:

- Constructor.
  The type specified by the constructor is directly used as the `cctype` of the attribute.

  > **Note**: when Javascript built-in constructors `Number`, `String`, `Boolean` A warning will be given when used as a `cctype`, and they are regarded as `cctype`s `CCFloat`, `CCString`, and `CCBoolean` respectively.

- Cocos Creator 3D built-in attribute type identification. 

  - `CCInteger`, `CCFloat`, `CCBoolean`, and `CCString` are built-in attribute type identifiers.
  - `CCInteger` declares the type as **Cocos Creator 3D integer**.
  - `CCFloat` declares the type as **Cocos Creator 3D floating point number**.
  - `CCString` declares the type as **Cocos Creator 3D string**.
  - `CCBoolean` declares the type as **Cocos Creator 3D Boolean**.

- Array.

  When using the constructor, Cocos Creator 3D built-in property type identification or array as the array element, the properties are specified as **Cocos Creator 3D** array. For example, `[CCInteger]` declares the type as a **Cocos Creator 3D** array whose elements are **Cocos Creator 3D** integers.

If the attribute does not specify the `cctype`, **Cocos Creator 3D** will derive its `cctype` from the default value of the attribute or the evaluation result of the initialization formula:

  - If the value type is Javascript primitive type `number`, `string`, `boolean`, the `cctype`s are **Cocos Creator 3D** floating point numbers, **Cocos Creator 3D** strings, and **Cocos Creator 3D** Boolean values.
  - Otherwise, if the value is an object type, it is equivalent to using the object's constructor to specify the `cctype`.
  - Otherwise, the `cctype` of the attribute is **undefined**.

Generally, you only need to explicitly declare the `cctype` in the following situations:

  - When the attribute needs to be displayed as an integer.
  - When the actual value of the attribute may be of multiple types.

For how the `cctype` affects the cc attribute and the treatment of attributes that do not define the `cctype`, see:

  - [Attribute Type](#AttributeParameters)
  - [Serialization parameter](#serializableparameter)

For convenience, the following decorators are additionally provided to quickly declare the `cctype`:

| Type | Equivalent to |
|---------- |---------------------- |
| @type(t) | @property(t) |
| @integer | @property(CCInteger) |
| @float | @property(CCFloat) |
| @string | @property(CCString) |
| @boolean | @property(CCBoolean) |

The following code demonstrates the declaration of `ccattributes` of different `cctype`s:

```ts
import { _decorator, CCInteger, Node } from "cc";
const { ccclass, property, integer, float, boolean, string, type } = _decorator;
@ccclass
class MyClass {
    @integer // Declare that the cc type of the attribute _id is a Cocos integer
    private _id = 0;

    @type(Node) // Declare that the cc type of the attribute _targetNode is Node
    private _targetNode: Node | null = null;

    @type([Node]) // declare the cc type of the attribute _children as a Node array
    private _children: Node[] = [];

    @property
    private _count = 0; // the cc type is not declared, and it is inferred from the evaluation result of the initializer as a Cocos floating point number

    @type(String) // Warning: Constructor should not be used String
                // equivalent to CCString
    private _name: string = '';

    @property
    private _children2 = []; // The cc type is not declared, inferred from the evaluation result of the initializer: the element is an undefined Cocos array
}
```

#### Defaults

The option `default` specifies the default value of the cc attribute.

### Constructor

#### Defined by constructor

The constructor of `CCClass` is defined by `constructor`. To ensure that deserialization can always run correctly, `constructor` **is not allowed to define **constructor parameters**.

> **Note**: If developers really need to use construction parameters, they can get them through `arguments`, but remember that if this class will be serialized, you must ensure that the object can still be new when the construction parameters are all default.

## Judging the type

### Judgment example

When you need to make type judgments, you can use **TypeScript** native `instanceof`:

```typescript
class Sub extends Base {

}
let sub = new Sub();
console.log(sub instanceof Sub);  // true
console.log(sub instanceof Base);  // true

let base = new Base();
console.log(base instanceof Sub);  // false
```

## Members

### Instance variables

The instance variables defined in the constructor cannot be serialized, nor can they be viewed in the **Property Inspector**.

```typescript
class Sprite {
    // Declare variables
    url: string;
    id: number;
    constructor() {
        // assignment
        this.url = "";
        this.id = 0;
    }
}
```

> **Note**: If it is a private variable, it is recommended to add an underscore `_` in front of the variable name to distinguish it.

### Example Method

Please declare the instance method in the prototype object:

```typescript
class Sprite{
    text: string;
    constructor() {
        this.text = "this is sprite"
    }
    // Declare an instance method named "print"
    print(){
        console.log(this.text);
    }
}
let obj = new Sprite();
// call instance method
obj.print();
```

### Static variables and static methods

Static variables or static methods can be declared with `static`:

```typescript
class Sprite{
    static count=0;
    static getBounds(){

    }
}
```

Static members will be inherited by subclasses. When inheriting, the static variables of the parent class will be **shallowly copied** to the subclass. Therefore:

```typescript
class Object{
    static count= 11;
    static range: { w: 100, h: 100 }
}
class Sprite extends Object{

}
console.log(Sprite.count);    // The result is 11 because count inherits from the Object class

Sprite.range.w = 200;
console.log(Object.range.w);  // The result is 200, because Sprite.range and Object.range point to the same object
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
