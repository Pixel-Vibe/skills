# Mekari Agent Skills

A collection of agent skills for building Mekari product UIs with the Pixel 3 design system in Vue/Nuxt. These skills are loaded by AI coding agents (GitHub Copilot, Claude, etc.) to automate the PRD-to-code and Figma-to-code workflow across all Mekari products.

## Skills

| Skill                                                 | Trigger                                              | Role                                                         |
| ----------------------------------------------------- | ---------------------------------------------------- | ------------------------------------------------------------ |
| [`implement-to-pixel`](./implement-to-pixel/SKILL.md) | Confluence PRD URL, Figma URL, `/implement-to-pixel` | Orchestrator — routes PRD or Figma input to code generation  |
| [`mekari-taste`](./mekari-taste/SKILL.md)             | Loaded by `implement-to-pixel` (PRD path only)       | Imagination phase — reads PRD, draws wireframes, drafts plan |
| [`pixel`](./pixel/SKILL.md)                           | Component/token questions, any UI build task         | Pixel 3 component API — validates props, resolves tokens     |

## How it works

```
User input
    │
    ▼
implement-to-pixel
    ├── Confluence / PRD text ──► mekari-taste (wireframes + plan)
    │                                   │
    │                            User confirms
    │                                   │
    └── Figma URL ─────────────────────►│
                                        ▼
                              Vue/Nuxt code output
                              (Pixel 3, token 2.4)
```

**Branch A — PRD path:** `implement-to-pixel` loads `mekari-taste` to read the PRD, imagine the screen, draw low-fidelity wireframes, and draft an implementation plan. The user reviews and confirms before code is generated.

**Branch B — Figma path:** `implement-to-pixel` fetches the Figma file directly, skips the imagination phase, and generates Vue/Nuxt code from the design with Mekari taste conventions applied.

Both branches use `pixel` to resolve component props and token names — never guessing Pixel 3 API.

## Installation

From your project root, run:

```bash
npx skills add https://github.com/pixel-vibe/skills --skill pixel
```

Read more at [skills.sh](https://www.skills.sh/pixel-vibe/skills)

or,

Just clone or download the repo and copy the skills to your project.

## Usage

Once installed, skills are auto-discovered by your agent. Invoke directly with a slash command or attach a URL:

```
/implement-to-pixel https://yourcompany.atlassian.net/wiki/spaces/EXP/pages/123456
/implement-to-pixel https://www.figma.com/design/abc123/Mekari-Expense?node-id=1:2
```

Or let the agent pick it up automatically when you paste a Confluence or Figma URL in chat.

## Token mode

All skills default to **Pixel 2.4** tokens. Switch to 2.1 only if the user or the Figma file explicitly targets a legacy project.

## Products supported

1. Expense
2. Talenta
3. Qontak
4. Jurnal
5. Flex
6. KlikPajak
7. Sign
8. Officeless

## Requirements

- Copilot, Codex or Claude with agent skill support
- Access to Mekari's Pixel MCP server for component resolution
- Access to Figma MCP server for Branch B (Figma path)
