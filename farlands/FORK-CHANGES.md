# FORK-CHANGES — Farlands Engine divergence ledger

Required by `DEVELOPMENT.md` §4 and `FORWARD-COMPAT.md` L1. **Every modification to a Godot-owned file gets an entry here, in the same commit.** Farlands-only files under `farlands/` or clearly-new top-level files are additive and do NOT need entries — only changes to upstream's tree do.

The purpose: at any moment we can answer "exactly how do we differ from stock Godot 4.5, and why?" — which is what makes upstream merges tractable instead of terrifying.

---

## Base
- **Upstream:** `godotengine/godot`
- **Base tag/branch:** `4.5-stable`
- **Bootstrapped:** 2026-07-13

## In-tree modifications to Godot files
_(what / why / file / re-merge risk)_

### 1. Engine branding — `version.py` (2026-07-13)
- **What:** `short_name` `godot`→`farlands`; `name` `Godot Engine`→`Farlands Engine`; `website`→`https://farlands.world`. Version numbers (4.5.3) and `docs` left as upstream (we ARE Godot 4.5.3 + our layer — honest, and doc links still resolve).
- **Why no extension path:** engine identity is compile-time — `version.py` is Godot's single centralised branding source (feeds `GODOT_VERSION_NAME` into `core/version_generated.gen.h` via `core/core_builders.py` at build time). Changing this one file is the whole rebrand; satisfies FORWARD-COMPAT L10 (centralise, don't sprinkle).
- **Re-merge risk:** LOW. Upstream edits `patch`/`status` here on version bumps → a trivial conflict where we keep our name fields and take their number. Our `name`/`short_name`/`website` lines are otherwise stable upstream.
- **Note:** `short_name` change also moves the editor's user config/data dir to a Farlands namespace (desirable — we don't want to share Godot's config).

_Visual branding (boot splash image, editor icon) deferred — needs Farlands engine art assets; will be a follow-up entry._

## Planned in-tree changes (known unavoidable)
| Change | Why no extension path | Status |
|---|---|---|
| Branding — text identity (name/short_name/website) | Editor identity is baked into core; centralise per FORWARD-COMPAT L10 | ✅ Done (entry #1) |
| Branding — visual (boot splash, editor icon) | Splash/icon are engine assets | Deferred — needs art |
| Double-precision / large-world coords build flag | Compile-time engine option, not an extension | Not started |
| World streaming (if not achievable via modules) | Evaluate GDExtension path first | Evaluate |

## Farlands additions (extensions / modules — additive, listed for orientation only)
| Addition | Form | Status |
|---|---|---|
| Farlands SDK (identity, portal link, PoP signals) | GDExtension/module (planned) | Built as standalone lib; engine packaging TBD |
| MCP server (agentic editor access) | Editor plugin/module (planned) | Not started — first real feature |
| AI NPC runtime hook | Module (planned) | Not started |
