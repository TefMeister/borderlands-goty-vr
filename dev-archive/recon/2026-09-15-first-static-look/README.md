# First static look (2026-09-15)

Read from the installed Steam copy on the home PC (`C:\Steam\steamapps\common\BorderlandsGOTYEnhanced`), without launching the game.
Every claim below is `[inferred-static 2026-09-15]` unless tagged otherwise: it comes from reading file
headers and strings, not from running anything.

- **Install:** 19 GB.
- **Identity:** Borderlands Game of the Year Enhanced, Steam build (app 729040), exe `Binaries\Win64\BorderlandsGOTY.exe` (linked 2022-07-22), with `Launcher.exe` beside it.
- **Engine:** **Unreal Engine 3** (`UE3ShaderCompileWorker.exe`, `UE3 Editor`, `Engine Version: %d` strings) `[inferred-static 2026-09-15]`. PhysX, FMOD Ex, Bink 2, NVIDIA Texture Tools, wxWidgets `[inferred-static 2026-09-15]`. Two other projects on this account are UE3 (Alice: Madness Returns, Enslaved), so their camera work is a direct reference.
- **Binary:** **64-bit** (PE32+), 40.4 MB, image base `0x140000000`, ASLR on. ⚠️ `.bind` section plus a `.text` with entropy 8.0 — the **Steam DRM wrapper**, code encrypted on disk `[inferred-static 2026-09-15]`.
- **Renderer:** Direct3D 9: `d3d9.dll` and `d3dx9_43.dll` are static imports `[inferred-static 2026-09-15]` (a proxy DLL is a straightforward way in). `d3dx11_43.dll` is imported too, and `NvStereoFixTexture` (UE3's NVIDIA 3D Vision hook) is in the strings.
- **Protection:** Steam DRM wrapper. No Denuvo string `[inferred-static 2026-09-15]`. The game has an optional SHiFT online login `[reported]`; not tested live.
- **Other:** Data under `WillowGame\`, not yet looked at.

## Method

PE headers and import tables read with `pefile`: machine type, link timestamp, image base, ASLR
flag, section names, sizes and entropy. Then a case-insensitive search of each binary for renderer
DLL names (`d3d9`, `d3d11`, `d3d12`, `dxgi`, `vulkan-1`, `opengl32`), headset runtimes (`openvr`,
`openxr`, `oculus`), protection markers (`denuvo`, `securom`, `.bind`) and middleware names, with
readable strings pulled around the interesting hits. A string match shows a name is present in the
file, not that the code path is used. A `.text` section with entropy near 8.0 is encrypted or
compressed, not normal code.

## Risks noted

- ⚠️ The Steam DRM wrapper hides the code from static reading until unwrapped `[hypothesis]`.
- Otherwise friendly: Unreal Engine 3 on Direct3D 9, the same combination as two existing projects here.
