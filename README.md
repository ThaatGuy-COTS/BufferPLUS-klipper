# Mellow LLL Filament Buffer - Klipper Configuration

Complete Klipper configuration for the Mellow LLL Filament Plus Buffer with automatic filament feeding and buffer management.

## Features

- ✅ **Automatic Buffer Control** - Fills buffer automatically when filament is detected
- ✅ **Smart Feed Bursts** - HALL2 sensor triggers small feed bursts during printing
- ✅ **Overfill Protection** - HALL1 prevents buffer from jamming into extruder
- ✅ **Manual Feed/Retract** - Physical buttons for manual filament loading
- ✅ **Filament Runout Detection** - Optional pause on filament runout

## Hardware Setup

### Sensor Configuration
- **ENDSTOP3 (PB7)**: Filament entrance sensor - detects when filament is loaded
- **HALL3 (PB4)**: Initial fill sensor - switches from continuous to burst mode
- **HALL2 (PB3)**: Primary buffer control - triggers feed bursts when neck extends
- **HALL1 (PB2)**: Overfill limiter - prevents buffer from over-filling

### Button Configuration
- **Feed Button (PB12)**: Manual continuous feed (hold to feed)
- **Retract Button (PB13)**: Manual continuous retract (hold to retract)

## Installation

1. Copy `mellow.cfg` to your Klipper config directory
2. Add to your `printer.cfg`:
   ```cfg
   [include mellow.cfg]
   ```
3. Update the MCU serial ID in `mellow.cfg`:
   ```cfg
   [mcu LLL_PLUS]
   serial: /dev/serial/by-id/usb-Klipper_stm32f072xb_YOUR_ID_HERE-if00
   ```
4. Restart Klipper: `FIRMWARE_RESTART`

## How It Works

### Initial Loading
1. Insert filament into entrance sensor (ENDSTOP3/PB7)
2. Buffer starts **continuous feeding** automatically
3. When neck reaches top (HALL3 triggers), switches to **burst mode**

### During Printing
1. Printer pulls filament → Buffer neck extends
2. When neck reaches mid-point (HALL2 releases) → **15mm feed burst**
3. Neck retracts back into housing
4. Repeat as needed

### Overfill Protection
1. If buffer overfills and neck extends too far (HALL1 releases)
2. **Auto-feed pauses** until neck retracts
3. Prevents jamming against extruder

## Configuration Tuning

### Adjust Feed Burst Amount
Change the burst size in `_BUFFER_FEED_BURST`:
```cfg
G1 E15 F3000  # Change E15 to desired amount (mm)
```

### Adjust Feed Speed
Change feed/retract speed (currently 3000 mm/min = 50 mm/s):
```cfg
G1 E10 F3000  # Change F3000 to desired speed
```

### Motor Current
Adjust TMC2208 current if needed:
```cfg
[tmc2208 extruder1]
run_current: 0.35  # Increase if motor skips, decrease if overheating
```

## Troubleshooting

**Buffer feeds continuously and won't stop:**
- Check HALL3 sensor is working properly
- Verify neck can physically reach HALL3 when extended

**HALL2 bursts happen too frequently:**
- Increase burst amount (E15 → E20)
- Check reverse bowden tension

**Buffer overfills (HALL1 warning):**
- Decrease burst amount (E15 → E10)
- Check that printer is actually pulling filament

**Manual buttons don't work:**
- Verify button wiring to PB12/PB13
- Check console for "button pressed" messages

## Credits

Configuration developed by ss1gohan13 for the Mellow LLL Filament Plus Buffer.

Based on the [original Mellow design](https://github.com/mellow-3d/lll-filament-buffer).

## License

MIT License - Feel free to use and modify!
