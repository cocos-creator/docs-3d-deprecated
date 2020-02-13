# Effect Syntax Guide

Writing your own effect can give you all the capabilities to customize the rendering process.<br>
Cocos Effect is a single-source embedded domain-specific language, based on YAML and GLSL.<br>
The YAML part declares the general framework, while GLSL part specifies the actual shader.<br>
Together they form a complete specification for the rendering process.

We recommend editing effect files using Visual Studio Code with the official `Cocos Effect` plugin from the marketplace.

> Note: This document is targeted at technical artists or graphic designers, if you are a design artist who needs specific shader customizations, please contact your technical artist or programmer for support.

## Framework Syntax
Using `builtin-unlit.effect` as an example, it looks something like this:

![effect](effect.png)

## About the Effect Name
Effect names can be directly used at runtime to acquire the `EffectAsset`:
```js
const effect = cc.EffectAsset.get('builtin-unlit'); // this is the EffectAsset resource instance
const mat = new cc.Material();
mat.initialize({ effectName: 'builtin-standard' }); // now `mat` is a valid standard material
```
And the names are automatically generated based on its file path (relative to the `assets/effects` folder in your project) and file name(extension excluded). <br>
The builtin effects are located directly inside the `assets/effects` folder in the internal database, so the effect names don't contain a path.<br>

## About YAML
YAML is a human-readable data-serialization language, with a flexible, minimal syntax and easily configurable, which makes it an ideal choice.<br>
But the syntax maybe somewhat unique, at first for those who are unfamiliar with the language,<br>
so we made a quick intro to the most commonly used syntaxes and language features [here](yaml-101.md) for your reference.

## Configurable Pass Parameters
The shader entries are the only required fields, namely `vert` and `frag`, in the format of `shaderChunkName:entryFunctionName`.<br>
Normally the `main` function shouldn't be specified in shader, for version-specific wrappers will be inserted at compile-time,<br>
assigning the return value of the specified function to the ouput of current shader stage. (gl_Position or the final output color).<br>
You can find all the optional parameter list [here](pass-parameter-list.md).

## Shader Chunks
Syntactically shader chunks are a superset of GLSL, all the extended features will be processed immediately at resource compile-time.<br>
It can either be written inside the `CCProgram` block in `.effect` file, or directly in a seperate `.chunk` file.
We recommend limiting the syntax within GLSL version 300 ES range to maintain best portability,<br>
although you can always write your own porting using GLSL built-in macro `__VERSION__`.<br>

This section will introduce the 'domain-specific' extended features,<br>
all of which will be processed immediately at resource compile-time.<br>
Also feel free to look around the built-in effects, which are always excellent concrete examples.

On top of GLSL, several c-style extensions are naturally introduced：

### Chunk Include
Just like the `include` directive in C/C++, you can include other shader chunks：
```c
#include <cc-global>
#include "../headers/my-shading-algorithm.chunk"
```
Relevant details:
* The `.chunk` extension can be omitted; quotation marks and angle brackets means the same;
* Every included header is guaranteed to be expanded only once; so every module could (and in fact should) include all its dependencies, even if there are overlaps;
* Dead code elimination is done at compile-time too, so including lots of unused utility functions shouldn't be of concern;
* There are two ways to reference a external chunk file: using the relative path to current file ('relative path'), or the relative path to `assets/chunks` folder of current project ('project absolute path'). If the specified file interpreted under both rules are present, the latter is preferred;
* When including files from other databases (like the internal database), only project absolute path is supported; when there are multiple databases have the same specified file, the priority is: User Project DB > Plugin DB > Internal DB;
* The built-in chunks are located directly inside `assets/chunks` in internal database, so you can include these without path prefix;
* All `CCProgram` blocks in the same effect file can include each other;

### Pre-processing Macros
Currently the effect system tends to take advantage of the language built-in pre-processing macros to create shader variants.<br>
The effect compiler will collect the macros that appear in shaders, and declarations will be inserted accordingly at runtime.<br>
So for the most part you can use them without thinking about the effect compiler,<br>
while material inspector will automatically integrate both macros and shader properties into a natual editting interface.<br>
Relevant details:
* To type check as many branches as possible at effect compile-time, the strategy currently taken is to set all macros to `1` (or its given default value) before doing the actual check; so make sure this combination works (or if not, maybe you need numerical macros, specified in the next section);
* All the macros that are not enabled at runtime will be explicitly given a value of 0, so please avoid using `#ifdef` or `#if defined` (**except the GLSL built-in macros, like extension indicators with `GL_` prefix**), for they would alway be true；
* Hash values will be calculated for each macro combination at runtime. For a single shader, the process is the most efficient when there are less than 2^32 combinations (a standard integer range), that means 32 boolean macros switches (or less if there are numerical macros). So please try to keep in this range to maintain optimal performance;

