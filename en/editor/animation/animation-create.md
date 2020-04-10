# Create Animation components and animation clips

## Creating Animation Components

On each __Node__, we can add different __components__. If we want to create an __animation__ on this node, we must also create a new __Animation component__ for it.

There are two ways to create a new __Animation component__:

 - Select the corresponding node, click __Add Component__ below in the __Property Inspector__, and select __AnimationC omponent__ in __Components__.
 - Open the animation editor, then select the node in which to add then animation in the __Hierarchy Manager__, and click the __Add Animation component__ button in the animation editor.

![add-component](animation-clip/add-component.png)

Specifics about __Animation component__ parameters can be found in [Animation Component Reference](./../../engine/animation/animation-component.md) documentation.

## Creating and mounting animation clips

Even though there is an __Animation component__ on the __Node__, there is no corresponding __animation clip data__. There are two ways to create __animation clips__:

  - Click the `+` on the upper left in the __Assets manager__, or right-click the blank area and select __Animation Clip__. Now, a clip file named 'New AnimationClip' will be created in the __Assets manager__.

It is not enough to just create it. We need to click on the node, in the __Hierachy manager__ and find __Animation__ in the __Property Inspector__. At this time, __Clips__ shows __0__, and it needs to be changed to __1__. __Next__, drag the __New AnimationClip__ that was just created in the __Explorer Manager__, and drag it into the __animation-clip selection box__ that just appeared.

  - If you have not added an __animation clip file__ in the __Animation component__, you can directly click the __New AnimationClip__ button in the animation editor to create a new animation clip file. The newly created animation clip will be automatically attached to the animation component.

> **Note**: If you choose to overwrite the existing clip file, the content of the overwritten file will be cleared.

![add-clip](animation-clip/add-clip.png)

---

So far, we have completed the preparations before the animation, the next step is to edit the [animation sequences](animation-clip.md).