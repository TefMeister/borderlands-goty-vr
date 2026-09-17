# 2026-09-17 — first live look: Borderlands GOTY Enhanced

Home PC `RTX`, `/lm` session. The user asked for a first look at six games: does each run, and does it still run with our own file added.

## Does it run?

⚠️ **Steam launch opens the 2K launcher, which failed with `Initialization error. Please ensure the game files have not been corrupted or moved.`** on this install `[verified-live 2026-09-17, n=1]`. **Starting `Binaries\Win64\BorderlandsGOTY.exe` directly works** and reaches the `Press [ENTER]` title `[verified-live 2026-09-17, n=3]`.

## With our file added

A 64-bit `d3d9.dll` proxy loads and the game runs with it `[verified-live 2026-09-17, n=1]` — **but it only saw `D3DPERF_BeginEvent`/`D3DPERF_EndEvent`, and `d3d11.dll` + `dxgi.dll` are loaded in the process: the Enhanced edition draws with Direct3D 11**, not 9 `[verified-live 2026-09-17, n=1]`. This corrects the 2026-09-15 static reading (`d3d9.dll` as the renderer, `[inferred-static]`). The VR foothold should be a `dxgi.dll`/`d3d11.dll` proxy. The proxy comes from the shared generator `staging/_shared/proxy-gen/` (every export of the real system dll re-exported with the same ordinals; first call of each export logged). 

## Windowed mode

Command line `-windowed ResX=1280 ResY=720` → 1280×720 client window `[verified-live 2026-09-17, n=1]`. `WillowEngine.ini` `WindowMode=1` also gives a window, but the game rewrote `ResX/ResY` back to 1920×1080 at launch `[verified-live 2026-09-17, n=1]` — so use the command line for size. Backup: `WillowEngine.ini.bak-2026-09-17`.

## How it was driven

Start the exe directly with the flags above. First run shows a Windows Firewall prompt (Cancel pressed). `WM_CLOSE` exits cleanly.

## Dead ends

The 2K launcher route failed on a clean install (`Initialization error…`) `[verified-live 2026-09-17, n=1]`. `WillowEngine.ini` `ResX/ResY` as the size route: rewritten by the game `[verified-live 2026-09-17, n=1]`. `d3d9.dll` as the renderer: disproved, it draws with D3D11 `[disproved 2026-09-17]`.

## Not established

- Nothing past the menus: no gameplay was loaded, no camera data read.
- Every result is from one machine (`RTX`, 21:9 desktop) on one day.

## Next

- [PD] build a `dxgi.dll` proxy for it (generator one-liner) — the game draws with D3D11, not D3D9
- [PD] unwrap the Steam DRM layer on a copy, then compare against the Alice/Enslaved UE3 camera notes and fill in dossier §2–4
- [FLAT] run with the `dxgi.dll` proxy (direct exe launch, `-windowed ResX=1280 ResY=720`) and check whether the UE3 console opens
