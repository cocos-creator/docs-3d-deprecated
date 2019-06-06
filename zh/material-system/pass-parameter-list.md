# Pass 可配置参数

默认值为加粗项，并可在Effect文件中直接省略。<br>
所有参数不区分大小写。

| Name                                        | Options                                                                         |
|:-------------------------------------------:|:-------------------------------------------------------------------------------:|
| switch                                      | ***undefined**, could be any valid macro name that's not defined in the shader  |
| priority                                    | **default**(128), could be any number between max(255) and min(0)               |
| stage                                       | **default**, could be the name of any registered stage in your runtime pipeline |
| customization                               | **[]**, could be the name of any registered runtime customization               |
| property                                    | *see the following section                                                      |
| primitive                                   | point_list, line_list, line_strip, line_loop,<br>**triangle_list**, triangle_strip, triangle_fan,<br>line_list_adjacency, line_strip_adjacency,<br>triangle_list_adjacency, triangle_strip_adjacency,<br>triangle_patch_adjacency, quad_patch_list, iso_line_list |
| dynamics                                    | **[]**, an array containing any of the following:<br>viewport, scissor, line_width, depth_bias, blend_constants,<br>depth_bounds, stencil_write_mask, stencil_compare_mask |
| rasterizerState.<br>cullMode                | front, **back**, none                                                           |
| depthStencilState.<br>depthTest             | **true**, false                                                                 |
| depthStencilState.<br>depthWrite            | **true**, false                                                                 |
| depthStencilState.<br>depthFunc             | never, **less**, equal, less_equal, greater, not_equal, greater_equal, always   |
| blendState.targets[i].<br>blend             | true, **false**                                                                 |
| blendState.targets[i].<br>blendEq           | **add**, sub, rev_sub                                                           |
| blendState.targets[i].<br>blendSrc          | **one**, zero, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendDst          | one, **zero**, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendSrcAlpha     | **one**, zero, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendDstAlpha     | one, **zero**, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendAlphaEq      | **add**, sub, rev_sub                                                           |
| blendState.targets[i].<br>blendColorMask    | **all**, none, r, g, b, a, rg, rb, ra, gb, ga, ba, rgb, rga, rba, gba           |
| blendState.<br>blendColor                   | **0** or **[0, 0, 0, 0]**                                                       |
| depthStencilState.<br>stencilTestFront      | true, **false**                                                                 |
| depthStencilState.<br>stencilFuncFront      | never, less, equal, less_equal, greater, not_equal, greater_equal, **always**   |
| depthStencilState.<br>stencilReadMaskFront  | **0xffffffff** or **[1, 1, 1, 1]**                                              |
| depthStencilState.<br>stencilWriteMaskFront | **0xffffffff** or **[1, 1, 1, 1]**                                              |
| depthStencilState.<br>stencilFailOpFront    | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilZFailOpFront   | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilPassOpFront    | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilRefFront       | **1** or **[0, 0, 0, 1]**                                                       |
| depthStencilState.<br>stencilTestBack       | true, **false**                                                                 |
| depthStencilState.<br>stencilFuncBack       | never, less, equal, less_equal, greater, not_equal, greater_equal, **always**   |
| depthStencilState.<br>stencilReadMaskBack   | **0xffffffff** or **[1, 1, 1, 1]**                                              |
| depthStencilState.<br>stencilWriteMaskBack  | **0xffffffff** or **[1, 1, 1, 1]**                                              |
| depthStencilState.<br>stencilFailOpBack     | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilZFailOpBack    | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilPassOpBack     | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilRefBack        | **1** or **[0, 0, 0, 1]**                                                       |
| depthStencilState.<br>stencilReadMask       | **convenient setter for both front/back stencil read mask,<br>always use this for web platforms* |
| depthStencilState.<br>stencilWriteMask      | **convenient setter for both front/back stencil write mask,<br>always use this for web platforms* |
| depthStencilState.<br>stencilRef            | **convenient setter for both front/back stencil ref,<br>always use this for web platforms* |

