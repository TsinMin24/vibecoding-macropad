# ZMK Firmware for Nice Nano

Custom ZMK firmware for Nice Nano keyboard controller, optimized for Vibe Coding workflow.

## Features

- **2x5 Compact Layout** - Horizontal row design
- **Dual-mode Support** - USB wired + Bluetooth wireless
- **Special Key Bindings** - F5, F13-F20, Enter, Delete

## Physical Layout

```
┌─────┬─────┬─────┬─────┬─────┐
│ F5  │ F13 │ F14 │ F15 │ F16 │
├─────┼─────┼─────┼─────┼─────┤
│ F17 │ F18 │ F19 │ F20 │Enter│
└─────┴─────┴─────┴─────┴─────┘
         Delete (matrix position 10)
```

## Key Mappings for Vibe Coding

| Key   | Usage                    |
|-------|--------------------------|
| F5    | Re-run/test code         |
| F13   | AI code completion       |
| F14   | Code refactoring         |
| F15   | Generate documentation   |
| F16   | AI code explanation      |
| F17-F20 | Code navigation/editing |
| Enter | Confirm/execute          |
| Delete | Delete selected code    |

## Building Firmware

### Option A: GitHub Actions (Recommended)

1. Fork this repository
2. Push changes to `main` branch
3. GitHub Actions automatically builds firmware
4. Download `.uf2` from Actions tab

### Option B: Local Build

```bash
# Install Zephyr SDK
pip3 install west

# Initialize workspace
west init -m https://github.com/zmkfirmware/unified-zmk --mr main zmk-config
cd zmk-config
west update

# Build firmware
west build -p -b nice_nano -- -DZMK_CONFIG=config
```

Build output: `build/zephyr/zmk.uf2`

## Flashing Firmware

### Step 1: Enter Bootloader Mode

1. Connect Nice Nano via USB
2. **Double-tap RST and GND pins quickly**
   - Use a wire or paperclip to short these pins twice rapidly
3. Board will appear as USB mass storage device
   - Name: `NICE_NANO` or similar

### Step 2: Flash Firmware

**Linux/Mac:**
```bash
cp build/zephyr/zmk.uf2 /media/$USER/NICE_NANO/
```

**Windows:**
```powershell
copy build\zephyr\zmk.uf2 E:\
```

**Or manually:**
- Open file manager
- Drag and drop `.uf2` file to the mounted device

### Step 3: Restart

- Board auto-restarts after flashing
- Wait for Bluetooth pairing (if using BLE)

## Testing

### USB Mode

1. Connect USB cable
2. Visit [Keyboard Tester](https://www.keyboardtester.com)
3. Press each key and verify:
   - F5 sends F5 key code
   - F13-F20 send respective codes
   - Enter sends Return key
   - Delete sends Delete key

### Bluetooth Mode

1. Enable Bluetooth on your computer
2. Put Nice Nano in pairing mode
   - Press and hold function key (if available)
3. Select "Nice Nano" from Bluetooth devices
4. Test all keys

### macOS Special Functions

F13-F20 map to system shortcuts by default:
- F13 = Show Desktop
- F14 = Dashboard
- F15 = Show All Windows
- And more in System Preferences → Keyboard

## Configuration Files

| File | Purpose |
|------|---------|
| `build.yaml` | Build configuration |
| `config/nice_nano.keymap` | Key bindings |
| `config/nice_nano.conf` | Compile options |
| `.github/workflows/build.yml` | Auto-build workflow |

## Customization

### Changing Key Bindings

Edit `config/nice_nano.keymap`:

```c
bindings = [
    &kp F5  &kp F13 &kp F14 &kp F15 &kp F16
    &kp F17 &kp F18 &kp F19 &kp F20 &kp RET
];
```

### Adding More Keys

Modify the bindings array with additional key codes:

```c
// Available key codes
&kp F1  through  &kp F24
&kp RET          // Enter
&kp DEL          // Delete
&kp SPC          // Space
```

### Enabling/Disabling Features

In `config/nice_nano.conf`:

```ini
# Disable Bluetooth
CONFIG_ZMK_BLE=n

# Disable USB
CONFIG_ZMK_USB=n

# Change sleep timeout (in ms)
CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=3600000  # 1 hour
```

## Troubleshooting

### Board Not Recognized

- Ensure you're in bootloader mode (double-tap RST+GND)
- Try different USB cable
- Check USB drivers (nRF52 drivers may be needed)

### Keys Not Working

- Verify `.uf2` file was copied completely
- Check keymap syntax
- Use keyboard tester to verify HID codes

### Bluetooth Not Connecting

- Ensure BLE is enabled in config
- Clear old pairings from computer
- Reset Nice Nano (hold RST+GND for 10 seconds)

## HID Reference

| Key | HID Code | ZMK Syntax |
|-----|----------|-----------|
| F5 | 0x3C | `&kp F5` |
| F13 | 0x44 | `&kp F13` |
| F14 | 0x45 | `&kp F14` |
| F15 | 0x46 | `&kp F15` |
| F16 | 0x47 | `&kp F16` |
| F17 | 0x48 | `&kp F17` |
| F18 | 0x49 | `&kp F18` |
| F19 | 0x4A | `&kp F19` |
| F20 | 0x4B | `&kp F20` |
| Enter | 0x28 | `&kp RET` |
| Delete | 0x4C | `&kp DEL` |

## Resources

- [ZMK Documentation](https://zmk.dev)
- [Nice Nano Documentation](https://nicekeyboards.com/nice-nano)
- [ZMK Keycodes Reference](https://zmk.dev/docs/keycodes)

## License

MIT License
