# Material System Overview

The material system plays an essential role in any game engine infrastructure:<br>
it controls the way everything is drawn on screen, and many more.<br>
The general structure of the system is as follows:

[![Assets](material.png "Click to view diagram source")](material.dot)

## EffectAsset
`EffectAsset` is an shading procedure description file, written by both engine and game developers.<br>
Detailed syntax instructions can be found in [here](effect-syntax.md).<br>

Right after an effect file is created, the in-editor effect compiler comes to do its job.<br>
Also using `builtin-unlit.effect` as an example, the compiler output for this file will look like this:
```json
{
  "name": "builtin-unlit",
  "techniques": [
    {"name":"opaque", "passes":[{"program":"builtin-unlit|unlit-vs:vert|unlit-fs:frag", "properties":{"mainTexture":{"value":"grey", "type":28}, "tilingOffset":{"value":[1, 1, 0, 0], "type":16}, "mainColor":{"value":[1, 1, 1, 1], "editor":{"type":"color"}, "type":16}, "colorScale":{"value":[1, 1, 1], "type":15, "handleInfo":["colorScaleAndCutoff", 0, 15]}, "alphaThreshold":{"value":[0.5], "editor":{"parent":"USE_ALPHA_TEST"}, "type":13, "handleInfo":["colorScaleAndCutoff", 3, 13]}, "color":{"editor":{"visible":false}, "type":16, "handleInfo":["mainColor", 0, 16]}, "colorScaleAndCutoff":{"type":16, "editor":{"visible":false, "deprecated":true}, "value":[1, 1, 1, 0.5]}}, "migrations":{"properties":{"mainColor":{"formerlySerializedAs":"color"}}}}]},
  ],
  "shaders": [
    {
      "name": "builtin-unlit|unlit-vs:vert|unlit-fs:frag",
      "hash": 2093221684,
      "glsl4": {
        "vert": "// glsl 460 vert source, omitted here for brevity",
        "frag": "// glsl 460 frag source, omitted here for brevity",
      },
      "glsl3": {
        "vert": "// glsl 300 es vert source, omitted here for brevity",
        "frag": "// glsl 300 es frag source, omitted here for brevity",
      },
      "glsl1": {
        "vert": "// glsl 100 vert source, omitted here for brevity",
        "frag": "// glsl 100 frag source, omitted here for brevity",
      },
      "attributes": [
        {"tags":["USE_BATCHING"], "name":"a_dyn_batch_id", "type":13, "count":1, "defines":["USE_BATCHING"], "location":1},
        {"name":"a_position", "type":15, "count":1, "defines":[], "location":0},
        {"name":"a_weights", "type":16, "count":1, "defines":["USE_SKINNING"], "location":2},
        {"name":"a_joints", "type":16, "count":1, "defines":["USE_SKINNING"], "location":3},
        {"tags":["USE_VERTEX_COLOR"], "name":"a_color", "type":16, "count":1, "defines":["USE_VERTEX_COLOR"], "location":4},
        {"tags":["USE_TEXTURE"], "name":"a_texCoord", "type":14, "count":1, "defines":["USE_TEXTURE"], "location":5}
      ],
      "varyings": [
        {"name":"v_color", "type":16, "count":1, "defines":["USE_VERTEX_COLOR"], "location":0},
        {"name":"v_uv", "type":14, "count":1, "defines":["USE_TEXTURE"], "location":1}
      ],
      "builtins": {"globals":{"blocks":[{"name":"CCGlobal", "defines":[]}], "samplers":[]}, "locals":{"blocks":[{"name":"CCLocalBatched", "defines":["USE_BATCHING"]}, {"name":"CCLocal", "defines":[]}, {"name":"CCSkinningTexture", "defines":["USE_SKINNING", "ANIMATION_BAKED"]}, {"name":"CCSkinningAnimation", "defines":["USE_SKINNING", "ANIMATION_BAKED"]}, {"name":"CCSkinningFlexible", "defines":["USE_SKINNING"]}], "samplers":[{"name":"cc_jointsTexture", "defines":["USE_SKINNING", "ANIMATION_BAKED"]}]}},
      "defines": [
        {"name":"USE_BATCHING", "type":"boolean", "defines":[]},
        {"name":"USE_SKINNING", "type":"boolean", "defines":[]},
        {"name":"ANIMATION_BAKED", "type":"boolean", "defines":["USE_SKINNING"]},
        {"name":"CC_SUPPORT_FLOAT_TEXTURE", "type":"boolean", "defines":["USE_SKINNING", "ANIMATION_BAKED"]},
        {"name":"USE_VERTEX_COLOR", "type":"boolean", "defines":[]},
        {"name":"USE_TEXTURE", "type":"boolean", "defines":[]},
        {"name":"FLIP_UV", "type":"boolean", "defines":["USE_TEXTURE"]},
        {"name":"CC_USE_HDR", "type":"boolean", "defines":[]},
        {"name":"USE_ALPHA_TEST", "type":"boolean", "defines":[]},
        {"name":"ALPHA_TEST_CHANNEL", "type":"string", "defines":["USE_ALPHA_TEST"], "options":["a", "r", "g", "b"]}
      ],
      "blocks": [
        {"name": "TexCoords", "defines": ["USE_TEXTURE"], "binding": 0, "members": [
          {"name":"tilingOffset", "type":16, "count":1}
        ]},
        {"name": "Constant", "defines": [], "binding": 1, "members": [
          {"name":"mainColor", "type":16, "count":1},
          {"name":"colorScaleAndCutoff", "type":16, "count":1}
        ]}
      ],
      "samplers": [
        {"name":"mainTexture", "type":28, "count":1, "defines":["USE_TEXTURE"], "binding":30}
      ]
    }
  ]
}
```
There is a lot to unpack here, but for the most part the details won't be of any concern to game deverlopers, and the key insight is this:

