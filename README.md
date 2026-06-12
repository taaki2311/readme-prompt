# README Authoring Prompt

A portable, vendor-neutral prompt that lets **any AI agent** create a new
`README.md` — or improve an existing one — for **any project**, in any language or
stack.

The prompt's whole design goal is accuracy: AI-written READMEs tend to invent
install commands, badges, and features that don't exist. This one forces an
**evidence-first** workflow (read the project, then write) and a strict
**no-fabrication** policy, with visible `TODO` placeholders for anything that can't
be verified.

## Contents

| File | Purpose |
| --- | --- |
| [`PROMPT.md`](./PROMPT.md) | The portable prompt — the main artifact. Give this to your agent. |
| [`templates/README.template.md`](./templates/README.template.md) | An annotated README skeleton the agent uses as a scaffold. |
| [`examples/before-after.md`](./examples/before-after.md) | A worked thin→good example showing the rules in action. |

## How to use it

1. Open your AI agent (Claude, ChatGPT, Gemini, a local model, an IDE assistant —
   any of them).
2. Give it the contents of [`PROMPT.md`](./PROMPT.md): paste the text, attach the
   file, or reference its path if the agent can read your filesystem.
3. Point the agent at the target project (open the repo, set the working directory,
   or share the files).
4. Optionally add context — audience, tone, required sections (see "Inputs you
   may receive" in `PROMPT.md`).
5. Let it run.

## What it does

- **Create mode** — no README found: builds one from scratch.
- **Modify mode** — README found: improves it in place, preserving the author's
  voice.

In both modes it documents only what it can verify in the project, and it leaves
clearly-marked `TODO` placeholders (or asks you) when a critical fact can't be
determined.
