# 单元测试规范

## 框架说明

Cocos Creator 3D 引擎中的测试框架使用的是 [ts-jest](https://kulshekhar.github.io/ts-jest/) 测试引擎的 TS 模块，也允许直接编写 TS 测试用例。

ts-jest 的基础测试框架是 [Jest](https://jestjs.io/)，一个开箱即用的 JS 单元测试框架，由 Facebook 在维护。

由于引擎需要浏览器环境的 API 才可以正常运行，而单元测试框架运行在 Node 环境中，我们使用 Jest 提供的 [jsdom](https://github.com/jsdom/jsdom) 来模拟浏览器环境。同时 Jest 也支持通过 [Puppeteer](https://pptr.dev/) 让单元测试直接运行在 Chrome 内核浏览器中，这对于未来我们做自动化图形测试很有帮助。

除此之外，ts-jest 还会在测试前对引擎代码的 typescript 语法和模块依赖进行校验，出现问题都会导致测试无法通过。

## 目录和文件规范

所有的单元测试都应该放在引擎的 `tests/` 目录中，`tests/` 目录结构如下：

- init.ts：通用的初始化模块，会引入引擎的 `exports/base` 模块，如果有必要可以修改，不过记住这个脚本的内容在所有单元测试执行前都会执行。
- 其他模块目录：测试例应该按照模块来划分，模块名和引擎模块保持一致，在模块文件夹下按照功能或者类来放置所有测试脚本。

所有测试脚本的命名都应该遵循：`类名|子模块名.test.ts`，比如：

```
vec3.test.ts
misc.test.ts
```

如果需要对 Jest 配置进行修改，需要先提出需求和 Issue 供讨论，Jest 配置存放在引擎目录下的 `jest.config.js` 中。

## 测试用例编写

请参考 Jest API Doc 来编写单元测试用例，常用的 API 有：

- beforeEach(fn, timeout)：在当前文件所有测试用例之前执行一次
- describe(name, fn)：定义一系列相关联的测试
- describe.each(table)(name, fn, timeout)：用于提供一系列不同的入口参数给同一套测试用例
- test(name, fn, timeout)：一个单元测试，也可以使用 it 代替 test
- expect(value)：当你需要对某个值或结果进行测试的时候，都应该以 expect 开始。

具体 API 请参考：

- [Globals](https://jestjs.io/docs/en/api)
- [Expect](https://jestjs.io/docs/en/expect)

## 如何编写优秀的单元测试

1. 原则：单元测试的目标是测试每一个功能在特定输入下，输出结果和预期结果的一致性。
2. 单一性：一个单元测试只应该关注一个功能点，单元测试不是用来覆盖复杂复合用例，不能杂糅这样的目标到单元测试中。
3. 可维护性：尽可能简单，清晰，易懂，任何单元测试都不应该包含复杂的逻辑。
4. 边界测试：单元测试应该考虑一些不以达到的边界条件。
5. 测试隔离：除了系列测试之外，应该尽量保持测试例之间的逻辑和测试环境隔离，也就是说测试后有必要的话应该清理测试例对环境造成的污染。
6. 保持维护：这可能是单元测试中最重要的一条铁律，任何功能的修改和增删都会对单元测试造成影响，应该持续对单元测试进行更新和维护。
