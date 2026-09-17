# A 3DMigoto stereo fix exists for this exact build, and the BL1 PythonSDK gives script-level access

**Status:** 🆕 new · **Priority:** medium-high.

## What is public

- **DJ-RK's 3D fix for Borderlands GOTY Enhanced** uses **3DMigoto 1.3.15** (a D3D11 wrapper) and fixes
  lighting and dynamic shadow placement, distant light clipping, decals, water and halos, with a dynamic
  3D crosshair and cyclable HUD depth `[reported]`. Sun shafts stayed unfixed because "the game uses an
  unconventional shader method compared to other UE3 titles" `[reported]`. It installs to
  `Binaries\Win64\` and the game is "very intolerant of Alt+Tab" `[reported]`.
- **The BL1 PythonSDK** (`bl-sdk/willow1-mod-manager`, built on `bl-sdk/unrealsdk`) supports **BL1
  Enhanced** and adds a MODS menu; community mods run console commands from hotkeys `[reported]`.
- apple1417's write-up on handling Unreal Engine's changing object layouts is about exactly this SDK
  family `[reported]`.

## Why it matters here

1. **3DMigoto is D3D11, and so is this game** (board: it draws with D3D11). The fix's `d3dx.ini` and
   shader overrides name the shaders that break under stereo, and which constant buffer the fix reads
   the projection from `[hypothesis]` — a head start on the camera search this project shares with Alice
   and Enslaved.
2. **The PythonSDK reaches UE3 objects from script**, including the player camera, which may make the
   console question moot and gives a way to read camera values without disassembly `[hypothesis]`.
3. The Alt+Tab fragility is worth knowing before any windowed, unattended test.

## Next step

Read the fix's `d3dx.ini` and `ShaderFixes` file names; check the PythonSDK's documentation for camera
access on BL1E.

## Sources

- Helix Mod, Borderlands GOTY Enhanced — <https://helixmod.blogspot.com/2019/10/borderlands-game-of-year-enhanced.html>
- bl-sdk — <https://github.com/bl-sdk/unrealsdk>, <https://github.com/bl-sdk/willow1-mod-manager>, <https://bl-sdk.github.io/willow1-mod-db/>
- apple1417, "Handling Unreal Engine's changing object layouts while modding" — <https://apple1417.dev/posts/2025-01-15-unreal-object-layouts>
