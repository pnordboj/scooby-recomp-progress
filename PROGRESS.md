# Scooby-Doo! Night of 100 Frights — Native PC Port (GameCube Recomp)

_Progress generated 2026-07-10 by `tools/gen_progress.py`. Run it after a session to refresh._

![progress](progress.svg)

## Overall (boot → playable milestones)

`█████████████████████░░░░░░░░░` **72%**

## Metrics

| Metric | Value | |
|---|---|---|
| Static recompilation | 416,072 / 416,072 instructions (100%) | `████████████████████` |
| Runtime code-map coverage | 321 / 407 code pages entered (78.9%) | `████████████████░░░░` |
| Runtime dispatch entry points executed | 10,181 | |
| Deepest instrumented run | 2,766,021,032 dispatch blocks | |

_Static recompilation = share of the game's PowerPC code DolRecomp emitted C for (the Gekko decoder handled everything, incl. paired singles)._
_Code-map coverage = share of 4KB pages of game code the harness has entered — grows as the game gets deeper into boot/gameplay. (Menus/boot exercise a small slice of a game's code; in-game play is what pushes this up.)_

## Milestones

- [x] Game extracted (RVZ -> DOL + 329 asset files)
- [x] main.dol statically recompiled (0 unknown instructions)
- [x] Runtime harness: DOL loads, recompiled code executes
- [x] Hardware init survives (PI / DSP / ARAM / AI mailboxes)
- [x] OS threading + interrupt delivery (retrace, wakeups)
- [x] DVD/DI serving: inquiry + real file reads from disc image
- [x] Engine type-registry boot (bss-loader root-cause fix)
- [x] VI framebuffer presented in a host window (first pixels)
- [x] GX software rasterizer: 2D UI renders (textures + CI4/CI8)
- [x] SI controller detected (transfer buffer + SI interrupt)
- [x] Memory-card dialog advances (input gate cracked)
- [x] Post-dialog level/world data streams from disc
- [x] World loads: RenderWare models instantiate (chars/powerups)
- [x] Post-load scene begins rendering (loading UI + sky)
- [x] 3D intro scene renders natively (Scooby on screen!)
- [x] Intro cutscene plays (gang animated, Z-buffer, blending)
- [x] TITLE SCREEN renders (logo + 3D haunted house)
- [x] First level loads + world renders in-game (h001)
- [ ] Title screen menu interactive (input-driven)
- [ ] Full input path (game input manager fed each frame)
- [ ] 3D rendering (Z-buffer, TEV stages, lighting)
- [ ] In-game / level playable
- [ ] Audio (DSP-HLE / music + SFX)
- [ ] Stability / performance / packaging pass

## Subsystem status

| Subsystem | Status | Notes |
|---|---|---|
| CPU (Gekko/PPC750 + paired singles) | ✅ done | DolRecomp static recompilation, 0 unknown instructions |
| OS threading / interrupts | ✅ done | retrace emulation, OSWakeupThread, DI/SI delivery |
| DVD / DI | ✅ works | inquiry + file reads from disc.iso; async completion latency to tune |
| VI (video out) | ✅ works | XFB → Win32 window blit each vsync |
| GX (GPU) | 🟨 renders | software rasterizer: tris/strips, textures, TLUT, Z-buffer, blending; 3D level scenes render (lighting/TEV polish pending) |
| SI (controllers) | 🟨 detected | pad probed + detected; not yet driving gameplay |
| EXI / memory card | 🟨 stub | "no card" path works; saving not implemented |
| DSP / AI (audio) | 🟥 stubs | mailbox handshake HLE only; no sound |

## Current frontier

**The first level (h001) loads and renders natively** — the intro scene (Shaggy with
Scooby Snacks) and the level world stream in and draw (millions of triangles/frame).
Unblocked by fixing a DVD-completion timing race that starved the level streamer.
Next: wire controller input to gameplay, then lighting/TEV polish and audio.
