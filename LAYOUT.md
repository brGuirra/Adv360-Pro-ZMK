# Advantage 360 Pro — layout v1

This keymap (`config/adv360.keymap`) matches the **stock Kinesis ADV360 Pro layout** described in the older `adv360pro.keymap` / `current_layout.md` flow, **plus** a dedicated **symbols (Sym)** layer for programming punctuation. It is intentionally **not** the Polarity default (no homerow mods, comma/dot morph, or Nav layer).

## Layer stack

| Index | Name   | Activation | Purpose |
|-------|--------|------------|---------|
| 0     | Base   | —          | QWERTY, `Esc`, `Tab`, thumbs, `mo 2` Fn, `mo 3` Mod |
| 1     | Keypad | `&tog 1` (left number row) | Numpad on the right half |
| 2     | Fn     | `&mo 2` (bottom corners) | F1–F12 on the number row |
| 3     | Mod    | `&mo 3` (top-right number row) | Bluetooth, bootloader, backlight, RGB |
| 4     | Sym    | `&mo 4` (row below numbers, outer columns left of `T` / right of `Y`) | Programmer symbols |

Reserved slots `extra1`–`extra4` remain for tooling/UI.

## Base layer (summary)

- **Number row:** `=` … `5`, keypad toggle, **Mod**, `6` … `0`, `-`
- **QWERTY row:** `Tab` … `T`, **`mo 4` Sym** on both sides of the inner gap, `Y` … `\`
- **Home row:** `Esc`, letters, inner thumbs `LCtrl` / `LAlt` / `LGui` / `RCtrl`
- **Shift row:** shifts, `Z` … `B`, `Home` / `PgUp`, `N` … `/`
- **Bottom row:** **`mo 2` Fn**, grave, Caps, arrows, thumbs (Bksp, Del, End, PgDn, Enter, Space), `↑` `↓` `[` `]`, **`mo 2` Fn**

This aligns with the usual stock ADV360 Pro ZMK diagram (equal/minus on the outer number row, `Esc` on the left home row, Caps on the bottom cluster, etc.).

## Symbols layer (Sym)

Hold **`mo 4`** with either hand on the **second row** outer keys (same keys that are `mo 4` on Base). While Sym is active, **`tog 1`**, **`mo 2`**, and **`mo 3`** are repeated on the same positions as Base so keypad, Fn, and Mod still work.

### Sym — number row

`! @ # $ %` — keypad toggle — **Mod** — `^ & * ( )` (outer columns `none`).

### Sym — QWERTY row

Left: `\ | { } [` — inner `none` pair — Right: `] < > = +` (trailing outer column `none`).

### Sym — home row

Left: `` ~ ` ' " ; `` — modifiers unchanged — Right: `[ ] { } |`.

### Sym — shift row

Left: `_ - = * /` — `Home` / `PgUp` — Right: `n` `m` `,` `.` `\` (letters kept for quick labels; change to more symbols if you prefer).

### Sym — bottom row

Same as Base: **`mo 2`**, grave, Caps, arrows, thumbs, `↑` `↓` `[` `]`, **`mo 2`**.

## Differences from the Polarity fork default

If you previously used the stock **Adv360-Pro-ZMK** `adv360.keymap` from this repo (before v1):

- **Removed:** Nav layer, homerow mods (`hm`), comma/dot morph, `studio_unlock` / STP battery / `macro_ver` on Mod (that map targeted a different product bundle).
- **Restored:** Stock layer indices `tog 1`, `mo 2`, `mo 3` as in `current_layout.md`.
- **Added:** Layer 4 Sym and `mo 4` on the second-row outer columns.

## Building

Edit only `config/adv360.keymap` (both halves `#include` it). Build with `make` or GitHub Actions per [`README.md`](README.md).

## Changelog

- **v1:** Port stock `adv360pro`-style layout; add Sym layer (`mo 4`); document here.
