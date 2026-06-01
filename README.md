# MAnasLatif Skills Library

Welcome to the **MAnasLatif** AI Agents skills library. This repository contains a
collection of skills and tools designed to extend the capabilities of AI assistants
(GitHub Copilot, Claude Code, Cursor, Windsurf, Cline, Aider, Codex, Gemini, and
more) when working inside MAnasLatif / MAL Devs projects.

## How to Use

You can easily add any of these skills to your projects using the `skills` CLI,
similar to `vercel-labs/skills`.

To add a specific skill to your current workspace, run:

```bash
npx skills add MAnasLatif/skills --skills <skill-name>
```

## Available Skills

Currently, the following skills are available in this library:

### `mal-ui`

A skill for building React / Next.js user interfaces with **mal-ui**, the MAL Devs
component library. Every component, hook, and type is available via `mal-ui/*`
subpath imports, pre-wired to the MAL brand theme.

Use this skill for components, layouts, forms with validation, charts, date pickers,
modals, notifications, spotlight search, rich-text editing, dropzone uploads,
carousels, navigation progress, schedules, and brand theming. It ensures correct
subpath imports, provider setup (`MALUIProvider` + `malTheme`), peer dependencies,
and brand colors.

**To add this skill:**

```bash
npx skills add MAnasLatif/skills --skills mal-ui
```

**What's inside:**

- `SKILL.md` — golden rules, setup, subpath map, branded aliases, common patterns.
- `references/` — deep-dive references for every subpath:
  `setup`, `theme`, `core`, `hooks`, `form`, `charts`, `dates`, `notifications`,
  `modals`, `spotlight`, `code-highlight`, `tiptap`, `dropzone`, `carousel`,
  `nprogress`, `schedule`, and `component-api`.
- `agents/` — per-agent configuration files (Copilot, Claude Code, Cursor,
  Windsurf, Cline, Aider, Codex, Gemini, OpenAI, Roo Code, and a generic profile).

## Adding New Skills

As this library grows, more skills will be added. To add a new skill:

1. Create a new directory under the `skills/` folder with your skill name.
2. Add a `SKILL.md` (and any other necessary agent instructions) inside that
   directory.
3. Update this `README.md` to list the new skill under "Available Skills".

---

_Maintained by MAnasLatif._
