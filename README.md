<!--
© 2024 Carl Åstholm
SPDX-License-Identifier: MIT
-->

# SDL ported to the Zig build system

This is a port of [SDL](https://libsdl.org/) to the Zig build system, packaged for the Zig package manager.

## Usage

Requires Zig 0.14.0 or 0.15.0-dev (master).

```sh
zig fetch --save git+https://github.com/stark26583/SDL.git
```

```zig
const sdl_dep = b.dependency("sdl", .{
    .target = target,
    .optimize = optimize,
    //.preferred_linkage = .static,
    //.strip = null,
    //.pic = null,
    //.lto = null,
    //.emscripten_pthreads = false,
    //.install_build_config_h = false,
});
const sdl_lib = sdl_dep.artifact("SDL3");
const sdl_test_lib = sdl_dep.artifact("SDL3_test");
```

## Examples

Example projects using this SDL package:

- [castholm/zig-examples/breakout](https://github.com/castholm/zig-examples/tree/master/breakout)
- [castholm/zig-examples/snake](https://github.com/castholm/zig-examples/tree/master/snake)
- [castholm/zig-examples/opengl-hexagon](https://github.com/castholm/zig-examples/tree/master/opengl-hexagon)

## Supported targets

Target \\ Host|Windows|Linux|macOS
-|:-:|:-:|:-:
`x86_64-windows-gnu`|✅|✅|✅
`aarch64-windows-gnu`|🧪|🧪|🧪
`x86_64-linux-gnu`|✅|✅|✅
`aarch64-linux-gnu`|🧪|🧪|🧪
`x86_64-macos-none`|❌|❌|🉑
`aarch64-macos-none`|❌|❌|🉑
`wasm32-emscripten-musl`|🉑|🉑|🉑
`wasm64-emscripten-musl`|🉑|🉑|🉑

Legend:

- ✅ Supported
- 🉑 Supported, but requires external SDKs
- 🧪 Experimental
- ❌ Not supported

### Windows

Building for x86-64 Windows from any host system works out of the box. AArch64 Windows support is experimental and not yet actively tested.

### Linux

Building for x86-64 Linux from any host system works out of the box. AArch64 Linux support is experimental and not yet actively tested.

The [SDL_linux_deps](https://github.com/castholm/SDL_linux_deps) package provides supplementary headers and source files required for compiling for Linux.

### macOS

Building for x86-64 or AArch64 macOS requires Xcode 14.1 or later to be installed on the host macOS system.

> [!NOTE]
> **Cross-compiling for macOS from Windows or Linux host systems is not supported** because [the Xcode and Apple SDKs Agreement](https://www.apple.com/legal/sla/docs/xcode.pdf) explicitly prohibits using macOS SDK files from non-Apple-branded computers or devices.

When building for non-native macOS targets (for example for x86-64 from an AArch64 Mac), you need to provide a path to the macOS SDK sysroot via `--sysroot`:

```sh
sysroot_path=$(xcrun --sdk macosx --show-sdk-path)
zig build -Dtarget=x86_64-macos-none --sysroot "$sysroot_path"
```

### Emscripten (web)

> [!IMPORTANT]
> Before you continue, please understand that **Emscripten is an advanced target** and that **building an SDL app for the Web is significantly more complicated compared to Windows, Linux or macOS**:
>
> - You will need to compile your app into a static library instead of an executable.
> - If you use libc headers (e.g. by translating C code to Zig), you will need to add the `include` directory inside the Emscripten sysroot to your header search paths.
> - To build the final HTML/JS/Wasm artifacts, you will need to invoke `emcc` using run steps.
> - You will likely need to do a lot of your own research and try out different combinations of `emcc` options to get satisfactory results. Make sure you read [the official Emscripten documentation](https://emscripten.org/docs/index.html) as well as [SDL's README on Emscripten](https://wiki.libsdl.org/SDL3/README/emscripten).
>
> In addition, note that Emscripten 4.0.4 or later will provide its own official port of SDL3 if you pass `--use-port=sdl3` to `emcc`. Depending on your use case, **you might not even need this package at all**.
>
> Refer to [the example projects](#examples) for examples on how to set up your `build.zig` for building for the Web.

Building for Emscripten requires an Emscripten development environment to be set up on the host system. It is strongly recommended that you use [the Emscripten SDK](https://emscripten.org/docs/getting_started/downloads.html) to install and manage Emscripten.

When building for Emscripten, you need to provide a path to the Emscripten sysroot via `--sysroot`:

```sh
cache_path=$(em-config CACHE)
sysroot_path="$cache_path/sysroot"
zig build -Dtarget=wasm32-emscripten-musl --sysroot "$sysroot_path"
```

Depending on the state of your Emscripten cache, you might need to run `embuilder build sysroot` to ensure that the Emscripten sysroot is built before you run `zig build`.

To build with [pthreads support](https://emscripten.org/docs/porting/pthreads.html), specify `.emscripten_pthreads = true`.

## License

This repository is [REUSE-compliant](https://reuse.software/). The effective SPDX license expression for the repository as a whole is:

```
(BSD-3-Clause OR GPL-3.0 OR HIDAPI) AND Apache-2.0 AND BSD-3-Clause AND CC0-1.0 AND HIDAPI AND HPND-sell-variant AND MIT AND SunPro AND Zlib
```

(This is identical to the upstream SDL repository, just expressed in more explicit terms.)

Copyright notices and license texts have been reproduced in [`LICENSE.txt`](LICENSE.txt), for your convenience.
