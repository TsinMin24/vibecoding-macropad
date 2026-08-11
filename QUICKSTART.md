# Quick Start Guide

## Your ZMK Configuration Files

All files are in: `zmk-config/`

### Files Created

```
zmk-config/
├── build.yaml                           # Build configuration
├── config/
│   ├── nice_nano.keymap                # Key bindings (2x5 layout)
│   └── nice_nano.conf                  # Compile options
├── .github/workflows/build.yml         # GitHub Actions workflow
└── README.md                           # Complete documentation
```

### Next Steps

**Option 1: GitHub Actions (Recommended)**

1. Create a new GitHub repository
2. Upload all files from `zmk-config/`
3. GitHub automatically builds firmware
4. Download `.uf2` from Actions tab
5. Flash to Nice Nano (see below)

**Option 2: Local Build**

```bash
cd zmk-config
pip3 install west
west init -m https://github.com/zmkfirmware/unified-zmk --mr main .
west update
west build -p -b nice_nano -- -DZMK_CONFIG=config
```

### Flash to Nice Nano

1. **Enter Bootloader Mode:**
   - Double-tap RST and GND pins quickly
   - Board appears as USB drive

2. **Copy Firmware:**
   ```bash
   # Linux/Mac
   cp build/zephyr/zmk.uf2 /media/$USER/NICE_NANO/

   # Or just drag-and-drop the .uf2 file
   ```

3. **Board auto-restarts and ready!**

### Key Layout

```
┌─────┬─────┬─────┬─────┬─────┐
│ F5  │ F13 │ F14 │ F15 │ F16 │
├─────┼─────┼─────┼─────┼─────┤
│ F17 │ F18 │ F19 │ F20 │Enter│
└─────┴─────┴─────┴─────┴─────┘
```

### Test It

Visit https://www.keyboardtester.com and press each key to verify.

For complete documentation, see `README.md`
