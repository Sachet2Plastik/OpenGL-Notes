
After a lot of matrices shenanigans here we are, 3D. The point of this matrices transformation was to transform 3D coordinates to 2D coordinates that can be localized on our screen.

Lets first define a model matrix, the model matrix contain all the *transformations* we'd like to apply to object in the global world space.

We will just try to angle the square me made from the texture example.

```cpp
auto model = glm::mat4(1.0f);
model = glm::rotate(model, glm::radians(-55.0f), glm::vec3(1.0f, 0.0f, 0.0f))
```

We will also need to move slightly back since from start we (the camera) is located at 0,0,0.

also OpenGL is a **right-handed** system, this mean that the X-axis is going to the right, the Y-axis is up and the Z-axis is going "toward" the screen instead of "forward" (the ambiguity is real the image will be more understandable):

![](../Assets/coordinate_systems_right_handed.png)

We are not using a proper camera for now so we will just define manually the camera position.

```cpp
auto view = glm::mat4(1.0f);
view = glm::translate(view, glm::vec3(0.0f, 0.0f, -3.0f))
```

And at last, the projection matrix to create the perspective we are looking for.

```cpp
glm::mat4 projection;
projection = glm::perspective(glm::radians(45.0f), 800.0f/600.0f, 0.1, 100.0f);
```

And finally, update the our vertex shader.

```c
#version 330 core
layout (location = 0) in vec3 aPos;
...
uniform mat4 model;
uniform mat4 view;
uniform mat4 projection;

void main() {
	// note that we read the multiplication from right to left
	gl_Position = projection * view * model * vec4(aPos, 1.0);
	...
}
```

And send everything with `glGetUniform` and `glUniform` function.

---

## Z-buffer

OpenGL store the depth information inside a "Z-buffer" that avoid fragments that are behind another fragment to be rendered, in other words fragments hidden behind another fragment are discarded. This is what we call **depth testing** and it is done automatically by OpenGL under the hood.

However it is not enabled by default, so we need to specify OpenGL that we want it to use depth testing with the `glEnable` function. (the `glEnable` and `glDisable` function are used to enable/disable certain functionality of OpenGL)

```cpp
glEnable(GL_DEPTH_TEST);
```

Now that we are utilizing the depth buffer we also want to clear it before each render loop.

```cpp
glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);
```

