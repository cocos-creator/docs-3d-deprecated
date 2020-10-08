# Language Support

## JavaScript

### Language Features

The **JavaScript** language specification supported by **Cocos Creator 3D** is **ES6**.

In addition, the following language features or proposals updated in the **ES6** specification are still supported:

- [Class Field](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Class_elements)
- [Promise Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [Optional chaining operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
- [Null merge operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
- [Global Objects globalThis](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis)

The following language features are also supported, but the relevant compilation options need to be turned on:

- [Asynchronous Function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

In particular, **Cocos Creator 3D** currently supports the __Legacy__ decorator proposal. For its usage and meaning, please refer to [babel-plugin-proposal-decorators](https://babeljs.io/docs/en/babel-plugin-proposal-decorators). Since the [proposal](https://github.com/tc39/proposal-decorators) is still in Phase 2 all decorator-related functional interfaces exposed by the engine are in [`_decorator` namespace](https://github.com/cocos-creator/engine/blob/3d-v1.2/cocos/core/data/class-decorator.ts#L28).

### Operating environment

From the user's point of view, __Cocos Creator 3D__ is not bound to any __JavaScript__ implementation. Therefore, it is recommended that developers write scripts strictly in accordance with the __JavaScript__ specification to obtain better cross-platform support.

For example, when *global objects* are needed, use the standard feature `globalThis`. Example:

```js
globalThis.blahBlah // `globalThis` must exist in any environment
```

Instead of `window`, `global`, `self`, or `this`:

```js
typeof window // may be'undefined'
typeof global // may be'undefined' in the browser environment
```

__Cocos Creator 3D__ does not provide a __CommonJS__ module system. This means that code snippets, such as these, will cause problems:

```js
const blah = require('./blah-blah'); // Error: `require` is undefined
module.exports = blah; // Error: `module` is undefined
```

Instead, use the standard module syntax:

```js
import blah from './blah-blah';
export default blah;
```

## TypeScript

__Cocos Creator 3D__ uses [babel](https://babeljs.io/) instead of [tsc](https://www.typescriptlang.org/) to compile __TypeScript__ scripts. In particular, the [@babel/plugin-transform-typescript](https://babeljs.io/docs/en/babel-plugin-transform-typescript) plugin is used. For this reason, TypeScript support has certain limitations. 

Some important considerations are listed below.
For a complete description, see [@babel/plugin-transform-typescript](https://babeljs.io/docs/en/babel-plugin-transform-typescript).

- `tsconfig.json` will not be read.
- Implied with `isolatedModules` option, which means:
  - [Const enums](https://www.typescriptlang.org/docs/handbook/enums.html#const-enums) is not supported.
  - TypeScript types and interfaces should not be exported in the export declaration.
- `Export =` and `import =` are not supported.
- Variables exported by namespace must be declared as `const` instead of `var` or `let`.
- Different declarations in the same namespace will not share the scope and require explicit use of qualifiers.

`tsconfig.json` will not be read during compilation means that the compilation options in `tsconfig.json` does not affect compilation.
However, there are exceptions, please refer to the [Module Analysis](####ModuleAnalysis) documentation.

Developers can still use `tsconfig.json` in the project to cooperate with the IDE to implement type checking and other functions.
In-order to make the IDE’s TypeScript checking function compatible with __Cocos Creator 3D__'s behavior, pay extra attention to some things, please referto the [tsconfig](./tsconfig.md) documentation.

#### Module Analysis

__Cocos Creator 3D__ uses the __NodeJS__ module analysis algorithm.
It is equivalent to the following `tsconfig.json`:

```json
{"compilerOptions": {
    "moduleResolution": "node"
}}
```

Typescript's path mapping function is also supported. The following `tsconfig.json` options will be read and retain the same semantics as `tsc`:

  - [compilerOptions.baseUrl](https://www.typescriptlang.org/docs/handbook/module-resolution.html#base-url)
  - [compilerOptions.paths](https://www.typescriptlang.org/docs/handbook/module-resolution.html#path-mapping)