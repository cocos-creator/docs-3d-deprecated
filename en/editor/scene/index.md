# Scene Editor
<img src="images/scene-editor.png" width="100%" height="100%"/>

**Scene Editor** is the core working area of content creation, You will use it to choose an place the scene image, character, effect, UI and other game elements. in this working area, you can select and use **transform tool** to change the position, rotation, scale, size and other attributes of node, also you will preview the WYSIWYG scene.

## View Introduction
### Navigation
There are some differences between 3D view and 2D view navigation. you can change between 3D view and 2D view by click the 3D/2D button on the toolbar.3D view is used for 3D scene editing，2D is used for UI,Sprite editing.

#### 3D View
In 3D view, you can move and rotate view of **Scene Editor** by following operations:
- left mouse button + Alt：rotate editor camera around view center.
- middle mouse button：pan view.
- mouse scroll：move editor camera back and forth.
- right mouse button + WASD：wandering in scene editor.
- **F** shortcut：focus editor to the selected node.

#### 2D View
In 2D view, you can move and rotate view of **Scene Editor** by following operations:
- middle mouse button：pan view.
- mouse scroll：scale view based on the current mouse position。
- right mouse button：pan view.
- **F** shortcut：focus editor to the selected node.


### Coordinate System And Grid
Grid in the scene is important reference information for us to place the scene elements. For information of the relationship between coordinate system and node attributes like position, please read [Transform](../../concepts/scene/coord.md) section.

### Scene Gizmo
The Scene Gizmo is in the upper-right corner of the Scene View. It indicates the view direction of editor camera in scene view. You can change view direction quickly by clicking on it.

![Scene Gizmo](images/scene-gizmo.png)

- Click on the six arrows, you can change to the top, down, left, right, front, back view separately.
- Click on the cube in the center, you can switch between ortho camera mode and perspective camera mode.

## Select a node
The node will be selected if you left click the mouse on the node in **Scene Editor**. You can use transform tools (like position, rotation) to do some node operations only if you have selected it.

## Gizmo Operation Introduction
The main function of **Scene Editor** is to edit and arrange the visible elements in the scene and get a WYSIWYG scene immediately, we mainly use **Gizmo** tools to assist in the visual editing of the scene。

- [Transform Gizmo](./transform-gizmo.md)
- [Camera Gizmo](./camera-gizmo.md)
- [Light Gizmo](./light-gizmo.md)
- [Collider Gizmo](./collider-gizmo.md)
- [ParticleSystem Gizmo](./particle-system-gizmo.md)
