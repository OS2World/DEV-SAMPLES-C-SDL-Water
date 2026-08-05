# Water — SDL2 Water Simulation Demo for ArcaOS/OS2

A real-time interactive water ripple simulation originally written in C for
DJGPP/SDL1 by Scott Scriven. This repository contains an SDL2 port targeting
**ArcaOS** and **OS/2** (GCC 9.2 / kLIBC).

![Water simulation running on ArcaOS](water320.bmp)

![Water ScreenShot](/wiki/water.png)

## Description

The demo renders a 320×200 water surface over a paletted BMP background image.
A cellular-automata height field drives the ripple physics. Multiple interactive
effects can be combined: rain, surfer, swirl, and blobs triggered by the mouse
or keyboard.

The original algorithm was conceived by Federico 'pix' Feroldi. Jason Hood
contributed optimizations. Scott Scriven wrote the SDL version available at
[http://www.xyzz.org/](http://www.xyzz.org/).

## Changes from the Original

- Migrated from SDL1 to **SDL2** (`SDL_CreateWindow`, `SDL_GetWindowSurface`,
  `SDL_UpdateWindowSurface`, `SDL_SetPaletteColors`).
- Removed `SDL_EnableUNICODE` / `event.key.keysym.unicode`; key handling now
  uses `event.key.keysym.sym`.
- Merged `BkGdImagePre`, `BkGdImage`, `BkGdImagePost` into a single contiguous
  buffer (`BkGdBuffer`) to guarantee adjacency required by the render algorithm
  (GCC does not guarantee consecutive placement of separate static arrays).
- Added `Makefile.os2` for GCC 9.2 / kLIBC / WLINK on ArcaOS/OS2.
- Source files reorganised into `src/`.

## Requirements

- ArcaOS 5.x or OS/2 Warp 4.x with kLIBC
- GCC 9.2 (OS/2 RPM build)
- SDL2 and SDL2 development headers (`SDL2_dll.a`)
- Open Watcom `wrc` / `wl` (for the OMF linker step via EMXOMFLD)

## Build

Open a `sh` session and set the EMXOMFLD environment variables so GCC uses
WLINK as the OMF linker:

```sh
export EMXOMFLD_TYPE=WLINK
export EMXOMFLD_LINKER=wl.exe
export EMXOMFLD_PRELINK=0
make -f Makefile.os2
```

To clean build artifacts:

```sh
make -f Makefile.os2 clean
```

## Usage

```
water.exe [background.bmp]
```

If no file is given, `water320.bmp` is loaded from the current directory.
The background must be a **320×200, 8-bit paletted BMP**.

## Controls

| Key / Action | Effect |
|---|---|
| Mouse button 1 | Small water blob |
| Mouse button 2 | Large water blob |
| `1` | Toggle surfer mode |
| `2` | Toggle rain mode |
| `3` | Toggle blob mode |
| `4` | Toggle swirl mode |
| `b` / `B` | Bump mode (positive / negative) |
| `Space` | Turn off all automatic effects |
| `6` | Random large waterdrop |
| `7` | Waterdrop at center |
| `z` | Distort / exaggerate water |
| `d` / `D` | Decrease / increase water density |
| `h` / `H` | Decrease / increase splash height |
| `r` / `R` | Decrease / increase waterdrop radius |
| `m` | Toggle water movement |
| `l` / `L` | Cycle light level |
| `w` | Water physics preset |
| `j` | Jelly physics preset |
| `s` | Sludge physics preset |
| `S` | SuperSludge physics preset |
| `` ` `` | Pause |
| `?` | Print help to stdout |
| `Esc` | Quit |

## File Layout

```
Makefile.os2      Build file for ArcaOS/OS2 (GCC 9.2/kLIBC)
water320.bmp      Default 320×200 paletted background image
src/
  water.c         Main program and water simulation
  fixsin.c        16.16 fixed-point sine/cosine lookup tables
  fps.c           Frames-per-second counter
  fixsin.h        fixsin declarations
  fps.h           fps declarations
  datatype.h      Portable type aliases (byte, word, dword, …)
```

## License

GNU General Public License v2 — see [COPYING](COPYING).