All the necessary info for runtime shading procedure setup on any target platform (and even editor support) is here in advance to guarantee portability and performance, and we will trim out all the redundant info at build-time to make sure maximized space efficiency.

## Material
`Material` is the actual instance that used to draw every specific models.<br>
Essential parameters for setting up an `Material` are:
* `effectAsset` or `effectName`: effect reference: which `EffectAsset` will be used? (must specify)
* technique: inside the `EffectAsset`, which technique will be used? (default to 0)
* defines: what value the shader macros have? (for shader variants) (default all to 0, or in-shader specified default value)
* states: if any, which pipeline state to override? (defaul to nothing, keep everything the same as how they are specified in effect)

```ts
const mat = new Material();
mat.initialize({
  effectName: 'pipeline/skybox',
  defines: { USE_RGBE_CUBEMAP: true }
});
```
With all these information, the `Material` is properly initialized, and ready to use in any Renderable Component.

Note，**if the material is intended to be used on any kind of `SkinningModel`, be sure to enable the `USE_SKINNING` shader macro.**

Knowing which `EffectAsset` is currently using, we can specify all the shader properties:
```ts
mat.setProperty('cubeMap', someCubeMap);
console.log(mat.getProperty('cubeMap') === someCubeMap); // true
```
These properties are assigned inside the material, which is just an asset on itself, and have't connected to the any model.

To make the material taking effect on some specific model, it needs to be attached to a `RenderableComponent`.<br>
Any component that accepting a material parameter (ModelComponent, SkinningModelComponent, etc.) is inherited from it.
```ts
const comp = someNode.getComponent(ModelComponent);
comp.material = mat;
comp.setMaterial(mat, 0); // same as last line
```
According to the number of submodels, `RenderableComponent` may reference multiple `Material`s:
```ts
comp.setMaterial(mat, 1); // assign to second submodel
```

The same `Material` can be attached to multiple `RenderableComponent` too, we omit that here.<br>
But when one of the material-sharing models needs to customize some property,<br>
you need to get a copied instance of the material asset, aka. `MaterialInstance`, from the `RenderableComponent`, by calling:
```ts
const comp2 = someNode2.getComponent(ModelComponent);
const mat2 = comp2.material; // copy constructor, now `mat2` is an `MaterialInstance`, and every change made to `mat2` only affect the `comp2` instance
```
The biggest difference between `Material` asset and `MaterialInstance` is,<br>
`MaterialInstance` is definitively attached to one `RenderableComponent` at the beginning of its life cyle, while `Material` has no such limit.

