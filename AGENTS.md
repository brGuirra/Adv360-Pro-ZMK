# Project Guidance

## Purpose

This repository is a personal firmware configuration for a Kinesis Advantage 360 Pro split keyboard. It builds ZMK firmware for both keyboard halves using the Kinesis-compatible ZMK fork pinned in `config/west.yml`.

Use this repository to customize the keyboard's ZMK configuration directly. Do not use Kinesis Clique as the customization workflow: advanced ZMK features must remain available, including custom behaviors, macros, combos, and layers.

## Primary Customization Points

- Edit `config/adv360.keymap` for key bindings, layers, custom behaviors, and includes.
- Add reusable custom behaviors or macros in `config/macros.dtsi` or a focused included `.dtsi` file.
- Keep `config/adv360_left.keymap` and `config/adv360_right.keymap` as thin entry points that include the shared keymap unless a real per-half difference is required.
- Use `config/boards/arm/adv360/` only for hardware, DeviceTree, pin, or Kconfig changes. Do not alter board definitions for ordinary keymap changes.
- Keep `config/west.yml` pinned to the supported Kinesis ZMK fork unless an intentional firmware-platform upgrade is requested.

## Build And Flash Workflow

- Build both halves locally with `make`; use `make left` only when the right-side binary is not needed.
- Build output is written to `firmware/` as timestamped `.uf2` files.
- Flash the left and right `.uf2` files to their matching keyboard halves while each is in bootloader mode.
- Prefer the legacy/non-Clique build path for GitHub Actions and local workflows. Do not enable ZMK Studio/Clique solely for configuration editing.

## Change Guidelines

- Preserve the physical key order and number of bindings in every layer. A shifted or missing binding changes the keyboard matrix mapping and can produce an incorrect keymap.
- Use documented ZMK Devicetree syntax and keycode/behavior names compatible with the revision pinned in `config/west.yml`.
- Keep custom changes minimal and localized to the keymap or behavior definitions.
- Preserve Bluetooth profile selection and bootloader bindings unless their behavior is intentionally being changed; they are needed to pair devices and flash recovery firmware.
- Do not commit generated firmware (`firmware/*.uf2`), build directories, container artifacts, or macOS metadata such as `.DS_Store`.
- Avoid unrelated changes to the custom board definitions and dependency revisions.

## Validation

- For keymap or behavior changes, run `make` and confirm that both left and right `.uf2` files are produced.
- Test changed bindings on hardware after flashing both halves.
- For upgrades to the ZMK fork or board definitions, inspect `UPGRADE.md` and validate pairing, bootloader access, Bluetooth profiles, lighting, and all custom layers.
