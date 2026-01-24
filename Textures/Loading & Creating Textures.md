
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

**textureFormat** : the format we want to store the texture data.

**width** : the width of the stored texture

**height** : the height of the stored texture

**0** : just let zero.

**formatSource** : the format of the source texture image.

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

