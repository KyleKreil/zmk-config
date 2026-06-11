# zmk-config

Personal [ZMK](https://zmk.dev) firmware configuration for a **Corne** (CRKBD) split keyboard with **nice!view** displays running the [nice-view-gem](https://github.com/M165437/nice-view-gem) custom status screen.

## Hardware

| Component | Detail |
|-----------|--------|
| PCB       | Corne / CRKBD |
| Controller | nice!nano v2 (`nice_nano_v2`) |
| Display   | nice!view (SPI), via the `nice_view_adapter` shield |
| Display UI | [nice-view-gem](https://github.com/M165437/nice-view-gem) animated status screen |

## Firmware builds

Builds run automatically in GitHub Actions on every push (see [`.github/workflows`](.github/workflows)). The build matrix is defined in [`build.yaml`](build.yaml):

| Artifact | Shields | Notes |
|----------|---------|-------|
| `corne_left_studio` | `corne_left nice_view_adapter nice_view_gem` | Central half. Includes **ZMK Studio** support |
| (right) | `corne_right nice_view_adapter nice_view_gem` | Peripheral half |

Shield order matters: `nice_view_adapter` defines the `nice_view_spi` SPI node that `nice_view_gem` draws onto.

### Getting the firmware

1. Open the **Actions** tab on GitHub and select the most recent successful run.
2. Download the build artifacts (`.uf2` files).
3. Put each half into bootloader mode (double-tap reset) and copy the matching `.uf2` to the `NICENANO` USB drive.

Flash the `corne_left_studio` firmware to the **left** half and the right-side firmware to the **right** half.

## ZMK Studio

The left (central) half is built with [ZMK Studio](https://zmk.dev/docs/features/studio) support, so you can edit your layout live over USB — no rebuild or keycode knowledge required.

1. Flash the `corne_left_studio` firmware to the left half.
2. Connect the keyboard over USB and open [ZMK Studio](https://zmk.studio).
3. Press the **unlock** key — bound to `&studio_unlock` on the **lower layer**, right pinky (hold the lower layer, then tap it).
4. Edit layers and keys; changes apply instantly.

## Editing the keymap without Studio

[Nick Coutsos' Keymap Editor](https://nickcoutsos.github.io/keymap-editor/) connects to this repo, lets you edit [`config/corne.keymap`](config/corne.keymap) visually, and commits the result back — which retriggers the CI build.

## Layout

Three layers, defined in [`config/corne.keymap`](config/corne.keymap):

- **Default** — QWERTY alphas, mods, thumb keys (`GUI` / lower / `SPACE` · `ENTER` / raise / `ALT`)
- **Lower** (`&mo 1`) — number row, Bluetooth profile selection, arrow keys, Studio unlock
- **Raise** (`&mo 2`) — symbols and brackets

## Repo layout

```
config/
  corne.keymap   # key layout (edit this)
  corne.conf     # feature flags (display, etc.)
  west.yml       # ZMK + module dependencies
build.yaml       # CI build matrix
```

## Configuration notes

[`config/corne.conf`](config/corne.conf) enables the display and the custom status screen:

```ini
CONFIG_ZMK_DISPLAY=y
CONFIG_ZMK_DISPLAY_STATUS_SCREEN_CUSTOM=y
```

[`config/west.yml`](config/west.yml) pins ZMK and `nice-view-gem` to matching revisions (`v0.3` / `v0.3.0`) — keep these aligned when upgrading.

- [ZMK Firmware](https://zmk.dev)
- [nice-view-gem](https://github.com/M165437/nice-view-gem) by [@M165437](https://github.com/M165437)
- [Corne keyboard](https://github.com/foostan/crkbd) by foostan
