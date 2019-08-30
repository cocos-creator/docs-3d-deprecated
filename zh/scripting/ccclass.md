
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
import { _decorator, Component } from "cc";
const { ccclass, property } = _decorator;

@ccclass("test")
export class test extends Component {
    start(){
       var ntest = new test();
       console.log(ntest instanceof test);  //true
       console.log(ntest instanceof Component);  //true

       var component = new Component();
       console.log(component instanceof test);  //false
    }
}
```

## 成员

### 实例变量

在构造函数中定义的实例变量不能被序列化，也不能在 **属性检查器** 中查看。

```typescript
import { _decorator, Component, CCInteger } from "cc";
const { ccclass, property } = _decorator;

@ccclass("test")
export class test extends Component {
    //声明实例变量
    id:number;
    url:string;
    constructor() {
		super();
        //赋值
        this.url = "";
        this.id = 0;
    }
}
```

> 如果是私有的变量，建议在变量名前面添加下划线 `_` 以示区分。

