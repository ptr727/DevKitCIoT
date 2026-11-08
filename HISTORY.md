# DevKitCIoT

ESP32-DevKitC IoT controller board with a PSU, AC relay, dry contact inputs, and temperature sensor.

## Release History

- Version 0.3:
  - Migrated the design to KiCad 10.
  - Updated every standard symbol and footprint to the KiCad 10 library, so ERC and DRC report no warnings. The symbol fields are synced into the board. The copper layout is unchanged, apart from rounded pin 1 pads and diagonal thermal spokes on four pads that were oval before.
  - Copied every custom part into a project-local library, so the repository needs no external part library.
  - The board's version text and title block revision read 0.3.
  - Brought the repository up to the fleet standard, with carried governance docs and LF line endings.
  - Enabled every ERC and DRC check, with no check set to ignore. The high-voltage graphics are exempt from the courtyard requirement, and the relay driver no longer joins four connections at one point.
  - Added a CI gate that runs the KiCad ERC and DRC checks. It fails on warnings as well as errors, in a KiCad 10.0.7 image matching the local install.
- Version 0.2:
  - Designed in KiCad 5.99, ordered from JLCPCB in September 2021, and released as `v0.2-alpha`. The original order archive is kept in `fabrication/`.
