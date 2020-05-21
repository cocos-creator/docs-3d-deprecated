# Canvas Component Reference

The __Canvas__ component can obtain the actual resolution of the device screen at any time and zoom in/out of all the rendered elements in the scene. There can be multiple __Canvas__ in the scene, and all UI elements must be placed under __Canvas__ and its child nodes to be rendered.<br>
The Canvas __design resolution__ and __adaptation scheme__ are uniformly set in the __Project Setting__. A camera is provided inside the __Canvas__, and the default z axis direction is __-1000 ~ 998__, so the z axis on the UI must be within this range to display properly (without taking the threshold value).

![](canvas/canvas.png)

In the previous design, __Canvas__ was last rendered, meaning it could mask the rendering of all 3D content, but this was far from sufficient for project development needs, such as a 2D map with a 3D character. So, we've added the __RenderMode__ property, which allows developers to sort the rendering of the 3D Camera and UI Camera by changing the behavior of the UI Camera, which needs to be used with __ClearFlag__.

The __Canvas__ component is not only the root of the UI rendering, but also a very important feature in the game production is the multi-resolution adaptation, please refer to the [Multi-Resolution Adaption](../engine/multi-resolution.md) documentation.

## Canvas Properties

| Properties    | Function Explanation  |
| ------------- | ----------- |
| __ClearFlag__     | Clean up the flag of the screen buffer.<br>__DONT_CLEAR__: No cleanup.<br>__DEPTH_ONLY__: Clear the depth buffer.<br>__SOLID_COLOR__: Clear the color depth buffer. |
| __Color__     | The color after the color buffer cleanup. |
| __Priority__       | Camera sort priority.<br>When RenderMode is __INTERSPERSE__, specifies the rendering order with other cameras.<br>When RenderMode is __OVERLAY__, specify the order with other __Canvas__. |
| __RenderMode__    | Render mode of __Canvas__.<br>When set to __INTERSPERSE__, you can specify the rendering order of the __Canvas__ and the Camera in the Scene. When set to __OVERLAY__, __Canvas__ will render after the camera in all scenes are rendered.<br>__Note__: When __INTERSPERSE__ mode is enabled, the camera's __ClearFlags__ should be set to `Dont_Clear` if the 3D scene's camera is to be displayed above the __Canvas__. |
| __TargetTexture__ | Rendering texture of the target. |

## Detailed Explanation

Make sure that at least one __Canvas__ or __Camera__'s __ClearFlag__ property is set to `Solid_Color` in each scene, otherwise it will cause a splash screen to appear during rendering.

To set the __ClearFlag__ property, please refer to the following situations:
- If there is only one __2D Canvas__ or __3D Camera__ in the scene, then the __ClearFlag__ property is set to `Solid_Color`.
- If the scene contains 2D background layer, 3D scene layer, 2D UI layer, then:
  - 2D background layer: __ClearFlag__ property is set to `Solid_Color`.
  - 3D scene layer: __ClearFlag__ property is set to `Depth_Only`.
  - 2D UI layer: If the model is included, the __ClearFlag__ property is set to `Depth_Only` to avoid a model splash screen. If there is no model, the __ClearFlag__ property can be set to `Dont_Clear`/`Depth_Only`.
