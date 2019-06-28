
## 语言支持

### Javascript

Cocos3D 对脚本中 Javascript 语言的支持为 ECMAScript 2015。

### Typescript

Cocos3D 对脚本中 Typescript 语言的支持为 Typescript 3.5.0。

Typescript 脚本的编译效果就相当于使用了以下编译选项的 `tsc` 进行编译：
```json
{
    "compilerOptions": {
        "target": "es2015",
        "module": "es2015",
        "experimentalDecorators": true,
        "strict": false,
        "noImplicitAny": false,
    }
}
```

Typescript 脚本的编译完全由 Cocos3D 完成，
这意味着项目中 `tsconfig.json` 的*绝大多数*编译选项并不影响编译，
但以下选项将被 Cocos3D 读取并影响编译：
- `compilerOptions.baseUrl` 和 `tsc` 的语义相同，见 []()；
- `compilerOptions.paths` 和 `tsc` 的语义相同，见 []()。

你仍然可以在项目中使用 `tsconfig.json` 以配合 IDE 实现类型检查等功能。
为了使得 IDE 的 Typescript 检查功能 和 Cocos3D 行为兼容，
你需要额外注意一些事项，见 [tsconfig](./tsconfig.md)。