## Switch
指定这个 pass 的执行依赖于哪个 define，它不应与使用到的 shader 中定义的任何 define 重名。
这个字段默认是不存在的，意味着这个 pass 是无条件执行的。

## Priority
指定这个 pass 的渲染优先级，数值越小越优先渲染；default 代表默认优先级 (128)，min 代表最小（0），max 代表最大（255），可结合四则运算符指定相对值。

## Stage
指定这个 pass 归属于管线的哪个 stage，对 forward 管线，只有 default 一个 stage。

## Customization
指定当这个 pass 被应用到场景模型上后执行的自定义回调函数，用于实现显示相关的特殊逻辑需求，可在脚本中通过 cc.customizationManager.register 注册。

## Property
properties 存储着这个 Pass 哪些 uniform 需要在 Inspector 上显示,<br>
未指定的 uniform 将由引擎在运行时根据自动分析出的数据类型给予[默认初值](#default-values)。

同样地，任何字段如为默认值也都可以省掉。

effect 资源导入器会自动从 shader 中读取 uniform 的类型等相关信息。<br>
另外，各 uniform 会根据 `inspector` 属性调整在编辑器内的显示，如使用 color picker，或设置 tooltip 等。<br>

为方便声明各 property 子属性，可以直接在 properties 内声明 `__metadata__` 项，所有 property 都会继承它声明的内容，如：
```yaml
properties:
  __metadata__: { inspector: { visible: false } }
  a: { value: [1, 1, 0, 0] }
  b: { inspector: { type: color } }
  c: { inspector: { visible: true } }
```
这样 uniform a 和 b 已声明的各项参数都不受影响，但全部不会显示在 inspector 上（visible 为 false），而 uniform c 还会正常显示。

## Property Param List
| Param                     | Options                              |
|:-------------------------:|:------------------------------------:|
| type                      | float, vec2, vec3, vec4, sampler2D, samplerCube, ***extracted from shader** |
| value                     | **see the following section*         |
| sampler.<br>minFilter     | none, point, **linear**, anisotropic |
| sampler.<br>magFilter     | none, point, **linear**, anisotropic |
| sampler.<br>mipFilter     | **none**, point, linear, anisotropic |
| sampler.<br>addressU      | **wrap**, mirror, clamp, border      |
| sampler.<br>addressV      | **wrap**, mirror, clamp, border      |
| sampler.<br>addressW      | **wrap**, mirror, clamp, border      |
| sampler.<br>maxAnisotropy | **16**                               |
| sampler.<br>cmpFunc       | **never**, less, equal, less_equal, greater, not_equal, greater_equal, always |
| sampler.<br>borderColor   | **[0, 0, 0, 0]**                     |
| sampler.<br>minLOD        | **0**                                |
| sampler.<br>maxLOD        | **0**, **remember to override this when enabling mip filter* |
| sampler.<br>mipLODBias    | **0**                                |
| inspector.<br>displayName | (any string), ***property name**     |
| inspector.<br>type        | **vector**, color                    |
| inspector.<br>visible     | **true**, false                      |
| inspector.<br>tooltip     | (any string), ***property name**     |

## Default Values
| Type        | Default Value / Options                  |
|:-----------:|:----------------------------------------:|
| int         | 0                                        |
| ivec2       | [0, 0]                                   |
| ivec3       | [0, 0, 0]                                |
| ivec4       | [0, 0, 0, 0]                             |
| float       | 0                                        |
| vec2        | [0, 0]                                   |
| vec3        | [0, 0, 0]                                |
| vec4        | [0, 0, 0, 0]                             |
| sampler2D   | black, grey, white, normal, **default**  |
| samplerCube | black-cube, white-cube, **default-cube** |

对于 defines：<br>
boolean 类型默认值为 false。<br>
number 类型默认值为 0，默认取值范围 [0, 3]。<br>
string 类型默认值为 options 数组第一个元素。