### Macro Tags
Although the effect compiler will try to be smart and collect all pre-processing branches, sometimes there are more complicated cases:
```glsl
// macro defined within certain numerical 'range'
#if LAYERS == 4
  // ...
#elif LAYERS == 5
  // ...
#endif
// multiple discrete 'options'
float metallic = texture(pbrMap, uv).METALLIC_SOURCE;
```
For these special usages, you'll have to explicitly declare the macro, using macro tags:<br>

| Tag     | Tag Parameter | Default Value | Usage |
|:-------:|:-------------:|:-------------:|:----:|
| range   | A two-element array,<br>specifying minimum and maximum value, both inclusive | [0, 3] | For macros with bounded range<br>The bound should be as tight as possible |
| options | An arbitrary-length array,<br>specifying every possible options | nothing | For macros with discrete, explicit choices |

Declarations for the above case are：
```glsl
#pragma define LAYERS range([4, 5])
#pragma define METALLIC_SOURCE options([r, g, b, a])
```
The first line declares a macro named `LAYERS`, with possible range of [4, 5];<br>
The second line declares a macro named `METALLIC_SOURCE`, with four possible options: 'r', 'g', 'b', 'a'.<br>

> Note: every tag accepts a single parameter, in the syntax of YAML.

### Functional Macros
Due to lack of native support in WebGL platform, functional macros are provided as an effect compile-time feature, all references will be expanded in the output shader.<br>
This is an good match for inlining some simple utility functions, or similar code repeating several times.<br>
In fact, many built-in utility functions are functional macros:
```glsl
#define CCDecode(position) \
  position = vec4(a_position, 1.0)
#define CCVertInput(position) \
  CCDecode(position);         \
  #if CC_USE_SKINNING         \
    CCSkin(position);         \
  #endif                      \
  #pragma // empty pragma trick to get rid of trailing semicolons at effect compile time
```
Meanwhile, same as the macro system in C/C++, the mechanism does nothing on checking [macro hygiene](https://en.wikipedia.org/wiki/Hygienic_macro),<br>
So any problem comes with it will have to be dealt with by developers manually:
```glsl
// please do be careful with unhygienic macros like this
#define INCI(i) do { int a=0; ++i; } while(0)
// when invoking
int a = 4, b = 8;
INCI(b); // correct, b would be 9 after this
INCI(a); // wrong! a would still be 4
```

### Vertex Input<sup id="a1">[1](#f1)</sup>
To encapsulate in-shader data pre-processing like data decompression and vertex skinning, utility function `CCVertInput` is provided;<br>
For all shaders used to draw mesh assets, you need something like this:
```glsl
#include <input>
vec4 vert () {
  vec3 position;
  CCVertInput(position);
  // ... do your thing with `position` (models space, after skinning)
}
```
If normals are required, use the standard version:
```glsl
#include <input-standard>
vec4 vert () {
  StandardVertInput In;
  CCVertInput(In);
  // ... now use `In.position`, etc.
}
```
This will acquire position, normal and tangent data, all after vertex skinning.<br>
Other vertex data (uv, color, etc.) can be declared directly.

To support dynamic batching, use `CCGetWorldMatrix`:
```glsl
#include <cc-local-batch>

vec4 vert () {
  // ...
  // unlit version (when normal is not needed)
  mat4 matWorld;
  CCGetWorldMatrix(matWorld);
  // standard version
  mat4 matWorld, matWorldIT;
  CCGetWorldMatrixFull(matWorld, matWorldIT);
  // ...
}
```
You can find the complete built-in shader uniform list [here](builtin-shader-uniforms.md).

### Fragment Ouput<sup id="a1">[1](#f1)</sup>
To encapsulate render pipeline complexities, use `CCFragOutput`:
For unlit shaders:
```glsl
#include <output>
vec4 frag () {
  vec4 o = vec4(0.0);
  // ... do the computation
  return CCFragOutput(o);
}
```
So the code can work in both HDR and LDR pipeline.<br>
If there's lighting involved, combine with `CCStandardShading` to form a surface shader structure:
```glsl
#include <shading-standard>
#include <output-standard> // note the header file change
void surf (out StandardSurface s) {
  // fill in your data here
}
vec4 frag () {
  StandardSurface s; surf(s);
  vec4 color = CCStandardShading(s);
  return CCFragOutput(color);
}
```
Under the framework writing your own surface shader or even shading algorithm becomes staightforward;<br>

> Note: the `CCFragOutput` function should not be overriden, unless using custom render pipelines.

### WebGL 1 fallback Support
The effect compiler provides fallback conversion from GLSL 300 ES to GLSL 100 automatically, for WebGL 1.0 only support GLSL 100 syntax. this should be transparent to developers for the most time.<br>
Currently the automatic conversion only supports some basic usage, and if some post-100 features or extensions were used, (texelFetch, textureGrad, etc.)<br>
you have to do your own porting using the language built-in \_\_VERSION__ macro:
```glsl
vec4 fragTextureLod (samplerCube tex, vec3 coord, float lod) {
  #if __VERSION__ < 130
    #ifdef GL_EXT_shader_texture_lod
      return textureCubeLodEXT(tex, coord, lod);
    #else
      return textureCube(tex, coord, lod); // fallback to bias
    #endif
  #else
    return textureLod(tex, coord, lod);
  #endif
}
```
The effect compiler will finally split these compile-time constant branches into different output versions.

### About UBO Memory Layout
First the conclusion, the final rules are, every non-sampler uniform should be specified in UBO blocks, and for every UBO block:
* there should be no vec3 typed members；
* for array typed members, size of each element should be no less than a vec4;
* any member ordering that introduces a padding will be rejected;

These rules will be checked rigorously at effect compile-time and throws detailed, implicit padding related compile error.

This might sound overly-strict at first, but it's for a few good reasons:<br>
First, UBO is a much better basic unit to efficiently reuse data, so discrete declaration is no longer an option;<br>
Second, currently many platforms, including WebGL 2.0 only support one platform-independent memory layout, namely **std140**, and it has many restrictions<sup id="a2">[2](#f2)</sup>:
* All vec3 members will be aligned to vec4：
```glsl
uniform ControversialType {
  vec3 v3_1; // offset 0, length 16 [IMPLICIT PADDING!]
}; // total of 16 bytes
```
* Any array member with element size less than a vec4 is aligned to vec4 element-wise:
```glsl
uniform ProblematicArrays {
  float f4_1[4]; // offset 0, stride 16, length 64 [IMPLICIT PADDING!]
}; // total of 64 bytes
```
* All UBO members are aligned to the size of itself<sup id="a3">[3](#f3)</sup>:
```glsl
uniform IncorrectUBOOrder {
  float f1_1; // offset 0, length 4 (aligned to 4 bytes)
  vec2 v2; // offset 8, length 8 (aligned to 8 bytes) [IMPLICIT PADDING!]
  float f1_2; // offset 16, length 4 (aligned to 4 bytes)
}; // total of 32 bytes*
uniform CorrectUBOOrder {
  float f1_1; // offset 0, length 4 (aligned to 4 bytes)
  float f1_2; // offset 4, length 4 (aligned to 4 bytes)
  vec2 v2; // offset 8, length 8 (aligned to 8 bytes)
}; // total of 16 bytes
```

This means lots of wasted space, and some driver implementation might not completely conform to the standard<sup id="a4">[4](#f4)</sup>, hence all the strict checking procedure help to clear some pretty insidious bugs.<br>

> Note: the actual uniform type can differ from the public interfaces the effect exposes to artists and runtime properties. Through [property target](pass-parameter-list.md#Properties) system, every single channel can be manipulated independently, without restriction of the original uniform.

<b id="f1">[1]</b> Shaders for systems with procedurally generated mesh, like particles, sprites, post-effects, etc. may handle things a bit differently [↩](#a1)<br>
<b id="f2">[2]</b> [OpenGL 4.5, Section 7.6.2.2, page 137](http://www.opengl.org/registry/doc/glspec45.core.pdf#page=159) [↩](#a2)<br>
<b id="f3">[3]</b> In the example code, UBO `IncorrectUBOOrder` has a total size 32. Actually this is still a platform-dependent data, due to what it seems like an oversight in the GLSL specification. More discussions can be found [here](https://bugs.chromium.org/p/chromium/issues/detail?id=988988) [↩](#a3)<br>
<b id="f4">[4]</b> [Interface Block - OpenGL Wiki](https://www.khronos.org/opengl/wiki/Interface_Block_(GLSL)#Memory_layout) [↩](#a4)
