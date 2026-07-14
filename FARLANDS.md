# Farlands Engine — Core

This repository is the **Farlands Engine**, a fork of [Godot](https://godotengine.org) `4.5-stable`, maintained by Pilbara Gaming Development PTY LTD (Red Dog Studios).

> **This is engine source only.** Project direction, planning, the Farlands SDK, and all design docs live in the sibling planning repo **`Farlands-Game-Engine`** (`github.com/HiImRedDog/Farlands-Game-Engine`). Read that first.

## What this is (and the discipline that keeps it maintainable)

The Farlands Engine's differentiators — Farlands SDK (chain identity + PoP signals), MCP/agentic editor access, AI NPC runtime — are built to touch **as little Godot source as possible**. See the planning repo's `FORWARD-COMPAT.md` (L1) and `DEVELOPMENT.md` §4.

- **GDExtension-first.** Our features live as extensions/modules that don't modify Godot's tree wherever possible.
- **Pristine upstream.** All Farlands-specific files live under `farlands/` or as clearly-new top-level files (like this one). We do **not** modify Godot's own files unless there is no other path — and every such change is logged in [`farlands/FORK-CHANGES.md`](farlands/FORK-CHANGES.md).
- **Upstream stays reachable.** The `upstream` remote points at `godotengine/godot`; we merge each stable Godot release on a schedule. That's the whole point of a thin fork — we keep inheriting their work.

## Fork lineage

| | |
|---|---|
| Upstream | `github.com/godotengine/godot` (remote: `upstream`) |
| Base | `4.5-stable` |
| Our remote | `origin` → `github.com/HiImRedDog/farlands-engine-core` |
| Divergence ledger | [`farlands/FORK-CHANGES.md`](farlands/FORK-CHANGES.md) |

Sync upstream later with: `git fetch upstream && git merge upstream/4.5` (fetch unshallows history on demand).

## Build recipes (committed so build knowledge never rots — FORWARD-COMPAT L6)

### macOS (Intel x86_64) — verified 2026-07
```bash
brew install scons molten-vk
scons platform=macos arch=x86_64 target=editor \
  vulkan_sdk_path=/usr/local/opt/molten-vk/Frameworks/MoltenVK.xcframework -j<cores>
# → bin/godot.macos.editor.x86_64
```
Notes: Homebrew's `vulkan-sdk` cask no longer exists; the `molten-vk` formula ships the `MoltenVK.xcframework` Godot's build detection looks for (a static `libMoltenVK.a`). Apple Silicon: swap `arch=arm64` and MoltenVK ships `macos-arm64_x86_64` too.

### Windows / Linux
_TBD — capture the exact recipe the first time each is built (FORWARD-COMPAT L6). Windows is the player-representative target; recipe lives on the game PC._

## License

Godot is MIT-licensed; this fork retains that license (`LICENSE.txt`). Farlands-specific additions are (c) Pilbara Gaming Development PTY LTD. The "Farlands Engine" name is a Red Dog Studios trademark and is not granted by the code license (Godot's own model). Open-core boundary + which components ever open: see planning repo `project.md` decisions.
