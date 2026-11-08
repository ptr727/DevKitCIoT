# DevKitCIoT

ESP32-DevKitC IoT controller board with a PSU, AC relay, dry contact inputs, and temperature sensor.

This board was originally designed in KiCad 5.99, a nightly pre-release of KiCad 6.0, with parts from a private part library. Version 0.2 was ordered from JLCPCB in September 2021. The design was later revamped for KiCad 10. Its parts were copied into a project-local library, so the repository is self-contained. The repository was then made public and archived. The design files are kept for reference, and no further changes are planned.

## Build and Distribution

- **Source Code**: [GitHub][github-link] for the KiCad design files, fabrication outputs, and CI pipelines.
- **Versioned Releases**: [GitHub Releases][releases-link] for version-tagged source code.

### Build Status

[![Build Status][buildstatus-shield]][actions-link]\
[![Last Commit][lastcommit-shield]][commits-link]

### Releases

[![GitHub Release][releaseversion-shield]][releases-link]\
[![GitHub Pre-Release][prereleaseversion-shield]][releases-link]

### Release Notes

**Version: 0.3**:

**Summary**:

- Migrated the design to KiCad 10, with every custom part in a project-local library.
- Updated every standard symbol and footprint to the KiCad 10 library, so ERC and DRC report no warnings.
- Regenerated the fabrication outputs and documentation from the KiCad 10 design.

> [!WARNING]
> **Breaking Changes**:
>
> - The design files are in the KiCad 10 format and do not open in earlier versions of KiCad.

See [Release History][history] for complete release notes and older versions.

## Table of Contents

