Mipmaps is a type of LOD but for textures instead of geometry. The idea is to use a lower level (or lower resolution) texture for object that are far from the camera.

Mipmaps share similar options that of [Texture Filtering](Texture%20Filtering.md) :

- `GL_NEAREST_MIPMAP_NEAREST` : use the *nearest* mipmap level and uses the *nearest* neighbor option for the texture sampling.

- `GL_LINEAR_MIPMAP_NEAREST` : use the *nearest* mipmap level and uses the *linear* option for texture sampling.

- `GL_NEAREST_MIPMAP_LINEAR` : use the *linear* interpolation between the 2 closest mipmaps levels and use the *nearest* neighbor option for texture sampling.

- `GL_LINEAR_MIPMAP_LINEAR` : use the *linear* interpolation between the 2 closest mipmaps levels and use the *linear* option for texture sampling.

## Related Functions

### `glTexParameteri(textureTarget, optionToSet, mipmapsOption)`

**Similar to texture filtering but you can also use the options above for mipmaps***

**textureTarget** : specify if we are working with 1D,2D or 3D texture.

**optionToSet** : what is the option we want to set. (for this example we want to set a minifying (downscale) mipmaps option on the texture)

**mipmapsOption** : the mipmap option we want to use.

>[!warning]
> The mipmapsOption only works on minifying filter, using it on a magnifying option will produce an `GL_INVALID_ENUM` error.

#### Example usage
```rust
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MIN_FILTER, GL_LINEAR_MIPMAP_LINEAR);
glTexParameteri(GL_TEXTURE_2D, GL_TEXTURE_MAG_FILTER, GL_LINEAR);
```

### `glGenerateMimap(textureTarget)`

**This function is used to automatically generate the different mipmaps levels instead of manually generate them manually**

**textureTarget** : specify if we are working with 1D,2D or 3D texture.

>[!info]
> This function need to be called after loading the texture with the `glTexImage*D` function

