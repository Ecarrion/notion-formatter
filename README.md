# Notion Document Formatter

A formatting skill that makes Notion documents visually appealing and easy to scan.

## What it does

Takes rough content and applies a consistent formatting system:
- Color-coded section hierarchy (blue for context, purple for gaps, green for integrations)
- Status callouts with specific purposes (where we are, scope notes, critical gaps, open questions)
- Two-column capability tables with gray label columns
- Toggle headings for collapsible detail
- Conversational tone, no AI-sounding language

## Install

### Claude Code

Add this repo as a plugin marketplace, then install the plugin:

```bash
claude /plugin marketplace add github:Ecarrion/notion-formatter
claude /plugin install notion-formatter
```

### Cursor

Copy the skill into your project:

```bash
mkdir -p .cursor/skills/notion-formatter
cp skills/notion-formatter/SKILL.md .cursor/skills/notion-formatter/SKILL.md
```

Or add as a remote rule in Settings > Rules > Add Remote Rule (GitHub) with this repo's URL.

### Codex

Use the built-in `$skill-installer` to install directly from GitHub:

```
$skill-installer install https://github.com/Ecarrion/notion-formatter/tree/main/skills/notion-formatter
```

Or copy manually into your project or home directory:

```bash
# Project-level (auto-discovered from .agents/skills/)
mkdir -p .agents/skills/notion-formatter
cp skills/notion-formatter/SKILL.md .agents/skills/notion-formatter/SKILL.md

# Or user-level (all projects)
mkdir -p ~/.codex/skills/notion-formatter
cp skills/notion-formatter/SKILL.md ~/.codex/skills/notion-formatter/SKILL.md
```

## Usage

Once installed, just ask:

- "Format this for Notion" (with content pasted or a URL)
- "Improve this Notion page: [URL]"
- "Write a Notion doc about X"
- "Restructure this document to be more scannable"

With Claude Code + Notion MCP connected, the skill can read and write Notion pages directly.
