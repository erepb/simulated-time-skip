# Simulated Time Skip

S.T.A.L.K.E.R. Anomaly mod that replaces vanilla single-jump time skips with incremental steps running full ALife simulation each tick.

Vanilla sleep calls `level.change_game_time()` once — pure arithmetic, no simulation. Squads don't move, offline combat doesn't trigger, territory doesn't change. This mod breaks the skip into 3-minute steps, calling `alife():force_update()` after each one.

## Requirements

Modded exes 2026.3.29 or newer

## Installation

FOMOD installer for MO2. Core module is always installed; hooks are individually selectable:

| Module | What it hooks                              | Label |
|--------|--------------------------------------------|-------|
| Sleep | `ui_sleep_dialog`                          | Sleeping |
| Travel (catch-all) | `level.change_game_time` globally          | *(sync, no overlay)* |
| Anabiotic | `bind_stalker_ext.anabiotic_callback`      | Unconscious |
| Campfire | `campfire_placeable` (Placeable Campfires) | Preparing campfire / Gathering wood |
| Books | `Wait.wait` (G.A.M.M.A. Books Pass Time)                        | Reading |
| Soulslike | `soulslike.dream_callback` (Soulslike Gamemode - Jabbers)         | Recovering |
| Flash anomaly | `drx_da_main` (DAO)                        | Disoriented |

The **Travel** module acts as a universal catch-all — it intercepts any `level.change_game_time()` call >= 3 minutes that isn't already handled by an individual hook. This covers fast travel, backpack travel, kairan travel, and any future mod.

## API

```lua
simulated_time.advance(120)                          -- 2 hours, no callback
simulated_time.advance(60, function() end)           -- with callback
simulated_time.advance(180, { label = "Waiting" })   -- custom label
simulated_time.is_active()                           -- check if running
```
