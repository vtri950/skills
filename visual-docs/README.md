# visual-docs skill

Teaches your AI coding assistant to author and convert documentation as rich
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

Once installed, your assistant gains two capabilities:

### `/visual-docs <topic>`
Author a new `.mdx` document from scratch using planview components and save
it to your docs directory.

### `/visual-docs convert <file.md>`
Convert an existing Markdown file to planview-compatible MDX — replaces
tables, blockquotes, and ordered lists with the appropriate components.

---

## Supported MDX components

| Component | Use for |
|-----------|---------|
| `<Callout type="info\|warning\|tip\|danger">` | Notes, warnings, tips |
| `<Tabs labels={["A","B"]}><Tab>...</Tab></Tabs>` | Grouped options or categories |
| `<Steps><Step n={1}>...</Step></Steps>` | Ordered procedures |
| `<Badge color="blue\|green\|amber\|red\|gray\|purple">` | Inline status labels |
| `<CodeBlock language="ts">` | Copy-button prompt or command templates |
| `<ModelGuide />` | Model/tier comparison |
| `<DelegationGuide />` | Delegation ownership chart |
| ` ```mermaid ` fenced block | Flow and sequence diagrams (click to expand, scroll to zoom) |
| ` ```lang ` fenced block | Syntax-highlighted code (`github-dark-dimmed` theme) |
| `- [ ]` / `- [x]` | GFM task lists |

> Narrow markdown tables (≤4 columns, short cells) render with full styling. Wide or semantic tables should use a component instead.

---

## Bundled reference file

The skill ships one reference file the assistant reads before writing any `.mdx`:

| File | Purpose |
|------|---------|
| `references/planview-components.md` | Full component catalogue, markdown → component substitution map, direct conversion workflow, and pre-save checklist. |

---

## Workflow

1. Start planview pointing at your docs directory:
   ```bash
   npx @vtripathi/planview@latest --dir ./docs
   ```
2. Ask your assistant:
   - `/visual-docs my new feature` — author a doc from scratch
   - `/visual-docs convert docs/my-doc.md` — convert an existing file
3. The assistant writes the `.mdx` file and gives you the preview URL.
4. Open the URL, click any heading to add a note, use the side drawer for git history.
5. Use the "Export HTML" button to save as PDF.
