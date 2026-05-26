# Repository Guidelines

## Diagram and SVG Style

When creating or updating SVG diagrams in this repository, use the shared editorial diagram style unless a local file clearly uses a different established style.

### Palette

- Background: `#F3EFE7` or `#F5F1EA`
- Primary text: `#050505`
- Secondary text: `#6F6A63`
- Accent red/orange: `#D9392E`
- Main line: `#111111`
- Subtle border/grid: `#D8D1C7`

### Layout

- Prefer clean technical diagrams over slide-like hero layouts.
- Do not include a large title/subtitle inside the SVG when the README already introduces the figure.
- Keep diagrams flow-oriented, with thin line boxes and minimal filled color.
- Use accent red only for the most important warning, sync, loss, bottleneck, or critical path.
- Prefer whitespace, alignment, and line hierarchy over colored cards.
- Keep labels inside the viewBox; verify text is not clipped.
- Use large, bold text only for key numbers or core messages.
- Use small gray text for secondary explanations.

### Implementation

- SVGs should be hand-editable and use inline `<style>` classes.
- Avoid heavy shadows, gradients, decorative blobs, or multicolor palettes.
- Validate SVGs with:

```bash
xmllint --noout path/to/file.svg
```

- Before finishing, run:

```bash
git diff --check
```

## Git Commits

Use Git Conventional Commits for commit messages.

Examples:

```text
feat: add week03 tensor parallelism diagrams
fix: adjust SVG label positions
docs: translate week03 README to Korean
style: apply repository SVG diagram palette
chore: update repository guidelines
```

Prefer concise scopes when useful:

```text
docs(week03): add practical training notes
fix(svg): prevent QKV label clipping
```
