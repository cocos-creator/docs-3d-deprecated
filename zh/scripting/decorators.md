
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

#### ⚠️ 兼容行为

在 1.2 版本之前，`@type` 隐含着可序列化以及可编辑器交互。为了兼容，我们在 1.2 及后续的版本保留了此行为，并对其做出限制：当仅存在 `@type` 或 `@property` 装饰器时，`@type` 才隐含可序列化以及可编辑器交互：
```ts
@type(Node) // 隐含 `@serializable` 和 `@editable`：
            // 同时参与序列化以及编辑器交互；
            // 不推荐的做法；
            // 显式声明 `@serializable` 和 `@editable` 使得逻辑更清晰
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