
# Tutorial: how to use ImGui with GLUT

<p  align="center">
  <img src="https://user-images.githubusercontent.com/8225057/46304087-00035580-c5ae-11e8-8904-f27a9434574a.gif" width="600px">
</p>

[ImGui](https://www.ImGui.com/) is an open source C++ physics engine library that can be used in 3D simulations and games.
It's quite easy to use and you'll probably be familiar with some of its basic concepts if you used a game engine like Unity before.
In this tutorial we'll see how to build and install it, how to link it inside a Visual Studio project and how to use it together with GLUT.

Useful links:
- [GitHub repository](https://github.com/DanielChappuis/ImGui)
- [Documentation](https://www.ImGui.com/documentation.html)

## Installing the library

### Download the library
If you don't need the latest version of the library you can skip the building step and download files from [here]() (these files were built using Debug mode and Win32 platform).
Once you've done that you can go to the [install section](#use-the-library-inside-your-vs-project).

### Use the library inside your VS project
Now that you have all the files you need you can open your Visual Studio project and edit the following properties (be sure to edit properties of the right configuration).

Add the path to ImGui's include directory inside the "Additional include directories" setting under the "C/C++" section of the properties.

## Usage
In the user manual you can find detailed descriptions of how to use the various features of the library, [section 6](https://www.ImGui.com/usermanual#x1-170006) is about what you have seen in the HelloWorld. There's also an [API documentation](https://www.ImGui.com/documentation/api/html/index.html).

### Integration with GLUT

Now that you have ImGui at your disposal you can build a basic application that applies physics to objects rendered using GLUT. The result will look like this:

## Examples
### ImGui HelloWorld
```c++

```

### Integration of ImGui with GLUT
```c++

```
