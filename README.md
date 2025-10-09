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

1. **DEFAULT** - Standard QWERTY layout
2. **ALTERNATIVE** - Same as default with modified Windows key behavior (tap dance)
3. **RAISE** (Right Alt) - Function keys, navigation (arrows, home/end, page up/down), mouse buttons, media controls
4. **CONFIGURATION** (Context Menu) - Bluetooth pairing, output selection (USB/BT), RGB controls, bootloader access, system reset
5. **UNDERGLOW** - RGB effect and color controls

### Quick Access

- **Bootloader Mode**: Hold ESC while plugging in, or press bootloader key in Configuration layer
- **Volume**: Rotary encoder twist (or Vol Up/Down in Configuration layer)
- **Play/Pause**: Press rotary encoder
- **Bluetooth Profiles**: Configuration layer → Number keys 1-4
- **Clear BT Profile**: Configuration layer → Backspace
- **Studio Mode**: Configuration layer → Caps Lock (studio_unlock)

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
