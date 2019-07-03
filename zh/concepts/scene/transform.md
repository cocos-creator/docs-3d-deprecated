# 坐标系和变换

## Cocos Creator 3D 坐标系

Cocos Creator 3D 的坐标系与 Cocos Creator 和 cocos2d-x 引擎的坐标系完全一致，采用笛卡尔右手坐标系，原点在左下角，x 向右，y 向上，z 向外。

![right hand](transform/right_hand.png)

### 屏幕坐标系

Cocos Creator 3D 的屏幕坐标系原点为屏幕左下角，x 向右，y 向上。

![screen vs cocos](transform/screen_space.png)

### 世界坐标系（World Coordinate）和本地坐标系（Local Coordinate）

世界坐标系也称为绝对坐标系，在 Cocos Creator 3D 游戏开发中表示场景空间内的统一坐标体系，「世界」就用来表示我们的游戏场景。

本地坐标系也称为相对坐标系，是和节点相关联的坐标系。每个节点都有独立的坐标系，当节点移动或改变方向时，和该节点关联的坐标系将随之移动或改变方向。

Cocos Creator 3D 中的 **节点（Node）** 之间可以有父子关系的层级结构，我们修改节点的 **位置（Position）** 属性设定的节点位置是该节点相对于父节点的 **本地坐标系** 而非世界坐标系。最后在绘制整个场景时 Cocos Creator 3D 会把这些节点的本地坐标映射成世界坐标系坐标。

### 子节点的本地坐标系

当场景中的节点存在父子层级关系的结构时，如下图所示：

![node tree](transform/node_tree.png)

我们按照以下的流程确定每个节点在世界坐标系下的位置：

1. 从场景根级别开始处理每个节点，上图中 `NodeA` 就是一个根级别节点。首先根据 NodeA 的 **位置（Position）** 属性，在世界坐标系中确定 NodeA 的显示位置和坐标系原点位置。
2. 接下来处理 NodeA 的所有直接子节点，也就是上图中 `NodeB` 以及和 NodeB 平级的节点。根据 NodeB 的位置，在 NodeA 的本地坐标系中确定 NodeB 在场景空间中的位置和坐标系原点位置。
3. 之后不管有多少级节点，都继续按照层级高低依次处理，每个节点都使用父节点的坐标系和自身位置来确定在场景空间中的位置。

## 变换属性

节点主要包括 **位置（Position）**、**旋转（Rotation）**、**缩放（Scale）** 变换属性:

![transform properties](transform/transform_props.jpg)
