# UIStaticBatch Component References

The UIStaticBatch component is used to improve UI rendering performance. The script will collect all the rendering data under the UI node tree (except Model, Mask, and Graphice) during the initialization of the current frame rendering and store it as a static IA rendering data. And rendering with fixed data in the subsequent rendering process, no longer traversing its node tree, after which the coordinate transformation will no longer take effect.

## Detailed Explanation

The following points need to be noted when using this component:

- Do not trigger static batches frequently, as the original stored IA data will be emptied and re-collected, resulting in some performance and memory loss.
- Not applicable for child node tree which contains Mask, Graphics and Model.
- For a node that will not have any changes in the node tree (e.g. 2D Map), all child nodes can be deleted after data collection to get the best performance and memory performance.
