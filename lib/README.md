# Library Notes

The project-local KiCad library this board uses, under the `DevKitCIoT` nickname.

- [`DevKitCIoT.kicad_sym`][symbol-library]: the custom symbols.
- [`DevKitCIoT.pretty/`][footprint-library]: the custom footprints.
- [`packages3d/`][packages3d]: the 3D models the footprints reference through `${KIPRJMOD}/lib/packages3d/`.

## Source

Every part here was copied from release `2.0.0` of the author's private ptr727 KiCad part library. Only the library nickname and the 3D model paths were changed. The project's [`sym-lib-table`][sym-lib-table] and [`fp-lib-table`][fp-lib-table] register it, so the board opens and checks without any global library setup.

## Attribution

The original work, the source recorded for each part, and the terms the third-party content stays under are in the repository's [Attribution][attribution] notes.

<!-- Repo -->

[attribution]: ../ATTRIBUTION.md
[footprint-library]: ./DevKitCIoT.pretty/
[fp-lib-table]: ../fp-lib-table
[packages3d]: ./packages3d/
[sym-lib-table]: ../sym-lib-table
[symbol-library]: ./DevKitCIoT.kicad_sym
