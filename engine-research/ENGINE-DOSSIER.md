# Engine Dossier — Borderlands GOTY Enhanced (Unreal Engine 3)

> One consolidated, living reference for this game's engine, filled in as the
> `PLAYBOOK.md` phases are worked. Chronological blow-by-blow belongs in the
> `dev-archive/` and `modding-notes/` folders; this file is the *distilled current
> truth*. Update it whenever a fact changes; correct false leads in place.

**Status:** M0, first static look (2026-09-15); the game has not been launched yet. · **VR-readiness verdict:** TBD.

## 1. Identity
- Game / build / version: Borderlands Game of the Year Enhanced, Steam build (app 729040), exe `Binaries\Win64\BorderlandsGOTY.exe` (linked 2022-07-22), with `Launcher.exe` beside it.
- Platform & store; unofficial port? (extra fragility/legal notes): Steam (PC). Official release, not a fan port.
- Legitimacy: owned copy confirmed.

## 2. Engine lineage
- Family / base engine and how it was modified: **Unreal Engine 3** (`UE3ShaderCompileWorker.exe`, `UE3 Editor`, `Engine Version: %d` strings) `[inferred-static 2026-09-15]`. PhysX, FMOD Ex, Bink 2, NVIDIA Texture Tools, wxWidgets `[inferred-static 2026-09-15]`. Two other projects on this account are UE3 (Alice: Madness Returns, Enslaved), so their camera work is a direct reference.
- Middleware (animation, audio, physics, megatexture, CUDA, etc.):
- Distinctive file formats / build tags / symbol naming: Data under `WillowGame\`, not yet looked at.

## 3. Binary & memory
- 32/64-bit, size, module base, ASLR behaviour (stable base? relocations?): **64-bit** (PE32+), 40.4 MB, image base `0x140000000`, ASLR on. ⚠️ `.bind` section plus a `.text` with entropy 8.0 — the **Steam DRM wrapper**, code encrypted on disk `[inferred-static 2026-09-15]`.
- Renderer API (D3D11/12, DXGI, GL, Vulkan) with evidence: Direct3D 9: `d3d9.dll` and `d3dx9_43.dll` are static imports `[inferred-static 2026-09-15]` (a proxy DLL is a straightforward way in). `d3dx11_43.dll` is imported too, and `NvStereoFixTexture` (UE3's NVIDIA 3D Vision hook) is in the strings.
- Developer console / cvar system present? how opened?: not yet investigated.

## 4. DRM / anti-debug & injection foothold
- DRM (CEG/Denuvo/GOG/none); launch-time-debugger behaviour: Steam DRM wrapper. No Denuvo string `[inferred-static 2026-09-15]`. The game has an optional SHiFT online login `[reported]`; not tested live.
- Attach workflow that works: not yet tested.
- Injection vector that works (proxy DLL name / injector / framework): not yet tested.

**🎮 2026-09-17 (home PC `RTX`, `/lm`) — FIRST LIVE LOOK.**
- **Runs:** ⚠️ **Steam launch opens the 2K launcher, which failed with `Initialization error. Please ensure the game files have not been corrupted or moved.`** on this install `[verified-live 2026-09-17, n=1]`. **Starting `Binaries\Win64\BorderlandsGOTY.exe` directly works** and reaches the `Press [ENTER]` title `[verified-live 2026-09-17, n=3]`.
- **With our file added:** A 64-bit `d3d9.dll` proxy loads and the game runs with it `[verified-live 2026-09-17, n=1]` — **but it only saw `D3DPERF_BeginEvent`/`D3DPERF_EndEvent`, and `d3d11.dll` + `dxgi.dll` are loaded in the process: the Enhanced edition draws with Direct3D 11**, not 9 `[verified-live 2026-09-17, n=1]`. This corrects the 2026-09-15 static reading (`d3d9.dll` as the renderer, `[inferred-static]`). The VR foothold should be a `dxgi.dll`/`d3d11.dll` proxy. The proxy comes from the shared generator `staging/_shared/proxy-gen/` (every export of the real system dll re-exported with the same ordinals; first call of each export logged). 
- **Windowed (for measuring; 1280×720 keeps aspect-keyed numbers the same on both PCs):** Command line `-windowed ResX=1280 ResY=720` → 1280×720 client window `[verified-live 2026-09-17, n=1]`. `WillowEngine.ini` `WindowMode=1` also gives a window, but the game rewrote `ResX/ResY` back to 1920×1080 at launch `[verified-live 2026-09-17, n=1]` — so use the command line for size. Backup: `WillowEngine.ini.bak-2026-09-17`.
- **Driving it:** Start the exe directly with the flags above. First run shows a Windows Firewall prompt (Cancel pressed). `WM_CLOSE` exits cleanly.
- **Dead ends:** The 2K launcher route failed on a clean install (`Initialization error…`) `[verified-live 2026-09-17, n=1]`. `WillowEngine.ini` `ResX/ResY` as the size route: rewritten by the game `[verified-live 2026-09-17, n=1]`. `d3d9.dll` as the renderer: disproved, it draws with D3D11 `[disproved 2026-09-17]`.

## 5. Threading & frame structure
- Immediate context only, or deferred contexts + command lists?:
- Which thread(s) do what; render-thread name(s):
- One-frame walkthrough (record → replay → present):

## 6. Camera & projection delivery (the crucial section)
- How the world transform reaches the GPU (shared VP buffer / per-draw MVP /
  other), with **shader-reflection / disassembly evidence**:
- Exact constant-buffer slot, parameter name(s), byte offset(s), layout,
  handedness, row/column convention:
- Where projection `P` / FOV comes from:
- The per-eye override maths (`K_eye = …`):

## 7. Constant-buffer fill mechanism
- Map/DISCARD ring / UpdateSubresource / D3D11.1 offset / **persistent map +
  memcpy** (trap):
- Can source contents be read cheaply (captured CPU pointer) or need staging
  read-back?:
- The chosen override patch point and why:

## 8. Pass inventory (by render target)
- Main scene (res/formats):
- Shadow passes (depth-only sizes):
- Post / AA chain (SMAA/TAA/motion vectors; downscale sizes):
- UI / HUD (how it's kept separate):

## 9. cvar / console cheat sheet
| command / cvar | effect | use |
|---|---|---|
| | | |

## 10. Autonomous harness recipe (this game)
- Launch to a known scene (commands used):
- In-process input / camera drive method that worked:
- Frame-capture method; where images land:

## 11. Dead ends & false leads (save future time)
- none yet.

## 12. Open risks toward the North Star
- ⚠️ The Steam DRM wrapper hides the code from static reading until unwrapped `[hypothesis]`.
- Otherwise friendly: Unreal Engine 3 on Direct3D 9, the same combination as two existing projects here.
