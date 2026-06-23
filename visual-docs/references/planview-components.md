# Planview component library — MDX authoring rules

This file governs how to author `.mdx` files that will be rendered in the
**planview** local viewer at `/home/vtripathi/git_clones/AI/planview/`.
Read it in full before authoring or converting any document to MDX for planview.

---

## Direct conversion workflow — "convert X.md for planview"

When asked to convert a Markdown file to planview MDX, execute these steps
directly without exploration:

1. **Read** the source `.md` file.
2. **Determine output path** — the output is a flat `.mdx` file written to
   `PLANS_DIR`. For this project, `PLANS_DIR` is:
   `/home/vtripathi/git_clones/ai_first/air.ai-assisted-delivery-pipeline/pipeline/`
   Output path: `<PLANS_DIR>/<slug>.mdx` where `<slug>` is the source filename
   without `.md`.
3. **Add frontmatter** — every planview MDX file must start with:
   ```
   ---
   title: <document title>
   ---
   ```
4. **Apply component substitutions** — use the map below. The checklist at the
   bottom of this file is the gate before saving.
5. **Write** the `.mdx` file to the output path.
6. **Confirm preview URL**: `http://localhost:3000/plans/<slug>`
   (WSL2 Windows browser: `http://10.255.255.254:3000/plans/<slug>`)
   The dev server is persistent — no need to start it.

### What the planview renderer supports (current state)

| Feature | Supported? | Notes |
|---------|------------|-------|
| ` ```mermaid ` fenced code blocks | **Yes** | Rendered as SVG via `MermaidBlock`. Click to open a full-screen lightbox with scroll-to-zoom and drag-to-pan. Keep as-is — do NOT convert. |
| ` ```lang ` fenced code blocks (non-mermaid) | **Yes** | Rendered with **syntax highlighting** (`rehype-highlight`, `github-dark-dimmed` theme). Always include a language specifier — ` ```typescript `, ` ```yaml `, ` ```bash `, etc. |
| `- [ ]` / `- [x]` GFM task lists | **Yes** | Rendered as checkboxes via remark-gfm. Keep as-is. |
| MDX components (`<Tabs>`, etc.) | **Yes** | See component catalogue below. |
| Markdown tables | **Yes (styled)** | Tables have full prose styling (header bg, row borders). Prefer components for semantic structure, but narrow reference tables are fine as markdown. |
| `> blockquote` | Partial | Rendered but unstyled. Replace with `<Callout>` for callouts. |

---

## When to use markdown tables vs components

Markdown tables are now fully styled — header background, row borders, responsive text. Use them for narrow reference tables (2–4 columns, short cell text). Prefer components for semantic structure:

| Use this | Instead of |
|---|---|
| `<ModelGuide />` | Any model/tier comparison table |
| `<DelegationGuide />` | Any delegation/ownership table |
| `<Tabs>` | Sections that share the same shape / mutually exclusive options |
| `<Steps>` | Ordered process tables (Step 1 / Step 2 ...) |
| Markdown table | Short lookup tables, quick reference grids with 2–4 columns |

**Rule**: if a table has 5+ columns, or any cell contains more than one sentence, replace it with a component.

---

## Component catalogue

All components are registered in planview's MDX component map. Import nothing —
they are globally available in every `.mdx` file served by planview.

### `<Callout>`

Renders a colored alert box with an optional title. Use for warnings, tips,
decisions, important facts, or anything that must not be missed.

**Props**
- `type`: `"info"` (blue) · `"warning"` (amber) · `"tip"` (green) · `"danger"` (red)
- `title` (optional): short heading above the body

**When to use**
- Inline warnings, tips, or constraints within prose
- Replaces `> blockquote` used as an aside
- Replaces bold-only emphasis boxes that need color coding
- Never use for multi-paragraph documents — keep Callout content tight

**Example**
```mdx
<Callout type="warning" title="Start cheap">
  Use the smallest model and lowest reasoning that can solve the task.
  Escalation is a deliberate engineering decision — not a default.
