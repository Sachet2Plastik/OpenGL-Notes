Like said in the [Textures](Textures.md) part, the texture coordinate range from (0,0) to (1,1) but it is technically possible to specify a coordinate outside that range. So what happen ?

By default, OpenGL repeat the texture but we can manually specify 3 other behavior:

- `GL_REPEAT` : The default behavior. It repeat the texture when the coordinate goes out of bound.

- `GL_MIRRORED_REPEAT` : Similar to `GL_REPEAT` but it mirror the image at each repeat.

- `GL_CLAMP_TO_EDGE` : Clamps the coordinates between 0 and 1, so this result in "stretching" the image.

- `GL_CLAMP_TO_BORDER` : Different to `GL_CLAMP_TO_EDGE` this completely limit the texture to the texture border.


## Related Functions

### `glTexParameteri(textureTarget, optionToSet, wrappingMode)`

**Used to set a wrapping mode for a texture**

**textureTarget** : specify if we are working with 1D,2D or 3D texture.

**optionToSet** : what is the option we want to set. (for this example we want to set both the S and T texture axis (similar to X and Y) to a `GL_MIRRORED_REPEAT` mode)

**wrappingMode** : the wrapping mode we wan to use.

#### Example usage
```cpp
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_S, GL_MIRRORED_REPEAT);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_WRAP_T, GL_MIRRORED_REPEAT);
```

### `glTexParameterfv(textureTarget, optionToSet, borderColor)`

**If you previously set a `GL_CLAMP_TO_BORDER` option you need to set the border color using this function**

**textureTarget** : specify if we are working with 1D,2D or 3D texture.

**optionToSet** : what is the option we want to set. (for this example we want to set the border color)

**borderColor** : an array of float defining a RGBA value.

#### Example usage
```cpp
float borderColor[] = { 1.0f, 1.0f, 0.0f, 1.0f }; // a RGBA array value
glTexParameterfv(GL_TEXTURE_2D, GL_TEXTURE_BORDER_COLOR, borderColor);
```

