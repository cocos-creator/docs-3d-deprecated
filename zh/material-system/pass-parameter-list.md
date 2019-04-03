# Pass 可配置参数

默认值为加粗项, 并可在Effect文件中直接省略.<br>
所有参数不区分大小写.

| Name                                        | Options                                                                          |
|:-------------------------------------------:|:--------------------------------------------------------------------------------:|
| switch                                      | ***undefined**, could be any valid macro name that's not defined in the shader   |
| stage                                       | **default**, could be the name of any stage registered in your runtime pipeline  |
| primitive                                   | point_list, line_list, line_strip, line_loop,<br>**triangle_list**, triangle_strip, triangle_fan,<br>line_list_adjacency, line_strip_adjacency,<br>triangle_list_adjacency, triangle_strip_adjacency,<br>triangle_patch_adjacency, quad_patch_list, iso_line_list |
| rasterizerState.<br>cullMode                | front, **back**, none                                                            |
| depthStencilState.<br>depthTest             | **true**, false                                                                  |
| depthStencilState.<br>depthWrite            | **true**, false                                                                  |
| depthStencilState.<br>depthFunc             | never, **less**, equal, less_equal, greater, not_equal, greater_equal, always    |
| blendState.targets[i].<br>blend             | true, **false**                                                                  |
| blendState.targets[i].<br>blendEq           | **add**, sub, rev_sub                                                            |
| blendState.targets[i].<br>blendSrc          | **one**, zero, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendDst          | one, **zero**, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendAlphaEq      | **add**, sub, rev_sub                                                            |
| blendState.targets[i].<br>blendSrcAlpha     | **one**, zero, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendDstAlpha     | one, **zero**, src_alpha_saturate,<br>src_alpha, one_minus_src_alpha,<br>dst_alpha, one_minus_dst_alpha,<br>src_color, one_minus_src_color,<br>dst_color, one_minus_dst_color,<br>constant_color, one_minus_constant_color,<br>constant_alpha, one_minus_constant_alpha |
| blendState.targets[i].<br>blendColorMask    | **all**, none, r, g, b, a, rg, rb, ra, gb, ga, ba, rgb, rga, rba, gba            |
| blendState.<br>blendColor                   | **0** (as a number) or **0x0** (as a string) or **[0, 0, 0, 0]**                 |
| depthStencilState.<br>stencilTestFront      | true, **false**                                                                  |
| depthStencilState.<br>stencilFuncFront      | never, less, equal, less_equal, greater, not_equal, greater_equal, **always**    |
| depthStencilState.<br>stencilReadMaskFront  | **4294967295** (as a number) or **0xffffffff** (as a string) or **[1, 1, 1, 1]** |
| depthStencilState.<br>stencilWriteMaskFront | **4294967295** (as a number) or **0xffffffff** (as a string) or **[1, 1, 1, 1]** |
| depthStencilState.<br>stencilFailOpFront    | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert                |
| depthStencilState.<br>stencilZFailOpFront   | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert                |
| depthStencilState.<br>stencilPassOpFront    | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert                |
| depthStencilState.<br>stencilRefFront       | **1** (as a number) or **0x1** (as a string) or **[0, 0, 0, 1]**                 |
| depthStencilState.<br>stencilTestBack       | true, **false**                                                                  |
| depthStencilState.<br>stencilFuncBack       | never, less, equal, less_equal, greater, not_equal, greater_equal, **always**    |
| depthStencilState.<br>stencilReadMaskBack   | **4294967295** (as a number) or **0xffffffff** (as a string) or **[1, 1, 1, 1]** |
| depthStencilState.<br>stencilWriteMaskBack  | **4294967295** (as a number) or **0xffffffff** (as a string) or **[1, 1, 1, 1]** |
| depthStencilState.<br>stencilFailOpBack     | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert                |
| depthStencilState.<br>stencilZFailOpBack    | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert                |
| depthStencilState.<br>stencilPassOpBack     | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert                |
| depthStencilState.<br>stencilRefBack        | **1** (as a number) or **0x1** (as a string) or **[0, 0, 0, 1]**                 |
| depthStencilState.<br>stencilReadMask       | *convenient setter for both front/back stencil read mask,<br>always use this for web platforms*  |
| depthStencilState.<br>stencilWriteMask      | *convenient setter for both front/back stencil write mask,<br>always use this for web platforms* |
| depthStencilState.<br>stencilRef            | *convenient setter for both front/back stencil ref,<br>always use this for web platforms*        |
| dynamics                                    | **[]**, an array containing any of the following:<br>viewport, scissor, line_width, depth_bias, blend_constants,<br>depth_bounds, stencil_write_mask, stencil_compare_mask |
| property                                    | *see the following section                                                       |

