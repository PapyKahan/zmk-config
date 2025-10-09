# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a ZMK (Zephyr Mechanical Keyboard) firmware configuration repository for custom keyboard builds. ZMK is a modern, open-source keyboard firmware built on the Zephyr RTOS.

## Keyboard Configurations

### CYBER60 Rev D
- **Board**: Custom ARM-based keyboard using nRF52840 (Nordic Bluetooth chip)
- **Layout**: 60% keyboard with rotary encoder
- **Features**:
  - Bluetooth LE with passkey entry
  - USB connectivity
  - RGB underglow (16 LEDs via WS2812)
  - Pointing device support
  - ZMK Studio support (runtime configuration)
  - Optional PWM buzzer (currently disabled)

### TOFU60 v2 & BT60 v2
Legacy configurations mentioned in README with 3-layer keymaps.

## Build System

### Building Firmware

The repository uses GitHub Actions for automated builds. The workflow is triggered on push, pull request, or manual dispatch.

**Build configuration**: `build.yaml`
- Builds for `cyber60_rev_d` board
- Includes ZMK Studio RPC support via USB UART
- Uses ZMK v0.3.0 (defined in `config/west.yml`)

**GitHub Actions**: `.github/workflows/build.yml`
- Uses the official ZMK build workflow: `zmkfirmware/zmk/.github/workflows/build-user-config.yml@v0.3.0`

To trigger a build:
- Push changes to the repository
- Create a pull request
- Manually trigger via GitHub Actions UI

### Local Development

For local builds, you would need to:
1. Install West (Zephyr's meta-tool)
2. Initialize the workspace with `config/west.yml`
3. Build using ZMK's standard build commands

## File Structure

### Core Configuration Files

- **`config/cyber60_rev_d.keymap`**: Main keymap configuration with 5 layers:
  - Layer 0: DEFAULT - Standard QWERTY layout
  - Layer 1: ALTERNATIVE - Same as default but with different Win key behavior (tap dance)
  - Layer 2: RAISE - Function keys, navigation (arrows, home/end), media controls
  - Layer 3: CONFIGURATION - Bluetooth pairing, output selection, bootloader, reset, media controls
  - Layer 4: UNDERGLOW - RGB lighting controls

- **`config/cyber60_rev_d.conf`**: Feature flags and configuration options
  - USB/Bluetooth settings
  - RGB underglow configuration
  - Pointing device support
  - Battery reporting fixes for Windows

### Hardware Definition Files

Located in `config/boards/arm/cyber60_rev_d/`:

- **`cyber60_rev_d.dts`**: Device tree source - hardware pinout and peripherals
  - GPIO matrix configuration (10 rows × 7 columns)
  - Encoder configuration (pins P1.01, P1.03)
  - RGB LED strip (SPI3, 16 LEDs)
  - Battery voltage divider
  - USB CDC ACM for ZMK Studio
  - LED indicators (green, red, blue)
  - PWM buzzer (disabled by default)

- **`cyber60_rev_d-pinctrl.dtsi`**: Pin controller configuration for SPI and PWM
- **`cyber60_rev_d-layouts.dtsi`**: Physical layout definitions
- **`cyber60_led.c`** & **`cyber60_buzzer.c`**: Custom C implementations for LED and buzzer
- **`Kconfig.board`**, **`Kconfig.defconfig`**: Kernel configuration
- **`CMakeLists.txt`**: Build system integration

## Keymap Architecture

### Custom Behaviors

**Tap Dance** (`td0`):
- Single tap: Left GUI (Windows key)
- Double tap: Right GUI + Right Alt (for specific keyboard shortcuts)
- Defined in `config/cyber60_rev_d.keymap:20-25`

**Key Toggle** (`kt_on`):
- Custom behavior for toggle-on-only functionality
- Defined in `config/cyber60_rev_d.keymap:26-31`

### Layer Activation

- **RAISE layer**: Momentary activation via `mo RAISE` on right Alt position
- **CONFIGURATION layer**: Layer-tap on context menu key (`lt CONFIGURATION K_CMENU`)
- **ALTERNATIVE layer**: Toggle via `tog ALTERNATIVE` in configuration layer
- **UNDERGLOW layer**: Momentary activation by holding key in configuration layer

### Special Features

**Rotary Encoder**:
- Default action: Volume up/down
- Encoder click: Play/Pause
- Sensor bindings defined per layer

**Pointing Device**:
- Mouse movement and buttons configured in RAISE and CONFIGURATION layers
- Custom move value: 1500 (vs default 600)

## Common Modifications

### Updating Keymaps

Edit `config/cyber60_rev_d.keymap`:
- Each layer uses a `bindings = <>` array with key codes
- Key positions correspond to the matrix transform in the DTS file
- Use ZMK key code documentation for available bindings
- Comments show ASCII art layout for reference

### Bluetooth Configuration

Bluetooth pairing managed in CONFIGURATION layer:
- `bt BT_SEL 0-3`: Select bluetooth profile 0-3
- `bt BT_CLR`: Clear current bluetooth profile
- `out OUT_BLE`: Force bluetooth output
- `out OUT_USB`: Force USB output

### RGB Underglow

UNDERGLOW layer controls RGB via `rgb_ug` prefix:
- `RGB_TOG`: Toggle on/off
- `RGB_BRI/BRD`: Brightness increase/decrease
- `RGB_HUI/HUD`: Hue increase/decrease
- `RGB_SAI/SAD`: Saturation increase/decrease
- `RGB_EFF/EFR`: Effect forward/reverse

### Bootloader Access

From CONFIGURATION layer:
- `bootloader` binding enters DFU mode for firmware updates
- `sys_reset` performs system reset
- Default CYBER60 bootloader: Hold ESC while plugging in (or FN+ENTER for BT60)

## ZMK Studio

ZMK Studio support is enabled via:
- `studio_unlock` binding in configuration layer
- CDC ACM UART for runtime communication
- Snippet: `studio-rpc-usb-uart` in build.yaml

## Hardware Details

**MCU**: nRF52840 (ARM Cortex-M4)
- Bluetooth 5.0
- USB 2.0
- 1MB Flash / 256KB RAM

**Power Management**:
- Battery monitoring via voltage divider (P0.03)
- External power control (P0.31) for RGB

**Matrix Scanning**:
- Column-to-row diode direction
- Wakeup source enabled for power efficiency
