# Light sources

__Light source__ in game development represents a light-emitting object that can illuminate the surrounding environment

![light scene](light/lighting.png)

## additive per-pixel lights

Until today, Uber Shader is still the mainstream solution on some performance-constrained platforms. With the enhancement of hardware performance and the improvement of image quality requirements, a fixed number of light sources can no longer meet the needs of practical applications. People naturally think of a solution to support multiple light sources: `Multi-pass drawing`.

For a detailed introduction, please read:

- [additive per-pixel lights](light/additive-per-pixel-lights.md)

## Physically Based Lighting

__Cocos Creator__ uses optical measurement units to describe light source parameters. Based on optical measurement units, we can convert all relevant parameters of the light source into physical values in the real world. In this way, the designer can adjust the __light intensity__, __color__, __range__ and __other__ parameters according to the industrial standards of parameters and the physical parameters of a real environment. The overall lighting effect is more in line with the real natural environment.

For a detailed introduction, please read:

- [Physically Based Lighting](light/pbr-lighting.md)

__Cocos Creator__ supports three types of light sources:

- [Directional Light](light/dir-light.md)
- [Spherical Light](light/sphere-light.md)
- [Spotlights](light/spot-light.md)
