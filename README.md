# Flappy Bird — Powered by Bitty

A Flappy Bird clone for the **Nintendo Game Boy / Game Boy Color**, written in C and
built with [GBDK-2020](https://github.com/gbdk-2020/gbdk-2020). The build produces a
real `.gb` ROM that runs on emulators and original hardware.

## Gameplay

Tap **A** to flap and keep the bird in the air. Fly through the gaps between the
pipes — each pipe cleared scores a point. Hitting a pipe, the floor, or the ceiling
ends the run. Your best score is kept as the high score for the session, and a
bronze/silver/gold/platinum medal is awarded on the end screen based on how well
you did.

| Input | Action |
| ----- | ------ |
| **A** | Flap / start / restart |

## Project structure

```
make.bat                  Build script (Windows)
lib/romusage.exe          ROM size report tool (run after build)
headers/                  Header files
  Common.h                Game state IDs and shared globals
  Graphics/               Generated asset headers
  States/                 Per-state function declarations
source/default/           C source
  main.c                  Entry point + main loop / state machine
  Common.c                Bird physics, score rendering
  Utilities.c             Shared helpers
  States/                 GameFirstLoad, GameplayStart, CoreGameLoop, GameplayEnd
  Graphics/               Generated tile/map/sprite data
graphics/                 Source art (PNG, Aseprite, GBTD/GBR)
dist/                     Build output (gitignored)
bin/                      Intermediate objects (gitignored)
```

The game runs as a simple state machine driven from `main.c`:
`GameFirstLoad → GameplayStart → CoreGameLoop → GameplayEnd → GameplayStart …`

## Building

**Requirements**

- Windows
- [GBDK-2020](https://github.com/gbdk-2020/gbdk-2020) installed at `C:\gbdk`

`make.bat` expects GBDK at `C:/gbdk` (`SET GBDK_HOME=C:/gbdk`). If yours is
installed elsewhere, edit that line in `make.bat`.

**Build**

```bat
make.bat
```

This compiles every `.c` file under `source/default/`, links them into a ROM, and
prints a ROM-usage breakdown. The output is:

```
dist/Flappy-Bird-Powered-by-Bitty.gb
```

`bin/` and `dist/` are regenerated on every build and are not tracked in git.

## Running

Open `dist/Flappy-Bird-Powered-by-Bitty.gb` in a Game Boy emulator (e.g.
[BGB](https://bgb.bircd.org/), [SameBoy](https://sameboy.github.io/),
[Emulicious](https://emulicious.net/)) or flash it to a flash cart for real
hardware.

## Regenerating graphics

The committed files under `source/default/Graphics/` and `headers/Graphics/` are
generated from the PNGs in `graphics/` using GBDK's `png2Asset`. The exact commands
are kept (commented out) near the top of `make.bat` for reference if the art is
changed.

## License

[MIT](LICENSE) — Copyright (c) 2026 Bitty
