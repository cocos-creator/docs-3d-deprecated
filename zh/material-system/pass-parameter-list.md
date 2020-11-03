# Pass 可选配置参数

默认值为加粗项，所有参数不区分大小写。

| Name                                        | Options                                                                         |
|:-------------------------------------------:|:-------------------------------------------------------------------------------:|
| switch                                      | **\*undefined**, could be any valid macro name that's not defined in the shader |
| priority                                    | **default**(128), could be any number between max(255) and min(0)               |
| stage                                       | **default**, could be the name of any registered stage in your runtime pipeline |
| phase                                       | **default**, could be the name of any registered phase in your runtime pipeline |
| propertyIndex                               | **\*undefined**, could be any valid pass index                                  |
| embeddedMacros                              | **\*undefined**, could be an object containing any macro key-value pairs        |
| properties                                  | *see the following section                                                      |
| migrations                                  | *see the following section                                                      |
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
| depthStencilState.<br>stencilTest           | true, **false**                                                                 |
| depthStencilState.<br>stencilFunc           | never, less, equal, less_equal, greater, not_equal, greater_equal, **always**   |
| depthStencilState.<br>stencilReadMask       | **0xffffffff** or **[1, 1, 1, 1]**                                              |
| depthStencilState.<br>stencilWriteMask      | **0xffffffff** or **[1, 1, 1, 1]**                                              |
| depthStencilState.<br>stencilFailOp         | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilZFailOp        | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilPassOp         | **keep**, zero, replace, incr, incr_wrap, decr, decr_wrap, invert               |
| depthStencilState.<br>stencilRef            | **1** or **[0, 0, 0, 1]**                                                       |
| depthStencilState.<br>stencil\*Front/Back   | *\*set above stencil properties for specific side*                              |

## Switch
指定这个 pass 的执行依赖于哪个 define，它不应与使用到的 shader 中定义的任何 define 重名。<br>
这个字段默认是不存在的，意味着这个 pass 是无条件执行的。

## Priority
指定这个 pass 的渲染优先级，数值越小越优先渲染；default 代表默认优先级 (128)，min 代表最小（0），max 代表最大（255），可结合四则运算符指定相对值。

## Stage
指定这个 pass 归属于管线的哪个 stage，对默认 forward 管线，只有 `default` 一个 stage。

## Phase
指定这个 pass 归属于管线的哪个 stage，对默认 forward 管线，可以是 `default`, `forward-add`, `shadow-caster` 几个。

## PropertyIndex
指定这个 pass 的运行时 uniform 属性数据要和哪个 pass 保持一致，比如 forward add 等 pass 需要和 base pass 一致才能保证正确的渲染效果。<br>
一旦指定了此参数，材质面板上就不再会显示这个 pass 的任何属性。

## embeddedMacros
指定在这个 pass 的 shader 基础上额外定义的常量宏。在多个 pass 的 shader 只有宏定义不同时可使用此参数来复用 shader 资源。

