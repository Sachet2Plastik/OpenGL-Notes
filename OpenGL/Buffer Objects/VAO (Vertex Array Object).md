
A Vertex Array Object is a "list of instruction" to tell opengl how to operate on VBO(s) binded to it, and how is formatted the data for shaders.
It define ***the layout*** of the VBO(s).

given this exemple:
![[vertex_attribute_pointer_interleaved.png]]
we can define the position of the data inside the [VBO](obsidian://open?vault=OpenGL&file=VBO%20(Vertex%20Buffer%20Object))
```c++
// position attribute
glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)0); glEnableVertexAttribArray(0);
// color attribute
glVertexAttribPointer(1, 3, GL_FLOAT, GL_FALSE, 6 * sizeof(float), (void*)(3* sizeof(float)));
glEnableVertexAttribArray(1);
```