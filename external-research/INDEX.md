# Research index

**Last `/gr` pass: 2026-09-23 (estate sweep) — FULL.** Checked against phunkaeg's *VR Modding Playbook*: a working, headset-tested VR mod for this exact build exists (BL1GOTYVR); topic written and a pointer dropped in `engine-research/inbox/`.

_Previous: **Last `/gr` pass: 2026-09-17 (estate sweep) — CHECK-IN.** First pass: folder bootstrapped; one topic on the 3DMigoto stereo fix for this exact build and the BL1 PythonSDK (console and object access)._

Every research topic gathered for this project, newest first. Each row links to a self-contained
write-up in `topics/`. Status tags:

- 🆕 **new** — found, not yet acted on by the modding side.
- 👀 **looked at** — the modding side has read it; no verdict yet.
- ✅ **used / confirmed** — acted on, and it held.
- ❌ **dead end** — tried, and it did not work (kept so it is not re-proposed).

| Date | Topic | Status | Why it matters |
| --- | --- | --- | --- |
| 2026-09-23 | [BL1GOTYVR: someone has already built a working VR mod for this exact game](topics/2026-09-23-bl1gotyvr-a-working-vr-mod-for-this-exact-build.md) | 🆕 | Uses the dxgi.dll route our board plans, names the live camera fields, and warns that calling Draw twice corrupts the heap. ⚠️ Raises whether this project should continue. |
| 2026-09-17 | [A 3DMigoto stereo fix exists for this exact build, and the BL1 PythonSDK gives script-level access](topics/2026-09-17-3dmigoto-fix-and-bl1-python-sdk.md) | 🆕 | A D3D11 stereo reference for this UE3.5 renderer, and a public route to the engine's objects |