## Switch
指定这个 pass 的执行依赖于哪个 define, 它不应与使用到的 shader 中定义的任何 define 重名.
这个字段默认是不存在的, 意味着这个 pass 是无条件执行的.

## Property
properties 存储着这个 Pass 哪些 uniform 需要在 Inspector 上显示,<br>
未指定的 uniform 将由引擎在运行时根据自动分析出的数据类型给予[默认初值](#default-values).

同样地, 任何字段如为默认值也都可以省掉.<sup>[1](#footnote-1)</sup><br>

effect 资源导入器会自动从 shader 中读取 uniform 的类型等相关信息. <br>
但由于 GLSL 中没有 `color` 类型, 所有的 `color` 类 uniform 需要在 property 中显式指定它的类型, 才可以在运行时 `setProperty` 传入 `cc.color` 类型数据来设置.

## Property Param List
| Param                     | Options                                           |
|:-------------------------:|:-------------------------------------------------:|
| type                      | float, vec2, vec3, vec4, color4, sampler2D, samplerCube, ***extracted from shader** |
| value                     | *see the following section                        |
| displayName               | (any string), ***extracted from shader**          |
| sampler.<br>minFilter     | [0, 0, 0, 0]                             |
| sampler.<br>magFilter     | [0, 0, 0, 0]                             |
| sampler.<br>mipFilter     | [0, 0, 0, 0]                             |
| sampler.<br>addressU      | [0, 0, 0, 0]                             |
| sampler.<br>addressV      | [0, 0, 0, 0]                             |
| sampler.<br>addressW      | [0, 0, 0, 0]                             |
| sampler.<br>maxAnisotropy | [0, 0, 0, 0]                             |
| sampler.<br>cmpFunc       | [0, 0, 0, 0]                             |
| sampler.<br>borderColor   | [0, 0, 0, 0]                             |
| sampler.<br>minLOD        | [0, 0, 0, 0]                             |
| sampler.<br>maxLOD        | [0, 0, 0, 0]                             |
| sampler.<br>mipLODBias    | [0, 0, 0, 0]                             |

## Default Values
| Type        | Default Value / Options                           |
|:-----------:|:-------------------------------------------------:|
| int         | 0                                                 |
| ivec2       | [0, 0]                                            |
| ivec3       | [0, 0, 0]                                         |
| ivec4       | [0, 0, 0, 0]                                      |
| float       | 0                                                 |
| vec2        | [0, 0]                                            |
| vec3        | [0, 0, 0]                                         |
| vec4        | [0, 0, 0, 0]                                      |
| color4      | [0, 0, 0, 1]                                      |
| sampler2D   | black, grey, white, normal, **default**           |
| samplerCube | black-cube, white-cube, **default-cube**          |

对于 defines:<br>
boolean 类型默认值为 false<br>
number 类型默认值为 0, 默认取值范围 [0, 4].

<a name="footnote-1">[1]</a> 在引擎实际的 builtin effect 文件中, 所有的 property 字段都没有被省略, 这只是为了起更好的参考作用.<br>
