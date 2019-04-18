This page lists generators that can produce ninja files. Some of them are generally usable; some are one-off scripts specific to a given project.

## General Purpose

- [bfg9000](https://github.com/jimporter/bfg9000) is a cross-platform build configuration system with an emphasis on making it easy to define how to build your software. It converts a Python-based build script into the appropriate files for your underlying build system of choice (Make, Ninja, or MSBuild).

- [Blueprint](https://github.com/google/blueprint/) takes build descriptions written in Go and generates ninja files from them.

- [BuildFox](https://github.com/beardsvibe/buildfox) is a minimalistic ninja generator with focus on less overhead and explicit configuration files. Plus it also generates IDE projects that use ninja as build system.

- [CMake](http://www.cmake.org/) is a general meta-build system. CMake runs on most platforms, and can generate project files in many formats, including ninja (use `-GNinja`).

- [Craftr](https://github.com/craftr-build/craftr) is a powerful, modular Python based build system that aims for cross-platform compatibility and native support for various toolchains and libraries.

- [GENie](https://github.com/bkaradzic/GENie#genie---project-generator-tool) - Project generator tool.

- [GN](https://gn.googlesource.com/gn/) is the current meta-build system for the Chromium project and aims to be faster than GYP (the previous system) while generating more readable, maintainable build files.

- [GYP](https://code.google.com/p/gyp/) is also a general meta-build system that runs on most platforms. Among other formats, it can generate ninja manifest files as well as Visual Studio and Xcode project files that can optionally delegate to ninja for the actual build (use `-f ninja`).

- [Jagen](https://github.com/bazurbat/jagen) just another build system generator tool with focus on management of projects consisting of multiple packages. It compiles declarative rules to build file for Ninja which does the actual work.

- [kati](https://github.com/google/kati) is a GNU make clone that converts Makefiles to ninja build files. The main goal of this tool is to speed up incremental builds of Android.

- [Meson](http://mesonbuild.com/) is another general-purpose build system that generates Ninja scripts; to be precise, it uses Ninja as its default build generator.

- [pyrate](https://github.com/pyrate-build/pyrate-build) is a tool to generate ninja files for simple projects using a python based build configuration script

- [Rōnin](https://github.com/tliron/ronin) is a straightforward but powerful build system based on Ninja and Python

## One-offs

These links are to projects that have written their own custom ninja generation logic:

- [TextMate 2's gen_build script](https://github.com/textmate/textmate/blob/master/bin/gen_build) is what generates ninja files for TextMate's build.

- [Ninja's configure.py script](https://github.com/martine/ninja/blob/master/configure.py) is the script that generates ninja files for building ninja itself.

- [IRPF90 has a ninja.py](https://github.com/scemama/irpf90/blob/master/src/ninja.py) seems to use a combination of Makefiles and ninja files. 

- [kninja](https://github.com/rabinv/kninja/blob/master/kninja.py) is an experimental ninja file generator for the Linux kernel's build system.

- [mongo_module_ninja](https://github.com/RedBeard0531/mongo_module_ninja/) A module for MongoDB's build system to turn SCons into a ninja generator.

- [BuckleScript's `bsb` build tool](http://bucklescript.github.io/) uses Ninja to compile OCaml to JavaScript.

## Utilities

- [Ninja's ninja_syntax.py script](https://github.com/martine/ninja/blob/master/misc/ninja_syntax.py) (also available from PyPI as [ninja-syntax](https://pypi.python.org/pypi/ninja_syntax) package) is a low-level Python module that generates ninja files.  It is just a generator of the ninja syntax, and unlike the above build tools it doesn't provide any higher-level assistance in setting up your build.

- [ninja_syntax.lua](https://github.com/juntalis/ninja_syntax.lua) is a Lua-based clone of the above python script.

- [ninja-build-gen](https://www.npmjs.com/package/ninja-build-gen) is Ninja generator library for that runs on NodeJS. 

- [language-ninja](https://hackage.haskell.org/package/language-ninja) is a Haskell library for parsing, pretty-printing, and "compiling" Ninja files.