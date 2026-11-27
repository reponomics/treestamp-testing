# Rendering samples

These samples support informal rendering experiments within the scope of [POLICY.md](POLICY.md). They are disposable, may be changed or removed, and establish no durable contract or expectation that repository history will be preserved. Their presence alone does not establish a passing result.

| Sample | Inputs and purpose | Notes and possible checks |
| --- | --- | --- |
| `adaptive-svg-experiment` | Root README and `assets/treestamp-adaptive.svg`; one relative Markdown image with internal appearance CSS | Hand-adjusted experiment. Inspect the actual README in different browser/theme combinations. Its metadata declares `theme="light"`; it does not demonstrate a round trip through the current adaptive exporter. |
| `paired-svg-control` | Root README and `assets/treestamp-light.svg` / `assets/treestamp-dark.svg`; fixed appearances selected through `picture` | Visual comparison sample. Compare image loading, contrast, transparency, and layout with the single-file experiment. |

The SVG trees are illustrative and differ from this repository's actual files. Loading them against the repository can invoke maintenance or note transfer; that is not a pure document round trip. A meaningful local/hosted comparison would need matching source inputs and equivalent operations.

This document records no new visual inspection, real filename-encoding validation, or cross-editor parity result.
