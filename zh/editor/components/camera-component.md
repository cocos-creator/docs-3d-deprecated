# 相机组件

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