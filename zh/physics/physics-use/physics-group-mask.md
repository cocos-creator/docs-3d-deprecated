# 分组和掩码

在 Cocos Creator 3D 中，物理组件提供了设置组和掩码的接口，以下代码示例介绍了所有的接口

## 分组

- 设置分组值，以下代码中的 group 的值十进制为 3，二进制为 011，分组可看作是 0 或 1 的

  `const group = 1 << 0 + 1 << 1;`
  `Collider.setGroup(group);`

- 获取分组值，此示例中为 3。

  `Collider.getGroup()`

- 添加分组值，经过以下代码后，分组可以是 0 或 1 或 2 的。

  `const group = 1 << 2;`
  `Collider.addGroup(group);`

- 减少分组值，经过以下代码后，分组可以是 0 或 1 的。

  `const group = 1 << 2;`
  `Collider.removeGroup(group);`

## 掩码

- 设置掩码值，以下代码中的 mask 的值十进制位为 3，二进制为 011，表示检测的分组为 0 或 1 的。

  `const mask = 1 << 0 + 1 << 1;`
  `Collider.setMask(mask);`

- 获取掩码值，此示例中为 3。

  `console.log(Collider.getMask());`

- 添加掩码值，经过以下代码后，该掩码表示检测的分组为 0 或 1 或 2 的。

  `const mask = 1 << 2;`
  `Collider.addMask(mask);`

- 减少掩码值，经过以下代码后，该掩码表示检测的分组为 0 或 1 的。

  `const mask = 1 << 2;`
  `Collider.removeMask(mask);`

**注：灵活使用分组和掩码可以减少额外检测的消耗**。

<!--
TODO :

## 举例

以下代码区域列举了一个简单的使用示例

```

```
-->

## [**回到** 物理使用](./../physics-use.md)

## [**或者回到** 物理简介](./../physics.md)
