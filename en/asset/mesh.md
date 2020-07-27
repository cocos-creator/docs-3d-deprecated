# Model assets

__Currently__, model files in __FBX__ and __glTF__ formats. For how to export these two model files from third-party tools, you can refer to the [__DCC Export Mesh__](./dcc-export-mesh.md) documentation.

## Model importing

After importing into the editor, from the outside, the corresponding model asset file can be obtained in the __Assets Panel__. It's directory structure is as follows: 

  - The structure of a model file without animations is as follows:

  ![](mesh/mesh_list.png)

  - The structure of the model file that contains animations is as follows:

  ![](mesh/mesh_list_1.png)

The various __file types__ are described:
  - `.material` files are material files
  - `.mesh` files are model files
  - `.texture` files are model texture files
  - `.animation` files are model animation files
  - `.skeleton` files are model bone files
  - `.prefab` files are prefab files that are automatically generated on import

## Using Models

After importing a __model__ file, drag the __root node__ of the __model__ file directly from the __Assets Panel__ to the __node__ you want to place in the __Hierarchy Manager__ to complete the node creation. At this point the model is successfully created in the scene . <br>
Alternatively, you can expand the node of the __model__ file, select the `.prefab` file under the model file node, and drag it from the __Assets Panel__ into the __Hierarchy Manager__ to complete the creation.

![](mesh/mesh_use.gif)

## Model asset Properties panel description

When the model asset file is selected in the __Asset Explorer__ (.fbx or .gltf), the properties of the model asset can be set in the __Property Inspector__.

### Model module

![](mesh/mesh_model.jpg)

- __Normals information__: *Optional*, *Exclude*, *Require*, *Recalculate*
- __Tangents information__: *Optional*, *Exclude*, *Require*, *Recalculate*
- __SkipValidation__: whether to skip standard checks

### Animation Module

![](mesh/mesh_animation.jpg)

The above image is all the animation asset information under the current model, and the editing area of ​​the specific frame number information of the currently selected animation. You can change the animation name or perform simple animation cropping here. To do so:

  - Click the `+` button in the red box on the image to add an animation clip asset. The new file added by default copies a complete clip data. You can input the number of frames in the `Start` `End` input box to crop the animation. (Drag and drop animation is not currently supported)

  - Click the `-` button in the red box on the image to delete the currently selected animation file

### Material module

![](mesh/mesh_material.jpg)

- __DumpMaterial__: When you are not satisfied with the material that comes with the model file and want to modify it, you need to enable this option to dump the material files in the file structure directory out of the model assets. You can adjust and modify the materials.

- __Dumper Directory__: here you can specify or view the directory location for the dumped files.

## About glTF

Currently, the glTF 2.0 specification is supported, but the following features are unavailable until __Cocos Creator 3D__ __v1.1__:

  - sparse accessor
  - morph target