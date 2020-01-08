# 用户自定义构建模板测试细则

## 基础功能介绍

目前除了原生以外的平台，都做了自定义构建模板的支持，每个平台最基本的支持就是，在项目目录下新建 `build-templates` 文件夹，再新建平台对应名称的文件夹例如 `wechatgame` (平台名称具体可以参照后面的表格)，在对应的文件夹内放置任意文件，在构建后会直接拷贝到构建后的包内。

以上是最基本的支持，在这个基础上每个平台对构建模板的支持有差异。通常的分为两类：

- 一是在菜单上可以创建的构建模板，通常为 ejs ，用户使用该模板编写的内容构建会对其渲染，这份文件升级改动的几率比较小，是为了避免因为内部结构调整而导致用户经常的更新模板。测试生成 ejs 模板的使用是否有效，可以在 ejs 内加上 `console.log('test')` 等任意文字，打包后能正常输出到 index.html 或 game.js 内即可。

- 二是小游戏平台通常会带上一些配置 `json` ，例如微信的 `game.json`，这类 json 文件基础处理是会和构建原本出来的 `json` 做数据融合，其中用户模板内的 json 权重大于构建原本的 `json` 文件，想要测试此类功能，通常是选择一些构建面板上配置的最终写入到 `json` 文件内的字段比如屏幕方向，把值改为任意值后构建观察构建后的 json 是否为你设置的值。

以下为各个平台自定义构建模板支持差异点
平台 | 微信 | web mobile | web desktop | 小米 | 华为 | 即刻玩 | 百度 | OPPO | vivo | 支付宝 | 原生
---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | --- | --- | ---
平台实际名称 | wechatgame | web-mobile | web-desktop | xiaomi-mini-game | huawei-mini-game | cocos-play | baidu-mini-game | oppo-mini-game| vivo-mini-game | alipay-mini-game | native 
自定义构建模板| game.ejs, game.json, project.config.json | index.ejs | index.ejs | manifest.json | 使用面板上的 | game.config.json | game.json, project.swan.json | manifest.json | project.config.json | game.json | X

> Note： 自定义构建模板默认全平台支持基础的拷贝流程，上图标识的构建有处理的部分，即会渲染会整合的部分

以下描述的测试流程，在测试过一遍之后会有很多现成的构建模板，这份模板可以配置一次后可以在之后的测试流程里面循环利用

### 1. web 两个平台

基础测试流程:
1. 在菜单上点击 `项目-新建构建模板-web 平台`，生成对应的 index.ejs 
2. 修改该 ejs 文件的任意内容
3. 在对应的平台文件夹内添加任意文件，点击构建

预期结果：正常构建，添加的任意文件有被拷贝到生成目录内，ejs 修改的内容在最终的 index.html 内可以找到

### 2. 微信平台

基础测试流程：
1. 在菜单上点击 `项目-新建构建模板-微信平台`，生成对应的 game.ejs 
2. 修改该 ejs 文件的任意内容
3. 在对应的平台文件夹内添加任意文件
4. 在模板文件夹内添加 `project.config.json` 文件，内容为
```json
{
    "projectname": "wechat-user-template"
}
```
5. 在模板文件夹内添加 `game.json` 文件，内容为
```json
{
    "deviceOrientation":"portrait"
}

```
6. 构建配置设置为横屏后，点击构建

预期结果：
- 正常构建
- 添加的任意文件有被拷贝到生成目录内
- ejs 修改的内容在最终的 game.js 内可以找到
- json 内修改的内容有生效（构建结果可以正常预览屏幕方向为竖屏，可不真机测试）

### 3. 支付宝
基础测试流程：
1. 在对应的平台文件夹内添加任意文件，点击构建
2. 在模板文件夹内添加 `game.json` 文件，内容为
```json
{
    "screenOrientation":"portrait"
}

```
3. 构建配置设置为横屏

预期结果：
- 正常构建
- 添加的任意文件有被拷贝到生成目录内
- json 内修改的内容有生效（构建结果可以正常预览同时屏幕方向时竖屏，可不真机测试）

### 4. 百度小游戏
基础测试流程：
1. 在对应的平台文件夹内添加任意文件，点击构建
2. 在模板文件夹内添加 `game.json` 文件，内容为
```json
{
    "deviceOrientation":"portrait"
}
```
3. 在模板文件夹内添加 `project.swan.json` 文件，内容为
```json
{
    "projectname": "baidu-user-template"
}
```
4. 构建配置设置为横屏

预期结果：
- 正常构建
- 添加的任意文件有被拷贝到生成目录内
- json 内修改的内容有生效（构建结果可以正常预览，项目名称变为修改的名字，屏幕方向为竖屏，可不真机测试）

### 5. 即刻玩

cocos-play

基础测试流程：
1. 在对应的平台文件夹内添加任意文件，点击构建
2. 在模板文件夹内添加 `game.config.json` 文件，内容为
```json
{
    "deviceOrientation":"portrait"
}
```
4. 构建配置设置为横屏

预期结果：
- 正常构建
- 添加的任意文件有被拷贝到生成目录内
- json 内修改的内容有生效（构建结果可以正常预览，屏幕方向为竖屏，可不真机测试）

### 6. 小米 | OPPO

xiaomi-mini-game | oppo-mini-game
基础测试流程：
1. 在对应的平台文件夹内添加任意文件，点击构建
2. 在模板文件夹内添加 `manifest.json` 文件，内容为
```json
{
    "orientation":"portrait"
}
```
4. 构建配置设置为横屏

预期结果：
- 正常构建，将 .rpk 文件改为 .rar 解压缩查看内部文件
- 添加的任意文件有被拷贝到 rpk 目录内
- json 内修改的内容有生效（构建结果可以正常预览，屏幕方向为竖屏，可不真机测试）

### 7. vivo 平台
xiaomi-mini-game | oppo-mini-game
基础测试流程：
1. 在对应的平台文件夹内添加任意文件，点击构建
2. 在模板文件夹内添加 `src/manifest.json` 文件，内容为
```json
{
    "orientation":"portrait"
}
```
4. 构建配置设置为横屏

预期结果：
- 正常构建，将 .rpk 文件改为 .rar 解压缩查看内部文件
- 添加的任意文件有被拷贝到 rpk 目录内
- rpk 内 src/manifest.json 路径下 json 内修改的内容有生效（构建结果可以正常预览，屏幕方向为竖屏，可不真机测试）
