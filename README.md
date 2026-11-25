# Treestamp SVG rendering check

## Single adaptive SVG — experiment

This image references **one SVG file** through an ordinary relative Markdown image link. Its internal CSS uses `@media (prefers-color-scheme: dark)` to switch black ink to white. There is no `<picture>` element selecting between files for this image.

![Single adaptive treestamp](assets/treestamp-adaptive.svg)

Switch GitHub between light and dark appearance and compare this image with the two-file control below. Test both an explicit GitHub theme and a theme that follows the system; note your browser and whether the system appearance differs. The adaptive tree should have black text on light backgrounds and white text on dark backgrounds.

[Open the adaptive SVG](assets/treestamp-adaptive.svg). The repository file preview and the README image are separate rendering contexts; check the README itself.

```markdown
![Repository tree](assets/treestamp-adaptive.svg)
```

This is a compatibility probe, not a change to the application's exporter. It retains the light sample's metadata; the current application will regenerate a fixed light appearance when reopening it.

## Two-file control

A transparent directory tree with monochrome text, connected branches, and inline `#` annotations. The README selects white ink for dark mode and black ink for light mode. Both SVGs contain the same editable treestamp metadata.

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

The directories drawn inside the image are illustrative; this repository contains the README, two fixed SVG assets, and the adaptive SVG experiment.