## Properties
properties 存储着这个 Pass 有哪些可定制的参数需要在 Inspector 上显示，<br>
这些参数可以是 shader 中的某个 uniform 的完整映射，也可以是具体某个分量的映射 (使用 target 参数)：
```yaml
albedo: { value: [1, 1, 1, 1] } # uniform vec4 albedo
roughness: { value: 0.8, target: pbrParams.g } # uniform vec4 pbrParams
offset: { value: [0, 0], target: tilingOffset.zw } # uniform vec4 tilingOffset
# say there is another uniform, vec4 emissive, that doesn't appear here
# so it will be assigned a default value of [0, 0, 0, 0] and will not appear in the inspector
```
运行时可以这样使用：
```js
// as long as it is a real uniform
// it doesn't matter whether it is specified in the property list or not
mat.setProperty('emissive', Color.GREY); // this works
mat.setProperty('albedo', Color.RED); // directly set uniform
mat.setProperty('roughness', 0.2); // set certain component
const h = mat.passes[0].getHandle('offset'); // or just take the handle,
mat.passes[0].setUniform(h, new Vec2(0.5, 0.5)); // and use Pass.setUniform interface instead
```
未指定的 uniform 将由引擎在运行时根据自动分析出的数据类型给予[默认初值](#default-values)。

为方便声明各 property 子属性，可以直接在 properties 内声明 `__metadata__` 项，所有 property 都会继承它声明的内容，如：
```yaml
properties:
  __metadata__: { editor: { visible: false } }
  a: { value: [1, 1, 0, 0] }
  b: { editor: { type: color } }
  c: { editor: { visible: true } }
```
这样 uniform a 和 b 已声明的各项参数都不受影响，但全部不会显示在 inspector 上（visible 为 false），而 uniform c 还会正常显示。

## Migrations
一般来说使用材质资源时希望底层的 effect 接口能始终向前兼容，但依然有时面对新的需求最好的解决方案是含有一定 breaking change 的，<br>
这时为了保持项目中已有的材质资源数据不受影响，或至少能够更平滑的升级，可以使用 effect 的迁移系统，<br>
在 effect 导入成功后会 **立即更新工程内所有** 依赖于此 effect 的材质资源，<br>
对每个材质资源，尝试寻找所有指定旧参数数据（包括 property 和宏定义两类），复制或重组到新属性名下。<br>
这个过程不会自动删除旧数据，但是会将旧属性的 editor.deprecated 标为 true（如果新数据在 inspector 上可见的话）。<br>
如果一个材质资源内既有旧数据，又有新数据，则不会做任何迁移（强制更新模式除外）。

对于一个现有 effect，声明如下迁移字段：
```yaml
migrations:
  # macros: # macros follows the same rule as properties, without the component-wise features
  #   USE_MIAN_TEXTURE: { formerlySerializedAs: USE_MAIN_TEXTURE }
  properties:
    newFloat: { formerlySerializedAs: oldVec4.w }
```
对于一个依赖于这个 effect，并在对应 pass 持有这样的属性的材质：
```json
{
  "oldVec4": {
    "__type__": "cc.Vec4",
    "x": 1,
    "y": 1,
    "z": 1,
    "w": 0.5
  }
}
```
在 effect 重新导入后，这些数据会被立即转换成：
```json
{
  "oldVec4": {
    "__type__": "cc.Vec4",
    "x": 1,
    "y": 1,
    "z": 1,
    "w": 0.5
  },
  "newFloat": 0.5
}
```
在编辑器内重新编辑并保存这个材质资源后会变成（假设 effect 和 property 数据本身并没有改变）：
```json
{
  "newFloat": 0.5
}
```
注意这里的通道指令只是简单的取 `w` 分量，事实上还可以做任意的 shuffle：
```yaml
    newColor: { formerlySerializedAs: someOldColor.yxx }
```
甚至基于某个宏定义：
```yaml
    occlusion: { formerlySerializedAs: pbrParams.<OCCLUSION_CHANNEL|z> }
```
这里声明了新的 occlusion 属性会从旧的 `pbrParams` 中获取，而具体的分量取决于 OCCLUSION_CHANNEL 宏定义，且如材质资源中未定义此宏，默认取 `z` 通道。<br>
但如果某个材质在迁移升级前就已经存着 `newFloat` 字段的数据，则不会对其做任何修改，除非指定为强制更新模式：
```yaml
    newFloat: { formerlySerializedAs: oldVec4.w! }
```
这会强制更新所有材质的属性，无论这个操作是否会覆盖数据。<br>
注意强制更新操作会在编辑器的每次资源事件中都执行（几乎对应每一次鼠标点击，相对高频），<br>
因此只是一个快速测试和调试的手段，一定不要将处于强制更新模式的 effect 提交版本控制。

## Property Param List
同样地，任何可配置字段如与默认值相同都可以省掉。

| Param                     | Options                              |
|:-------------------------:|:------------------------------------:|
| target                    | **undefined**, (any valid uniform components, no random swizzle) |
| value                     | *\*see the next section*             |
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
| sampler.<br>maxLOD        | **0**, *\*remember to override this when enabling mip filter* |
| sampler.<br>mipLODBias    | **0**                                |
| editor                    | *\*see the next section*             |
| editor.<br>displayName    | (any string), **\*property name**    |
| editor.<br>type           | **vector**, color                    |
| editor.<br>visible        | **true**, false                      |
| editor.<br>tooltip        | (any string), **\*property name**    |
| editor.<br>range          | **undefined**, [ min, max, [step] ]  |
| editor.<br>deprecated     | true, **false**, *\*for any material using this effect,<br> delete the existing data for this property after next saving* |

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
