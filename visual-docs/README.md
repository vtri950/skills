# visual-docs skill

This skill teaches your AI agent to author and convert documentation as rich
`.mdx` files for **planview** — a local web viewer that renders them in your
browser with interactive components, inline comments, and PDF export.

---

## What is planview?

planview is a local web app that renders `.mdx` files with rich components:
tabs, steps, callouts, badges, code blocks, and mermaid diagrams. It also
supports inline commenting on headings, git history per file, and HTML/PDF
export.

**It runs entirely on your machine — no cloud, no account, no database.**

---

## Install planview

```bash
npx @vtripathi/planview@latest --dir ./your-docs-directory
```

Then open [http://localhost:3000](http://localhost:3000).

| Flag | Description | Default |
|------|-------------|---------|
| `--dir <path>` | Directory containing `.mdx` files | current directory |
| `--port <port>` | Port to listen on | `3000` |

**WSL2:** open `http://10.255.255.254:3000` in your Windows browser.

---

## What this skill does

Once installed, your agent gains two capabilities:

### `/visual-docs <topic>`
Author a new `.mdx` document from scratch using planview components and save
it to your docs directory.

### `/visual-docs convert <file.md>`
Convert an existing Markdown file to planview-compatible MDX — replaces
markdown tables, blockquotes, and other unsupported syntax with the correct
components automatically.

---

## Supported MDX components

| Component | Use for |
|-----------|---------|
| `<Callout type="info\|warning\|tip\|danger">` | Notes, warnings, tips |
| `<Tabs labels={["A","B"]}><Tab>...</Tab></Tabs>` | Grouped options or categories |
| `<Steps><Step title="...">...</Step></Steps>` | Ordered procedures |
| `<Badge variant="success\|warning\|error\|info">` | Status labels |
| `<CodeBlock language="ts" title="file.ts">` | Code with syntax highlighting |
| `<ModelGuide />` | Model/tier comparison tables |
| `<DelegationGuide />` | Delegation ownership charts |
| ` ```mermaid ` fenced block | Flow and sequence diagrams |
| `- [ ]` / `- [x]` | GFM task lists |

> Narrow markdown tables (≤4 cols, short cells) are fine. Wide or semantic tables (model comparisons, delegation charts, step sequences) should use a component instead.

---

## Bundled reference file

The skill ships one reference file the agent reads before writing any `.mdx`:

| File | Purpose |
|------|---------|
| `references/planview-components.md` | Full component catalogue, markdown → component substitution map, direct conversion workflow, and pre-save checklist. |

---

## Workflow

1. Start planview:
   ```bash
   npx @vtripathi/planview@latest --dir ./docs
   ```
2. Ask your agent:
   - `/visual-docs my new feature` — author from scratch
   - `/visual-docs convert docs/my-doc.md` — convert existing file
3. The agent writes the `.mdx` file and gives you the preview URL.
4. Open the URL, click headings to comment, use the side drawer for git history.
5. Use the "Export HTML" button to save as PDF.
