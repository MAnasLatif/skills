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

### `tailwind-v4`

A skill for building scalable design systems with **Tailwind CSS v4**, using
CSS-first configuration (`@theme`), design tokens, component variants, responsive
patterns, and accessibility.

Use this skill for component libraries, design tokens and theming, dark mode with
native CSS features, responsive and accessible components, and migrating from
Tailwind v3 to v4. It covers OKLCH color tokens, native CSS animations, custom
`@utility` directives, container queries, and the v3→v4 migration checklist.

**To add this skill:**

```bash
npx skills add MAnasLatif/skills --skills tailwind-v4
```

**What's inside:**

- `SKILL.md` — key v4 changes, CSS-first quick start, design token hierarchy, and
  component architecture.
- `references/` — deep-dive references: `advanced-patterns` (native CSS
  animations, dark mode, custom `@utility`, theme modifiers, namespace overrides,
  semi-transparent color variants, container queries, migration checklist, best
  practices) and `tailwind-reference` (official v4 core concepts: directives,
  functions, theme namespaces, colors, dark mode, responsive design, container
  queries, custom styles).
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
