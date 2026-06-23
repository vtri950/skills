---
name: visual-docs
description: >-
  Author or convert documentation to planview MDX — rich local viewer with
  Tabs, Steps, Callout, CodeBlock, Badge, and mermaid diagrams.
metadata:
  visibility: exported
---

# Visual Docs — Planview MDX Authoring

Create or convert documentation as `.mdx` files for the local **planview**
viewer at `http://localhost:3000`.

**Always read `references/planview-components.md` in full before writing any
`.mdx` file.** It is the single source of truth for the component catalogue,
substitution rules, output paths, and the pre-save checklist.

---

## When to use

- **`/visual-docs convert <file.md>`** — convert an existing Markdown file to
  planview MDX, applying the component substitution map from the reference.
- **`/visual-docs <topic or description>`** — author a new planview MDX document
  from scratch based on session context.

Both modes produce a flat `.mdx` file in `PLANS_DIR` and confirm the preview URL.

---

## Authoring from scratch

1. **Read `references/planview-components.md`** before writing a single line.
2. **Determine the slug** from the topic or user instruction.
3. **Output path**: `<PLANS_DIR>/<slug>.mdx`
4. **Add frontmatter**:
   ```
   ---
   title: <document title>
   ---
   ```
5. **Use the right component from the start** — do not write markdown tables or
   blockquotes and convert them later:
   - Grouped options or categories → `<Tabs>`
   - Ordered steps or procedures → `<Steps>`
   - Warnings, tips, constraints → `<Callout>`
   - Status labels → `<Badge>`
   - Command or prompt templates → `<CodeBlock>`
   - Model/tier comparison → `<ModelGuide />`
   - Delegation ownership → `<DelegationGuide />`
   - Diagrams → mermaid fenced code blocks
6. **Run the pre-save checklist** from `references/planview-components.md`
   before writing the file.
7. **Confirm preview URL**: `http://localhost:3000/plans/<slug>`
   (WSL2 Windows browser: `http://10.255.255.254:3000/plans/<slug>`)

---

## Converting an existing Markdown file

Follow the "Direct conversion workflow" in `references/planview-components.md`
exactly. Key rules:

- Replace every markdown table with a planview component — never leave `|...|`
  syntax in the output.
- Replace `> blockquotes` with `<Callout>`.
- Keep mermaid fenced blocks and GFM task lists (`- [ ]`) as-is.
- Write `{placeholder}` template variables as `[placeholder]` inside component
  children — MDX evaluates `{...}` as JavaScript. Only backtick inline code
  spans (`` `{placeholder}` ``) are safe for curly-brace content.
