# OpenGL basic open wondow example

The following example is a working example of rendering a single window on OSX (OS Ver 10.13), and Linux (Arch 5.5.8). 
This example includes the necessary header and library file - GLEW and GLFW - for basic OpenGL rendering. Please note, that you will have to compile GLEW and GLFW before starting with this example.

Currently this C Library is wired up in the `CmakeLists.txt`, thus you should onlu have to compile each source once per os installation. What does this mean, if you download this repo to a mac computer and compile both GLEW & GLFW sources, you cannot use those compiled libraries with another system like linux (and probably even another mac os system). This is because each computer is different and GLFW in particular compiles to the operating system its being compiled on. 

## Compile deps and basic example

The root `CMakeLists.txt` handles building GLFW and GLEW automatically as part of the main build — no need to enter each dependency directory manually.

From the root of the project, run:
```
$ mkdir build
$ cmake -B build .
$ make -C build
```

If you are using VS Codium or want clangd support, add the compile commands flag:
```
$ cmake -DCMAKE_EXPORT_COMPILE_COMMANDS=1 -B build .
$ make -C build
```

This generates a `compile_commands.json` file so clangd can resolve includes and dependencies correctly.

The output binary will be located in the `build` dir and named `window`.
To run it: `./build/window`.



### Notes:
GLUS has been removed from this example.... If you like to experiment with a more established library that uses GLEW and GLFW just like we are doing here (just not as bare bones), I recommended looking into [McNoppers](https://github.com/McNopper)'s [OpenGL](https://github.com/McNopper/OpenGL), and or his [GLUS repo](https://github.com/McNopper/GLUS).