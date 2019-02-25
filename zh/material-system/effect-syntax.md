# Effect 语法

如果希望在引擎中实现自定义的着色效果, 需要书写自定义 Effect.<br>
一个 Effect 文件大体可以分为两部分的内容, 一份 JSON 格式<sup>[1](#footnote-1)</sup>的流程控制清单, 和相关的 (类)GLSL 语法的 shader 片段.<br>
这两部分内容上相互补充, 共同构成了一个完整的渲染流程描述.

## 语法框架
以 skybox.effect 为例, 这个 Effect 文件的内容大致是这样:

![effect](effect.png)

## Pass 中可配置的参数
vert 和 frag 声明了当前 pass 使用的 shader, 格式为 `片段名:入口函数名`<br>
这个名字可以是本文件中声明的 shader 片段名, 也可以是引擎提供的标准头文件.
片段中不应出现 main 函数入口, 预处理阶段会将指定入口函数的返回值赋值给当前 shader 的输出 (gl_Position 或最终的输出颜色).<br>
其他可配置 GL 参数及默认值见 [完整列表](pass-parameter-list.md).

## Shader 片段
Shader 片段在语法上是标准 GLSL 300 es 语法的一个超集, 在资源加载时有相应的预处理流程.

这一节会介绍所有的扩展语法, 更多实际使用示例, 可参考编辑器内提供的 builtin effect.

在标准 GLSL 语法上, 我们引入了一些非常自然的 C 风格语法扩展:

### include 机制
类似 c 的头文件 include 机制, 可供引用的 builtin 头文件都在 chunks 目录下, 主要包括一些常用的工具函数, 和标准光照模型等. 另外所有在当前 effect 文件中声明的 shader 片段都可相互引用.

### define 预处理
Effect 系统的设计倾向于在游戏项目运行时可以方便地利用 shader 中的各类预处理宏, 而减少 runtime branching.<br>
引擎会在资源读取时收集所有在 shader 中出现的 defines, 然后在运行时动态地将需要的声明加入 shader 内容.<br>
所以要使用这些预处理宏, 只需要如上面的例子中一样, 在 shader 中直接进行逻辑判断即可.<br>
所有的 define 都会被序列化到 inspector 上, 供用户调整.<br>
注意如果在 shahder 中声明了 extension, 这个 extension 必须有且只有一个 define 来控制启用与否.

### numeric define tag
对数字类型的 define, 可以这样规定它的取值范围: (这个范围在满足需求情况下越小越好, 因为和 [program key](https://github.com/cocos-creator/engine/blob/3d/cocos/renderer/core/program-lib.ts) 相关)
```glsl
#pragma range(0, 4) NUM_POINT_LIGHTS
```

### define code segments
也可以通过宏直接定义代码片段, 并可多层嵌套, 如:
```glsl
#define DECL_CURVE_STRUCT(name) \
  uniform int u_##name##_curveMode;
#define DECL_CURVE_STRUCT_INT(name) \
  DECL_CURVE_STRUCT(name) \
  uniform float u_##name##_minIntegral[MAX_KEY_NUM - 1];

DECL_CURVE_STRUCT_INT(velocity_pos_x)
```
这样最后一行会被展开变成:
```glsl
  uniform int u_velocity_pos_x_curveMode;
  uniform float u_velocity_pos_x_minIntegral[MAX_KEY_NUM - 1];
```

### WebGL 1 fallback 支持
由于 WebGL 1 仅支持 GLSL 100 标准语法, 在预处理阶段会提供 300 es 转 100 的 fallback shader, 用户不必关心这层变化.

### 关于 Uniform Block
出于性能考量, 我们规定在 shader 中所有非 sampler 的 uniform 都应以 block 形式声明, 并应满足每个 block member 16 字节对齐.

---
<a name="footnote-1">[1]</a> 准确地说引擎这里使用的是 HJSON parser, 所以这部份支持如注释, 省略引号逗号等所有 HJSON 格式写法.<br>