</Callout>
```

---

### `<Tabs>` + `<Tab>`

Renders a tabbed panel. Each `<Tab>` is one panel; the `labels` array on
`<Tabs>` names them.

**Props on `<Tabs>`**
- `labels`: string array — one entry per `<Tab>` child, matched by position

**When to use**
- Multiple mutually exclusive modes, options, or categories side by side
- Replaces markdown tables where each row is a distinct "type" or "mode"
- Replaces a sequence of `###` sections that share the same schema
- Can contain any MDX content: prose, Callout, Steps, Badge, CodeBlock

**Example**
```mdx
<Tabs labels={["Ask mode", "Agent Mode", "Plan mode"]}>
  <Tab>
    <Badge color="green">Cheapest</Badge>
    Quick explanations · Q&A · concepts. No file edits.
  </Tab>
  <Tab>
    <Badge color="blue">Most powerful</Badge>
    Multi-step work: planning · tool use · codebase exploration.
  </Tab>
  <Tab>
    <Badge color="purple">Safest for risky work</Badge>
    Review the plan before allowing any edits.
  </Tab>
</Tabs>
```

---

### `<Steps>` + `<Step>`

Renders a numbered vertical list with blue circle step indicators.

**Props on `<Step>`**
- `n`: number — the step number shown in the circle

**When to use**
- Ordered procedures, checklists, workflows, lifecycles
- Replaces `1. 2. 3.` ordered lists when the sequence is the main point
- Replaces a table whose first column is "Step 1 / Step 2 / Step 3"
- Keep each step to one short sentence or two bullet points max

**Example**
```mdx
<Steps>
  <Step n={1}>Write an outcome-first prompt — goal · context · constraints</Step>
  <Step n={2}>Choose the smallest suitable model</Step>
  <Step n={3}>Review the plan before allowing edits</Step>
</Steps>
```

---

### `<Badge>`

Renders a small inline color pill. Use inline within prose or inside Tab/Step
content to visually tag a status, tier, or label.

**Props**
- `color`: `"blue"` · `"green"` · `"amber"` · `"red"` · `"gray"` · `"purple"`

**When to use**
- Status tags in tab headers or step descriptions
- Model tier labels (green = mini, blue = coding, purple = strong, red = human-led)
- Cost or risk tier indicators (green = cheap, red = expensive/risky)
- Never use Badge as a standalone paragraph — always inside a sentence or component

**Example**
```mdx
<Badge color="green">Mini model · low reasoning · Ask mode</Badge>
```

---

### `<CodeBlock>`

Renders a dark code editor box with a copy button and optional language tag.

**Props**
- `language` (optional): syntax language hint shown in the header bar (e.g. `"markdown"`, `"bash"`, `"typescript"`)
- `children`: the code string — write it as the direct text child

**When to use**
- Prompt templates, fill-in-the-blank command patterns, or structured configuration examples where a copy button matters
- Use **fenced code blocks** (` ```bash `, ` ```typescript `) for real code that benefits from syntax highlighting
- Use `<CodeBlock>` for prose-like templates that aren't valid code (e.g. fill-in prompts, goal/context/constraint templates)
- Use inline `` `code` `` for short snippets in prose

**Important**: MDX passes children as `ReactNode`. The component handles this —
do not try to call `.trim()` or string methods on `children` in the component source.

**Example**
```mdx
<CodeBlock language="markdown">
Goal:
  - What should be achieved?

Context:
  - Relevant files, errors, tests
</CodeBlock>
```

---

### `<ModelGuide />`

Renders 5 tier cards: each shows the task type, model badge, reasoning level,
recommended mode, and escalation hint. Self-contained — takes no props.

**When to use**
- Anywhere a document explains "which model / reasoning level for which task"
- Replaces any 3-column table with columns like "Task type · Model · When to escalate"
- Use exactly once per document — it covers the full tier ladder

---

### `<DelegationGuide />`

Renders 3 color-coded cards: green (safe to delegate with review), amber (use as
assistant/adviser), red (human-led, AI-assisted only). Each card shows example
tasks as chips and the human role. Self-contained — takes no props.

**When to use**
- Anywhere a document explains delegation levels or task ownership
- Replaces any table with columns like "Delegation level · Examples · Human role"
- Use exactly once per document

---

## Markdown → component substitution map

When converting a markdown document to planview MDX, apply these substitutions:

