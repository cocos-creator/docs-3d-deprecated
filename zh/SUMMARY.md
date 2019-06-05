# Summary

## 新手入门
[介绍](introduction.md)
- [安装和启动](getting-started\install.md)
- [快速上手：制作第一个游戏]()

## 基础概念
- [场景](workflow/scene/index.md)
  - [场景结构](workflow/scene/scene.md)
  - [坐标系和变换](workflow/scene/transform.md)
  - [节点的层级](workflow/scene/node-tree.md)
  - [摄像机](workflow/scene/camera.md)
  - [光源](workflow/scene/light.md)

## 编辑器手册
- [快速入门](workflow/scene/index.md) 
  - [导航窗口]
  - [创建新场景]
  - [在场景中放置组件]
  - [灯光等设置]
  - [构建与运行]
- [场景](workflow/scene/index.md)
  - [环境光]
  - [阴影]
  - [天空盒]
- [节点]
- [组件](workflow/scene/index.md)
  - [AudioSourceComponent]
  - [ModelComponent]
  - [SkinningModelComponent]
  - [CameraComponent]
  - [DirectionalLightComponent]
  - [SphereLightComponent]
  - [SpotLightComponent]
  - [AnimationComponent]
  - [BillboardComponent]
  - [LineComponent]
  - [ParticleSystemComponent]
  - [BoxColliderComponent]
  - [RigidBodyComponent]
  - [UI Components](ui-system/components/index.md) @刘雅琼
    - [渲染组件参考](ui-system/components/index.md)
      - [SpriteComponent 参考](ui-system/components/sprite.md)
      - [LabelComponent 参考](ui-system/components/label.md)
      - [MaskComponent 参考](ui-system/components/mask.md)
    - [CanvasComponent 参考](https://docs.cocos.com/creator/2.1/manual/zh/components/canvas.html)
    - [WidgetComponent 参考](https://docs.cocos.com/creator/2.1/manual/zh/components/widget.html)
    - [ButtonComponent 参考](https://docs.cocos.com/creator/2.1/manual/zh/components/button.html)
    - [LayoutComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/layout.html)
    - [EditBoxComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/editbox.html)
    - [ScrollViewComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/scrollview.html)
    - [ScrollBarComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/scrollbar.html)
    - [ProgressBarComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/progress.html)
    - [ToggleComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/toggle.html)
    - [ToggleGroupComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/toggleContainer.html)
    - [SliderComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/slider.html)
- [创建](workflow/scene/index.md)
  - [3D对象]
  - [特效]
  - [光源]
  - [摄像机]

## -------------------------------

## 新手入门
[介绍](introduction.md) @王伟亮
- [新手上路](helloworld.md)
    - [关于 Cocos 3D]()
    - [安装和启动](getting-started\install.md)
    - [使用 Dashboard](getting-started\dashboard.md)
    - [第一个项目：Hello World](getting-started\helloworld.md)
    - [编辑器介绍]()
    - [配置代码编辑环境]()
    - [快速上手：制作第一个游戏]()
    - [获取帮助和支持]()
    - [FAQ]()

## 工作流
- [场景工作流](workflow/scene/index.md) @王伟亮
  - [场景结构](workflow/scene/scene.md)
  - [坐标系和变换](workflow/scene/transform.md)
  - [节点的层级](workflow/scene/node-tree.md)
  - [摄像机](workflow/scene/camera.md)
  - [光源](workflow/scene/light.md)
- [资源工作流](workflow/resources/index.md) @武云潇
  - [UI 资源](workflow/resources/ui/index.md) @刘雅琼
    - [SpriteFrame 资源](workflow/resources/ui/spriteFrame/spriteFrame.md)
    - [图集资源](https://docs.cocos.com/creator/2.1/manual/zh/asset-workflow/atlas.html)
    - [字体资源](https://docs.cocos.com/creator/2.1/manual/zh/asset-workflow/font.html)
- [UI 工作流](workflow/ui/index.md)
- [脚本开发指南]() @朱旭光
- [发布跨平台游戏]() @王伟亮

## 子系统
- [材质系统](material-system/overview.md) @武云潇
- [粒子系统](particle-system/overview.md) @朱旭光
- [动画系统]() @李楠
- [物理系统](physics/physics.md) @赖嘉鑫
- [UI 系统](https://docs.cocos.com/creator/2.1/manual/zh/ui/) @刘雅琼
  - [多分辨率适配方案](https://docs.cocos.com/creator/2.1/manual/zh/ui/multi-resolution.html)
  - [对齐策略](https://docs.cocos.com/creator/2.1/manual/zh/ui/widget-align.html)
  - [文字排版](https://docs.cocos.com/creator/2.1/manual/zh/ui/label-layout.html)
  - [自动布局容器](https://docs.cocos.com/creator/2.1/manual/zh/ui/auto-layout.html)
  - [制作动态生成内容的列表](https://docs.cocos.com/creator/2.1/manual/zh/ui/list-with-data.html)
  - [制作可任意拉伸的 UI 图像](https://docs.cocos.com/creator/2.1/manual/zh/ui/sliced-sprite.html)
  - [UI 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/ui/ui-components.html)
    - [渲染组件参考](ui-system/components/index.md)
        - [SpriteComponent 参考](ui-system/components/sprite.md)
        - [LabelComponent 参考](ui-system/components/label.md)
        - [MaskComponent 参考](ui-system/components/mask.md)
    - [CanvasComponent 参考](https://docs.cocos.com/creator/2.1/manual/zh/components/canvas.html)
    - [WidgetComponent 参考](https://docs.cocos.com/creator/2.1/manual/zh/components/widget.html)
    - [ButtonComponent 参考](https://docs.cocos.com/creator/2.1/manual/zh/components/button.html)
    - [LayoutComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/layout.html)
    - [EditBoxComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/editbox.html)
    - [ScrollViewComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/scrollview.html)
    - [ScrollBarComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/scrollbar.html)
    - [ProgressBarComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/progress.html)
    - [ToggleComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/toggle.html)
    - [ToggleGroupComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/toggleContainer.html)
    - [SliderComponent 组件参考](https://docs.cocos.com/creator/2.1/manual/zh/components/slider.html)



## CC_HIDE_IN_SUMMARY_START

- [使用Dashboard](getting-started/dashboard.md)
- [Hello World项目](getting-started/helloworld.md)
- [安装和启动](getting-started/install.md)
- [关于 Cocos 3D](getting-started/introduction.md)
- [Effect 语法](material-system/effect-syntax.md)
- [Pass 可配置参数](material-system/pass-parameter-list.md)
- [YAML 101](material-system/yaml-101.md)
- [颜色模块(ColorOvertimeModule)](particle-system/color-module.md)
- [发射器模块(ShapeModule)](particle-system/emitter.md)
- [加速度模块(ForceOvertimeModule)](particle-system/force-module.md)
- [限速模块(LimitVelocityOvertimeModule)](particle-system/limit-velocity-module.md)
- [主模块(ParticleSystemComponent)](particle-system/main-module.md)
- [粒子系统模块](particle-system/module.md)
- [Particle Renderer](particle-system/renderer.md)
- [旋转模块(RotationOvertimeModule)](particle-system/rotation-module.md)
- [大小模块(SizeOvertimeModule)](particle-system/size-module.md)
- [贴图动画模块(TextureAnimationModule)](particle-system/texture-animation-module.md)
- [速度模块(VelocityOvertimeModule)](particle-system/velocity-module.md)

## CC_HIDE_IN_SUMMARY_END
