
For convenience, we will be using `stb_image.h` a very popular single header image loading library. Can be downloaded [here](https://github.com/nothings/stb/blob/master/stb_image.h).


We include like this:
```cpp
#define STB_IMAGE_IMPLEMENTATION
#include "stb_image.h"
```

---

Then to actually load a texture from an image file with the `stb_image` library we do it like this:
```cpp
int width, height, nrChannels;
unsigned char *data = stbi_load("container.jpg", &width, &height, &nrChannels, 0);
```

**nrChannels** is used to know if we are working with RGB or RGBA , for this exemple it is a `.jpg` image so there only 3 channels. If we were working with `.png` file there would be 4 channels so later we would have to use `GL_RGBA` instead of `GL_RGB`

---

The next step is to generate an OpenGL texture object (like any other OpenGL object, we use a glGen* function and hold its ID):
```cpp
unsigned int texture;
glGenTextures(1, &texture);
```

---


Then like other objects we bind it before any other actions:
```cpp
glBindTexture(GL_TEXTURE_2D, texture);
```

---

After binding the texture we can generate the actual texture from the data we loaded previously from the image file:
```cpp
glTexImage2D(GL_TEXTURE_2D, 0, GL_RGB, width, height, 0, GL_RGB, GL_UNSIGNED_BYTE, data);
glGenerateMipmap(GL_TEXTURE_2D);
```

Lets detail the functions.

`glTexImage2D(textureTarget, mipmapLevel, textureFormat, width, height, 0, formatSource, dataTypeSource, sourceData)`

**This function link the texture to the currently binded texture object**

**textureTarget** : specify if we are working with 1D, 2D or 3D texture.

**mipmapLevel** : specifie the mipmap level to create the texture for.

**textureFormat** : the format how opengl will store the texture data internally.

**width** : the width of the stored texture

**height** : the height of the stored texture

**0** : just let zero. (legacy and not used anymore so don't worry about it)

**formatSource** : the format of the source texture image. (retrieved from the `stbi_load()` function)

**dataTypeSource** : the datatype of the source texture image.

**sourceData** : the actual image data we loaded.

`glGenerateMipmap(textureTarget)`

**This function generate automatically all the mipmaps for the currently bound texture object**

**textureTarget** : specify if we are working with 1D,2D or 3D texture.

---

After generating and binding everything we can safely free the previously loaded data of the image, we do not need it anymore.
```cpp
stbi_image_free(data);
```

---

We then need to apply this texture. That is where we add the texture coordinates to the vertices data. For example:
```cpp
float vertices[] = {
// positions        // colors         // texture coords
 0.5f,  0.5f, 0.0f, 1.0f, 0.0f, 0.0f, 1.0f, 1.0f, // top right
 0.5f, -0.5f, 0.0f, 0.0f, 1.0f, 0.0f, 1.0f, 0.0f, // bottom right
-0.5f, -0.5f, 0.0f, 0.0f, 0.0f, 1.0f, 0.0f, 0.0f, // bottom left
-0.5f,  0.5f, 0.0f, 1.0f, 1.0f, 0.0f, 0.0f, 1.0f // top left
};
```

since we added a new attribute in ours vertices we need to set a new attribute pointer similar to when we added color attribute back in the [VAO (Vertex Array Object)](../Buffer%20Objects/VAO%20(Vertex%20Array%20Object).md) example.

![](../Assets/vertex_attribute_pointer_interleaved_textures.png)

```cpp
glVertexAttribPointer(2, 2, GL_FLOAT, GL_FALSE, 8 * sizeof(float), (void*)(6 * sizeof(float)));
glEnableVertexAttribArray(2);
```

We need to set a new layout, specify the number of elements for this attribute (2 in this case), update the stride, and offset.

---

Next up, we need to update our shaders.

First the fragment shader need to accept our new vertex attribute by defining a new **layout**.
```rust
layout (location = 2) in vec2 aTexCoord;
```

We also need to forward it to the fragment shader to then sample the texture to the object so for that we need to set another output variable.

```rust
#version 330 core
layout (location = 0) in vec3 aPos;
layout (location = 1) in vec3 aColor;
layout (location = 2) in vec2 aTexCoord;

out vec3 ourColor;
out vec2 TexCoord; // here we outpout the Texture coordinate retrieved from the vertices

void main() {
    gl_Position = vec4(aPos, 1.0);
    ourColor = aColor;
    TexCoord = aTexCoord;
}
```

And in our fragment shader we then add the new input variable and introduce 2 things (one glsl data type and a function).

```rust
#version 330 core
out vec4 FragColor;

in vec3 ourColor;
in vec2 TexCoord;

uniform sampler2D ourTexture; // sampler*D is a glsl data type

void main() {
    FragColor = texture(ourTexture, TexCoord); // texture() is a built-in function
}
```

A `sampler` is a data-type for texture objects, it can be 1D, 2D or 3D.

To then actually sample our texture we make use of the `texture()` function, that take as first argument a texture sampler and the textures coordinates as second arguments.

---

Then all that is left to do is bind the texture before a draw which will automagically link the texture to the fragment shader sampler we just defined.

>[!caution]
> It automatically link if there is only one texture if there is multiple we need to use [Texture Units](Texture%20Units.md) 

```cpp
glBindTexture(GL_TEXTURE_2D, texture); // first arg is the texture target, second is the id of our generated texture
glBindVertexArray(VAO);
glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
```

And we are done.