| Markdown pattern | Replace with |
|---|---|
| Model/tier comparison table | `<ModelGuide />` |
| Delegation/ownership table | `<DelegationGuide />` |
| Table with 5+ columns or multi-sentence cells | `<Tabs>` or `<Steps>` (pick closest fit) |
| Narrow reference table (≤4 cols, short cells) | Keep as markdown table — it's styled |
| `> blockquote` used as aside/warning | `<Callout type="info/warning/tip/danger">` |
| `1. 2. 3.` ordered list for a process | `<Steps>` |
| `###` sub-sections that all share the same shape | `<Tabs>` |
| Bold status label in prose | `<Badge color="...">` |
| Fenced block without a language specifier | Add the correct language tag for syntax highlighting |
| Prompt/command template needing a copy button | `<CodeBlock language="...">` |

---

## Authoring checklist before saving an MDX file

- [ ] Frontmatter `title:` present at the top of the file
- [ ] Output path is `<PLANS_DIR>/<slug>.mdx` (flat file, not a subfolder)
- [ ] All fenced code blocks have a language specifier (` ```typescript `, ` ```yaml `, etc.) so Shiki highlights them
- [ ] No fenced block uses ` ```text ` or ` ``` ` (no lang) unless the content truly has no language
- [ ] Markdown tables used only for narrow reference grids (≤4 columns, short cell text) — wide/semantic tables replaced with components
- [ ] No `> blockquote` used as a callout — replaced with `<Callout>`
- [ ] Every `<Tabs>` has `labels={[...]}` with the same count as child `<Tab>` elements
- [ ] Every `<Step>` has an `n={number}` prop
- [ ] `<Badge>` is inside a sentence or component — never a standalone line
- [ ] `<ModelGuide />` and `<DelegationGuide />` used where applicable (not tables)
- [ ] `<CodeBlock>` used for prompt/command templates that benefit from a copy button; fenced blocks used for real code
- [ ] No HTML tables (`<table>`, `<tr>`, `<td>`) — replace the same way as wide markdown tables
- [ ] Mermaid fenced code blocks kept as-is (NOT converted — the renderer handles them)
- [ ] GFM task lists (`- [ ]`) kept as-is (NOT converted — remark-gfm handles them)
- [ ] Tabs with many labels (5+) still work — the tab bar wraps automatically
- [ ] Preview URL reported to user: `http://localhost:3000/plans/<slug>`

---

## Planview setup reference

**Install / start**: `npx @vtripathi/planview@latest --dir <path-to-docs-dir>`
- The alias `planview` maps to `npx @vtripathi/planview@latest --dir` — so: `planview ./my-docs`
- `@latest` ensures the newest version is always used automatically — no manual updates needed
- Access: `http://localhost:3000` (WSL2 Windows browser: `http://10.255.255.254:3000`)
- The server runs until Ctrl+C — no need to restart between doc edits (pages are server-rendered on demand)

**PLANS_DIR** is the `--dir` argument passed at launch. For the pipeline project:
`/home/vtripathi/git_clones/ai_first/air.ai-assisted-delivery-pipeline/pipeline/`

Component source (inside the npm package, for reference only):
- `Callout.tsx` · `Tabs.tsx` · `Steps.tsx` · `Badge.tsx` · `CodeBlock.tsx`
- `ModelGuide.tsx` · `DelegationGuide.tsx`
- `MermaidBlock.tsx` — renders ` ```mermaid ` fenced blocks as SVG
- `PreBlock.tsx` — `pre` override; routes mermaid to `MermaidBlock`, all other fenced blocks styled with `github-dark-dimmed` theme (highlighted at compile time via `rehype-highlight`)
- `TableOfContents.tsx` — auto-generated sticky ToC from `h2`/`h3` headings (xl breakpoint+)
- `ReadingProgress.tsx` — thin blue progress bar at top of doc pages

Stack: Next.js 16 · React 19 · `@mdx-js/mdx` · `rehype-highlight` · Tailwind CSS + @tailwindcss/typography · mermaid · remark-gfm

### File naming & routing
- A file at `<PLANS_DIR>/foo.mdx` is served at `http://localhost:3000/plans/foo`
- The slug is the filename without `.mdx`
- Only `.mdx` files are picked up (not `.md`)
