Shaders are written in the C-like language GLSL. GLSL is tailored for use with graphics and contains useful features specifically targeted at vector and matrix manipulation.

Shaders always begin with a version declaration, followed by a list of input and output variables, uniforms and its main function. Each shader's entry point is at its main function where we process any input variables and output the results in its output variables. Don't worry if you don't know what uniforms are, we'll get to those shortly.

A shader typically has the following structure:
```c
#version version_number
in type in_variable_name;
in type in_variable_name;

out type out_variable_name;

uniform type uniform_name;

void main()
{
	// process input(s) and do some weird graphics stuff
	...
	// output processed stuff to output variable
	out_variable_name = weird_stuff_we_processed;
}
```

# Types

## Vectors

A vector in GLSL is a 2,3 or 4 component container for any of the basic types just mentioned. They can take the following form (`n` represents the number of components):

- ***vecn***: the default vector of `n` floats.
- ***bvecn***: a vector of `n` booleans.
- ***ivecn***: a vector of `n` integers.
- ***uvecn***: a vector of `n` unsigned integers.
- ***dvecn***: a vector of `n` double components.