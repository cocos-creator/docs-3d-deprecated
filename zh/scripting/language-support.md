
## 语言支持

### Javascript

Cocos Creator 3D 对脚本中 Javascript 语言的支持为 ECMAScript 2015。

### TypeScript

Creator 3D 使用 babel 而非 tsc（Microsoft TypeScript Compiler） 编译 TypeScript 脚本。
特别地，使用了 [@babel/plugin-transform-typescript](https://babeljs.io/docs/en/babel-plugin-transform-typescript) （版本 7.5.2）插件。基于此原因，TypeScript 的支持存在某些限制，以下列举出了一些重要的注意事项，
关于完整的说明，见 [@babel/plugin-transform-typescript](https://babeljs.io/docs/en/babel-plugin-transform-typescript)。

- `tsconfig.json` 不会被读取；
- 不支持 `const enum`、`export =` 和 `import =`；
- 仅含类型声明的命名空间应以修饰符 `declare` 来声明；
- 命名空间导出的变量必须声明为 `const` 而非 `var` 或 `let`；
- 同一命名空间的不同声明不会共享作用域，需要显式使用限定符。


[@babel/plugin-transform-typescript](https://babeljs.io/docs/en/babel-plugin-transform-typescript) 在编译时不会读取 `tsconfig.json`
意味着 `tsconfig.json` 的编译选项并不影响编译，
但存在例外。以下选项将被 Creator 3D 读取并影响编译：
- `compilerOptions.baseUrl` 和 `tsc` 的语义相同，见 []()；
- `compilerOptions.paths` 和 `tsc` 的语义相同，见 []()。

以下 `tsconfig.json` 选项是隐含的：

- `emitDecoratorMetadata: false`
- `esModuleInterop: true`
- `experimentalDecorators: true`
- `isolatedModules: true`
- `target: es2015`

你仍然可以在项目中使用 `tsconfig.json` 以配合 IDE 实现类型检查等功能。
为了使得 IDE 的 Typescript 检查功能 和 Creator 3D 行为兼容，
你需要额外注意一些事项，见 [tsconfig](./tsconfig.md)。