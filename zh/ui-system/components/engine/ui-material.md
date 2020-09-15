# UI 自定义材质
从 1.2 版本开始， UI 的 Sprite 组件支持自定义材质的使用，其使用界面如下图：

![](ui-material/UIMaterial.png)

其使用方法与其他材质并无不同，但由于 SpriteComponent 面板有基于 UI 内置材质的功能，所以有一些需要注意的点：

- 当设置自定义材质数量为 0 或为空时，会使用默认材质进行渲染，面板功能及使用方法可参考 [SpriteComponent](../editor/sprite.md)
- UI 并不支持多材质，自定义材质的数量最多为一个
- 当使用了 ui 自定义材质之后，面板上的 Grayscale 功能将会失效，用户可选择自己在材质中实现此功能
- 设置了自定义材质之后，面板上的 Color 属性会与本身材质中设置的颜色（如果有的话）混合影响精灵的渲染颜色
- 针对自定义材质，需要在 shader 中引入 cc-sprite-texture 头文件并对 cc_spriteTexture 进行采样，即可直接通过设置面板上的 SpriteFrame 设置 sprite 使用的图片资源
- 例如一个简单的使用了面板设置 SpriteFrame 来采样纹理的 fragment shader 应该是下面的样子：
    ```
    CCProgram sprite-fs %{
        precision highp float;
        #include <embedded-alpha>
        #include <cc-sprite-texture> // 引入预设头文件

        in vec4 color;
        in vec2 uv0;
        
        uniform Constant {
            vec4 mainColor;
        };
        
        vec4 frag () {
            vec4 o = vec4(1, 1, 1, 1);
            o *= texture(cc_spriteTexture, uv0); // 对 cc_spriteTexture 进行采样
            o *= color;
            o *= mainColor;
            return o;
        }
    }%
    ```
- 如果用户希望对自定义材质进行 uniform 赋值操作，可通过获取 SpriteComponent 上的 material 来进行操作，我们提供了不同的接口以应对不同的操作情况，如下代码所示：**（请一定注意看不同接口的注释说明！）**
    ```ts
        let spriteCom = this.node.getComponent(SpriteComponent);
        // 通过 sharedMaterial 方法获取到的为 共享材质资源，针对 material 进行的操作将会影响到所有使用此材质的渲染对象
        let material = spriteCom.sharedMaterial;

        // 通过 material 方法获取到的为 当前渲染组件使用的材质试例，针对 material Instance 进行的操作只会对当前组件产生影响
        let materialInstance = spriteCom.material;

    ```


