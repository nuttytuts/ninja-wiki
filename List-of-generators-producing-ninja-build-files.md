This page lists generators that can produce ninja files. Some of them are generally usable, some are one-off scripts specific to a given project.

[CMake](http://www.cmake.org/) is a general meta-build system. CMake runs on most platforms, and can generate project files in many formats, including ninja (use `-GNinja`).

[GYP](https://code.google.com/p/gyp/) is also a general meta-build system that runs on most platforms. Among other formats, it can generate ninja manifest files as well as Visual Studio and Xcode project files that can optionally delegate to ninja for the actual build (use `-f ninja`).

[Blueprint](https://github.com/google/blueprint/) takes build descriptions written in Go and generates ninja files from them.

[GN](https://code.google.com/p/chromium/wiki/gn) generates ninja files.