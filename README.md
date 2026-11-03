# RareScanner

RareScanner is a primarily Linux-oriented companion to RareInjector, for all your ripping needs.
It injects into a running application a PID, regardless of the graphics API in order to dump resources such as models, textures, and videos onto disk, preserving each asset's internal filepath AND a hash, so future rips don't duplicate assets.
The hashes and filepaths form a database that RareInjector uses to locate and substitute assets.

> **Project status:** Stubbed repository. Public release is tentatively expected by 15 November 2026.

For additional information, contact N. Evarine of Nomandible.
-- Sora (aka stinkymalinky), an anonymous friend :)
(Both of these tools were done as favours for him.)

---
## Disclaimer for Game Publishers and Copyright Holders

RareScanner is intended for use with games and other software that users have legitimate access to, that is, games you own.
Users are responsible for providing their own extracted assets and ensuring that they have the necessary rights or permissions to use them.
RareScanner does not distribute copyrighted game assets, games, or other third-party media.
Owning a copy of a game does not necessarily grant permission to extract, redistribute, or modify its copyrighted content. Extracted assets remain subject to the game's copyright.
On protected and encrypted files: such files are accessed by the GPU. To claim that merely viewing this content is infringement would require claiming that a screenshot of a model infringes on that model's copyright, or that game footage is copyrightable, just because you could "bypass encryption to view it." RareScanner uses the exact same tool calls already present just for software instead of the GPU.
The hashes produced by RareScanner are descriptive metadata pointing to the actual files. The extracted assets themselves still require that you own the media in question, taking a model of Nanako from the Steam release of Persona 4 Golden, for example, requires owning that game.
The project is intended for legitimate asset extraction, viewing, and companion use with RareInjector. It is not designed to facilitate software piracy, DRM circumvention, or anti-cheat evasion.
RareScanner is an independent project and is not affiliated with any game publisher or graphics hardware manufacturer unless explicitly stated otherwise.

## Overview

RareScanner works by injection. It targets an application's PID and intercepts and mirrors graphics API calls, Vulkan, OpenGL, and DirectX, whichever backend the game happens to be running on.
There are two modes:

- **Runtime mode:** Injects into the running game, the same way Reshade and other overlay tools do. If an asset loads through the graphics backend, it's an asset, and it gets dumped. If you can see something, you can rip it. Like tools such as Ninjaripper, this amounts to a filesystem walk during runtime.
- **Destructive mode:** Should be run offline. It mimics the application/engine somewhat in order to scan through every single path and directory the game has access to, looking for assets based on a bunch of common formats, including compressed archives.

In both modes, the asset itself is saved to disk in an unencrypted format, alongside a hash and the asset's internal filepath. Because the filepath and hash are preserved, re-ripping the same asset doesn't produce duplicates, the hashes form a database for RareInjector to use.
Assets are identified by filename, hash, and slight content analysis. Assets that can't be confidently identified are reported as "might be an asset." A per-game preset system exists, and some quirks require verification against a hash.
**A fundamental limitation:** if a file is "protected" and inaccessible by the GPU to arbitrarily call, destructive mode can't reach it. A warning is displayed telling the user to use runtime mode for those assets. Basically, it's a fundamental limitation of the tool.

## Asset Extraction

RareScanner is intended to support the extraction of several asset types:
| Asset type | Description |
|---|---|
| 3D Models | Dumped to disk in unencrypted form, with internal filepath and hash. |
| Textures | Same as above. |
| Videos | Same as above. |
Additional asset types may be supported as development progresses.

There are 2 types of extraction modes;
- **Runtime mode:** For assets the GPU actually loads. Injection-based, mirrors the game's graphics API calls.
- **Destructive mode:** Offline, engine-mimicking filesystem walk. For everything the game *could* access, based on common formats and compressed archives.

### Identification and Deduplication

- Filename, hash, and slight content analysis. If it loads in whatever graphics backend the game is running, it's an asset.
- Preserved filepath + hash means future rips don't duplicate assets.
- Compressed archives are handled.
- Unidentifiable assets are reported as "might be an asset"; per-game presets cover the quirks, with hash verification where needed.

## Graphics API Compatibility

