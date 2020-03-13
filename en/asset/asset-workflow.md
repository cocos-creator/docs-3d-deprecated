# Assets workflow

## Importing assets

There are three ways to **import assets**:

   - Create a new file through the **Explorer** panel in __Cocos Creator 3D__. Use the **Create button** to start the importing process.
   - By copying files, in the **file manager of the operating system**, to the project resource folder, and then open the editor or activate the editor window to automatically refresh the resource list of the **Explorer**  panel to finish importing assets.
   - Drag and drop resource files from the **file manager of the operating system** to a folder location on the **Explorer** panel. This will trigger an import of the selected assets.
 
## Syncing Assets

 The assets in the **Explorer** panel are synchronized with the project resource files seen in the **file manager of the operating system**. Assets are moved, renamed, and moved in the **Explorer** panel. 
   - If deleted in __Cocos Creator 3D__, it will be deleted in the **file manager of the operating system**.
   - If deleted in the **file manager of the operating system**, it will be deleted in __Cocos Creator 3D__.

## Resource Configuration Information .meta File

All resource files will generate a **.meta** configuration file with the same name when imported. This configuration file provides the unique identification (**uuid**) of the resource in the project, small image references, cropping data of texture assets, as well as other configuration information. This data is a necessary factor in identifying a legitimate resource that __Cocos Creator 3D__ is using.

The **.meta** file is not visible in the **Explorer** panel. The renaming, moving, and deleting of a resource will automatically synchronize the **.meta** file corresponding to the resource by the editor. To ensure that configuration information such as the **uuid** remains unchanged, that is, it does not affect existing references.

It is not recommended to operate the resource file directly in the __file manager of the operating system__. If there is such an operation, please synchronize the corresponding **.meta** file. The following suggestions are recommended:

  - Close the editor you are using to avoid update failures due to file locks or identical resource names.
  - When deleting, renaming, or moving assets, please delete, rename, and move the **.meta** file as needed.
  - When copying assets together with **.meta** files, the copied **.meta** files will be used directly instead of generating new **.meta** files; if only the resource files are copied, a new **.meta** file with the corresponding name will be generated.

## Assets in Library

After the resource is imported, new data will be generated and stored in the project's **Library** folder. The structure and assets of the files in **Library** are engine-oriented and the format required for the final game, that is, __machine-friendly__, but __not human-friendly__.

When a library is lost or damaged, just delete the entire library folder and open the project, and the resource library will be rebuilt.

## How to locate assets

A resource has a unique **uuid**, used  to locate the resource, but this method is not intuitive enough. There is another intuitive way: **Database URL** format, such as an `asset-db`. The corresponding protocol header is `db://assets`, the protocol header for `internal-db` is `db://internal`.

There are folder-level resource formats, such as `db://assets/prefabs/fire.prefab`

## SVN or GIT syncing of assets

> **Note**: there are line breaks in the **.meta** file. It is recommended to unify the line break styles and rules of the team members' computers to avoid opening the project after synchronizing the project assets.