# Optional Pass Parameters

Default value is in bold, all parameters are case-insensitive.

| Name                                        | Options                                                                         |
|:-------------------------------------------:|:-------------------------------------------------------------------------------:|
| switch                                      | **\*undefined**, could be any valid macro name that's not defined in the shader |
| priority                                    | **default**(128), could be any number between max(255) and min(0)               |
| stage                                       | **default**, could be the name of any registered stage in your runtime pipeline |
| phase                                       | **default**, could be the name of any registered phase in your runtime pipeline |
| propertyIndex                               | **\*undefined**, could be any valid pass index                                  |
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
Specifies a power switch for the current pass, if not enabled, the pass with be skipped completely.<br>
the macro name shouldn't collide with any existing macros inside the shader.<br>
This property doesn't exist by default, which means the pass is executed unconditionally.

## Priority
Specifies the rendering priority of the current pass, the bigger the number, the lower the priority. The default is (128), min is (0), max is (255), arithmetic operations between these constants and integer constants are supported.

## Stage
Specifies which render stage the current pass belongs to. For the built-in forward pipeline, the only available stage is 'default'.

## Phase
Specifies which phase the current pass belongs to. For the built-in forward pipeline, the available phases are 'default', 'forward-add' and 'shadow-caster'.

## PropertyIndex
Specifies the index of the pass to copy runtime property data from. Could be useful in some cases, e.g. the forward-add pass needs the same uniform data as the base pass.<br>
Once specified, all the properties for the current pass will not be visible on the material inspector.

## embeddedMacros
Specifies additional macro definitions on top of the current shader. Helpful for shader reuse when multiple passes' shader only differs at some macro definition.

## Properties
Specifies the public interfaces exposed to material instector and runtime API.<br>
It can be a direct mapping to shader uniforms, or specific channels of the uniform：
```yaml
albedo: { value: [1, 1, 1, 1] } # uniform vec4 albedo
roughness: { value: 0.8, target: pbrParams.g } # uniform vec4 pbrParams
offset: { value: [0, 0], target: tilingOffset.zw } # uniform vec4 tilingOffset
# say there is another uniform, vec4 emissive, that doesn't appear here
# so it will be assigned a default value of [0, 0, 0, 0] and will not appear in the inspector
```
Runtime reference is straightforward:
```js
// as long as it is a real uniform
// it doesn't matter whether it is specified in the property list or not
mat.setProperty('emissive', cc.Color.GREY); // this works
mat.setProperty('albedo', cc.Color.RED); // directly set uniform
mat.setProperty('roughness', 0.2); // set certain component
const h = mat.passes[0].getHandle('offset'); // or just take the handle,
mat.passes[0].setUniform(h, new Vec2(0.5, 0.5)); // and use Pass.setUniform interface instead
```
Shader uniforms that are not in the `properties` list will be given a [default value](#default-values).

For quick setup and experiment, the `__metadata__` feature is provided, which will be the 'base class' for all other properties:
```yaml
properties:
  __metadata__: { editor: { visible: false } }
  a: { value: [1, 1, 0, 0] }
  b: { editor: { type: color } }
  c: { editor: { visible: true } }
```
Here `a` and `b` will no longer appear in the inspector, while `c` stays visible.

## Migrations
Ideally the public interface of an effect should always be backward-compatible,<br>
but occasionally introducing breaking changes might become the only option as the project iterate.<br>
A smooth data transition would be much desired during the process, which leads to the migration system:<br>
After an effect with migrations is successfully compiled, all the dependent material assets will be immediately updated,<br>
new property will be automatically generated from existing data using specified rules.<br>
The migration process will not delete any data, and if the new data is visible in inspector, flag the source data as `deprecated`.<br>
If the new property already exists, no migration will be performed to prevent accidental data override. (except in force mode)

For a existing effect, declares the following migration rules:
```yaml
migrations:
  # macros: # macros follows the same rule as properties, without the component-wise features
  #   USE_MIAN_TEXTURE: { formerlySerializedAs: USE_MAIN_TEXTURE }
  properties:
    newFloat: { formerlySerializedAs: oldVec4.w }
```
Say we have a dependent material, with the following data:
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
After the effect is compiled, the material will be automatically updated to:
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
And after the next save operation on this material: (say the content is actually not changed)
```json
{
  "newFloat": 0.5
}
```
We are just using the `w` channel here, while in fact arbitrary shuffle is supported too:
```yaml
    newColor: { formerlySerializedAs: someOldColor.yxx }
```
Or even based on a target macro:
```yaml
    occlusion: { formerlySerializedAs: pbrParams.<OCCLUSION_CHANNEL|z> }
```
This means the new `occlusion` data will be extracted from `pbrParams` data,<br>
the specific channel depend on the `OCCLUSION_CHANNEL` macro of current pass,<br>
and default to channel `z` if macro data not present.

If `newFloat` property already exists before migration, nothing will happen, unless in force mode:
```yaml
    newFloat: { formerlySerializedAs: oldVec4.w! }
```
Then the migration is guaranteed to execute, regardless of the existing data.<br>

> **Note**: Migration in force mode will execute in every database event, which is basically every mouse click in editor. So use it as a quick-and-dirty test measure, and be sure not to submit effect files with force mode migrations into version control.

## Property Parameter List
All parameters are optional, with its default value in bold.

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

For macros:<br>
Macros with explicit tag declaration default to the first element of the tag parameter.<br>
All other macros default to 0.<br>
