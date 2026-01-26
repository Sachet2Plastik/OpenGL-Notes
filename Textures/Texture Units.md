
Texture Units are used to mix multiple texture in one. In the [Loading & Creating Textures](Loading%20&%20Creating%20Textures.md) example we did not have to manually locate the uniform and assign it a value but if we use multiple texture the auto linking cannot be done since by default it just link to the texture unit 0 so the bind just did a link to the default value.

So to bind multiple textures we just have to specify which texture unit we are working with before binding the texture.

For instance :
```rust
glActiveTexture(GL_TEXTURE0); // activate the texture unit first before binding texture
glBindTexture(GL_TEXTURE_2D, texture);
```

`GL_TEXTURE0` is always by default activated that is why we didn't need to activate it in the [Loading & Creating Textures](Loading%20&%20Creating%20Textures.md) example.

>[!info]
> OpenGL have ***at least*** 16 texture units from `GL_TEXTURE0` to `GL_TEXTURE15` defined in order so for example: `GL_TEXTURE2` can be accessed by `GL_TEXTURE0 + 2`.
> 
> **BUT** it depend on the GPU, to verify how many is available use this function.
> `glGetIntegerv(GL_MAX_COMBINED_TEXTURE_IMAGE_UNITS, &int);`

Then the fragment shader need to be able to accept more then one sampler:

```cpp
#version 330 core

uniform sampler2D texture1;
uniform sampler2D texture2;

void main() {
    FragColor = mix(texture(texture1, TexCoord), texture(texture2, TexCoord), 0.2);
}
```

By using the `mix()` built-in function we can combine 2 texture and based on the 3rd argument linearly interpolates between them, meaning that if the 3rd argument is `0.0` we return the 1st argument, if the 3rd argument is `1.0` we return the 2nd argument. So a value of `0.2` here return 80% of the 1st texture color and 20% of the 2nd texture color.

Loading another image is the same as previously seen.

but the binding is different.
```cpp
glActiveTexture(GL_TEXTURE0);
glBindTexture(GL_TEXTURE_2D, texture1);
glActiveTexture(GL_TEXTURE1);
glBindTexture(GL_TEXTURE_2D, texture2);
```

and we also actually need to set which uniform sampler is which texture units. (we do this before the main loop)
```cpp
ourShader.use(); // don't forget to activate the shader before setting uniforms!
glUniform1i(glGetUniformLocation(ourShader.ID, "texture1"), 0); // set it manually
ourShader.setInt("texture2", 1); // or with a custom shader class
```


## Related Functions

### `glActiveTexture(textureUnit)`

**Used to specify on which texture unit we are working with**

**textureUnit** : the targeted texture unit we want to activate.