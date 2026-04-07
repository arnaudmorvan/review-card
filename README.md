# review-card

A skill that audits Figma Card components and generates a structured UI audit with scoring, before/after comparison, and squint test — all directly on the Figma canvas via the MCP server.

Inspired by Adham Dannaway's [16 UI Design Tips](https://www.adhamdannaway.com/blog/ui-design/ui-design-tips) (Practical UI).

## What It Does

You provide a Figma link to a Card component. The skill:

1. **Captures** the card (screenshot + design properties)
2. **Analyzes** 35+ criteria across 16 categories (spacing, contrast, typography, shadows, etc.)
3. **Scores** the card on a 10-point scale with weighted categories
4. **Generates an audit table** directly in Figma next to the card
5. **Creates a corrected clone** (Before / After) with all fixes applied
6. **Runs a Squint Test** — blurred versions to validate visual hierarchy
7. **Screenshots** everything for review

## Prerequisites

- A Figma account with a personal access token
- An AI IDE with agent capabilities and MCP support (VS Code + GitHub Copilot, Cursor, Windsurf, Claude Code, etc.)
- The Figma MCP server connected to your AI agent

## Installation

### GitHub Copilot (VS Code)

1. Clone the repo into your skills directory:

```bash
cd ~/.copilot/skills
git clone https://github.com/arnaudmorvan/review-card.git review-card
```

2. The skill is available immediately — no restart needed.

### Claude Desktop

1. **Find your skills directory.** Open the Claude Desktop config file:

```bash
open ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

Look for the path pointing to your `skills/user` folder — that's where you'll clone the repo.

2. **Clone the repo:**

```bash
cd /path/to/your/skills/user
git clone https://github.com/arnaudmorvan/review-card.git review-card
```

3. **Make sure the Figma MCP server is connected.** In `claude_desktop_config.json`, verify you have:

```json
{
  "mcpServers": {
    "figma": {
      "command": "npx",
      "args": ["-y", "@anthropic-ai/figma-mcp-server@latest"],
      "env": {
        "FIGMA_ACCESS_TOKEN": "YOUR_FIGMA_ACCESS_TOKEN"
      }
    }
  }
}
```

> **Figma token:** Go to Figma → Settings → Security → Personal access tokens → Generate new token.

4. **Restart Claude Desktop.** Skills are loaded at startup.

### Claude Code (terminal)

Add to `~/.claude.json`:

```json
{
  "mcpServers": {
    "figma": {
      "url": "https://mcp.figma.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_FIGMA_ACCESS_TOKEN"
      }
    }
  }
}
```

## Connecting the Figma MCP Server (VS Code)

Add to your `.vscode/mcp.json`:

```json
{
  "servers": {
    "figma": {
      "type": "sse",
      "url": "https://mcp.figma.com/sse",
      "headers": {
        "Authorization": "Bearer YOUR_FIGMA_ACCESS_TOKEN"
      }
    }
  }
}
```

## How to Use

Provide a Figma link to a Card component:

- `"Audit this card: https://figma.com/design/xxx?node-id=1:2"`
- `"Review the UI of this card <figma-link>"`
- `"Run a UI audit on this Figma card"`

## Structure

```
SKILL.md                          # Skill entry point (7-step workflow)
references/
  checklist.md                    # 35+ audit criteria, 16 categories
  output-format.md                # Audit table specs (columns, colors, layout)
```

## MCP Tools Used

- `mcp_figma_get_screenshot` — Capture the card visually
- `mcp_figma_get_design_context` — Extract design properties (colors, fonts, spacing)
- `mcp_figma_use_figma` — Generate audit table, clone + fix card, squint test

## License

[MIT](LICENSE)
