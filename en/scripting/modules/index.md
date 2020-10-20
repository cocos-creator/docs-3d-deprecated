

Engine and editor expose their functionalities API through modules. Modules are in form of ECMAScript module format.

⚠️ Note, starting from 3.0, you can not access API through global variable `cc`!

## Engine modules

At present, engine only offers one public module `'cc'`.

Contents of module `'cc'` is dynamically decided,
which is relevant with engine modules setting in project settings.

### Example: engine logging

```ts
import { log } from 'cc';
log(`Hello world!`);
```

## Editor modules

Editor modules are prefixed by `'cce.'`("cce" is abbreviation of "**C**ocos**C**reator**E**ditor").

Except for module `cce.env`, all modules are visible only under editor environments. For example, they are not visible from previewing and after building. Instead, they are visible from scene editor.

<table>
    <tr>
        <td>Module name</td>
        <td>Use for</td>
    </tr>
    <tr>
        <td><code>cce.env</code></td>
        <td>Accessing build-time constants</td>
    </tr>
</table>

### Build-time constants

Editor module `'cce.env'` exposes some **constants** that are came from building environment. These constants may present execution environment, debugging level, platform identification and so on. Unlike other editor modules, `'cce.env'` is visible from non-editor environments.

Since these constants are declared with `const` qualifier, it's very friendly to code optimization.

#### Execution environment

<table>
    <tr><td>Name(all in type of <code>boolean</code>)</td> <td>Meaning</td></tr>
    <tr><td><code>BUILD</code></td> <td>Is executing after building</td></tr>
    <tr><td><code>PREVIEW</code></td> <td>Is executing during previewing</td></tr>
    <tr><td><code>EDITOR</code></td> <td>Is executing in editor environment</td></tr>
</table>

#### Debugging level

<table>
    <tr><td>Name(all in type of <code>boolean</code>)</td> <td>Meaning</td></tr>
    <tr><td><code>DEBUG</code></td> <td>Is under debug mode.<br/><code>false</code> if debug option is set when do building,<code>true</code> otherwise.
    </td></tr>
    <tr><td><code>DEV</code></td> <td>Equivalent to<code> DEBUG || EDITOR || PREVIEW || EDITOR</code></tr>
</table>

#### Platform identification

The following constants represent if is executing on some platform or some kind of platforms. All of these constants have type `boolean`.
<!-- Please sort the table in dictionary order -->
<table>
    <tr>
    <td>Name</td>
    <td>Platform</td>
    <td><code>MINIGAME<br/></code><br/>"mini game"</td>
    <td><code>RUNTIME_BASED</code><br/>based on Cocos Runtime</td>
    <td><code>SUPPORT_JIT</code><br/>JIT is supported</td>
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
    <td>Native platforms</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>❌</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>ALIPAY</code></td>
    <td>Alipay mini game</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>BAIDU</code></td>
    <td>Baidu mini game</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>BYTEDANCE</code></td>
    <td>Tik Tok mini game</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <td><code>WECHAT</code></td>
    <td>WeChat mini game</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>XIAOMI</code></td>
    <td>XiaoMi mini game</td>
    <td>✔️</td> <!-- MINIGAME -->
    <td>❌</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr><td><code>COCOSPLAY</code></td>
    <td>Cocos play</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>HUAWEI</code></td>
    <td>HuaWei quick game</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>OPPO</code></td>
    <td>OPPO quick game</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
    <td><code>VIVO</code></td>
    <td>vivo quick game</td>
    <td>❌</td> <!-- MINIGAME -->
    <td>✔️</td> <!-- RUNTIME_BASED -->
    <td>✔️</td> <!-- SUPPORT_JIT -->
    </tr>
    <!---->
    <tr>
</table>

#### Example: logging under development mode

```ts
import { log } from 'cc';
import { DEV } from "cce.env";

if (DEV) {
    log(`I'm in development mode!`);
}
```