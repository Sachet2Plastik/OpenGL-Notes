
A Vertex Array Object is a "list of instruction" to tell OpenGL how to operate on VBO(s) binded to it, and how is formatted the data for shaders.
It define ***the layout*** of the VBO(s).

given this exemple:
![](../vertex_attribute_pointer_interleaved.png)

we can define the position of the data inside the [VBO (Vertex Buffer Object)](VBO%20(Vertex%20Buffer%20Object).md) like so: [Exemple given the image above](#Example) [{Obsidian.}](#Example%20Usage%20(given%20the%20above%20example%20with%20the%20drawing))


## Related functions

### `glGenVertexArrays(n, buffers)`

***Used to generate a new Vertex Array(s)***

**n** : number of vertex arrays to generate.
**buffers** : an unsigned int or an array of unsigned int representing the id(s) of the generated vertex arrays.

#### Example usage
```c++
// single VAO
unsigned int VAO;
glGenVertexArrays(1, &VAO);

// multiple VAOs
unsigned int VAOs[i];
glGenVertexArrays(i, VAOs);
```

### `glVertexAttribPointer(layout, attributeSize, type, normalized, stride, offset)`

***Used to define a certain layout of data from a currently binded VBO***

**layout** : the identifier of this attribute layout.
**attributeSize** : the size of this attribute, THIS IS NOT A `sizeof()` this time, this is more the **number of elements** this attribute contain.
**type** : the type of value the attribute is made of (int, float, etc...)
	- `GL_FLOAT` : for float
	- `GL_INT` : for int
	- ...
**normalized** : a bool to tell opengl to normalize value or not (`GL_TRUE`, `GL_FALSE`)
**stride** : the 'distance' of which the next attribute is located (basically the size of data needed to jump to reach the next block of the same attribute)
**offset** : the starting pointer offset of the current attribute.

<a id="Example"></a>
#### Example Usage (given the above example with the drawing)
```c++
// position attribute
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)0); glEnableVertexAttribArray(0);
// color attribute
glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)(3* sizeof(float)));
glEnableVertexAttribArray(1);
```



### `glEnableVertexAttribArray(layout)`

***Used to Enable a vertex attribute layout***

>[!caution]
> Vertex Attribute Array layout is disable by default so you need to first enable them in order to use them.

**layout** : the identifier to the layout we want to enable.
