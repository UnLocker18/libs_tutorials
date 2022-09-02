
# Tutorial: how to use ImGui with GLUT

<p  align="center">
  <img src="https://user-images.githubusercontent.com/8225057/46304087-00035580-c5ae-11e8-8904-f27a9434574a.gif" width="600px">
</p>

[Dear ImGui](https://github.com/ocornut/imgui) is a graphical user interface library for C++. It is fast, portable, renderer agnostic and self-contained (no external dependencies).
It is designed to enable fast iterations and to empower programmers to create content creation tools and visualization / debug tools (as opposed to UI for the average end-user).

## Installing the library

The core of Dear ImGui is self-contained within a few platform-agnostic files which you can easily compile in your application/engine. They are all the files in the root folder of the repository (imgui*.cpp, imgui*.h).

No specific build process is required. You can add the .cpp files to your existing project.

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
