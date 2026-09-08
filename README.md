# 3DS Obby

A simple 3D obstacle-course platformer for the Nintendo 3DS, built with
libctru + Citro3D.

**Controls**
- Circle Pad — move
- A — jump
- START — quit

Reach the gold platform at the end to win (it resets after a few seconds).
Fall off and you respawn at the start.

## What's in here

```
3ds-obby/
├── Makefile          - devkitARM/Citro3D build rules (builds .3dsx and .cia)
├── app.rsf           - config makerom uses to package the .cia
├── icon.png          - 48x48 placeholder app icon (replace with your own)
├── banner.png        - 256x128 placeholder banner (replace with your own)
├── audio.wav         - silent placeholder banner sound
└── source/
    ├── main.c         - game logic: physics, camera, platforms, rendering
    └── vshader.v.pica - PICA200 vertex shader (position + uniform color)
```

## Requirements

You need [devkitPro](https://devkitpro.org/wiki/Getting_Started) installed
with the **3DS development** package group, which includes:
- `devkitARM` (the compiler toolchain)
- `libctru` (3DS system library)
- `citro3d` (3D graphics library)
- `general-tools` (provides `bannertool` and `makerom`, needed for the `.cia`)

### Quick install (via devkitPro's pacman)

```bash
# Linux/macOS, after installing devkitPro's package manager (dkp-pacman):
sudo dkp-pacman -S 3ds-dev
```

On Windows, use the devkitPro graphical installer and select the 3DS
development components.

Make sure the `DEVKITARM` and `DEVKITPRO` environment variables are set
(the devkitPro installer does this for you; on Linux/macOS you may need
to add this to your shell profile):

```bash
export DEVKITPRO=/opt/devkitpro
export DEVKITARM=$DEVKITPRO/devkitARM
export PATH=$DEVKITARM/bin:$DEVKITPRO/tools/bin:$PATH
```

## Building

From inside the `3ds-obby/` folder:

```bash
make
```

This produces:
- `3ds-obby.3dsx` — run via the Homebrew Launcher, no install needed
- `3ds-obby.cia` — install as a proper app via FBI or a CIA installer

If you only want the `.3dsx` (skip the icon/banner/makerom step entirely),
that's the easier path — just copy it to `/3ds/` on your SD card and
launch it from the Homebrew Launcher.

## Installing the .cia

1. Copy `3ds-obby.cia` to your SD card (anywhere, e.g. the root).
2. On your 3DS, open **FBI** (a homebrew CIA installer/manager — install it
   first if you don't have it).
3. Navigate to the file, select it, and choose **Install CIA**.
4. It'll appear on your Home Menu like any other app.

You can also install it onto Citra/Azahar (3DS emulators) by dragging the
`.cia` into the emulator, or just running the `.3dsx` directly.

## Customizing

- **Icon/banner**: replace `icon.png` (48x48) and `banner.png` (256x128)
  with your own art, and `audio.wav` with a short sound clip, then rebuild.
- **Level layout**: edit `buildLevel()` in `source/main.c` — each
  `addPlatform(x, y, z, sizeX, sizeY, sizeZ, r, g, b, moveAmplitude, moveSpeed, isGoal)`
  call adds one platform. Set `moveAmplitude` nonzero for a platform that
  slides back and forth.
- **Difficulty**: tweak `GRAVITY`, `JUMP_SPEED`, and `MOVE_SPEED` near the
  top of `main.c`.