- [Build and Distribution](#build-and-distribution)
  - [Build Status](#build-status)
  - [Releases](#releases)
  - [Release Notes](#release-notes)
- [Table of Contents](#table-of-contents)
- [Overview](#overview)
  - [GPIO](#gpio)
- [KiCad Setup](#kicad-setup)
- [Design](#design)
  - [Safety](#safety)
  - [Power Supply](#power-supply)
  - [Relay](#relay)
  - [Dry Contact Input](#dry-contact-input)
  - [DS18B20 Temperature Sensor](#ds18b20-temperature-sensor)
  - [PCB Terminal Connectors](#pcb-terminal-connectors)
  - [ESP32 DevKitC v4](#esp32-devkitc-v4)
  - [Enclosure](#enclosure)
- [Manufacturing](#manufacturing)
- [Questions or Issues](#questions-or-issues)
- [3rd Party Tools](#3rd-party-tools)
- [License](#license)

## Overview

> [!WARNING]
> The board switches mains AC voltage, with a danger of electrocution or fire.\
> Do not program the board while it is connected to AC power.\
> Use at your own risk.

This is an IoT controller board built around an [ESPRESSIF ESP32-DevKitC][esp32-devkitc-link] development board. It carries an onboard PSU, an AC relay, isolated dry contact inputs, and a DS18B20 temperature sensor.

[![3D Render][3d-small]][3d]\
[![PCB Layout][pcb-small]][pcb]

The [schematic][schematic] is available as a PDF. Firmware is out of scope for this repository. The GPIO assignments below are what a firmware configuration, such as [ESPHome][esphome-link], needs.

### GPIO

The [GPIO pins][esp32-gpio-link] are assigned based on [usage][esp32-pinout-link]:

- GPIO22 : Dallas 1-wire bus
- GPIO23 : Status output
- GPIO33 : Relay output
- GPIO34 : Dry contact input 1
- GPIO35 : Dry contact input 2

## KiCad Setup

- The project requires KiCad 10.0 or later, and the files are saved in the KiCad 10 format that earlier versions cannot open.
- The standard parts match the KiCad 10.0.7 libraries, the release CI checks against. Another point release can report library mismatch warnings.
- The custom symbols, footprints, and 3D models are in the project-local [`lib/`][lib] library, registered by the project's own [`sym-lib-table`][sym-lib-table] and [`fp-lib-table`][fp-lib-table] under the `DevKitCIoT` nickname.
- The custom 3D models are referenced through `${KIPRJMOD}`, so no global path variable or library registration is needed.
- The standard symbol, footprint, and 3D model libraries are bundled with KiCad 10 and registered automatically.

## Design

### Safety

The PCB layout keeps a clear separation between the AC and DC components and tracks. The board is not designed for high power applications, and the AC line input is protected with a 5A fuse. The PSU is isolated and safety certified, only the PSU and not the board, and does not require any external protection components.

### Power Supply

The 3W 5V PSU is a [RECOM RAC03-05SK][rac03-link], is very small, is safety certified, and does not require any external protection components.

### Relay

The relay is an [Omron G5Q][g5q-link] 10A miniature 5V relay. The relay driver is a [BS270][bs270-link] N-Channel MOSFET, with a [SB1H100][sb1h100-link] schottky diode for the snubber. For safety the load should be less than 5A, and the line input is protected with a 5A fuse.

### Dry Contact Input

The inputs are isolated using [ACPL-827][acpl-827-link] dual optoisolators. The dry contact inputs are pulled high to 5V, and close low to ground. Transistor output is pulled high to 3.3V, activating low to ground.

### DS18B20 Temperature Sensor

A [Dallas DS18B20][ds18b20-link] temperature sensor is mounted near the PSU to monitor board temperature. A connector allows connecting external DS18B20 sensors.

### PCB Terminal Connectors

The terminal connectors are [Phoenix SPTAF 1 EL][sptaf-link], using 5mm pitch for AC and 3.5mm pitch for signal.

### ESP32 DevKitC v4

The PCB can accommodate variants of the [ESPRESSIF ESP32-DevKitC v4][esp32-devkitc-link] boards, e.g. [ESP32-DevKitC-32E][esp32-wroom-32e-link] or [ESP32-DevKitCVE][esp32-wrover-e-link].

### Enclosure

The board is designed to fit a [Hammond RZ0212C][rz0212c-link] plastic enclosure.

## Manufacturing

The [`fabrication/`][fabrication] directory holds the Gerber, drill, and BOM files generated from the KiCad 10 design. The [Operations][operations] runbook has the `kicad-cli` commands that generate them. The BOM identifies a sourced part by its MPN, the manufacturer part number. It identifies a generic part by its CPL, a [Common Parts Library][cpl-link] or "jellybean" part number. The `JLCPCB_Y3-3712339A.zip` archive is the original version 0.2 order as submitted to [JLCPCB][jlcpcb-link] in 2021. The current design has the same copper layout as that order, apart from two changes the KiCad 10 library footprints brought. Pin 1 pads on C1, C3, D4, and U2 are rounded, along with their solder mask openings. U2 pads 5 and 7, R5 pad 2, and R9 pad 2 meet their copper pour through diagonal thermal spokes. The silkscreen also differs in the version and in the standard part outlines, which follow the KiCad 10 library. The title block differs too.

## Questions or Issues

This repository is archived and read-only, so it takes no new issues or pull requests. Earlier discussions remain readable in [GitHub Issues][issues-link].

## 3rd Party Tools

The third-party tools, libraries, and actions this project depends on.

- [cspell][cspell-link]: Spell checker.
- [editorconfig-checker][editorconfig-checker-link]: Line-ending and whitespace linter.
- [GitHub Actions][github-actions-link]: CI and automation runner.
- [GitHub Dependabot][dependabot-link]: Dependency update bot.
- [KiCad][kicad-link]: Open source electronic design automation suite.
- [kicad/kicad][kicad-docker-link]: Docker image providing `kicad-cli`.
- [markdownlint-cli2][markdownlint-link]: Markdown linter.
- [Nerdbank.GitVersioning][nbgv-link]: Version computation from git height.
- [pre-commit][pre-commit-link]: Git hook manager.
- [PyMuPDF][pymupdf-link]: Python library for rendering and reading PDF files.
- [uv][uv-link]: Python package and project manager.

## License

Licensed under the [MIT License][license]\
![GitHub License][license-shield]

The MIT license covers the author's original work, the circuit design and board layout included, and permits commercial use. The symbols, footprints, and 3D models in [`lib/`][lib], apart from one hand-drawn graphic, come from manufacturers, third-party providers, or unrecorded sources. The schematic, board, and generated outputs embed or reproduce them. They remain the property of their respective owners under their own terms. No ownership is claimed over them, and the MIT license does not extend to them. They are included for non-commercial reference use only, as is and without warranty. Standard KiCad library content is used under the KiCad libraries' own license. See [Attribution][attribution] for the sources and terms.

<!-- Shields -->

[buildstatus-shield]: https://img.shields.io/github/actions/workflow/status/ptr727/DevKitCIoT/test-pull-request.yml?logo=github&label=Build
[lastcommit-shield]: https://img.shields.io/github/last-commit/ptr727/DevKitCIoT?logo=github&label=Last%20Commit
[license-shield]: https://img.shields.io/github/license/ptr727/DevKitCIoT?label=License
[prereleaseversion-shield]: https://img.shields.io/github/v/release/ptr727/DevKitCIoT?include_prereleases&filter=*-g*&label=GitHub%20Pre-Release&logo=github
[releaseversion-shield]: https://img.shields.io/github/v/release/ptr727/DevKitCIoT?logo=github&label=GitHub%20Release

<!-- Distribution -->

[actions-link]: https://github.com/ptr727/DevKitCIoT/actions
[commits-link]: https://github.com/ptr727/DevKitCIoT/commits/main
[github-link]: https://github.com/ptr727/DevKitCIoT
[issues-link]: https://github.com/ptr727/DevKitCIoT/issues
[releases-link]: https://github.com/ptr727/DevKitCIoT/releases

<!-- Repo -->

[3d]: ./docs/DevKitCIoT_3D.png
[3d-small]: ./docs/DevKitCIoT_3D_Small.png
[attribution]: ./ATTRIBUTION.md
[fabrication]: ./fabrication/
[fp-lib-table]: ./fp-lib-table
[history]: ./HISTORY.md
[lib]: ./lib/
[license]: ./LICENSE
[operations]: ./OPERATIONS.md
[pcb]: ./docs/DevKitCIoT_PCB.png
[pcb-small]: ./docs/DevKitCIoT_PCB_Small.png
[schematic]: ./docs/DevKitCIoT_Schematic.pdf
[sym-lib-table]: ./sym-lib-table

<!-- External -->

[acpl-827-link]: https://docs.broadcom.com/doc/AV02-1429EN
[bs270-link]: https://www.onsemi.com/download/data-sheet/pdf/bs270-d.pdf
[cpl-link]: https://octopart.com/common-parts-library
[cspell-link]: https://cspell.org
[dependabot-link]: https://github.com/dependabot
[ds18b20-link]: https://datasheets.maximintegrated.com/en/ds/DS18B20-PAR.pdf
[editorconfig-checker-link]: https://github.com/editorconfig-checker/editorconfig-checker
[esp32-devkitc-link]: https://www.espressif.com/en/products/devkits/esp32-devkitc
[esp32-gpio-link]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/api-reference/peripherals/gpio.html
[esp32-pinout-link]: https://docs.espressif.com/projects/esp-idf/en/latest/esp32/_images/esp32-devkitC-v4-pinout.jpg
[esp32-wroom-32e-link]: https://www.espressif.com/sites/default/files/documentation/esp32-wroom-32e_esp32-wroom-32ue_datasheet_en.pdf
[esp32-wrover-e-link]: https://www.espressif.com/sites/default/files/documentation/esp32-wrover-e_esp32-wrover-ie_datasheet_en.pdf
[esphome-link]: https://esphome.io/
[g5q-link]: https://omronfs.omron.com/en_US/ecb/products/pdf/en-g5q.pdf
[github-actions-link]: https://github.com/actions
[jlcpcb-link]: https://jlcpcb.com/
[kicad-docker-link]: https://hub.docker.com/r/kicad/kicad
[kicad-link]: https://www.kicad.org/
[markdownlint-link]: https://github.com/DavidAnson/markdownlint-cli2
[nbgv-link]: https://github.com/dotnet/Nerdbank.GitVersioning
[pre-commit-link]: https://pre-commit.com/
[pymupdf-link]: https://pymupdf.readthedocs.io/
[rac03-link]: https://recom-power.com/pdf/Powerline_AC-DC/RAC03-K.pdf
[rz0212c-link]: https://www.hammfg.com/part/RZ0212C
[sb1h100-link]: https://www.vishay.com/docs/88716/sb1h90.pdf
[sptaf-link]: https://www.phoenixcontact.com/assets/downloads_ed/local_us/web_dwl_promotion/SPTAF1_Flash_2017.pdf
[uv-link]: https://docs.astral.sh/uv/
