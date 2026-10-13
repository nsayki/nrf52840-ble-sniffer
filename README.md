# nRF52840 BLE Sniffer

> Capture Bluetooth LE packets with a Nordic nRF52840 Dongle and inspect them in Wireshark.

This repository documents a working setup using Nordic's ready-made **nRF Sniffer
for Bluetooth LE** firmware and its official Wireshark capture plugin.

## Hardware and workflow

| Component | Setup |
| --- | --- |
| Board | nRF52840 Dongle, PCA10059 |
| Firmware | Nordic nRF Sniffer for Bluetooth LE 4.1.1 |
| Firmware programming | USB DFU from WSL, preserving the existing bootloader |
| Live capture | Windows Wireshark and the Windows `nrfutil ble-sniffer` plugin |
| Capture interface | `nRF Sniffer for Bluetooth LE COM28` on the verified computer |

The dongle captures BLE traffic, Nordic's host tool reads it over USB serial, and
Wireshark decodes the packets. The firmware is supplied by Nordic; an SDK or
application build is not required for this workflow.

A USB device attached to WSL is unavailable to Windows. Use WSL for firmware
programming, then return the dongle to Windows for live capture.

## Quick start

For a dongle already running the sniffer firmware, install the official capture
plugin in **Windows PowerShell**:

```powershell
nrfutil install ble-sniffer
nrfutil ble-sniffer bootstrap --extcap-dir "$env:APPDATA\Wireshark\extcap"
```

Restart Wireshark, select the **Default** profile, and start capturing on
**nRF Sniffer for Bluetooth LE COM28**. The COM number can differ on another
computer. Enter `btle` in the display filter bar to show Bluetooth LE packets.

For a fresh installation or a dongle that needs programming, follow the setup
guide first.

## Guides

- [Setup: tools, USB ownership, and USB DFU](docs/SETUP.md)
- [Usage: capture, standard filters, and troubleshooting](docs/USAGE.md)

## Verified environment

| Component | Version |
| --- | --- |
| Linux and Windows nRF Util core | 8.2.1 |
| nRF Util `device` command | 2.22.0 |
| nRF Util `ble-sniffer` command | 0.21.0 |
| Dongle firmware | 4.1.1 |
| Windows Wireshark | 4.6.9 |

The host command and dongle firmware have separate version numbers. This table
records the tested installation rather than specifying the latest releases.

## Validation

Firmware programming through application-only USB DFU and capture with Nordic's
original tools were verified on **2026-10-09**:

| Capture path | Captured frames | Decoded BLE frames |
| --- | --- | --- |
| Windows nRF Util CLI | 26,900 | 26,900 |
| Windows Wireshark GUI, 15 seconds | 6,706 | 6,706 |

The stock Nordic configuration was revalidated on **2026-10-13**: a 10-second
Windows Wireshark capture decoded **6,697 BLE frames**. The built-in
Source column filter matched 82 frames for a sampled source value.

Local packet captures and runtime logs are excluded from Git. Keep captures in
`captures/` when reproducing the setup.

## Third-party software

Nordic supplies the firmware and capture tools through its official downloads
and nRF Util commands. Their own licenses apply separately to those components.
The repository contains independently written setup and usage documentation;
the Nordic binaries are downloaded directly from Nordic during setup.

The installed `nrfutil-ble-sniffer` 0.21.0 package identifies its host-tool license
as `LicenseRef-Nordic-1-Clause`, which restricts redistribution of that software.
Inspect the installed package's `licenses.json` under
`~/.nrfutil/share/nrfutil-ble-sniffer/` for the bundled license texts. Any license
chosen for this repository's original documentation does not replace Nordic's
licenses or apply to linked third-party material.

[Nordic licensing documentation](https://docs.nordicsemi.com/r/bundle/nrfutil/page/readme.html/licenses)

## References

- [Nordic sniffer setup](https://academy.nordicsemi.com/courses/bluetooth-low-energy-fundamentals/lessons/lesson-6-bluetooth-le-sniffer/topic/nrf-sniffer-for-bluetooth-le/)
- [Nordic sniffer user guide](https://docs.nordicsemi.com/r/bundle/nrfutil/page/nrfutil-ble-sniffer/guides/running_sniffer.html)
- [Wireshark BLE display filter reference](https://www.wireshark.org/docs/dfref/b/btle.html)
- [Wireshark column display filter reference](https://www.wireshark.org/docs/dfref/_/_ws.col.html)
