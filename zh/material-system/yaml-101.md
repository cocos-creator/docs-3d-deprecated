# YAML 101
我们使用的是符合 YAML 1.2 标准的解析器，这意味着首先它是与 JSON 完全兼容的，直接书写 JSON 完全不会有问题：

```yaml
CCEffect %{
  "techniques":
    [{
      "passes":
      [{
        "vert": "skybox-vs",
        "frag": "skybox-fs",
        "rasterizerState":
        {
          "cullMode": "none"
        }
        # ...
      }]
    }]
}%
```

当然这也意味着繁琐的语法，所以 YAML 提供了一些更简洁的数据表示方式：

* 所有的引号和逗号都可以省略

```yaml
key1: value1
key2: value2
```

* 行首的空格缩进数量代表数据的层级<sup>[1](footnote-1)

```yaml
object1:
  key1: value1
object2:
  key2: value2
  key3: value3
  object3:
    key4: value4
```

* 以 连字符+空格 开头代表数组元素

```yaml
array:
- element1:
    key1: value1
- element2:
    key2: value2
- element3:
    key3: value3
    key4: value4
```

综合这几点，上面的 effect 内容就可以很简洁地写成这样：

```yaml
CCEffect %{
  techniques:
  - passes:
    - vert: skybox-vs
      frag: skybox-fs
      rasterizerState:
        cullMode: none
      # ...
}%
```

另一个对我们的情况非常有用的 YAML 特性是数据间的引用与继承，先来看引用：

```yaml
object1: &o1
  key1: value1
object2:
  key2: value2
  key3: *o1
```

这个数据解析出来是这样的：

```json
{
  "object1": {
    "key1": "value1"
  },
  "object2": {
    "key2": "value2",
    "key3": {
      "key1": "value1"
    }
  }
}
```

再来看继承：

```yaml
object1: &o1
  key1: value1
  key2: value2
object2:
  <<: *o1
  key3: value3
```

这个数据解析出来是这样的：

```json
{
  "object1": {
    "key1": "value1",
    "key2": "value2"
  },
  "object2": {
    "key1": "value1",
    "key2": "value2",
    "key3": "value3"
  }
}
```

对应到我们的 effect 中，比如多个 pass 拥有相同的 property 内容，或很多其他情景下，都可以很方便地复用数据：

```yaml
CCEffect %{
  techniques:
  - passes:
    - # pass 1 specifications...
      properties: &props # declare once
        p1: { value: [ 1, 1, 1, 1 ] }
        p2: { sampler: { mipFilter: linear } }
        p3: { inspector: { type: color } }
    - # pass 2 specifications...
      properties: *props # reference anywhere
}%
```

# 更多参考资料
* https://en.wikipedia.org/wiki/YAML
* https://yaml.org/spec/1.2/spec.html

<a name="footnote-1">[1]</a> 标准 YAML 并不支持制表符，但在解析 effect 数据时，我们会先尝试把其中所有的制表符替换为 2 个空格，以避免偶然插入制表符带来的琐碎的麻烦。但整体上，请一定尽量避免插入制表符以获得更稳定的结果。
