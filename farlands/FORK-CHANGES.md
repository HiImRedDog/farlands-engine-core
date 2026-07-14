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

### 2. Editor theme default colours — `editor/themes/editor_theme_manager.cpp` + `editor/settings/editor_settings.cpp` (2026-07-13)
- **What:** Default editor preset accent `Color(0.44,0.73,0.98)` (Godot blue) → `Color(1.0,0.784,0.0)` (**#ffc800**, Farlands gold); default base `Color(0.21,0.24,0.29)` (blue-grey) → `Color(0.15,0.13,0.10)` (warm charcoal). Same two values mirrored as the raw setting defaults in `editor_settings.cpp` so there's no stray Godot blue on any path.
- **Why no extension path:** these are compile-time defaults for a fresh install's editor theme — nothing rebrands the editor out of the box at runtime. Users can still pick any built-in preset; we only changed what "Default" *is*.
- **Re-merge risk:** LOW–MED. Upstream occasionally tunes the Default preset; a bump could conflict on these exact lines — trivial to resolve (keep our gold/charcoal). Localised to 3 lines across 2 files.
- **Brand note:** #ffc800 is the confirmed Farlands yellow (from `farlands-mark-yellow.svg`), superseding the washed-out `#ffcf60` in older portal tokens. Real brand vectors vendored at `farlands/brand/` for the pending splash + icon work.

### 3. Editor imagery — Godot logos/icon/splash → Farlands (2026-07-13)
- **What:**
  - `editor/icons/Logo.svg` (Project Manager logo) → Farlands full-logo (mark + FARLANDS), gold #ffc800
  - `editor/icons/TitleBarLogo.svg` (editor title bar) → Farlands mark
  - `main/app_icon.png` (128×128 window/app icon) → Farlands mark
  - `main/splash.png` (800×600 boot splash) → Farlands full-logo centered on warm ink
- **Why no extension path:** built-in engine brand imagery, compiled/embedded — no runtime hook rebrands these out of the box.
- **Method:** PNGs rasterized by the engine itself (headless `Image.load_svg_from_string`) — no external rasterizer dependency. Verified the render is gold (#ffc800) via pixel sample, not black (thorvg handled the CSS-class fill).
- **Re-merge risk:** LOW. Upstream rarely touches these beyond its own logo refreshes; a Godot logo update would conflict trivially (keep ours).
- **Source:** `farlands/brand/` vendored vectors.

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
