

引擎和编辑器通过模块向开发者暴露功能接口，模块以 ECMAScript 模块形式存在。

⚠️ 注意，从 3.0 开始，将不能通过全局变量 `cc` 访问引擎功能！

## 引擎模块

目前，引擎只提供了一个公开模块 `'cc'`。

模块 `'cc'` 的内容是动态的，其内容和项目设置中的引擎模块设置有关。

### 示例：引擎日志输出

```ts
import { log } from 'cc';
log(`Hello world!`);
```

## 编辑器模块

编辑器模块都以 `'cce.'` 作为前缀（“cce”代表“**C**ocos**C**reator**E**ditor”）。

除模块 `'cce.env'` 外，所有编辑器模块仅在编辑器环境下有效。例如，预览和构建后的环境中是不能访问编辑器模块的，相反，场景编辑器中则可以访问到。

<table>
    <tr>
        <td>模块名称</td>
        <td>用于</td>
    </tr>
    <tr>
        <td><code>'cce.env'</code></td>
        <td>访问构建时常量。</td>
    </tr>
</table>

### 构建时常量

编辑器模块 `'cce.env'` 暴露了一些构建时的**常量**，这些常量代表执行环境、调试级别或平台标识等。不像其它编辑器模块，`'cce.env'` 允许在非编辑器环境中访问。

由于这些常量都以 `const` 声明，提供了很好的代码优化机会。

#### 执行环境

<table>
    <tr><td>名称（类型都为 <code>boolean</code>）</td> <td>含义</td></tr>
    <tr><td><code>BUILD</code></td> <td>是否正处于构建后的运行中</td></tr>
    <tr><td><code>PREVIEW</code></td> <td>是否正在预览中运行</td></tr>
    <tr><td><code>EDITOR</code></td> <td>是否正在编辑器中运行</td></tr>
</table>

#### 调试级别

<table>
    <tr><td>名称（类型都为 <code>boolean</code>）</td> <td>含义</td></tr>
    <tr><td><code>DEBUG</code></td> <td>是否处于调试模式<br/>仅当构建时未勾选调试选项的情况下为 <code>false</code>；
    <br/>其它情况下都为 <code>true</code>
    </td></tr>
    <tr><td><code>DEV</code></td> <td>等价于<code> DEBUG || EDITOR || PREVIEW || EDITOR</code></tr>
</table>

#### 平台标识

下表列出的常量代表是否正在**某个**或**某一类**平台上运行，类型都为 `boolean`。
<!-- 下表请按字典序排序 -->
<table>
    <tr>
    <td>名称</td>
    <td>代表平台</td>
    <td><code>MINIGAME<br/></code><br/>“小游戏”</td>
    <td><code>RUNTIME_BASED</code><br/>基于 Cocos Runtime</td>
    <td><code>SUPPORT_JIT</code><br/>支持 JIT</td>
    </tr>
    <!---->
    <tr>
    <td><code>HTML5</code></td>
    <td>Web</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>❌</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>NATIVE</code></td>
    <td>原生平台</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>❌</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>ALIPAY</code></td>
    <td>支付宝小游戏</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>BAIDU</code></td>
    <td>百度小游戏</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>BYTEDANCE</code></td>
    <td>抖音小游戏</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <td><code>WECHAT</code></td>
    <td>微信小游戏</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>XIAOMI</code></td>
    <td>小米小游戏</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr><td><code>COCOSPLAY</code></td>
    <td>即刻玩</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>HUAWEI</code></td>
    <td>华为快游戏</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>OPPO</code></td>
    <td>OPPO 快游戏</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>VIVO</code></td>
    <td>vivo 快游戏</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
</table>

#### 示例：调试模式下的输出

```ts
import { log } from 'cc';
import { DEV } from 'cce.env';

if (DEV) {
    log(`I'm in development mode!`);
}
```