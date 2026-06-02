---
name: notion-formatter
description: Format and improve Notion documents to be visually appealing, scannable, and human-friendly. Uses color-coded sections, callouts, capability tables, toggle headings, and conversational tone. Outputs Notion Enhanced Markdown.
---

# Notion Document Formatter

Format and improve Notion documents so they are visually appealing, scannable, and written for humans. Output uses Notion Enhanced Markdown (the format used by the Notion MCP integration).

## When to use

Use this skill when the user asks to:
- Format or improve a Notion document
- Write content for a Notion page
- Restructure a document to be more readable
- Make a document "look good" or "read well" in Notion

## Workflow

1. If the user provides a Notion URL, fetch it with `notion-fetch` to see the current content.
2. Analyze the content: what are the major sections, what is context vs. detail, what are the key items.
3. Apply the formatting rules below.
4. Write the result back to Notion with `notion-update-page`, or output the enhanced markdown if the user wants to paste it themselves.

---

## Document structure

### Page setup
- Pick a relevant emoji for the page icon.
- Start with `<table_of_contents/>` so readers can jump around.
- If the document has a natural "here's where things stand today" section, put it first.

### Section hierarchy and color coding
Assign each major section (h2) a distinct color and keep it consistent through all its subsections (h3, h4). This gives readers a visual anchor so they always know which part of the document they're in.

Good color assignments:
- **Blue** for context, current state, background
- **Purple** for analysis, gaps, requirements
- **Green** for integrations, external systems, technical
- **Orange** for timelines, milestones, roadmap
- **Red** for risks, blockers, critical items (use sparingly)

Example:
```
## Part A: Requirements {color="purple"}
### A1. Authentication {color="purple"}
### A2. Data Model {color="purple"}

## Part B: Integrations {color="green"}
### B1. External APIs {color="green"}
```

Use `---` dividers between major parts (h2 sections), not between subsections.

### Numbered subsections
Number subsections within a part (A1, A2, B1, B2) so people can reference them in conversation: "let's talk about A3."

---

## Block patterns

### Callouts — use them to set context, not to decorate
Each callout type has a specific purpose. Do not mix them up or overuse them.

| Purpose | Icon | Color | When to use |
|---------|------|-------|-------------|
| Current status | 📍 | `gray_bg` | Before a subsection's details, explain where things stand today |
| Scope note | ℹ️ | `blue_bg` | Clarify what is and isn't covered in a section |
| Critical gap | 🚨 | `red_bg` | Something important is completely missing or broken |
| Open question | ❓ | `yellow_bg` | Decisions that haven't been made yet |
| Technical explainer | 🔌 | `green_bg` | Explain how a system or integration works |

Example:
```
<callout icon="📍" color="gray_bg">
	**Where we are:** the portal can create service requests but customers can't edit them after submission.
</callout>
```

**Rules:**
- Always bold the lead phrase inside a callout ("Where we are:", "Scope note:", "What is X?").
- One callout per subsection max for status context. Don't stack callouts.
- A callout should be 1-3 sentences. If you need more, use a toggle instead.

### Capability tables — the workhorse for listing items
When a section lists capabilities, features, or requirements, use a two-column table.

```
<table fit-page-width="true" header-row="true">
<colgroup>
<col color="gray_bg">
<col width="520">
</colgroup>
<tr>
<td>Capability</td>
<td>What's needed</td>
</tr>
<tr>
<td>**Feature name**</td>
<td>Plain language description of what this means</td>
</tr>
</table>
```

**Rules:**
- Always `fit-page-width="true"` and `header-row="true"`.
- First column gets `color="gray_bg"` in the colgroup. This makes labels stand out.
- Bold the label/name in the first column.
- Second column is plain text, no bold. Write like you're explaining to someone who just joined the project.
- Keep second column under ~120 words per cell.
- For tables that belong to a specific color-coded section, you can color the header row to match: `<tr color="green_bg">`.

