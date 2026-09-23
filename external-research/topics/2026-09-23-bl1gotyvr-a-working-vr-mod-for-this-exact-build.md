# BL1GOTYVR: someone has already built a working VR mod for this exact game

**Found:** 2026-09-23, `/gr` estate sweep, through phunkaeg's *VR Modding Playbook* (`sources.yml` →
`BL1GOTYVR`), then read at the source.
**Source:** Mastersellz, *BL1GOTYVR* — <https://github.com/Mastersellz/BL1GOTYVR> (last commit
2026-09-13; release V0.5.6 and later; Nexus page <https://www.nexusmods.com/borderlandsgotyenhanced/mods/95>).
**Licence:** none found in the repository (no `LICENSE` file, GitHub reports none). Read-only as
always; nothing is copied from it.

## What it is

A native OpenXR VR mod for **Borderlands GOTY Enhanced (2019), Win64 / D3D11** — the same build this
project targets. The author reports it working and headset-tested on SteamVR/Steam Link and on
Virtual Desktop (VDXR), with geometric stereo, positional 6DoF, motion-controller aiming through an
XInput bridge, a VR HUD and a settings tool `[reported]`. It loads through a **`dxgi.dll` proxy** and
never patches the game executable `[reported]`.

## Why it matters here

1. **It answers two of our open rows before we build anything.** Our board's first `[PD]` row is
   "build a `dxgi.dll` proxy — the game draws with D3D11, not D3D9". That is exactly the door this
   mod uses, and its hook notes confirm the whole frame ends in a standard D3D11 swap-chain back
   buffer (`R8G8B8A8_UNORM`, UI included) `[reported]`.
2. **It says where the camera is, and where it is not.** The author's `docs/HOOK_RESEARCH.md`
   reports, for game version 1.5.0.0 `[reported]`:
   - The usual UE3 `PlayerController.PlayerCamera` is **null during normal play** in this edition.
     The live view sits directly on `WillowPlayerController` as `CalcViewLocation`,
     `CalcViewRotation` and `CachedFOVAngle`, found at run time through UE3's own property
     reflection rather than fixed offsets.
   - `Default__*` objects (class defaults) look like cameras and are not. An earlier heuristic of
     theirs accepted `Default__WillowPlayerCamera` and read nonsense.
   - The per-frame hook point is `WillowGameViewportClient::Draw`, reached through a secondary
     vtable on the live viewport object.
3. **It records a trap that would cost us a crash hunt.** Calling `GameViewportClient::Draw` **twice
   in one frame corrupts the UE3 heap** in this build and ends in `0xC0000374` `[reported]`. Their
   working stereo therefore either alternates eyes (one Draw per frame, camera saved, posed,
   restored) or, experimentally, asks UE3's own render-command constructor to build **two views in
   one command**, so the engine does every allocation and teardown itself.
4. **Matrices written after the render command runs are too late.** They report that the scene view
   is owned by a render-thread command that may destroy it before returning, and that restoring its
   matrices afterwards caused a general protection fault `[reported]`. Two separate restore caches
   inside the view also have to receive the pose.

## What this means for the project

⚠️ **The bigger question is not technical, and it is Tefa's.** A working, headset-tested VR mod for
this exact game already exists. Before our own build goes further, it is worth deciding whether this
project should (a) carry on as its own mod, (b) stop and simply play theirs, or (c) aim at something
theirs does not do. Nothing here decides that; it only puts the fact on the table.

If the project carries on, the concrete next step is cheap: play their release once in the headset
as a **live reference** for what "correct" looks like on this build, then use their hook notes as
the map for our own `dxgi.dll` route, rebuilt in our own code.

## Transfers to the other UE3 projects

The "never call `Draw` twice" trap and the "ask the engine for two views" route are UE3-general
`[hypothesis]` for Bulletstorm (UE3, D3D11, 64-bit, the closest sibling), Enslaved and Alice (UE3,
D3D9, 32-bit). Handed to the cross-engine library for `/sr`, not copied into each sibling.

## Credits

Mastersellz (BL1GOTYVR); phunkaeg (*VR Modding Playbook*, where this was found).
