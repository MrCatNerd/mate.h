# Mate
Still not super stable but there are running tests for most platforms. It's already useable and should serve 99% of your builds as far as your manage dependencies.
<div align="left">
  <img src="https://img.shields.io/github/stars/TomasBorquez/mate.h?style=flat&color=blue&label=Stars" alt="Stars">
  <img src="https://img.shields.io/github/commit-activity/t/TomasBorquez/mate.h?style=flat&color=blue&label=Total%20Commits" alt="Commits">
  <img src="https://img.shields.io/github/actions/workflow/status/TomasBorquez/mate.h/run-tests-linux.yml?style=flat&label=Linux%20Pipeline" alt="Linux Pipeline">
  <img src="https://img.shields.io/github/actions/workflow/status/TomasBorquez/mate.h/run-tests-windows.yml?style=flat&label=Windows%20Pipeline" alt="Windows Pipeline">
  <img src="https://img.shields.io/badge/license-MIT-yellow.svg" alt="License: MIT">
</div>

<br>

[![Discord](https://discord.com/api/guilds/1372689586341285979/widget.png?style=banner2)](https://discord.gg/TSuGhzas5V)

## About
Mate is a build system for C in C, and all you need to run it is a C compiler. If you want to learn more about it, check out our [examples](./examples), which should
teach you everything you need to know to use it.

Here is a simple example of a `mate.c`:

```c
#define MATE_IMPLEMENTATION // Adds function implementations
#include "mate.h"

i32 main() {
  StartBuild();
  {
    Executable executable = CreateExecutable((ExecutableOptions){
        .output = "main",   // output name, in windows this becomes `main.exe` automatically
        .flags = "-Wall -g" // adds warnings and debug symbols
    });

    // Files to compile
    AddFile(executable, "./src/main.c");

    // Compiles all files parallely with samurai
    InstallExecutable(executable);

    // Runs `./build/main` or `./build/main.exe` depending on the platform
    RunCommand(executable.outputPath);
  }
  EndBuild();
}
```

To use it all you need is [mate.h](./mate.h) file on the same folder you have the `mate.c` example file and you can run by `gcc ./mate.c -o ./mate && ./mate`, after running once
it will auto detect if you need to rebuild based on `mate-cache.ini`, so you can just run `./mate` directly and it'll rebuild itself.

Under the hood we use [Samurai](https://github.com/michaelforney/samurai), which is a ninja rewrite in C. We boostrapped it to `mate.h`, which compiles it on the first run. After that it is cached;
thanks to this, builds are extremely fast (the first build usually takes a bit longer, but the second one should be very fast). If you want even faster builds, you can use [TCC](https://bellard.org/tcc/),
which is a tinier and faster C compiler. On medium sized projects, our builds go from `600ms` to `20ms` because `tcc` was made to be **very fast** to compile.

## Operating Systems
| OS | Status | Notes |
|---|---|---|
| **Linux** | ✅ Supported | Uses [Samurai](https://github.com/michaelforney/samurai) under the hood |
| **Windows** | ✅ Supported | Requires Ninja build system (Samurai [in progress](https://github.com/TomasBorquez/mate.h/issues/2)) |
| **[MacOS](https://github.com/TomasBorquez/mate.h/issues/17)** | ⚠️ Supported | Limited testing done |
| **FreeBSD** | ✅ Supported | TCC is not stable on FreeBSD, use GCC or Clang(LLVM) |

## Compilers
| Compiler | Status | Notes |
|---|---|---|
| [**GCC**](https://gcc.gnu.org/) | ✅ Supported | Primary development target with best support |
| [**Clang**](https://releases.llvm.org/) | ✅ Supported | Works perfectly well too |
| [**TinyC**](https://bellard.org/tcc/) | ✅ Supported | Recommended if you want fast compile times |
| [**MSVC**](https://visualstudio.microsoft.com/vs/features/cplusplus/) | ⚠️ Supported | Limiting testing done, MinGW is recommended instead |

## Contributing
You wanna help out contributing? Great, read our [contribution guidelines](./.github/CONTRIBUTING.md) for information about the project and look into our [TODOS](./TODOS.md) or 
our [issues](https://github.com/TomasBorquez/mate.h/issues) to see what you would like to contribute on, documentation contributions are also welcomed :)

## Acknowledgments
This project incorporates code from [Samurai](https://github.com/michaelforney/samurai), which is primarily licensed under ISC license by Michael Forney,
with portions under Apache License 2.0 and MIT licenses. The full license text is available in the [LICENSE-SAMURAI.txt](./LICENSE-SAMURAI.txt) file.

Inspired from `build.zig`, `nob.h`, `CMake` and `Meson` build systems, aswell as other unrelated C libraries like `raylib` and `clay.h` in terms of interfaces and implementations.
