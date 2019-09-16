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

参考内部编辑器测试文档

## 测试用例

1. [测试例](https://github.com/cocos-creator/test-cases-3d)
2. [范例集合](https://github.com/cocos-creator/example-3)
3. [一步两步](https://github.com/cocos-creator/tutorial-mind-your-step-3d)
4. [UI Demo](https://github.com/cocos-creator/demo-ui/tree/3d)

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

## Issue 管理

1. 请将 issue 提交到 3d-tasks 仓库
2. 指派责任人
3. 设置 Milestone 为当前 Sprint
4. 添加 QA-open 标签
