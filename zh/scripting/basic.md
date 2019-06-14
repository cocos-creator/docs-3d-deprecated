
## 语言支持

### Javascript

Cocos3D 对脚本中 Javascript 语言的支持为 ECMAScript 2015。

### Typescript

Cocos3D 对脚本中 Typescript 语言的支持为 Typescript 3.5.0。

一般情况下，标准的 Typescript 脚本由 `tsc` 编译，
`tsc` 提供了配置文件（一般名为`tsconfig.json`）以允许用户控制 `tsc` 的编译过程。
但在 Cocos3D 中，Typescript 脚本的编译完全由 Cocos3D 完成。
然而，Cocos3D 仍允许用户通过配置 `tsc` 配置文件的方式改变 Cocos3D 的编译行为，
以使得用户可以使用 Typescript 特有的某些功能，如模块解析等。

---

Cocos3D 在编译 Typescript 时就如同使用了以下 `tsc` 编译选项：
```json
{
    "compilerOptions": {
        // 必须保持以下属性使得 tsc 的检查行为和 Cocos3D 的编译行为一致
        "target": "es2015",
        "module": "es2015",
        // 可以自由地改变以下属性：
        "experimentalDecorators": true,
        "strict": false,
        "noImplicitAny": false,
    }
}
```

除 `compilerOptions.target` 和 `compilerOptions.module`外，
你仍然可以改变上述的其它选项以实现（在 IDE 中）类型检查等功能，
但这些将不会影响 Cocos3D 对 Typescript 的编译结果。
例如，当你希望禁止你项目中所有 Typscript 脚本对隐式 `any` 的使用，
你就可以在 `tsconfig.json` 中将 `compilerOptions.noImplicitAny` 设为 `true`，
如此当你用 Visual Studio Code 等 IDE 检查该文件时就会收到相应的错误提示。

然而，请勿修改和设置 `compilerOptions.target` 和 `compilerOptions.module`，
例如，若将 `tsconfig.json` 设置为：
```json
{
    "compilerOptions": {
        "target": "es5",
        "module": "cjs"
    }
}
```
那么脚本代码：

```ts
const myModule = require("path-to-module");
```
对于 `tsc` 来说是合法的，因为`compilerOptions.module` 设置为了 `cjs`。
然而 Cocos3D 隐含的 `compilerOptions.module` 是 `es2015`，
因此在运行时它可能提示 "require 未定义" 等错误。

脚本代码：

```ts
const set = new Set();
```
对于 Cocos3D 来说是合法的，但对于 `tsc` 来说是非法的，
因为`compilerOptions.target` 设置为了 `es5`：ECMAScript 2015 才引入 `Set`。

------

以下罗列了支持的 `tsc` 编译选项，
这些选项将影响 Cocos3D 对 Typescript 的编译：
- `compilerOptions.baseUrl`
- `compilerOptions.paths`

其余 `tsc` 选项你可以自由更改以实现类型检查、`.d.ts`文件关联等功能。

## 运行环境

Cocos3D 引擎的 API 都存在于模块 `Cocos3D`中，
使用标准的 ES6 模块导入语法将其导入：

```
import { Component, _decorator } from "Cocos3D";
import * as cocos3d from "Cocos3D";

@_decoarator.ccclass("MyComponent")
export class MyComponent extends Component {
    public v = new cocos3d.Vec3();
}
```

