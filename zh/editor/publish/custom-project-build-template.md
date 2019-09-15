# 定制项目构建流程

## 自定义发布模版

Cocos Creator 3D 支持对每个项目分别定制发布模板，只需要在项目路径下添加一个 `build-templates` 目录，里面按照平台路径划分子目录，然后里面的所有文件在构建结束后都会自动按照对应的目录结构复制到构建出的工程里。

结构类似：

```
project-folder
 |--assets
 |--build
 |--build-templates
      |--web-mobile
            |--index.html
```

这样如果当前构建的平台是 `web-mobile` 的话，那么 `build-templates/web-mobile/index.html` 就会在构建后被拷贝到对应构建任务文件夹下。

由于构建出来包的内容是不能保证每个版本完全一样，按照之前的使用方式，每次构建内模板修改都会需要更新自己的构建模板，因而现在加入新的模板使用方式。可以点击菜单里的 `项目 -> 创建构建模板`选择对应平台后将会生成对应平台支持的模板文件。
例如：
```
project-folder
 |--assets
 |--build
 |--build-templates
      |--web-mobile
            |--index.ejs
```
构建会对这些模板做参数注入，构建经常改动的东西都会放在该模板引用的子模板内，使用时只需要修改添加需要的内容即可，这样之后模板更新自己的模板可以不用频繁跟进。

> 需要**注意**的是，拷贝用户模板是发生在渲染对应支持模板之后的，也就是假如该目录下同时存在 index.ejs 与 index.html, 最终打包出来的是 index.html 的文件而不是 index.ejs 渲染出来的文件。