# Personal ZMK Keyboard Configurations

ZMK firmware configurations for custom mechanical keyboards.

## CYBER60 Rev D

Custom 60% keyboard powered by nRF52840 with advanced features.

### Features
- **Connectivity**: Bluetooth LE (4 profiles) + USB
- **Lighting**: RGB underglow (16 WS2812 LEDs)
- **Input**: Rotary encoder for volume control
- **Extras**: Pointing device support, ZMK Studio compatible
- **Security**: Bluetooth passkey entry enabled

### Layers

1. **DEFAULT (Layer 0)** - Standard QWERTY with GRAVE key, long spacebar, no right Win key
2. **ALTERNATIVE (Layer 1)** - Same as DEFAULT but Win key outputs Win+Alt combo
3. **RAISE (Layer 2)** - F1-F12, navigation (HOME/END/arrows/PG↑↓), media controls, mouse buttons (MB4/MB5)
4. **CONFIGURATION (Layer 3)** - Bluetooth profiles, mouse movement, output selection (USB/BLE), media controls, system functions
5. **UNDERGLOW (Layer 4)** - RGB lighting controls (effects, brightness, hue, saturation)

**Layer Access:**
- **RAISE**: Hold RAISE key (Row 5, right side between ALT and CTRL)
- **CONFIGURATION**: Tap/hold CFG key (far right of Row 4)
- **ALTERNATIVE**: Toggle from CONFIGURATION → TALT (Win position)
- **UNDERGLOW**: From CONFIGURATION → hold UND (ESC position)

See [detailed layer documentation](docs/keybinding-schema.md) for complete keymap visualization with SVG diagrams.

### Quick Access

- **Bootloader Mode**: Hold ESC while plugging in, or CFG → BOOT (far right Row 3)
- **Volume**: Rotary encoder twist
- **Play/Pause**: Press rotary encoder
- **Function Keys**: RAISE + 1-9, 0, -, = for F1-F12
- **Arrow Keys**: RAISE + Q/W/E for HOME/UP/END, A/S/D for arrows
- **Bluetooth Profiles**: CFG → 1/2/3/4/5 for BT0-BT4
- **Clear BT Profile**: CFG → BTCLR (backspace position)
- **RGB Toggle**: CFG → UND (hold) → TOG
- **RGB Controls**: CFG → UND → (-, =) for effects, ([, ], \) for brightness, (', #) for hue, (., /) for saturation
- **System Reset**: CFG → RST (far right Row 5)
- **Studio Mode**: CFG → STU (GRAVE position)
- **Alternative Mode**: CFG → TALT (Win position) to toggle Win+Alt combo

### Building

Firmware builds automatically via GitHub Actions on push. Download artifacts from the Actions tab.

For local builds:
```bash
# Initialize workspace
west init -l config
west update

# Build firmware
west build -b cyber60_rev_d
```

---

## Legacy Keyboards

### TOFU60 v2
**Bootloader**: Unplug keyboard, hold ESC, plug back in

3 layers: Default, Raise (navigation), Configuration (volume, bootloader, reset)

### BT60 v2
**Bootloader**: FN+ENTER

3 layers: Default, Raise (navigation), Configuration (Bluetooth, backlight, volume, bootloader, reset)
