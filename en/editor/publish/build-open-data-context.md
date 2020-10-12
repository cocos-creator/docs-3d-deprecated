# Building Open Data Context Project

## Introduction to Open Data Context

Currently, **WeChat**, **Baidu** and **ByteDance** are small game platforms that add the concept of **Open Data Context**, which is a separate game execution environment, in order to protect their social relationship chain data. The resources, engines, and applications in the **Open Data Context** are completely isolated from the main game, and only in the **Open Data Context** can developers access the relationship chain data through the open interface provided by the platform to implement some features such as ranked version.

In **Cocos Creator 3D**, we deprecates the maintenance of the Canvas Renderer module. The **Open Data Context** now only supports Canvas rendering.
In the previous scheme, **Cocos Creator 2D** was used to build **Open Data Context** projects. As of **v3.0**, this practice has been deprecated and a built-in Canvas engine has been provided. The engine is provided by the WeChat team and is a front-end lightweight Canvas engine based on XML+CSS design.

And with a few basic front-end skills, a developer can simply implement a ranked version of the feature.

## Building processes

1. Add the `SubContextView` component to nodes that need to display **Open Data Context** in the project.

2. The platforms that currently support building **Open Data Context** projects are **WeChat**, **Baidu** and **ByteDance**. In the build options of these platforms, check **Generate Open Data Context Template**, click **Recompile**.

    ![](./build-open-data-context/generate-template.png)

3. After the build is complete, a project named **openDataContext** will be generated in the directory, which is a built-in template project in Cocos Creator.  

    ![](./build-open-data-context/build-output.png)

> **Note**: Next time you build, if the **openDataContext** directory exists in the build directory, it will not be overwritten.

4. Use the developer tools provided by the platform to open the built main project, you can see the effect of the **Open Data Context Template Project**.

    ![](./build-open-data-context/show-in-devtool.png)

## Customization on Open Data Context Project

Before we can prepare for custom **Open Data Context** projects, it is important to understand some basic skills:
- [minigame-canvas-engine quick start](https://wechat-miniprogram.github.io/minigame-canvas-engine/api/guide.html#%E5%AE%89%E8%A3%85)
- [doT template engine use](http://olado.github.io/doT/?spm=a2c6h.12873639.0.0.36f45227oKu0XO)

After understanding these basic skills, we can easily customize the **Open Data Context Project**.  
The generated **Open Data Context Project** template has the following directory structure:

![](./build-open-data-context/folder-structure.png)

- **render/template.js**：To record XML text information, refer to [Tag Document](https://wechat-miniprogram.github.io/minigame-canvas-engine/api/tags.html#%E6%A0%87%E7%AD%BE%E5%88%97%E8%A1%A8). The project uses the template engine to generate XML text by default.
- **render/style.js**：To record CSS style text information，refer to [Style Document](https://wechat-miniprogram.github.io/minigame-canvas-engine/api/style.html#%E5%B8%83%E5%B1%80)
- **render/dataDemo.js**：Simulates some random data of the ranked version, where the developer can request the relational chain data from the platform and pass it to the doT template engine to generate relevant XML text
- **render/avatar.png**：Header images for display in template project, can be deleted.
- **engine.js**：source code of Canvas engine
- **index.js**：**Open Data Context Project** entry file where the **Open Data Context** is rendered by passing XML text and CSS styles to the Canvas engine

## SubContextView Component Description

The `SubContextView` component contains two main properties, **Design Resolution** and **FPS**.

![](./build-open-data-context/sub-context-view.png)

The rendering process of the **Open Data Context** is to render the content onto the sharedCanvas, and then the primary project renders the sharedCanvas as a texture map onto the `SubContextView`.  

### Design Resolution

The image above sets the component's design resolution to **640** * **960**, so at the component onLoad phase, the size of the sharedCanvas will be set to **640** * **960**, which means that after the build, the open data project will be rendered on a **640** * **960** off-screen canvas, which requires the maximum width of the element is **640** and the maximum height is **960** when setting style in `style.js`, otherwise the rendered content will exceed the canvas. Example:

```js
// style.js
export default {
    container: {
        width: 640, // max width
        height: 960,  // max height
    },
}
```

To avoid this part of the data coupling, setting a percentage adaptation to the size is supported. Example:

```js
// style.js
export default {
    container: {
        width: '100%',
        height: '100%',
    },
}
```

In the actual rendering process, the `sharedCanvas` can be interpreted as content and the `SubContextView` node as a container, and the engine will adopt the adaptation strategy of `SHOW ALL` to render the `sharedCanvas` to the screen to avoid the UI distortion caused by stretching during rendering.

![](./build-open-data-context/adaption-1.png)

![](./build-open-data-context/adaption-2.png)

### Setting FPS

This property mainly sets the frequency of updating `sharedCanvas` to the screen by the primary project, to avoid frequent updating of the **Open Data Context** texture by the primary project, in order to avoid some performance loss.

## Some recommended practices

1. As previously mentioned, the next time an **Open Data Context Project** template is built, it will not be overwritten if the **openDataContext** directory already exists in the build directory. This prevents overwriting a project that the developer has customized. Since the build directory is ignored by **git** by default, if the **openDataContext** project needs to be included in version control consider putting the built project into the build-templates directory. Please refer to [Custom Project Build Process](./custom-project-build-template.md) documentation.

2. In an **Open Data Context Project**, if you need to listen to messages from the primary project, you need to first determine whether the message comes from the primary project engine, using the WeChat interface as an example.

```js
wx.onMessage(res => {
    if (!(res && res.type === 'engine')) {
        console.log('do something...');
    }
});
```

It is also recommended to send messages to **Open Data Context** with a type message, where `res.type === 'engine'` means the message came from the engine, to avoid handling the wrong source.

## Known issues

In the **Open Data Context** of **Baidu** platform, since the image can only load player avatars returned from Baidu, the local avatar image may not be loaded in the generated template project.

## Reference documentation

- [minigame-canvas-engine document](https://wechat-miniprogram.github.io/minigame-canvas-engine/)
- [minigame-canvas-engine repository](https://github.com/wechat-miniprogram/minigame-canvas-engine)
- [doT template engine](http://olado.github.io/doT/?spm=a2c6h.12873639.0.0.36f45227oKu0XO)
