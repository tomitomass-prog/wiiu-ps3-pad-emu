# Wii U USB Bridge – DS3-like experimental USB profile

## Status and limitations

**EXPERIMENTAL, NOT a complete DualShock 3 emulator.** The project sends DS3-layout 49-byte input reports (report ID `0x01` + 48 payload bytes), with a minimal HID descriptor and Sony VID/PID for local compatibility research. It **does not** emulate the full original 148-byte DS3 HID descriptor, DS3-specific Feature GET/SET reports or DS3 initialization. Nintendont may ignore it. Do **not** treat a passing GitHub build as proof of Nintendont support.

Firmware for LOLIN32 Lite is unchanged; it streams existing 20-byte UART packets to S2. The existing `usb-s2` code stays as your tested Windows baseline. This ZIP is an **overlay**: place these folders at the root of your existing GitHub repository, preserving its other folders. Do not replace the whole repo with only this ZIP.

## UART wiring

- LOLIN32 Lite GPIO17 (TX) -> S2 Mini GPIO16 (RX)
- GND -> GND
- Reverse GPIO16 <- GPIO17 optional, not used by this prototype
- USB data cable to S2 Mini; power each device safely, do not join independent 5V sources.

## GitHub Actions

Add `.github/workflows/ps3.yml` and `usb-s2-ps3/`. Add `shared/pad_packet.h` only if not already present (it matches prior version). Run **Build experimental DS3 USB (ESP32-S2)** in Actions. Download `lolin-s2-ps3-experimental` after success.

**Do not flash until the build is successful.** For a typical Arduino ESP32-S2 PlatformIO image, bootloader is at 0x1000, partitions at 0x8000, firmware at 0x10000; confirm against the actual generated artifacts before flashing.

## Test

1. Keep backup of your old known-working S2 image.
2. Connect both modules, test USB enumeration and actual input on PC.
3. Connect S2 to Wii U, launch an injected GameCube title from the Wii U menu, check controls.
4. If Wii U ignores it, this does **not** prove USB HID support is absent: this firmware still needs real DS3 descriptor, feature reports, and endpoint behavior. Next step is USB capture or reviewing Nintendont's DS3 initialization path.

### USB identity note

Sony VID/PID is used only for prototype emulation/testing; do not redistribute as a real Sony product. This firmware is not a licensed Sony USB implementation.
