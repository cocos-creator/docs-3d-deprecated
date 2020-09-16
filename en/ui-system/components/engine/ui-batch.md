# UI Batch Rules

UI batching adopts the same hash material and same image principle for batching. The same hash means that even if the same material is used, if their hash values ​​are different, the batch will be interrupted. For example, the macros enabled by the two materials involve the use of `Uniform`, resulting in failure to batch. Sprite and text are the same because their images are different. Of course, it can be done under strict control, but this is another story.

UI rendering data collection is based on the Hierarchy, so in the process of recursive Hierarchy, if you encounter the three components `UIMeshRenderer`, `Mask` and `Graphics`, it will definitely interrupt the batch. If the images used before and after are different, the batch will be interrupted. Therefore, we recommend that the images of different modules be batched separately to reduce DrawCall. However, if you want to batch a large amount of data, you need to strictly layout the nodes.
