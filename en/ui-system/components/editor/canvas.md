# Canvas Component Reference

The **Canvas** component can obtain the actual resolution of the device screen at any time and zoom in / out of all the rendered elements in the scene. There can be multiple Canvas in the scene, and all UI elements must be placed under Canvas and its child nodes to be rendered.
The Canvas design resolution and adaptation scheme are uniformly set in the **Project Setting**. A camera is provided inside the Canvas, and the default z axis direction is **-1000 ~ 998**, so the z axis on the UI must be within this range to display properly (without taking the threshold value).

![](canvas/canvas.png)

In the previous design, Canvas was last rendered, meaning it could mask the rendering of all 3D content, but this was far from sufficient for project development needs, such as a 2D map with a 3D character. So, we've added the **RenderMode** property, which allows developers to sort the rendering of the 3D Camera and UI Camera by changing the behavior of the UI Camera, which needs to be used with **ClearFlag**.

The Canvas component is not only the root of the UI rendering, but also a very important feature in the game production is the multi-resolution adaptation, please refer to the [Multi-Resolution Adaption](../engine/multi-resolution.md).

## Canvas Properties

| Properties    | Function Explanation  |
| ------------- | ----------- |
| ClearFlag     | Clean up the flag of the screen buffer.<br>**DONT_CLEAR**: No cleanup.<br>**DEPTH_ONLY**: Clear the depth buffer.<br>**SOLID_COLOR**: Clear the color depth buffer. |
| Color     | The color after the color buffer cleanup. |
| Priority       | Camera sort priority.<br>When RenderMode is **INTERSPERSE**, specifies the rendering order with other cameras.<br>When RenderMode is **OVERLAY**, specify the order with other Canvas. |
| RenderMode    | Render mode of Canvas.<br>When set to **INTERSPERSE**, you can specify the rendering order of the Canvas and the Camera in the Scene. When set to **OVERLAY**, Canvas will render after the camera in all scenes are rendered.<br>**Note**: When **INTERSPERSE** mode is enabled, the camera's **ClearFlags** should be set to `Dont_Clear` if the 3D scene's camera is to be displayed above the Canvas. |
| TargetTexture | Rendering texture of the target. |

## Detailed Explanation

Make sure that at least one Canvas or Camera's **ClearFlag** property is set to `Solid_Color` in each scene, otherwise it will cause a splash screen to appear during rendering.

To set the **ClearFlag** property, please refer to the following situations:
- If there is only one 2D Canvas or 3D Camera in the scene, then the **ClearFlag** property is set to `Solid_Color`.
- If the scene contains 2D background layer, 3D scene layer, 2D UI layer, then:
  - 2D background layer: **ClearFlag** property is set to `Solid_Color`.
  - 3D scene layer: **ClearFlag** property is set to `Depth_Only`.
  - 2D UI layer: If the model is included, the **ClearFlag** property is set to `Depth_Only` to avoid a model splash screen. If there is no model, the **ClearFlag** property can be set to `Dont_Clear` / `Depth_Only`.
