
# 装饰器

你可以通过装饰器来控制类对象的序列化以及类对象在编辑器中的表现。

装饰器存在于命名空间 `_decorator` 下。

注意：命名空间 `_decorator` 带有下划线是因为 [JavaScript 装饰器提案](https://github.com/tc39/proposal-decorators) 
未正式通过且经历过比较大的重设计。
`_decorator` 命名空间下的装饰器都基于 [Legacy decorators](https://github.com/tc39/proposal-decorators#comparison-with-babel-legacy-decorators)，一种目前广泛使用的装饰器旧案；它可能与目前的装饰器提案大相径庭。


## 类装饰器


## 属性装饰器

### 序列化装饰器

当 `@serializable` 作用于属性上时，该属性将参与序列化。除此之外，你可以使用下列装饰器来控制序列化的细节：

- `@formerlySerializedAs(name: string)`

注意：以上列表中的每一个装饰器都隐含着 `@serializable`。它们以及 `@serializable` 统称为**序列化装饰器**。

注意：**序列化装饰器将不会作用于访问器属性**。

以下是序列化装饰器的应用示例：

```ts
@serializable      // `count` 属性将参与序列化
count = 0;

// @serializable   // 序列化装饰器 @formerlySerializedAs 
                   // 隐含 @serializable
                   // 因此无需重复声明
@formerlySerializedAs('age')
_age = 0;
```

### 编辑器装饰器

当 `@editable` 作用于属性上时，编辑器将会控制该属性的展示、编辑等。除此之外，你可以使用下列装饰器来控制该属性在编辑器中的行为：

- `@readOnly`、`@readOnly(enabled: boolean)`
- `@visible(condition: boolean | (() => boolean))`
- `@displayName(name: string)`
- `@range([number: min, number: max] | [number: min, number: max, number: step])`、`@rangeMin(min: number)`、`@rangeMax(max: number)`、`@rangeStep(step: number)`
- `@slide`
- `@displayOrder(order: number)`
- `@unit(name: string)`
- `@radian`
- `@multiline`

注意：以上列表中的每一个装饰器都隐含着 `@editable`。它们以及 `@editable` 统称为**编辑器装饰器**。

特别地，在非编辑器环境下，所有编辑器装饰器都不会有任何行为。

以下是编辑器装饰器的应用示例：

```ts
@editable          // 允许在编辑器中操纵 `count` 属性
count = 0;

// @editable       // 编辑器装饰器 @range 隐含 @editable
                   // 因此无需重复声明
@range([0, 10, 1]) // 当在编辑器中设置该属性值时，`@range` 控制范围
age = 0;
```

### 类型装饰器

`@type` 用于向序列化和编辑器提供类型信息。

例如，我们有一个字段 `node`，它的类型可能为 `null` 或者 `Node`，
这时，我们需要向编辑器提供它的类型信息：
```ts
@editable
@type(Node)
node: Node | null = null;
```

`@type` 也向序列化提供了信息：
```ts
@serializable
@type(Node)
node: Node | null = null;
```

当属性同时需要序列化以及编辑器交互，也只用一个 `@type` 装饰器：
```ts
@serializable
@editable
@type(Node)
node: Node | null = null;
```

#### 什么时候需要 `@type` 装饰器？

在大多数情况下，即使未指定 `@type` 装饰器，属性也可以得以正确的
编辑和序列化，那么，何时 `@type` 装饰器是必要的呢？

##### 当需要区分整数和浮点数时

```ts
@editable
count = 0;
```

`count` 在编辑器将被允许设置为任何数值，包括浮点数。但假使你只希望它是整数的时候，你需要通过类型装饰器来告诉编辑器：

```ts
@editable
@integer // 仅接受整数
count = 0;
```

##### 当属性是枚举时

```ts
enum class Color { Black, White }
Enum.recognize(Color); // 将该对象标记为枚举

@editable
color = Color.Black;
```

枚举值 `Color.Black` 仅仅是一个数值。当希望编辑器中让它以枚举的形式（也就是说提供一个类似于列表的空间以选择 `Color.Black`、`Color.White` 等）时，你需要告诉编辑器 `color` 的类型是枚举：

```ts
@editable
@type(Color) // 属性 `color` 的值必须是枚举 `Color` 的成员
color = Color.Black;
```

##### 当属性是数组时

```ts
@editable
values: number[] = [];
```

因为在 JavaScript 中，数组是无类型的，你需要告诉编辑器数组元素的类型：

```ts
@editable
@float // 指定了数组元素的类型；当编辑器运行时发现 `values` 是数组
       // 又指定了 `@float` 时，`values` 会被当作一个
       // 接受浮点数数组的属性
values: number[] =[];
```

##### 当属性是可选的

在一些情况下，我们的属性被声明为是可选的：
```ts
@editable
node: Node | null = null;
```

当 `node` 的值只会是 `Node` 类型时，编辑器可以正确地将该属性以 `Node` 的身份进行编辑和展示。

但此例中，当 `node` 值是 `null` 时，编辑器并不知道它可以接受其它什么类型的值，因此，我们需要类型装饰器来让编辑器知道：

```ts
@editable
@type(Node) // 除了空值外，属性 `node` 的值还可能是 `Node` 对象
node: Node | null = null;
```

#### ⚠️ 兼容行为

在 1.2 版本之前，`@type` 隐含着可序列化以及可编辑器交互。为了兼容，我们在 1.2 及后续的版本保留了此行为，并对其做出限制：当仅存在 `@type` 或 `@property` 装饰器时，`@type` 才隐含可序列化以及可编辑器交互：
```ts
@type(Node) // 隐含 `@serializable` 和 `@editable`：
            // 同时参与序列化以及编辑器交互；
            // 这是不推荐的做法！
            // 反之，显式声明 `@serializable` 和 `@editable` 使得逻辑更清晰
node: Node | null = null;

@displayName("My Node")
@type(Node)
node2: Node | null = null; // `node2` 不会参与序列化！
```

我们建议开发者不依赖 `@type` 的隐含意义，
而是显式地使用序列化装饰器以及编辑器装饰器以使属性的用途更清晰。

### 其它装饰器

`@override` 显式声明此属性将覆盖基类的同一属性。

`@editorOnly` 将该属性声明为仅在编辑器环境下有效。