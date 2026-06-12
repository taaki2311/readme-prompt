# Worked example: improving a thin README

This walks through **Modify mode** on a small sample project, showing how the
prompt's rules play out in practice: ground every claim in real files, preserve
the author's voice, fix mismatches against the code, and leave a visible `TODO`
for a fact that can't be verified.

---

## The sample project

Assume the agent is pointed at a small CLI tool with this layout:

```text
slugify-cli/
├── package.json
├── src/
│   └── index.js
└── README.md
```

**`package.json`** (the evidence the agent reads):

```json
{
  "name": "slugify-cli",
  "version": "1.2.0",
  "description": "Turn text into URL-safe slugs from the command line.",
  "bin": { "slugify": "src/index.js" },
  "scripts": {
    "test": "node --test",
    "lint": "eslint src"
  },
  "engines": { "node": ">=18" }
}
```

Notes from investigation:
- The published binary is `slugify` (from `bin`), installed via npm.
- Real scripts are `npm test` and `npm run lint`. There is **no** `build` script.
- There is **no `LICENSE` file** and **no `license` field** in `package.json`.

---

## Before — the existing README

```markdown
# slugify-cli

A tool for slugs.

## Install

npm install

## Usage

Run `npm start` to slugify your text.

## Build

npm run build
```

What's wrong with it (found by reconciling against the code):
- Vague description; doesn't say what a "slug" is or who it's for.
- Install command is wrong for a CLI — it's a globally-installed binary, not `npm install` in the project.
- `npm start` **does not exist** in `scripts`; the real entry point is the `slugify` binary.
- The **Build** section documents `npm run build`, which **does not exist**.
- No language tags on code blocks; no license.

---

## After — the improved README

````markdown
# slugify-cli

Turn text into URL-safe slugs from the command line.

`slugify-cli` is a small Node.js CLI that converts arbitrary text into clean,
URL-safe slugs — handy for generating page paths, filenames, and identifiers.

## Prerequisites

- Node.js >= 18

## Installation

```bash
npm install -g slugify-cli
```

## Usage

```bash
slugify "Hello, World!"
# hello-world
```

## Development

```bash
npm run lint    # lint the source
npm test        # run the test suite
```

## License

<!-- TODO: confirm the license — no LICENSE file or license metadata was found -->
````

---

## Why each change is safe

- **Description** comes verbatim from `package.json` `description`, expanded with a
  factual sentence about what the tool is — no invented features.
- **Prerequisites** uses the real `engines.node` value (`>=18`).
- **Installation** reflects how a `bin` package is actually used: a global install
  exposing the `slugify` command. The old `npm install` (project-local) was wrong.
- **Usage** invokes the real `slugify` binary instead of the nonexistent `npm start`.
- **Development** lists only the scripts that exist (`lint`, `test`). The bogus
  **Build** section was removed because `npm run build` is not defined — and the
  final summary would note that removal.
- **License** can't be determined, so the agent leaves a visible `TODO` rather than
  guessing a license — exactly the "ask, then fall back" policy.

## What the agent reports back

> **Mode:** Modify.
> **Changed:** rewrote the description and usage to use the real `slugify` binary;
> corrected the install command to a global install; added Prerequisites from
> `engines`.
> **Removed:** the *Build* section — `npm run build` is not defined in
> `package.json`.
> **TODO:** no license found — left a placeholder; please confirm the intended license.
