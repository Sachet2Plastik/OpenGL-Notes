Instead of manually setting the color of each vertices or pixel, or even adding a lot more vertices to create a texture, we would prefer using an already existing **texture** (can be 1D, 2D or even 3D) to detail an object.

Given a 2D texture image, we will need to give each vertex of our object a **texture coordinate**. The texture coordinate range from 0 to 1 in the X and Y axis. finding the texture color from the texture coordinate is called **sampling**.

![](../Assets/tex_coords.png)

Given the above triangle with 3 vertices, we will have to specify 3 texture coordinate.

```cpp
float texCoords[] = {
	0.0f, 0.0f, // lower-left corner
	1.0f, 0.0f, // lower-right corner
	0.5f, 1.0f // top-center corner
};
```

