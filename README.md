# JetStrike — SDL3 port

A faithful C reimplementation of *JetStrike* (1994, Shadow Software / Rasputin Software, DOS CD-ROM edition),
running natively on SDL3. It is not an emulator: the original programs (`JS_CDROM.EXE` and the intro
`INTRO.EXE`) were reverse-engineered, documented and rewritten in C, and the port reads the original game's files
at run time. The original data is not redistributed: you need your own copy of the DOS CD-ROM version.

## How to play (Windows)

You need the installed files of the DOS CD version (`JS_CDROM.EXE`, `INTRO\INTRO.EXE` and the `DATA`, `GFX`,
`MAP`, `MISC` and `PLANE` folders) and, for the music, the CD image (`.cue` with `.img`/`.bin`). They are not
included.

1. Download `jsport-…-win64.zip` from the [latest release](https://github.com/kylofon/jetstrike-sdl3/releases/latest).
2. Put your game files in a folder named `Game`.
3. Copy all files from the zip into the folder that holds `Game`, so that `JetStrike.exe` sits next to it:

   ```text
   JetStrike\
   ├── Game\                <- your original game files (JS_CDROM.EXE, INTRO\, DATA\, GFX\, MAP\, MISC\, PLANE\)
   │   └── MUSIC\           <- the CD music, made by the launcher's "Rip CD music..." (optional)
   ├── JetStrike.exe        <- the launcher
   ├── jsport.exe           <- the game
   ├── SDL3.dll
   └── (the other files from the zip)
   ```

4. Double-click `JetStrike.exe`. To get the music, press **Rip CD music…** once and choose the CD image's `.cue`
   file: the 14 CD audio tracks are written to `Game\MUSIC`. Choose the options and keys, and press **Play**.
   (`jsport.exe` also starts on its own with `Game` beside it.)

Keep the folder somewhere you can write, such as Documents, not Program Files: the game writes `JS.CFG` and the
saved games (`js_save.000` … `js_save.009`) in the game folder. If Windows says "Windows protected your PC", click
**More info**, then **Run anyway**. Alt+Enter switches to full screen.

## Status

* The whole game is ported: the intro, the menus, campaign, training and practice, the Aerolympics for two
  players, briefings and story screens, plane and weapon selection, the flight model, every weapon, the enemies,
  convoys, ground defences, the airbase, support aircraft and the tanker, bonuses, the debrief, saved games and the
  endings. Sound effects through an emulation of the game's Sound Blaster mixer; the music is the CD audio.
* The original's bugs are kept as they were (`port/QUIRKS.md`); fixes are planned for a separate *JetStrike
  Enhanced*.
* Not ported: the Gravis UltraSound sound path (the Sound Blaster one is used).
* First release: please report any difference from the original in the issues.

## The launcher

`JetStrike.exe` replaces the original's `CONFIG.EXE` and starts the game (`jsport/launcher/README.md`):

* **Game files**: the game folder and `jsport.exe`, with a check that the files are there, and whether the CD
  music has been ripped. **Rip CD music…** writes the tracks from a CD image (`.cue` + raw `.img`/`.bin`).
* **JS.CFG**: CD music, sound effects, detail (parallax backdrops), joystick, and the 18 game keys, saved in the
  original's `JS.CFG` format.
* **Port options**: Sound Blaster rate (19 920 Hz as the game was designed, or 3 906 Hz, the rate the original
  actually programs because of a bug), window size, full screen, skip the intro.

Settings are remembered in `%APPDATA%\JetStrike\settings.ini`.

## Controls

The flight keys are set in the launcher (the original keys: arrows to fly, Space to fire the gun, Alt / Ctrl the
left / right weapon, Enter switches hover / agile, U undercarriage, E eject, P pause, Tab autothrottle, B
briefing, keypad `*` + arrows look around, Backspace follow view, Shifts change page in the selection screens).
Fixed keys: 1 … 9, 0 throttle; F1 … F10 save / load slot at the briefing; Space / Enter confirm; land and stop at
your base and hold Down to rearm; Esc or D ends the intro. A gamepad works as the joystick.

## How it was made

`PLAN.md` (phases and decisions), `FORMATS.md` and `port/formats/` (file formats), `port/RE_GUIDE.md` (executable
map), `port/spec/` (the subsystem specs the C code follows function by function), `jsport/PORTING.md` (the port's
structure, building and testing). The `tools/` folder has the LE loader, the Ghidra pipeline and decoders for
every file.

## Building

Windows with MSYS2 `mingw64` (gcc, ninja, `mingw-w64-x86_64-sdl3`, and for the launcher
`mingw-w64-x86_64-wxwidgets3.2-msw`):

```bash
cmake -S jsport -B jsport/build -G Ninja -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++ -DCMAKE_BUILD_TYPE=Release -DJS_LAUNCHER=ON
cmake --build jsport/build
```

## Licence

The port's code is MIT licensed (`LICENSE`). JetStrike is © Shadow Software / Rasputin Software; none of its
files are included.