For a already initialized material, if you need to re-initialize, just re-invoke the `initialize` function, to rebuild everything.
```ts
mat.initialize({
  effectName: 'builtin-standard',
  technique: 1
});
```

Specifically, if it is only the shader macros or pipeline states that you want to modify, there are more efficient ways:
```ts
mat2.recompileShaders({ USE_EMISSIVE: true });
mat2.overridePipelineStates({ rasterizerState: { cullMode: GFXCullMode.NONE } });
```
But remember these can only be called on `MaterialInstance`s, not `Material` asset itself.

Updating shader properties every frame is a common practice, under situations like this, where performance matters, use lower level APIs:
```ts
// Save these when starting
const pass = mat2.passes[0];
const hColor = pass.getHandle('albedo');
const color = new Color('#dadada');

// inside update function
color.a = Math.sin(director.getTotalFrames() * 0.01) * 127 + 127;
pass.setUniform(hColor, color);
```

## Builtins
Although the material system itself doesn't make any assumptions on the content,<br>
there are some built-in effects written on top of the system, provided for common usage:<br>
unlit, physically-based (standard), skybox, particle, sprite, etc.<br>
For a quick reference, here are all the properties and macros of the standard effect：

![Standard](builtin-standard.png)

| Property | Info |
|:--------:|:----:|
| tilingOffset | tiling and offset of the model UV,<br>`xy` channel for tiling, `zw` channel for offset |
| albedo/mainColor | albedo color, the main base color of the model |
| albedoMap/mainTexture | albedo texture, if present, will be multiplied by the `albedo` property |
| albedoScale | albedo scaling factor,<br>weighting the whole albedo factor before the final output |
| alphaThreshold | test threshold for discarding pixels,<br>any pixel with target channel value lower than this threshold will be discarded |
| normalMap | normal map texture, enhancing surface details |
| normalStrenth | strenth of the normal map, the bigger the bumpier |
| pbrMap | PBR parameter all-in-one texture: occlusion, roughness and metallic<br>sample result will be multiplied by the matching constants<br>source channel specified by `XX_CHANNEL` macros |
| metallicRoughnessMap | metallic and roughness texture<br>sample result will be multiplied by the matching constants<br>source channel specified by `XX_CHANNEL` macros |
| occlusionMap | independent occlusion texture<br>sample result will be multiplied by the matching constants<br>source channel specified by `XX_CHANNEL` macros |
| occlusion | occlusion constant |
| roughness | roughness constant |
| metallic | metallic constant |
| emissive | emissive color<br> |
| emissiveMap | emissive color texture, if present, will be multiplied by the `emissive` property,<br>so remember to set `emissive` property more close to white<br>(default black) for this to take effect |
| emissiveScale | emissive scaling factor<br>weighting the whole emissive factor before the final output |

Accordingly, these are the available macros：

| Macro | Info |
|:-----:|:----:|
| USE_BATCHING | is dynamic batching enabled? |
| USE_SKINNING | is vertex skinning enabled? **necessary for skinning models** |
| ANIMATION_BAKED | is baked skinning animation enabled? |
| OCCLUSION_CHANNEL | specifies the occlusion source channel, default to R channel |
| ROUGHNESS_CHANNEL | specifies the roughness source channel, default to G channel |
| METALLIC_CHANNEL | specifies the metallic source channel, default to B channel |
| HAS_SECOND_UV | is the second UV set present? |
| ALBEDO_UV | specifies the uv set to use when sampling albedo texture, default to the first set |
| EMISSIVE_UV | specifies the uv set to use when sampling emissive texture, default to the first set |
| ALPHA_TEST_CHANNEL | specifies the source channel for alpha test, default to A channel |
| USE_VERTEX_COLOR | if enabled, vertex color will be multiplied to albedo factor as well |
| USE_ALPHA_TEST | is alpha test enabled? |
| USE_ALBEDO_MAP | is albedo texture enabled? |
| USE_NORMAL_MAP | is normal map enabled? |
| USE_PBR_MAP | is PBR parameter texture enabled? |
| USE_EMISSIVE_MAP | is emissive texture enabled? |
