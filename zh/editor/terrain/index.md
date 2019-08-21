# 地形系统
地形系统以一种高效的方式来展示大自然的山川地貌。开发者可以很方便的使用画刷来雕刻出盆地、山脉、峡谷、平原等地貌。

![terrain](./images/terrain.png)

## 创建
有两种创建方式：
1. 在 `Hierarchy` 中点击鼠标右键，在弹出菜单中点击Create->Terrain来创建地形。
   
   ![create terrain](./images/create-terrain.png)

2. 在一个空节点上添加 Terrain 组件。
   
   ![create terrain component](./images/component-terrain.png)

## 编辑
Cocos Creator 3D中的地形编辑主要包括三大功能：管理（Manage），雕刻（Sculpt），描绘（Paint）。可以通过点击三个Tab标签页来进行三个功能的切换。

![terrain component](./images/terrain-component.png)

### 管理（Manage）
用于调整地形的各种参数。

参数| 描述
---|---
TileSize | 地形Tile的大小
BlockCount | 地形块在两个维度上的数量
WeightMapSize | 权重图大小
LightMapSize | 光照贴图大小

### 雕刻（Sculpt）
用于改变地形的形状。
#### 画刷功能
- 隆起/凹下，鼠标左键/Shift+鼠标左键。

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
1. 点击+/-可以进行Layer的添加和删除。

   ![add layer](./images/layer-add.png)

2. 选中某个Layer后可以编辑DetailMap和TileSize
   
   ![edit layer](./images/layer-edit.png)
   
#### 画刷类型
目前只支持圆形画刷

#### 画刷参数设置
参数| 描述
---|---
BrushSize | 画刷的大小
BrushStrength | 画刷的力度
Brush Falloff | 画刷衰减度，这个值决定了画刷边缘的锐利程度。0.0意味着画刷在整个范围内都有完全效果（全部被当前层纹理覆盖），具有尖锐的边缘，1.0意味着画刷仅在它中心具有完全效果，在到达边缘的过程中影响会衰减。