# Treestamp GitHub compatibility fixtures

This repository is a publicly visible, ephemeral workspace controlled by the [treestamp](https://github.com/reponomics/treestamp) maintainer, with write access currently restricted to that role. Its tentative purpose is live testing of GitHub rendering behavior, beginning with adaptive SVG inspection. It is isolated from treestamp's supply chain and development processes; its disposable contents are available for inspection and are not authoritative statements about the public product.

Read the [usage, access, and governance policy](POLICY.md) before publishing fixtures or running live checks. The [fixture inventory](FIXTURES.md) distinguishes historical experiments from current exporter output. Only the maintainer authorizes publication; routine tests do not push here. The retained images below are visual controls, not evidence that a current treestamp release has passed validation.

## Single adaptive SVG — experiment

This image references **one SVG file** through an ordinary relative Markdown image link. Its internal CSS uses `@media (prefers-color-scheme: dark)` to switch black ink to white. There is no `<picture>` element selecting between files for this image.

![Single adaptive treestamp](assets/treestamp-adaptive.svg)

Switch GitHub between light and dark appearance and compare this image with the two-file control below. Test both an explicit GitHub theme and a theme that follows the system; note your browser and whether the system appearance differs. The adaptive tree should have black text on light backgrounds and white text on dark backgrounds.

[Open the adaptive SVG](assets/treestamp-adaptive.svg). The repository file preview and the README image are separate rendering contexts; check the README itself.

```markdown
![Repository tree](assets/treestamp-adaptive.svg)
```

This is a historical, hand-adjusted compatibility probe. Its embedded metadata still declares the light appearance, even though its drawing adapts through CSS. Regeneration follows that metadata; this file is not a current adaptive export or a round-trip parity fixture.

## Two-file control

A transparent directory tree with monochrome text, connected branches, and inline `#` annotations. The README selects white ink for dark mode and black ink for light mode. Both SVGs embed the same editable treestamp document, with their respective light or dark appearance settings.

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/treestamp-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="assets/treestamp-light.svg">
  <img alt="Repository tree with inline annotations" src="assets/treestamp-light.svg">
</picture>

## What to check

- The image blends into the README background with no opaque panel.
- Switch GitHub between light and dark mode: names and annotations remain visible.
- Notes appear beside their entries, prefixed with `#`, rather than on nested rows.
- Branches connect, text is readable, and nothing is clipped.
- The ampersands in `notes & examples.md` and the parser annotation appear normally.

Inspect the individual files: [black ink / light appearance](assets/treestamp-light.svg) and [white ink / dark appearance](assets/treestamp-dark.svg). Each file has fixed ink; the README markup chooses which file to display.

## Relative-path markup

```html
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="assets/treestamp-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="assets/treestamp-light.svg">
  <img alt="Repository tree" src="assets/treestamp-light.svg">
</picture>
```

This follows [GitHub's documented theme-aware image pattern](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/quickstart-for-writing-on-github#adding-an-image-to-suit-your-visitors). Paths are relative to the README. A single fixed appearance also works with ordinary Markdown image syntax.

The directories drawn inside the images are illustrative and do not describe this repository's actual tree. Consult the fixture inventory before using them for an editor-loading comparison.
