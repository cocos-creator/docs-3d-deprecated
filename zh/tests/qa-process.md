# 测试组测试文档

## 编辑器运行平台

- Mac OS X
- Windows

## 测试构建平台

- 移动端和桌面端 Web 浏览器
  - iOS Safari
  - Android Chrome & QQ 浏览器
  - Mac OS X & Windows Chrome
- 微信小游戏平台
  - Android
  - iOS
- 支付宝小游戏平台
  - Android
  - iOS
- OPPO小游戏平台
  - OPPO手机

## 编辑器测试项
1. [命令行测试文档](./publish.md)

其他部分，参考内部编辑器测试文档

## 测试用例

**测试前请更新所有测试例**

1. [测试例](https://github.com/cocos-creator/test-cases-3d)
2. [范例集合](https://github.com/cocos-creator/example-3d)
3. [物理小车](https://github.com/cocos-creator/example-3d/tree/master/simple-car-game)
4. [下落球球](https://github.com/cocos-creator/example-3d/tree/master/falling-ball)
5. [滚动小球](https://github.com/cocos-creator/example-3d/tree/master/roll-a-ball)
6. [吞噬黑洞](https://github.com/cocos-creator/example-3d/tree/master/simple-hole)
7. [Simple-FPS](https://github.com/cocos-creator/example-3d/tree/master/simple-fps)
8. [一步两步](https://github.com/cocos-creator/tutorial-mind-your-step-3d)
9. [UI Demo](https://github.com/cocos-creator/demo-ui/tree/3d)
10. [Demo Ball](https://github.com/cocos-creator/demo-ball)

test-case 场景测试说明，可以参考 [文档](./test-case-docs.md)。

## 测试流程细节

Test Case 运行时需要分几种情况打包测试，以便覆盖各种使用场景：

- 测试组合一
  - 项目设置中选择物理模块为 Builtin
  - 渲染模块勾选 WebGL 2
  - 构建选项勾选 MD5 Cache（MD5 缓存）
  - 构建选项勾选 Merge all JSON that the Start Scene used（合并初始场景 JSON 资源）
  - 构建选项**不勾选** Debug（调试模式）
  - 构建选项勾选 CompressTexture（压缩纹理）
  - 构建选项勾选 packAutoAtlas（打包自动图集）
- 测试组合二
  - 项目设置中选择物理模块为 Cannon
  - 渲染模块**不勾选** WebGL 2
  - 构建选项**不勾选** MD5 Cache（MD5 缓存）
  - 构建选项**不勾选** Merge all JSON that the Start Scene used（合并初始场景 JSON 资源）
  - 构建选项勾选 Debug（调试模式）
  - 构建选项**不勾选** CompressTexture（压缩纹理）
  - 构建选项**不勾选** packAutoAtlas（打包自动图集）

### 其他平台测试项细节
1. [web 平台测试细则](./publish/web-build-docs.md)
2. [微信平台测试细则](./publish/wechat-build-docs.md)

### 粒子测试用例

 - [particle-color](./particle/particle-color.md)
 - [particle-force](./particle/particle-force.md)
 - [particle-limit-velocity](./particle/particle-limit-velocity.md)
 - [particle-main](./particle/particle-main.md)
 - [particle-renderer](./particle/particle-renderer.md)
 - [particle-rotation](./particle/particle-rotation.md)
 - [particle-shape](./particle/particle-shape.md)
 - [particle-size](./particle/particle-size.md)
 - [particle-texture-animation](./particle/particle-texture-animation.md)
 - [particle-trail](./particle/particle-trail.md)
 - [particle-velocity](./particle/particle-velocity.md)

## Issue 管理

1. 请将 issue 提交到 3d-tasks 仓库
2. 指派责任人
3. 设置 Milestone 为当前 Sprint
4. 添加 QA-open 标签
