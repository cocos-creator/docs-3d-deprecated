

## 运行环境

Cocos Creator 3D 引擎的 API 都存在于模块 `Cocos3D`中，
使用标准的 ES6 模块导入语法将其导入：

```ts
import {
    Component, // 导入类 Component
    _decorator, // 导入命名空间 _decorator
} from "Cocos3D";
import * as cocos3d from "Cocos3D"; // 将整个 Cocos3D 模块导入为命名空间 Cocos Creator 3D

@_decorator.ccclass("MyComponent")
export class MyComponent extends Component {
    public v = new cocos3d.Vec3();
}
```

## 保留标识符 cc

注意，由于历史原因，`cc` 是 Cocos Creator 3D 保留使用的标识符，
其行为*相当于*在任何模块顶部已经定义了名为 cc 的对象。
因此，你不应该将 `cc` 用作任何**全局**对象的名称：

```ts
/* const cc = {}; // 每个 Cocos3D 脚本都等价于在此处含有隐式定义 */

import * as cc from "Cocos3D"; // 错误：命名空间导入名称 cc 由 Cocos3D 保留使用

const cc = { x: 0 };
console.log(cc.x); // 错误：全局对象名称 cc 由 Cocos3D 保留使用

function f () {
    const cc = { x: 0 };
    console.log(cc.x); // 正确：cc 可以用作局部对象的名称

    const o = { cc: 0 };
    console.log(o.cc); // 正确：cc 可以用作属性名
}

console.log(cc, typeof cc); // 错误：行为是未定义的
```