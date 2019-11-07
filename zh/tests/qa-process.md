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

## 编辑器测试项
1. [命令行测试文档](./publish.md)

其他部分，参考内部编辑器测试文档

## 测试用例

1. [测试例](https://github.com/cocos-creator/test-cases-3d)
2. [范例集合](https://github.com/cocos-creator/example-3d)
3. [一步两步](https://github.com/cocos-creator/tutorial-mind-your-step-3d)
4. [UI Demo](https://github.com/cocos-creator/demo-ui/tree/3d)
5. [Demo Ball](https://github.com/cocos-creator/demo-ball)

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

### example-3d wx-open-data-project 测试说明

Cocos Creator 3D 接 Cocos Creator 2D 子域工程，需要打包微信平台使用。

目前工程内微信平台打包目录下自带微信子域打包工程 wx-open-data-project，在当前微信构建目录下。如果遇到误删除或者缺失，可从 creator 开放数据域 demo https://github.com/cocos-creator/demo-wechat-subdomain 处获得子域工程 wx-open-data-project，选择 ***2.1.x*** 及以上版本，打包构建到该工程相应平台的建构目录下，名字必须要和该工程打包配置里的微信开放水域的名字一致方可使用。

在执行完上述操作，如果因为项目内容修改需要对该工程进行重新打包发现子域工程路径不变但是原来打包平台路径下的子域文件夹丢失，向编辑器组反馈修复。

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
