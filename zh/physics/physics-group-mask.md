# 分组掩码

在 Cocos Creator 3D 中，部分物理组件（目前有刚体组件和碰撞器组件）提供了设置分组和掩码的接口。

## 共享

在 Cocos Creator 3D 中，目前物理元素和节点是一对一的关系，分组和掩码是属于物理元素的，单个节点上的物理组件修改的都是节点对应物理元素的分组和掩码。

## 原理

只要以下条件为真就会进行检测

```
(GroupA & MaskB) && (GroupB & MaskA)
```

例如：两个物理元素 A B。

A 的分组值为 1，掩码值为 3

B 的分组值为 2，掩码值为 2

算式 `(1 & 2) && (2 & 3)` 为假，所以这里`A`不会和`B`进行检测。

这里通过`B`的掩码值为`2`，可以知道`B`可检测的组是`1`，而`A`在的组是`0`，所以不检测。

**注：分组和掩码利用了位运算，`javascript`中位运算的操作数是`32`位的，并且最后一位是符号位，为避免超出运算范围，建议组的范围为`[0, 31)`**。

## 分组

- 设置分组值
  以下分组值为 3，二进制为 11，表示在第 0，1 组（从 0 开始）

  ```
  const group = (1 << 0) + (1 << 1);
  Collider.setGroup(group);
  ```

- 获取分组值

  ```
  Collider.getGroup();
  ```

- 添加分组值
  上述代码基础上，经过以下代码后，分组值为 7，二进制为 111，所以表示在 0，1，2 组。

  ```
  const group = 1 << 2;
  Collider.addGroup(group);
  ```

- 减少分组值
  上述代码基础上，经过以下代码后，分组值为 3，所以在 0，1 组。

  ```
  const group = 1 << 2;
  Collider.removeGroup(group);
  ```

**注：推荐固定在一个组中，可以把节点的`layer`属性当作组**。

**注：上述方法接收参数均为十进制数字，为方便理解，此处用二进制解释，开发者熟悉后也可直接传入十进制数字进行分组操作**。

## 掩码

- 设置掩码值
  以下 mask 的值 3，二进制为 11，表示可检测的组为 0，1 。

  ```
  const mask = (1 << 0) + (1 << 1);
  Collider.setMask(mask);
  ```

- 获取掩码值

  ```
  console.log(Collider.getMask());
  ```

- 添加掩码值
  上述代码的基础上，经过以下代码后，增加了一个可检测组 3 。

  ```
  const mask = 1 << 2;
  Collider.addMask(mask);
  ```

- 减少掩码值
  以下代码去掉了一个可检测组 3 。

  ```
  const mask = 1 << 2;
  Collider.removeMask(mask);
  ```

**注：加减符号的优先级高于位移符号**。

**注：灵活使用分组和掩码可以减少额外检测的消耗**。

**注：物理系统的分组和掩码后面可能会使用 Node 的 Layer 属性，并提供设置面板，请留意更新公告**。

## 举例

以下列举了一个简单的使用示例：

### 定义分组

方式一：定义在一个`object`中

```
export const PHY_GROUP = {
    Group0: 1 << 0, // 第 0 组，相当于给它取了一个 Group0 的别名。
    Group1: 1 << 1
};
```

方式二：定义在一个`enum`中 （`typescript only`）

```
enum PHY_GROUP {
    Group0 = 1 << 0,
    Group1 = 1 << 1
}
```

**注：这里可以考虑复用`Layer`中预设的层**。

为了可以在面板上设置分组，可以通过`cc`模块导出的`Enum`函数，将定义好的分组注册到编辑器中`Enum(PHY_CROUP)`。

**注：由于历史原因，`Enum`函数对`-1`有特殊处理，如果不熟悉，请勿定义值为`-1`的属性**。

### 使用掩码

掩码可以根据分组进行定义，例如以下示例

- 定义一个只检测 Group1 的掩码`const maskForGroup1 = PHY_GROUP.Group1;`
- 定义一个可检测 Group0 和 Group1 的掩码`const maskForGroup01 = PHY_GROUP.Group0 + PHY_GROUP.Group1;`
- 定义一个所有组都不检测的掩码`const maskForNone = 0;`
- 定义一个所有组都检测的掩码`const maskForAll = -1;`

### 查看二进制

在 javascript 的运行环境中通过执行`(value >>> 0).toString(2)`， 可以看到二进制的字符串表示。

![查看二进制](img/mask-all.jpg)

---

继续前往 [射线检测](physics-raycast.md) 说明文档。
