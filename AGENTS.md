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

## Practical Tips and Notes Sections

When adding a `Practical Tips and Notes` section to a chapter README, keep it clearly separate from the book summary.

### Purpose

- Use this section for field-tested operational guidance, implementation heuristics, and common failure modes that are relevant to the chapter but not necessarily stated in the source book.
- Do not present these notes as if they came from the book unless the book explicitly says them.
- Prefer practical judgment: what to measure, what can go wrong, what to baseline, and what trade-off to verify.

### Structure

- Add `Practical Tips and Notes` as a normal top-level section near the end of the chapter, usually after validation/checklist material and before roadmap/summary material.
- Add it to the table of contents.
- Use short `###` subsections with descriptive names instead of making every item a GitHub admonition.
- Keep the main content as normal paragraphs or compact tables.
- Use a final quick-reference table when it helps map symptoms to first checks.

### GitHub Admonitions

- Use `> [!TIP]` only for a small number of immediately actionable practices.
- Use `> [!WARNING]` only for risks that can cause incorrect conclusions, bad placement, data loss, wasted hardware, or major performance regressions.
- Avoid wrapping the whole section, or most of the section, in `TIP`, `NOTE`, or `WARNING` blocks. Too many admonitions make the section hard to scan.
- Prefer normal prose for ordinary explanatory notes.

### Content Style

- Keep tips concrete and measurable. Mention tools, metrics, commands, or observable symptoms when useful.
- Distinguish utilization from goodput, capacity from bandwidth, and allocation from topology.
- When discussing external images or references, prefer official sources for embedded images. Use third-party articles as additional references unless there is a strong reason to embed them.
- If a referenced image is similar but not the same system, add an explicit note explaining the difference.

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
