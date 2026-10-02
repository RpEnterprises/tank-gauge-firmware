# Tank Gauge Firmware Releases

Compiled firmware binaries for the Tank Level Gauge field units, fetched directly
by each unit's on-device "Firmware Update" screen (Diagnostics -> Firmware Update)
over WiFi. This repo holds compiled `.bin` files only - never the `.ino` source -
so a field unit (or anyone with the raw file link) can update without needing the
Arduino toolchain or the source code.

## Layout

Each sketch variant has three files, all flat at the repo root:

| File | Contents |
|---|---|
| `no_gallons_version.txt` | A single integer - the latest version number for the No_Gallons sketch |
| `no_gallons.sha256` | The lowercase hex SHA-256 of `no_gallons.bin`, nothing else |
| `no_gallons.bin` | The compiled firmware binary for the No_Gallons sketch |
| `with_gallons_version.txt` | A single integer - the latest version number for the With_Gallons sketch |
| `with_gallons.sha256` | The lowercase hex SHA-256 of `with_gallons.bin`, nothing else |
| `with_gallons.bin` | The compiled firmware binary for the With_Gallons sketch |

The device checks `*_version.txt` first (a few bytes) before downloading the full
`.bin`, and only proceeds if the published version is newer than the one it's
currently running. It downloads the whole `.bin` into RAM, verifies it against
`*.sha256` fully before writing anything to flash, and aborts if either check fails.

## Publishing a new release

1. In the Arduino IDE, bump `FIRMWARE_VERSION` in the sketch, then
   **Sketch -> Export Compiled Binary** - this produces a `.bin` in the sketch folder.
2. Replace the matching `.bin` here, update its `.sha256`, and bump the matching
   `_version.txt` to the same number used in step 1 - these three files are
   committed together, one release at a time, never partially.
