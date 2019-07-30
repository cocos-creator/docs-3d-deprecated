# 相机

游戏中的相机是用来捕捉场景画面的主要工具。我们通过调节相机相关参数来控制可视范围的大小，在 Cocos Creator 3D 编辑器中相机呈如下表示：

![camera](camera/camera.jpg)

相机的可视范围是通过 6 个平面组成一个 **视锥体（Frustum）** 构成， **近裁剪面（Near Plane）** 和 **远裁剪面（Far Plane）** 用于控制近处和远处的可视距离与范围，同时它们也构成了视口的大小。

![camera view](camera/camera-view.gif)

## 相机组件

相机组件是我们用来呈现场景画面的重要功能组件。

![camera component](camera-comp.jpg)

| 属性名称 | 说明 |
|:-------:|:---:|
| ClearFlags | 相机清空标识。包含：<br>DONT_CLEAR：不清空；<br>DEPTH_ONLY：只清空深度；<br> SLOD_COLOR：清空颜色、深度与模板缓冲|
| Color | 清空为指定的颜色 |
| Depth | 清空为指定的深度 |
| Stencil | 清空为指定的模板缓冲 |
| Far | 远裁剪距离 |
| Near | 近裁剪距离 |
| Fov | 视场角 |
| OrthoHeight | 正交相机的高度 |
| Priority | 优先级。在渲染流程中会优先渲染高优先级的相机 |
| Projection | 投影模式。分为 **透视投影（PERSPECTIVE）** 和 **正交投影（ORTHO）** |
| Rect | 相机的视口大小 |
| Visibility | 相机的可见性。用于控制不同模型在同一相机中的可见性。 |

---