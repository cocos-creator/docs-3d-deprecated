# Group And Mask

In Cocos Creator 3D, some physics components (there are currently rigid body components and collider components) provide interfaces for group and mask.

## Share

In Cocos Creator 3D, the physical elements and nodes are currently in a one-to-one relationship. The group and mask belong to the physical elements. The physics components on a single node modify the group and mask of the physical elements corresponding to the nodes.

## Principle

As long as the following conditions are true, it will be detected

```ts
(GroupA & MaskB) && (GroupB & MaskA)
```

For example: two physical elements `A` and `B`.

The group value of `A` is `1` and the mask value is `3`

The group value of `B` is `2`, and the mask value is `2`

The formula `(1 & 2) && (2 & 3)` is false, so here `A` will not be detected with `B`.

Here according to the mask value of `B` is `2`, we can know that the detectable group of `B` is `1`, and the group of `A` is `0`, so it is not detected.

**Note: The expression depends on bit operation, the bit operation of `javascript` is limited to `32` bits, and the last bit is the sign bit. To avoid exceeding the operation range, it is recommended that the range of the group is `[0, 31 )`**.

## Group

- Set Group Value
  
  The following group value is `3`, and the binary value is `11`, which means it is in the `0`, `1` group (starting from `0`)

  ```ts
  const group = (1 << 0) + (1 << 1);
  Collider.setGroup(group);
  ```

- Get Group Value

  ```ts
  Collider.getGroup();
  ```

- Add Group
  
  Based on the above code, after the following code, the grouping value is `7`, and the binary value is `111`, so it means that it is in the `0`, `1`, and `2` groups.

  ```ts
  const group = 1 << 2;
  Collider.addGroup(group);
  ```

- Remove Group
  
  Based on the above code, after the following code, the grouping value is `3`, so in the `0`, `1` group.

  ```ts
  const group = 1 << 2;
  Collider.removeGroup(group);
  ```

**Note: It is recommended to fix in a group, you can use the node's `layer` property as a group**.

**Note: The receiving parameters of the above methods are all decimal numbers. For easy understanding, binary explanation is used here. Developers can also directly input decimal numbers for group operation after they are familiar**.

## Mask

- Set Mask Value
  The value of the following mask is `3`, the binary value is `11`, indicating that the detectable group is `0`, `1`.

  ```ts
  const mask = (1 << 0) + (1 << 1);
  Collider.setMask(mask);
  ```

- Get Mask Value

  ```ts
  console.log(Collider.getMask());
  ```

- Add Mask
  
  On the basis of the above code, after the following code, a detectable group `3` was added.

  ```ts
  const mask = 1 << 2;
  Collider.addMask(mask);
  ```

- Remove Mask
  
  The following code removes a detectable group `3`.

  ```ts
  const mask = 1 << 2;
  Collider.removeMask(mask);
  ```

**Note: The addition and subtraction operation have higher priority than the shift operation**.

**Note: Flexible use of group and mask can reduce the cost of additional detecting**.

**Note: The group and mask of the physics system may continue to use the `Layer` property of `Node` and provide a setting panel, please pay attention to the update announcement**.

## Examples

Here is a simple example of usage:

### Define Group

Method 1: Defined in an `object`

```ts
export const PHY_GROUP = {
    Group0: 1 << 0, // Group 0 is equivalent to giving it an alias of Group0.
    Group1: 1 << 1
};
```

Method 2: Defined in an `enum` (`typescript only`)

```ts
enum PHY_GROUP {
    Group0 = 1 << 0,
    Group1 = 1 << 1
};
```

**Note: You can consider to reuse the preset layers in `Layer`**.

In order to be able to set up groups on the panel, you need to register the defined groups to the editor `Enum(PHY_GROUP)` through the `Enum` function exported by the `cc` module.

**Note: For historical reasons, the `Enum` function has special treatment for `-1`. If you are not familiar with it, do not define an attribute with a value of `-1`**.

### Using The Mask

The mask can be defined according to grouping, for example:

- Define a mask(`const maskForGroup1 = PHY_GROUP.Group1;`) that only detects `Group1` 
- Define a mask(`const maskForGroup01 = PHY_GROUP.Group0 + PHY_GROUP.Group1;`) that can detect `Group0` and `Group1` 
- Define a mask(`const maskForNone = 0;`) that is not detected by all groups 
- Define a mask(`const maskForAll = -1;`) for all groups to detect 

### View Binary

By executing `(value >>> 0).toString(2)` in the running environment of javascript, you can see the binary string representation.

![View binary](img/mask-all.jpg)

---

Continue to the [Raycast detection](physics-raycast.md) documentation.
