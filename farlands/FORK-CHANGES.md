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

**None yet.** — The fork currently contains only additive Farlands files (`FARLANDS.md`, `farlands/`). Zero Godot-owned files modified. This is the ideal state; keep it as long as possible (GDExtension-first, FORWARD-COMPAT L1).

## Planned in-tree changes (known unavoidable)
| Change | Why no extension path | Status |
|---|---|---|
| Branding (name, splash, version strings) | Editor identity is baked into core; centralise per FORWARD-COMPAT L10 | Not started |
| Double-precision / large-world coords build flag | Compile-time engine option, not an extension | Not started |
| World streaming (if not achievable via modules) | Evaluate GDExtension path first | Evaluate |

## Farlands additions (extensions / modules — additive, listed for orientation only)
| Addition | Form | Status |
|---|---|---|
| Farlands SDK (identity, portal link, PoP signals) | GDExtension/module (planned) | Built as standalone lib; engine packaging TBD |
| MCP server (agentic editor access) | Editor plugin/module (planned) | Not started — first real feature |
| AI NPC runtime hook | Module (planned) | Not started |
