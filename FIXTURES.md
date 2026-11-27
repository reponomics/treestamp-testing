# Fixture inventory

New scenarios follow [POLICY.md](POLICY.md). This inventory distinguishes what a fixture demonstrates from what remains to be tested. A fixture's existence is not a passing result.

| Identifier | Inputs and purpose | Status and required checks |
| --- | --- | --- |
| `adaptive-svg-experiment` | Root README and `assets/treestamp-adaptive.svg`; one relative Markdown image with internal appearance CSS | Historical hand-adjusted experiment from commit `5c066c76a9f6db2b1a2d28bc0815141cd87249bc`. Inspect the actual README in recorded browser/theme combinations. Its metadata declares `theme="light"`; it is not a current adaptive exporter round-trip fixture. |
| `paired-svg-control` | Root README and `assets/treestamp-light.svg` / `assets/treestamp-dark.svg`; fixed appearances selected through `picture` | Historical visual control, last changed at `a75d254973aad7bebbcf7a01cd4a90f50c13cdbc`. Compare image loading, contrast, transparency, and layout with the single-file experiment. |

The SVG trees are illustrative and differ from this repository's actual files. Loading them against the repository can invoke maintenance or note transfer; that is not a pure document round trip. Preserve the assets as controls. A current-export or local/hosted parity scenario needs matching source inputs, a generation command, producer version, and expected semantics.

No new visual inspection, real filename-encoding certification, or cross-editor parity result is claimed by this inventory. Add those scenarios and observations explicitly when exercised.