RareScanner hooks whichever backend the application is using:
- Vulkan
- OpenGL
- DirectX
It is graphics API-agnostic in the sense that it mirrors the API the game is actually running on, rather than tying the implementation to a single one.
Actual compatibility will depend on the implementation of each connector, the game being used, and the underlying hardware.

## Supported Hardware

The short version: if you can run the game, you can run RareScanner.
| Hardware | Status | Notes |
|---|---|---|
| NVIDIA GPU (model tbd) | Tested | Currently the only tested configuration. |
| Other NVIDIA GPUs | Untested | Compatibility has not been verified. |
| AMD GPUs | Untested | Potentially compatible through supported graphics APIs. |
| Intel GPUs | Untested | Potentially compatible through supported graphics APIs. |
| Integrated GPUs | Untested | Compatibility has not been verified. |
**Important:** Graphics API compatibility does not guarantee hardware compatibility.
This section should be expanded as more users test the project.

## Publisher and Anti-Cheat Compatibility

RareScanner is basically read-only: it dumps assets to disk and doesn't modify the game.
The project is not intended to provide cheats, wallhacks, competitive advantages, or mechanisms for circumventing anti-cheat software.
There is potential for cooperation with game publishers, and the project is fully prepared for it. The position is simple: you have to give them money first to even access the assets. RareScanner doesn't enable piracy, you already own the product.

### Build Verification

Build verification is technically possible, but given the nature of THIS tool, publishers and developers are unlikely to want it, you're paying for a product after all.
**Note:** Build verification alone does not constitute anti-cheat approval. Any integration with third-party anti-cheat software would require appropriate technical cooperation and authorization. Talk to N. Evarine about that.

## Contributing

Contributions are welcome.
To contribute:
1. Fork the repository.
2. Create a separate branch for your changes.
3. Implement and test your modifications.
4. Submit a merge request describing the changes and their purpose.
Contributions involving Windows support and game verification are particularly welcome.

### Contributor Attribution

Contributors will receive individual entries in `Contribs.md`, documenting their respective contributions.
The project intentionally uses a per-contribution attribution format rather than listing each contributor only once.
In other words, someone who makes 500 meaningful contributions could receive 500 individual entries.
Partly for documentation purposes, partly because having a contributor listed 500 times would be quite entertaining.

### Code Quality

AI-assisted development is not inherently discouraged. However, contributions should be reviewed, tested, and understood by the person submitting them.
Please avoid submitting automatically generated code that you cannot explain or maintain.
The project currently has a very small maintenance team, and contributions should be reasonably focused and straightforward to review.

## Issue Reporting

Bug reports are welcome, particularly those involving games that refuse to cooperate.
When reporting an issue, please include as much relevant information as possible:
- The game and version.
- GPU manufacturer and model.
- Operating system and version.
- Graphics API being used.
- Relevant driver versions.
- Error messages and logs.
- Steps required to reproduce the issue.

### Known Problem Areas

Some games are simply broken for this, especially gacha games, Nexon really likes locking their stuff down.
It might not work for yours: this has been tested on exactly 1 (1) ((ONE)) NVIDIA GPU.
Please understand that the maintainers cannot reasonably be expected to own and test every graphics card available.
If you identify a hardware-specific issue and are able to investigate or implement a fix, contributions are greatly appreciated.

## Licensing

RareScanner is distributed under the GPLv3 License.
See `LICENSE` for the complete licence text.
Third-party dependencies: tbd.
The output of RareScanner is, for the most part, stuff you already own in a different context.
Users are responsible for ensuring that their use and distribution of extracted assets complies with applicable licences and copyright law.

## Supported Games

See `Supported.md` for what games have been tested and on what graphics API.
(tbd the list is still being built.)

## Packaging

You have explicit permission to package it for whatever distribution or BSD you'd like, from Nix to Arch to even a flatpak if you want. We're not maintaining the packages, but we are upstream and none of the dependencies needed for this require anything distro-dependent or a specific init system. We do provide an AppImage!

## Building

tbd gcc mostly, but the toolchain is still settling ;w;
---
*RareScanner is an independent project and part of the Rare family of tools, alongside RareInjector. Compatibility, supported hardware, and feature availability are subject to change as development progresses.*
