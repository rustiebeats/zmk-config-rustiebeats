# Repository Guidelines

## Project Structure & Module Organization

This repository configures ZMK firmware for the 38-key TOTEM split keyboard.

- `config/totem.keymap`: active keymap, layers, and custom behaviors.
- `config/combos.dtsi`: combo definitions and timing constants.
- `config/totem.conf`: firmware settings, including Bluetooth, sleep, and combo limits.
- `config/boards/shields/totem/`: shield hardware definitions, left/right overlays, and default keymap.
- `zmk-nodefree-config/`: helper macros and named key positions.
- `config/west.yml`: ZMK dependency manifest; `build.yaml`: build matrix.
- `.github/workflows/`: firmware builds and keymap rendering.
- `keymap-drawer/`: generated YAML and SVG assets displayed in `readme.md`.

## Build, Test, and Development Commands

Firmware builds run on pushes, pull requests, and manual workflow dispatch. The matrix targets `xiao_ble//zmk` with `totem_left` and `totem_right`; only the left target includes `studio-rpc-usb-uart`.

With GitHub CLI installed and authenticated, run from the repository root:

```sh
gh workflow run build.yml          # Request a firmware build
gh run list --workflow build.yml   # Inspect recent builds
gh workflow run draw-keymap.yml    # Request diagram regeneration
git diff --check                  # Check whitespace errors
```

Manual dispatch runs the default branch unless `--ref <branch>` is supplied. There is no repository-local build wrapper; local compilation requires a separately configured ZMK/Zephyr workspace.

## Coding Style & Naming Conventions

Match existing formatting: four-space indentation in keymap behavior blocks, two spaces in workflow YAML, and aligned binding columns that preserve the physical keyboard layout. Keep preprocessor directives left-aligned. Use uppercase layer/timing constants (`BASE`, `COMBO_TERM_FAST`) and descriptive combo names (`lang_hangeul`). Prefer named positions such as `LT3` and `RM4` over numeric indexes. Respect `.editorconfig`; no dedicated lint or formatter command is configured.

## Testing Guidelines

No unit-test framework or coverage threshold is configured. Require successful builds for both halves. For keymap changes, inspect the generated diagram and test affected taps, holds, combos, and layer transitions on hardware. Review combo capacity when adding combinations. The drawing workflow regenerates and commits assets after relevant pushes; edit source configuration instead of generated files.

## Commit & Pull Request Guidelines

Follow the history's short, imperative commit subjects, such as `add enter combo` or `fix ZMK build matrix`. Keep changes focused. PR descriptions should explain changed behavior, affected layers or halves, and build/hardware validation. Link relevant issues and update `readme.md` when documented layout behavior changes.
