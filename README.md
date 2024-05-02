# SDL3 ported to the Zig build system

This is a port of SDL3 to the Zig build system, suitable for use with the Zig package manager. It supports cross-compiling from/to common desktop platforms (currently Windows and Linux) and attempts to closely replicate the results of building the upstream SDL using CMake with the default settings.

## Roadmap

The plan is to have completed the following tasks in time for the first official stable release of SDL3, SDL 3.2:

- [ ] Support cross-compiling from/to
  - [x] Windows (x86_64-windows-gnu)
  - [x] Linux (x86_64-linux-gnu)
  - [ ] macOS
- [ ] Build SDL3_test
- [ ] Trim the package size (prune unnecessary files, fetch dependencies lazily, etc.)

Non-goals:

- Configuring the build in great detail; you can choose between a shared and static library but that's it
- Building for non-desktop platforms like Android/iOS, the Web or consoles; you can invoke a different build system for these targets
- Providing Zig bindings (at least initially); `@cImport` works perfectly fine
- Disabling SDL's dynamic API; this goes against the wishes of SDL's authors and generally seems to be a bad idea

---

# Simple DirectMedia Layer (SDL) Version 3.0

https://www.libsdl.org/

Simple DirectMedia Layer is a cross-platform development library designed
to provide low level access to audio, keyboard, mouse, joystick, and graphics
hardware. It is used by video playback software, emulators, and popular games
including Valve's award winning catalog and many Humble Bundle games.

More extensive documentation is available in the docs directory, starting
with [README.md](docs/README.md). If you are migrating to SDL 3.0 from SDL 2.0,
the changes are extensively documented in [README-migration.md](docs/README-migration.md).

Enjoy!

Sam Lantinga (slouken@libsdl.org)
