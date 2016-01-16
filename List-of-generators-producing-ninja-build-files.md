This page lists generators that can produce ninja files. Some of them are generally usable; some are one-off scripts specific to a given project.

## General Purpose

- [CMake](http://www.cmake.org/) is a general meta-build system. CMake runs on most platforms, and can generate project files in many formats, including ninja (use `-GNinja`).

- [GYP](https://code.google.com/p/gyp/) is also a general meta-build system that runs on most platforms. Among other formats, it can generate ninja manifest files as well as Visual Studio and Xcode project files that can optionally delegate to ninja for the actual build (use `-f ninja`).

- [GN](https://chromium.googlesource.com/chromium/src/+/master/tools/gn/README.md) generates ninja files. It's also mean to be generally usable.

- [Blueprint](https://github.com/google/blueprint/) takes build descriptions written in Go and generates ninja files from them.

- [Meson](http://mesonbuild.com/) is another general-purpose build system that generates Ninja scripts; to be precise, it uses Ninja as its default build generator.

- [Craftr](https://github.com/craftr-build/craftr) is a Python based meta build system for Ninja

- [bfg9000](https://github.com/jimporter/bfg9000) is a cross-platform build configuration system with an emphasis on making it easy to define how to build your software. It converts a Python-based build script into the appropriate files for your underlying build system of choice (Make, Ninja, or MSBuild).

- [kati](https://github.com/google/kati) is a GNU make clone that converts Makefiles to ninja build files. The main goal of this tool is to speed up incremental builds of Android.

- [BuildFox](https://github.com/beardsvibe/buildfox) is a minimalistic ninja generator with focus on less overhead and explicit configuration files. Plus it also generates IDE projects that use ninja as build system.

- [pyrate](https://github.com/pyrate-build/pyrate-build) is a tool to generate ninja files for simple projects using a python based build configuration script

## One-offs

These links are to projects that have written their own custom ninja generation logic:

- [TextMate 2's gen_build script](https://github.com/textmate/textmate/blob/master/bin/gen_build) is what generates ninja files for TextMate's build.

- [Ninja's configure.py script](https://github.com/martine/ninja/blob/master/configure.py) is the script that generates ninja files for building ninja itself.

- [IRPF90 has a ninja.py](https://github.com/scemama/irpf90/blob/master/src/ninja.py) seems to use a combination of Makefiles and ninja files. 

## Utilities

- [Ninja's ninja_syntax.py script](https://github.com/martine/ninja/blob/master/misc/ninja_syntax.py) (also available from PyPI as [ninja-syntax](https://pypi.python.org/pypi/ninja_syntax) package) is a low-level Python module that generates ninja files.  It is just a generator of the ninja syntax, and unlike the above build tools it doesn't provide any higher-level assistance in setting up your build.