### Toggle headings — hide detail, show structure
Use toggle headings (h2 or h3 with `{toggle="true"}`) for explanations that are useful but not essential for a first read. Good candidates:
- "How it works" details
- "Who does what" role breakdowns
- Lifecycle or process descriptions
- Background context that only some readers need

```
## How it works {toggle="true"}
	- Step one explanation
	- Step two explanation
```

**Rules:**
- Children must be indented (tab) under the toggle heading.
- Don't nest toggles inside toggles. Keep it one level.
- The toggle title should make sense on its own, so someone can decide whether to expand it.

### Bullet lists with bold leads
For lists that aren't tabular but need structure:
```
- **Service requests** - customers can create, view, search, and filter them.
- **Work orders** - read-only view of the actual service jobs.
```

**Rules:**
- Bold the noun/concept, then dash, then plain explanation.
- Keep each bullet to one sentence if possible, two max.

### Flow diagrams in code blocks
For process flows or lifecycles, use a code block with ASCII art:
```javascript
Customer submits request (Open)
  → Team reviews (In Progress)
    → Team responds (Resolved)
      ↓
    Customer reviews and either:
    ├→ Accepts (Closed)
    └→ Reopens → Team reviews again
```

Use `javascript` as the language tag for basic syntax highlighting of arrows and symbols.

---

## Tone and writing rules

These rules are non-negotiable. They define how the document reads.

1. **No em-dashes.** Use commas, periods, or parentheses instead. Em-dashes scream "AI wrote this."
2. **No filler phrases.** Cut "it's important to note that", "it's worth mentioning", "as previously discussed."
3. **Explain jargon on first use.** If a term isn't obvious, add a short parenthetical: "work orders (the actual service job assigned to a technician)."
4. **Write short paragraphs.** 2-3 sentences max before a break, list, or table.
5. **Use "you" and "we" naturally.** "Customers can view their sites" not "The system provides site visibility to end users."
6. **Lead with what matters.** Don't build up to the point. State it, then explain.
7. **Be specific over vague.** "Download as CSV or PDF" not "export in various formats."

---

## Notion Enhanced Markdown Reference

This is the syntax to use when writing content for Notion pages.

### Text formatting
- Bold: `**text**`
- Italic: `*text*`
- Inline code: `` `code` ``
- Link: `[text](URL)`
- Colored text: `<span color="blue">text</span>`

### Block colors
Add `{color="Color"}` after any block's text:
```
## Heading {color="purple"}
- List item {color="blue"}
> Quote {color="green"}
```

Available colors:
- Text: gray, brown, orange, yellow, green, blue, purple, pink, red
- Background: gray_bg, brown_bg, orange_bg, yellow_bg, green_bg, blue_bg, purple_bg, pink_bg, red_bg

### Callouts
```
<callout icon="emoji" color="Color">
	Content here (indented with tab)
</callout>
```

### Toggle headings
```
## Title {toggle="true"}
	Indented children (hidden by default)
```

### Tables
```
<table fit-page-width="true" header-row="true">
<colgroup>
<col color="gray_bg">
<col width="520">
</colgroup>
<tr>
<td>Header 1</td>
<td>Header 2</td>
</tr>
<tr>
<td>**Bold label**</td>
<td>Description text</td>
</tr>
</table>
```

### Other blocks
- Divider: `---`
- Table of contents: `<table_of_contents/>`
- Empty line: `<empty-block/>`
- Code block: ` ```language ... ``` `
- Quote: `> text`
- To-do: `- [ ] text` / `- [x] text`
- Columns: `<columns><column>...</column><column>...</column></columns>`
- Image: `![Caption](URL)`

### Mentions
- Page: `<mention-page url="URL">Title</mention-page>`
- User: `<mention-user url="URL">Name</mention-user>`
- Date: `<mention-date start="YYYY-MM-DD"/>`
