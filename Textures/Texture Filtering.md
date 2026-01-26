Texture coordinate **do not** depend on resolution. But OpenGL need to know which texture pixel (or **Texel***) to map to the given texture coord.

Texture filtering is used when for example you have a large object but a low resolution texture.

There are different texture filtering option, but the "most" important ones are:
- `GL_NEAREST` : select the *texel* that is the closest to the texture coordinate
![](../Assets/filter_nearest.png)
- `GL_LINEAR` : create a *texel* based on the colors of the neighboring *texels*
![](../Assets/filter_linear.png)

Example of both mode on a large object and a low resolution texture:
![](../Assets/texture_filtering.png)
Texture filtering can be set independently for up or down, so you could for example use `GL_NEAREST` when textures are scaled downwards and `GL_LINEAR` for upscaled textures. We thus have to specify the filtering method for both options via `glTexParameter*`.

## Related Functions

### `glTexParameteri(textureTarget, optionToSet, filteringOption)`

**Used to set a texture filtering option**

**textureTarget** : specify if we are working with 1D,2D or 3D texture.

**optionToSet** : what is the option we want to set. (for this example we want to set different filtering option for magnifying and minifying (upscale/downscale))

**filteringOption** : the filtering option we want to set.

#### Example usage
```rust
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_NEAREST); glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```