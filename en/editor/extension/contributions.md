# Extend existing functionality

Cocos Creator 3D supports contributions between extensions.

When we are writing an extension, we can query whether the existing functions in the editor provide the function of receiving external contributions.

If the function provides contributions function, you can use these functions when writing extensions.

## Contribution data definition

In package.JSON you can define a contribution field.

```json5
{
    "name": "hello-world",
    "contributions": {}
}
```

contributions Definition Specification:

```typescript
interface contributions {
    [name: string]: any;
}
```

The name is the name of the function or extension, and the value is of any type, which is defined by the author of the name function (extension).

At this stage, only contributions to the internal functions of the editor are opened. In the future, we will provide the ability to use contributions between extensions.