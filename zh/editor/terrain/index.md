# 地形系统
地形系统以一种高效的方式来展示大自然的山川地貌。开发者可以很方便的使用画刷来雕刻出盆地、山脉、峡谷、平原等地貌。

![terrain](./images/terrain.png)

## 创建
创建需要两个步骤:

1. 在 `Hierarchy(层级管理器)` 中点击鼠标右键，在弹出菜单中点击`Create(创建)`->`Terrain(地形)`来创建地形节点(地形节点可移动,但不支持旋转与缩放)。
   
   ![create terrain](./images/create-terrain.png)

2. 在`Assets`中点击鼠标右键，在弹出菜单中点击`Create(创建)`->`Terrain(地形)`来创建地形资源。
   
   ![create terrain asset](./images/createTerrainAsset.png)

## 使用
点击创建后的地形节点,此时在`Inspector`中存在地形组件,把已经创建好的地形资源赋予地形组件中的`Asset`中。

![terrain inspector](./images/terrain-inspector.png)

## 编辑
赋值完地形资源后可在`Scene`中弹出编辑面板,Cocos Creator 中的地形编辑主要包括三大功能：管理（Manage），雕刻（Sculpt），描绘（Paint）。可以通过点击三个Tab标签页来进行三个功能的切换。

![terrain component](./images/terrain-panel.png)

除了编辑面板,也可以在toolbars上对各种不同模式进行切换。
![terrain component](./images/toolbar.png)
### 管理（Manage）
用于调整地形的各种参数。Tile是地形的最小单位，Tile组成地形块（Block），目前一个Block由32x32个Tile组成，一个地形由至少1个Block组成。

参数| 描述
---|---
TileSize | 地形Tile的大小，目前一个地形块由32 x 32个Tile组成，所以一个地形块的边长是32 x TileSize。
BlockCount | 地形块在两个维度上的数量(注意:该值调整过大会造成顶点数过多造成卡顿)
WeightMapSize | 权重图大小
LightMapSize | 光照贴图大小

### 雕刻（Sculpt）
用于改变地形的形状。
#### 画刷功能
- 隆起/凹下，鼠标左键/Shift+鼠标左键。
- 平滑,隆起凹下的操作往往会使地形看上去很尖锐,此时就可以使用平滑的功能。
#### 画刷类型
目前只支持圆形画刷

#### 画刷参数设置

参数| 描述
---|---
BrushSize | 画刷的大小
BrushStrength | 画刷的力度

### 描绘（Paint）
用于描绘地形的纹理

#### Layer编辑
1. 点击+/-可以进行Layer的添加和删除(最多支持4层layer)。

   ![add layer](./images/layer-plus-minus.png)

2. 选中某个Layer后可以编辑DetailMap和TileSize
   
   ![edit layer](./images/select-pic.png)

    参数| 描述
    ---|---
    DetailMap | 当前Layer的纹理
    TileSize | 纹理的平铺大小，值越小会在同样大小的区域内进行更多次的平铺
   
#### 画刷类型
目前只支持圆形画刷

#### 画刷参数设置
参数| 描述
---|---
BrushSize | 画刷的大小
BrushStrength | 画刷的力度
Brush Falloff | 画刷衰减度，这个值决定了画刷边缘的锐利程度。0.0意味着画刷在整个范围内都有完全效果（全部被当前层纹理覆盖），具有尖锐的边缘，1.0意味着画刷仅在它中心具有完全效果，在到达边缘的过程中影响会衰减。
