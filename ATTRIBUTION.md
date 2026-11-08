# Attribution

This repository holds content under three sets of terms. The author's original work is licensed under the MIT License, which permits commercial use. Third-party parts copied into `lib/` are included for non-commercial reference only, under their providers' own terms. Standard KiCad library content is used under the KiCad libraries' own license.

## Original Work

The author's original work is licensed under the [MIT License][license]. It allows any use, commercial use included, as long as the license and copyright notice are kept. The original work covers:

- the circuit design and the board layout, as drawn in the schematic and board files
- the hand-drawn high-voltage warning graphic
- the fabrication outputs, plots, and renders generated from the design, apart from the third-party and standard KiCad library content they reproduce
- the written documentation
- the scripts, workflows, and repository and project configuration files

## Third-Party Content

The custom symbols, footprints, and 3D models in [`lib/`][lib] were copied from the author's own part library. Apart from the hand-drawn graphic, that content was sourced from manufacturers and third-party providers, or from sources the part library did not record. KiCad embeds copies of those symbols and footprints in the schematic and board files. The fabrication outputs and the board plot reproduce the footprints, the schematic PDF reproduces the symbols, and the renders show the 3D models. The plots, the PDF, and the renders are under [`docs/`][docs].

That content belongs to its respective owners, and no copyright or ownership is claimed over it. The MIT License does not extend to it, and this repository grants no rights to it. It is included only so the design opens, checks, and renders as built, for non-commercial reference use, as is and without warranty of any kind. Any other use, commercial use included, is governed by each provider's own terms. So obtain the content from its provider under those terms.

## Sources

The table below gives the source the part library recorded for each copied part. A later part library revision sometimes recorded the source of content identical to the copy here, apart from descriptive text and model paths. In that case the table uses that record. "Not recorded" means the part library has no record of the original source.

| Part | Symbol | Footprint | 3D model |
| --- | --- | --- | --- |
| Phoenix Contact SPTAF-1 terminal blocks (1862055, 1862068, 1862453) | Not recorded | Not recorded | [Phoenix Contact][phoenixcontact-link], the manufacturer |
| RECOM RAC03-05SK power supply | [Component Search Engine][componentsearchengine-link] | [Component Search Engine][componentsearchengine-link] | [RECOM][recom-link], the manufacturer |
| Littelfuse 392 series fuse (39215000440) | Not recorded | [Component Search Engine][componentsearchengine-link] | [Component Search Engine][componentsearchengine-link] |
| Espressif ESP32-DevKitC module | Not recorded | Not recorded | [Espressif][espressif-link], the manufacturer |
| Omron G5Q-1 relay | Not recorded | Not recorded | Not recorded |
| Broadcom ACPL-827 optoisolator | Not recorded | Standard KiCad library, not copied to `lib/` | Standard KiCad library, not copied to `lib/` |
| High-voltage warning graphic | Not applicable | Hand-drawn, original work | Not applicable |

## Standard KiCad Library Content

The board also uses standard symbols, footprints, and 3D models from the [KiCad official libraries][kicad-libraries-link] bundled with KiCad. KiCad embeds copies of the standard symbols and footprints in the schematic and board files, and references the standard 3D models by path. The fabrication outputs, plots, and renders reproduce that content too. That content is used under the [KiCad libraries license][kicad-license-link], and the non-commercial note above does not apply to it.

## Removal Requests

If you are a rights holder and would like content here removed, or its attribution changed, please contact the repository owner through their [GitHub profile][owner-link].

<!-- Repo -->

[docs]: ./docs/
[lib]: ./lib/
[license]: ./LICENSE

<!-- External -->

[componentsearchengine-link]: https://componentsearchengine.com/
[espressif-link]: https://www.espressif.com/
[kicad-libraries-link]: https://www.kicad.org/libraries/
[kicad-license-link]: https://www.kicad.org/libraries/license/
[owner-link]: https://github.com/ptr727
[phoenixcontact-link]: https://www.phoenixcontact.com/
[recom-link]: https://recom-power.com/
