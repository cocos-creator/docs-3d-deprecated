# 废弃 API

## 框架说明

为了更友好和便利的维护废弃 API ，将通过三个函数来实现：

- `markAsWarning` 对给予对象上的属性中嵌入一个警告，给予对象需要存在该属性。

- `removeProperty` 重新定义给予对象上移除的属性，并嵌入一个报错，给予对象应不存在该属性。

- `replaceProperty` 重新定义给予对象上移除的属性，并嵌入一个警告和调用新的属性，参数不兼容的需要自己通过 custom 参数进行适配，给予对象应不存在该属性。

## 函数签名

```
interface IDeprecatedItem {
    name: string;           // 废弃属性的名称
    logTimes?: number;      // 信息输出的次数，默认将使用全局的默认次数
}

interface IReplacement extends IDeprecatedItem {
    newName?: string;       // 替换属性的名称，默认将使用 name 的值
    target?: object;        // 替换对象，默认将使用 owner 的值
    targetName?: string;    // 替换对象的名称，默认将使用 ownerName 的值
    custom?: Function;      // 自定义的函数封装，错误消息会自动输出一份默认的
}

interface IRemoveItem extends IDeprecatedItem {
}

interface IMarkItem extends IDeprecatedItem {
}

export let replaceProperty: (owner: object, ownerName: string, properties: IReplacement[]) => void;

export let removeProperty: (owner: object, ownerName: string, properties: IRemoveItem[]) => void;

export let markAsWarning: (owner: object, ownerName: string, properties: IMarkItem[]) => void;

// 此函数用于设置全局默认的信息输出次数，没有填 logTimes 将会使用这个值
export function setDefaultLogTimes (times: number): void;  
```

## 使用规范

按照模块划分，每个模块维护一份废弃文件，为了便于维护命名统一为 deprecated.ts ，并且放在相应模块的目录下，并需要在相应的模块的 index.ts 文件中 import 该文件，例如 import './deprecated'。

**注：cocos 目录下的 deprecated.ts 文件为声明和实现文件**。

## 使用示例

```
// 对于替换参数不兼容的API，使用自定义的方式
replaceProperty(AnimationComponent.prototype, 'AnimationComponent', [
    {
        name: 'removeClip',
        newName: 'removeState',
        custom: function (...args: any) {
            let arg0 = args[0] as AnimationClip;
            return AnimationComponent.prototype.removeState.call(this, arg0.name);
        }
    }
]);

replaceProperty(vmath, 'vmath', [
    {
        name: 'vec2',
        newName: 'Vec2',
        target: math,
        targetName: 'math',
        'logTimes': 1
    },
    {
        name: 'EPSILON',
        target: math,
        targetName: 'math',
        'logTimes': 2
    }
]);

removeProperty(vmath, 'vmath', [
    {
        'name': 'random'
    }
]);

markAsWarning(math, 'math', [
    {
        'name': 'toRadian'
    }
]);
```

## 使用注意

- 操作目标都是对象，如果想要修改类的成员函数，owenr 和 target 都应该为 **.prototype**。

- replaceProperty 不输入 newName 或 newTarget ，表示和 name 或 target 一致。

- 如果要控制次数，最好在使用之前调用 setDefaultLogTimes ，因为别的模块可能会把默认次数改了。
