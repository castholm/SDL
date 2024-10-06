# SDL3 ported to the Zig build system

This is a port of SDL3 to the Zig build system, packaged for the Zig package manager.

This port is under active development. Currently, only `x86_64-windows-gnu` is supported, with support for `x86_64-linux-gnu` to follow shortly.

## Usage

```sh
zig fetch --save git+https://github.com/castholm/SDL
```

```zig
const sdl_dep = b.dependency("SDL", .{
    .target = target,
    .optimize = optimize,
    .SDL3_static = false,
});
const sdl3_lib = sdl_dep.artifact("SDL3");
const sdl3_test_lib = sdl_dep.artifact("SDL3_test");
```

---

Original README:

# Simple DirectMedia Layer (SDL) Version 3.0

https://www.libsdl.org/

Simple DirectMedia Layer is a cross-platform development library designed
to provide low level access to audio, keyboard, mouse, joystick, and graphics
hardware. It is used by video playback software, emulators, and popular games
including Valve's award winning catalog and many Humble Bundle games.

More extensive documentation is available in the docs directory, starting
with [README.md](https://github.com/libsdl-org/SDL/blob/e292d1f5ace469f718d7b6b4dec8c28e37dcaa0e/docs/README.md). If you are migrating to SDL 3.0 from SDL 2.0,
the changes are extensively documented in [README-migration.md](https://github.com/libsdl-org/SDL/blob/e292d1f5ace469f718d7b6b4dec8c28e37dcaa0e/docs/README-migration.md).

Enjoy!

Sam Lantinga (slouken@libsdl.org)
