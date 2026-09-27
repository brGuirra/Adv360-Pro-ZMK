# Custom Layout Migration Compatibility

This repository builds against [ReFil/zmk `adv360-z3.5-2`](https://github.com/ReFil/zmk/tree/adv360-z3.5-2), as pinned in `config/west.yml`. It is not a build target from [urob/zmk-config](https://github.com/urob/zmk-config), and its external modules cannot be copied in without first checking compatibility with this older, Kinesis-specific ZMK fork.

The source layout reviewed is [brGuirra/zmk-config-custom](https://github.com/brGuirra/zmk-config-custom) at `4c70d34`. That config is an older Adv360 adaptation of urob's configuration and depends on urob's custom ZMK fork and `zmk-nodefree-config` helpers.

## Directly Implementable

| Prior-layout capability | Current firmware support | Migration approach |
| --- | --- | --- |
| 34-key base, symbol, number, navigation, function, mouse, and system layers | Yes | The target layout is intentionally 34 logical keys for compatibility with the user's smaller keyboard. Define all 76 physical positions in every `config/adv360.keymap` layer, binding the 42 positions outside the logical layout to `&none`. Do not remove bindings or use the old 34-key transform. |
| Home-row mods and long-press navigation keys | Yes | Define local `zmk,behavior-hold-tap` behaviors. The pinned fork supports `balanced`, `tap-preferred`, `quick-tap-ms`, `require-prior-idle-ms`, `hold-trigger-key-positions`, and `hold-trigger-on-release`. The existing `homerow_mods` definition is a starting point. |
| Positional combos for symbols and shortcuts | Technically supported, intentionally excluded | The target layout does not use combos because they are difficult to actuate on the Advantage 360. Use the dedicated momentary Symbol layer for coding symbols instead. |
| Macros, including paired punctuation and OS shortcuts | Yes | Define reusable `zmk,behavior-macro` nodes in `config/macros.dtsi`. The current config already has paired punctuation, Windows, macOS, and mouse-click macros. |
| Sticky modifiers | Yes | Use the built-in sticky-key behavior for ordinary one-shot modifiers. |
| Caps Word | Yes | Use the built-in `&caps_word`. Its continuation list can be customized, but it does not include urob's modifier-sensitive extensions. |
| Mouse movement, scroll, and buttons | Yes | The pinned firmware includes mouse-key behaviors and the current keymap includes the pointing key bindings. A regular mouse layer works. |
| Bluetooth profiles, profile clear, reset, bootloader, and layer colors | Yes | Retain the existing system-layer bindings unless intentionally changing their functions. |
| RGB underglow and backlight controls | Yes | The Kinesis fork supplies the Advantage 360 Pro hardware support. Keep its board definitions and use the existing `&rgb_ug` and `&bl` bindings. |

## Implementable With A Different Interaction

| Prior urob capability | Why an exact port is unavailable | Native replacement |
| --- | --- | --- |
| Smart Num: tap Num Word, double-tap persistent Num, hold momentary Num | The old `&num_word` behavior is not in the pinned firmware. | Compose sticky-key, hold-tap, tap-dance, toggle-layer, and to-layer behaviors for one-shot, momentary, persistent, and cancelable Number access. |
| One-shot sticky layer | The pinned behavior set has no native sticky-layer behavior. | Wrap `&mo` in a custom sticky-key behavior. It can provide a one-shot layer with a configurable timeout. |
| Smart Mouse: a mouse layer that automatically exits when a non-mouse key is pressed | It depends on urob's `zmk,behavior-tri-state`, which is absent. | Use a normal momentary mouse layer or a dedicated toggle key. It will not auto-cancel on the next ordinary key. |
| Alt-Tab swapper that retains Alt while consecutive tab presses continue | It depends on the same absent tri-state behavior. | Use a normal `Alt+Tab` macro or a momentary navigation layer with explicit Alt and Tab. |
| Unicode Greek/German layer activated with a sticky shifted-layer behavior | The prior config generates behaviors with `zmk-nodefree-config` and relies on mod-morph support not present in this fork. | Use direct host shortcut macros where the OS/application has a stable input method, or a regular Unicode layer with individually defined macros. This remains host-layout and OS dependent. |
| Modifier-sensitive punctuation and shifted deletion | The old implementation uses `zmk,behavior-mod-morph`, which the pinned fork does not ship. | Place the alternate result on a layer, use distinct keys, or encode fixed modifier combinations in macros. It cannot react dynamically to currently held modifiers. |
| Copy on single tap, cut on double tap | It uses a tap-dance behavior that is absent. | Use separate copy/cut keys or put one command on a layer. |

## Not Recommended To Copy

- `urobs_transform` and the Adv360-specific board files from the old urob fork: this repository already supplies its own Kinesis hardware definition and matrix transform.
- `zmk-nodefree-config`, `zmk-auto-layer`, `zmk-tri-state`, and `zmk-unicode` entries from current `urob/zmk-config`: they target newer ZMK APIs and have not been validated against `adv360-z3.5-2`.
- The old config's `global-quick-tap-ms`: this property was supplied by urob's fork. Configure `quick-tap-ms` individually on each local hold-tap instead.

## Recommended Migration Boundary

Build the new layout only from the **Directly Implementable** set first: the 34-key logical placement, home-row mods, combo-free layers, navigation, and system controls. This preserves the Kinesis-supported firmware framework and covers the majority of the prior layout. Every layer must still contain the complete 76-position matrix, using `&none` for the 42 intentionally unused positions.

The implemented layer access model is intentionally combo-free:

- Hold the legacy Space thumb: Navigation; tap it for Space.
- Hold the legacy Enter thumb: Symbol; tap it for Enter.
- Tap right inner thumb: one-shot Number for the next key; hold it for momentary Number; tap then hold for persistent Number; press it while persistent Number is active to return to Base.
- Tap right outer thumb: Sticky Shift; hold it for ordinary Shift.
- Hold Symbol and Number together: Utility, containing F-keys, media, Bluetooth, bootloader, and lighting controls.

After that is stable on hardware, choose between these two paths for the remaining smart behaviors:

1. Accept the native substitutes above. This is the lowest-risk path and keeps the current firmware pin.
2. Plan a firmware-platform upgrade and validate compatible versions of the urob modules. This is required for exact tri-state, modern auto-layer, Unicode helper, mod-morph, and tap-dance behavior rather than a keymap-only migration.

## Sources

- Current firmware pin: `config/west.yml` and [ReFil/zmk `adv360-z3.5-2`](https://github.com/ReFil/zmk/tree/adv360-z3.5-2)
- Existing local hold-tap example: `config/adv360.keymap`
- Existing local macros: `config/macros.dtsi`
- Prior custom layout: [brGuirra/zmk-config-custom](https://github.com/brGuirra/zmk-config-custom/tree/4c70d347a6aeff05406301ff2e5c6ef44795cf2e/config)
- Prior custom behavior definitions: [base.keymap](https://github.com/brGuirra/zmk-config-custom/blob/4c70d347a6aeff05406301ff2e5c6ef44795cf2e/config/base.keymap) and [combos.dtsi](https://github.com/brGuirra/zmk-config-custom/blob/4c70d347a6aeff05406301ff2e5c6ef44795cf2e/config/combos.dtsi)
- Current urob module dependencies: [urob/zmk-config `west.yml`](https://github.com/urob/zmk-config/blob/a7232a1c83135df375e1b0edcb9ad8da74fa5c85/config/west.yml)
