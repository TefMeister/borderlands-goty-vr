# A working VR mod for this exact build already exists, uses a dxgi.dll proxy, and names the camera

**From:** `/gr` estate sweep, home PC, 2026-09-23.
**Answers:** board `[PD]` "build a `dxgi.dll` proxy for it — the game draws with D3D11, not D3D9",
and dossier §2–4 "compare against the Alice/Enslaved UE3 camera notes".
**Topic:** `external-research/topics/2026-09-23-bl1gotyvr-a-working-vr-mod-for-this-exact-build.md`

Mastersellz's **BL1GOTYVR** (<https://github.com/Mastersellz/BL1GOTYVR>) is a headset-tested OpenXR
mod for Borderlands GOTY Enhanced, Win64/D3D11, loaded by a `dxgi.dll` proxy `[reported]`. Its hook
notes (game 1.5.0.0) say `[reported]`:

- `PlayerController.PlayerCamera` is null in normal play; the live view is
  `WillowPlayerController.CalcViewLocation / CalcViewRotation / CachedFOVAngle`, resolved through UE3
  property reflection. Reject `Default__*` objects.
- Per-frame hook: `WillowGameViewportClient::Draw` via the viewport's secondary vtable.
  **Calling it twice per frame corrupts the heap (`0xC0000374`).**
- The whole frame, UI included, ends in an `R8G8B8A8_UNORM` swap-chain back buffer.

**Suggested dossier change:** record the dxgi route as confirmed by an independent project, add the
camera fields and the double-Draw trap as `[reported]`, and raise with Tefa whether this project
should continue now that a working mod exists. No licence was found on that repository, so study
only; nothing may be copied.
