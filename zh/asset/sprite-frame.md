# 精灵帧资源（SpriteFrame）

Cocos Creator 3D SpriteFrame 是 UI 渲染基础图形的容器，其中包含 Texture2D 资源或 RenderTexture 资源。在资源面板里，默认管理一个 Texture2D 资源。

## 导入精灵帧资源

使用默认的资源导入方式就可以将图像资源导入到项目中，之后我们就可以在 **资源管理器** 中看到如下图所示的图像资源。

![imported texture](sprite-frame/imported_texture.png)

图像资源在 **资源管理器** 中会以自身图片的缩略图作为图标。在 **资源管理器** 中选中图像子资源后，**属性检查器** 下方会显示该图片的缩略图。

## 使用 SpriteFrame

** 1. 容器内包含对象是贴图的使用方式 **

在编辑器中，拖拽 SpriteFrame 资源到该 **Sprite** 组件的 `Sprite Frame` 属性栏中，来切换该 Sprite 显示的图像。在运行时，以上图中的 gold 图片为例，整个 SpriteFrame 资源氛围 `content`（图像源资源 ImageAsset）与其子资源 `spriteFrame`（精灵帧资源 SpriteFrame）。在游戏包内（也就是已经放在 resources 目录下）的资源可以通过

方法一（加载 ImageAsset）：
```typescript
const self = this;
const url = 'test_assets/test_altas/content';
cc.loader.loadRes(url, (err, imageAsset) => {
  const sprite = this.getComponent(SpriteComponent);
  const spriteFrame = new SpriteFrame();
  (spriteFrame.texture as Texture2D).image = imageAsset;
  sprite.spriteFrame = spriteFrame;
});
```

方法二（加载 SpriteFrame）：
```typescript
const self = this;
const url = 'test_assets/test_altas/content/spriteFrame';
cc.loader.loadRes(url, (err, spriteFrame) => {
  const sprite = this.getComponent(SpriteComponent);
  sprite.spriteFrame = spriteFrame;
});
```

在服务器上的资源只能加载到图像源 ImageAsset，具体方法请参考: [资源加载](./load-assets.md)。

** 2. 容器内包含对象是 RenderTexture 的使用方式 **

RenderTexture 是一个渲染纹理，它可以将摄像机上的内容直接渲染到一张纹理上而不是屏幕上。SpriteFrame 通过管理 RenderTexture 可以轻松的将 3D 相机内容显示在 UI 上。使用方法如下：

```typescript
const cameraComp = this.getComponent(CameraComponent);
const renderTexture = new RenderTexture();
rendetTex.reset({
   width: 512,
   height: 512,
   colorFormat: RenderTexture.PixelFormat.RGBA8888,
   depthStencilFormat: RenderTexture.DepthStencilFormat.DEPTH_24_STENCIL_8
});

cameraComp.targetTexture = renderTexture;
const spriteFrame = new SpriteFrame();
spriteFrame.texture = renderTexture;
const sprite = this.getComponent(SpriteComponent);
sprite.spriteFrame = spriteFrame;
```

<!-- API 接口文档如下：
* [SpriteFrame 资源类型](https://docs.cocos.com/creator/2.1/api/zh/classes/SpriteFrame.html) -->
