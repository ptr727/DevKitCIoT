# Operations

How this repo is run: verifying a board change, regenerating the fabrication outputs, and the KiCad tooling notes. The design itself is described in the [README](./README.md).

## Local Verification

CI runs the doc lint set and the KiCad checks on every pull request. The KiCad checks run in the official KiCad container image, which the validate hook pins by digest. The ERC and DRC checks fail the build on any error or warning, and print their reports in the job log. KiCad point releases ship different libraries, so the local install runs the same KiCad release the hook pins. CI cannot judge the mains AC side of the board. So a change near the AC section is not verified until a person has reviewed it.

- Run `kicad-cli sch erc --severity-all --exit-code-violations DevKitCIoT.kicad_sch` and `kicad-cli pcb drc --schematic-parity --severity-all --exit-code-violations DevKitCIoT.kicad_pcb`, the flags CI passes, and read the reports rather than only the counts. Both reports are clean, with no errors, no warnings, and no unconnected items. A library mismatch warning means a symbol or footprint has drifted from the library of the pinned KiCad release. Fix it by updating the part from the library, never by excluding the warning.
- Keep every ERC and DRC check at error or warning in the project file, none at ignore. `kicad-cli` cannot change a check's severity, since its severity flags only filter the report. The high-voltage warning graphics are exempt from the courtyard requirement, because a silkscreen-only graphic has no body to keep clear.
- Review the AC section for clearance against the `AC` net class in the board's design rules. DRC enforces the rules as configured, and cannot judge whether they are adequate.
- Compare pad geometry against the previous revision when a footprint is relinked. A relink can change a footprint silently while ERC and DRC stay clean. U1 uses the as-built `TO-92_Inline_Wide` footprint, while KiCad's DS18B20 symbol defaults to `TO-92_Inline`.
- View the 3D render after any footprint or model change, with `kicad-cli pcb render`.

## Runbooks

### Regenerate the Fabrication Outputs

Run these from the repository root. They write the Gerber, drill, and BOM files into `fabrication/` from the board and schematic.

```shell
kicad-cli pcb export gerbers -o fabrication/ -l F.Cu,B.Cu,F.Paste,B.Paste,F.Silkscreen,B.Silkscreen,F.Mask,B.Mask,Edge.Cuts --subtract-soldermask --precision 6 DevKitCIoT.kicad_pcb
kicad-cli pcb export drill -o fabrication/ --format excellon --drill-origin absolute --excellon-units mm --excellon-zeros-format decimal --excellon-oval-format alternate --excellon-separate-th DevKitCIoT.kicad_pcb
kicad-cli sch export bom -o fabrication/DevKitCIoT_BOM.csv --fields 'Reference,Value,Footprint,${QUANTITY},MPN,CPL' --labels 'References,Value,Footprint,Quantity,MPN,CPL' --group-by 'Value,Footprint,MPN,CPL' --sort-field Reference --ref-delimiter ' ' --ref-range-delimiter '' DevKitCIoT.kicad_sch
```

The Gerber export also writes a `DevKitCIoT-job.gbrjob` job file, which `.gitignore` excludes. Commit the regenerated files with the board change that caused them, so the outputs never describe a different board than the one committed. Keep the original JLCPCB order archive unchanged, since it records what was actually fabricated.

### Regenerate the Documentation Images

Run these from the repository root.

```shell
kicad-cli sch export pdf -o docs/DevKitCIoT_Schematic.pdf DevKitCIoT.kicad_sch
kicad-cli pcb render -o render-3d.png --width 2076 --height 1663 --side top --quality high --background opaque DevKitCIoT.kicad_pcb
kicad-cli pcb export pdf --mode-single -o board.pdf -l B.Cu,F.Cu,B.Silkscreen,F.Silkscreen,F.Courtyard,B.Courtyard,Edge.Cuts,User.Drawings --scale 1 --bg-color '#000000' --drill-shape-opt 2 DevKitCIoT.kicad_pcb
```

The schematic PDF is written in place. Crop `render-3d.png` to the board for `docs/DevKitCIoT_3D.png`. Rasterize `board.pdf` and crop it to the board outline for `docs/DevKitCIoT_PCB.png`, for example with PyMuPDF. The `_Small` variants are the same images scaled to 800 pixels wide. Delete the two intermediate files afterwards.

## Backup and Recovery

The repository is the backup. The design, the generated outputs, and the original order archive are all committed.

## Logs and Debugging

The ERC and DRC reports are printed in the CI job log of the validate hook.

## Tool Usage

- KiCad 10.0.7, installed per user, the release the validate hook pins. The files are in the KiCad 10 format and do not open in earlier versions.
- The validate hook runs the KiCad container as the runner's user with `HOME=/tmp`. That home has no global library tables, so the hook copies KiCad's template tables into it. Without them, every standard library reads as missing.
- KiCad rewrites `DevKitCIoT.kicad_pro` on every close, and the committed file is its own output, so opening and closing the project changes nothing. KiCad writes the schematic, board, symbol, footprint and library table files in text mode. Their line endings are the platform's own, CRLF on Windows and LF elsewhere. The project, local settings and custom rules files are LF everywhere. Git stores every tracked KiCad file as LF. It checks the text-mode group out with native line endings when `core.eol` is unset and `core.autocrlf` is false, or true on Windows. A KiCad save then changes nothing Git reports.
- A scripted edit to a KiCad file keeps the file's existing line endings and writes items in KiCad's save order. The next save then moves nothing. The schematic's cached library symbols sort by name in ASCII order, and so do the unit sub-symbols inside each one. Other items group by type, here in the order `text`, `junction`, `no_connect`, `wire`, `global_label`, then placed `symbol`, and sort by UUID within each type. Confirm the order of any file, the board included, by saving it in KiCad and checking that `git diff` shows no change.
- `kicad-cli --board-plot-params` uses the board's saved plot settings, which here produce PDF rather than Gerber, so the Gerber export passes its layers and options explicitly.
- KiCad 10's BOM export reads only the fields set on each placed symbol, and does not fall back to the library symbol's description or datasheet.

## Configuration Layout

- Every input is trusted: the KiCad files, the generated outputs, and the configuration are all maintainer-authored, and the repository takes no outside input.
- The project file `DevKitCIoT.kicad_pro` holds the net classes, design rules, and the ERC and DRC severities.
- The project library tables register the `DevKitCIoT` nickname from `lib/` through `${KIPRJMOD}`, and the footprints reference their 3D models the same way. The custom parts depend on no global path variable or library registration. The standard KiCad models resolve through KiCad's own `${KICAD10_3DMODEL_DIR}`.
