# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a ZMK firmware configuration repository for a **Corne split keyboard**. ZMK is a Zephyr-based keyboard firmware. The firmware is built via GitHub Actions — there is no local build step expected for most changes.

## Building

Firmware builds run automatically on push/PR via `.github/workflows/build.yml`, which delegates to ZMK's reusable `build-user-config.yml` workflow at `zmkfirmware/zmk@v0.3`.

For local builds, ZMK requires a full Zephyr toolchain. The local ZMK source is checked out at `.zmk/zmk/`. Typical west build command:

```sh
west build -s .zmk/zmk/app -b <board> -- -DSHIELD=<shield> -DZMK_CONFIG=$(pwd)/config
```

Updating the ZMK version is done by changing `revision` in `config/west.yml`.

## Repository Structure

- `config/corne.keymap` — keymap definition in ZMK's DeviceTree (`.dts`-like) format; defines all layers and key bindings
- `config/corne.conf` — Kconfig options (enables RGB underglow, OLED display, etc.)
- `config/west.yml` — west manifest; pins ZMK version and lists extra modules
- `boards/shields/` — placeholder for custom shield definitions (currently empty)
- `zephyr/module.yml` — declares this repo as a Zephyr module with `board_root` at the repo root

## Keymap Layers

The keymap has three layers:
- **Layer 0 (default)** — standard QWERTY; hold `LWR` (mo 1) or `RSE` (mo 2) thumb keys to access other layers
- **Layer 1 (lower)** — numbers row, Bluetooth profile selection (`BT_SEL 0-4`, `BT_CLR`), arrow keys
- **Layer 2 (raise)** — symbols, brackets, math operators

## Key ZMK Concepts

- `&kp KEY` — key press binding
- `&mo N` — momentary layer switch
- `&bt BT_SEL N` / `&bt BT_CLR` — Bluetooth profile selection / clear
- `&trans` — transparent (falls through to layer below)
- Requires `#include <dt-bindings/zmk/bt.h>` for Bluetooth bindings
- Key names: `SEMI` (`;`), `SQT` (`'`), `FSLH` (`/`), `BSLH` (`\`), `GRAVE` (`` ` ``), etc.
