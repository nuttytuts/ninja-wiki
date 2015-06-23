This page lists generators that can produce ninja files. Some of them are generally usable; some are one-off scripts specific to a given project.

## One-offs

- [TextMate 2's gen_build script](https://github.com/textmate/textmate/blob/master/bin/gen_build) is what generatest ninja files for TextMate's build. It's not meant to be generally useful.

- [Ninja's configure.py script](https://github.com/martine/ninja/blob/master/configure.py) is the script that generates ninja files for building ninja itself. This script is not meant to be generally usable.

## General Purpose

- [Ninja's ninja_syntax.py script](https://github.com/martine/ninja/blob/master/misc/ninja_syntax.py) (also available from PyPI as [ninja-syntax](https://pypi.python.org/pypi/ninja_syntax) package) is a low-level Python module that generates ninja files. This script is meant to be generally usable.

- [CMake](http://www.cmake.org/) is a general meta-build system. CMake runs on most platforms, and can generate project files in many formats, including ninja (use `-GNinja`).

- [GYP](https://code.google.com/p/gyp/) is also a general meta-build system that runs on most platforms. Among other formats, it can generate ninja manifest files as well as Visual Studio and Xcode project files that can optionally delegate to ninja for the actual build (use `-f ninja`).

- [GN](https://code.google.com/p/chromium/wiki/gn) generates ninja files. It's also mean to be generally usable.

- [Blueprint](https://github.com/google/blueprint/) takes build descriptions written in Go and generates ninja files from them.

- [Meson](https://jpakkane.github.io/meson/) is another general-purpose build system that generates Ninja scripts; to be precise, it uses Ninja as its default build generator.

- [Creator](https://github.com/NiklasRosenstein/creator) gives you the best of three worlds: The freedom of GNU Make, the power of Python and the performance of ninja.