# LabelAtlas Asset

**LabelAtlas** asset is a user defined asset, it's used for configuring LabelAtlas.

## Create LabelAtlas Asset

In **Assets Panel**  right-click on a folder, and click the following context menu item **Create -> Label Atlas**. It will create a **LabelAtlas.labelatlas** asset.

![create label atlas](label-atlas/create-label-atlas.png)

Before using **LabelAtlas** asset, you need some configurations. eg, configure a pre-drawn picture that contains the font style, as shown below:

![](label-atlas/raw_texture_file.png)

## Configuration of LabelAtlas asset

After selecting an **LabelAtlas** asset in the **Assets Panel**, the **Property Inspector** panel will display all configurable properties for the **LabelAtlas** asset.

| Properties       | Description
| --------------   | -----------
| SpriteFrame      | Specify a pre-drawn picture that contains the font style you want
| Item Width       | Specify the width of each character
| Item Height      | Specify the height of each character
| Start Char       | Specify the start char, even if the start char is a *space*, you also need insert a space. |

When the configuration is complete, click the tick button at the top right of the **Property Inspector** panel to save the settings.

![save label atlas](label-atlas/save-label-atlas.png)

## Using LabelAtlas asset

It's quite simple to use the **LabelAtlas** asset. You just need setup a new Label component and drag the **LabelAtlas** asset to the **Font** attribute of the Label component. Please refer to [Label Component](../ui-system/components/editor/label.md) for details.
