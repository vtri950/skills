# pstack — pointer, not a vendoring

The real pstack plugin already lives in this workspace at `../plugins/pstack`
(full skills with `references/`, `scripts/`, and `poteto-mode` playbooks:
`how`, `why`, `teach`, `recall`, `architect`, `technical-writing`, `unslop`,
`swarm`, `create-verification-skill`, `maintain-verification-skill`,
`poteto-mode`, plus `principle-*` skills, agents, and automations).

Do NOT vendor copies of those skills here — they would rot. Install or
reference them from the source of truth:

```bash
# example: pick single skills into an assistant
cp -r ../plugins/pstack/skills/how ~/.your-assistant/skills/
```

What stays in this directory:

- `SDLC-GIST.md` — distilled takeaway notes from lauren's
  ([@poteto](https://x.com/poteto))
  [Pt.1](https://x.com/poteto/article/2094457600259842065) +
  [Pt.2](https://x.com/poteto/status/2097732320606507506) posts
  (judgments + copy-paste prompts). Notes, not skills — no overlap
  with the plugin.
