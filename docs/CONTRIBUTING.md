---
layout: default
title: Contributing
nav_order: 6
permalink: /contributing/
---

# Contributing to Microsoft Cloud Advisor

## How to Extend the Skill

### Adding New Decision Trees

Add new decision trees in the `## Decision Trees` section of `SKILL.md`:

```markdown
### [Your Topic] Selection
```
What's the primary requirement?
├── Option A → Recommendation A
├── Option B → Recommendation B
└── Option C → Recommendation C
```
```

### Adding New MCP Tools

When new MCP tools become available, add them to the `### Step 3` tools table:

```markdown
| `mcp_azure_mcp_newtool` | Description of use case |
```

### Adding Slide Templates

Add new slide templates in `## Step 8` following the pattern:

```markdown
#### [Template Name] Slide
```markdown
---
# Title

Content structure...

> Speaker notes: Context for presenter

---
```
```

## Guidelines

- Keep procedures actionable and tool-specific
- Always reference the exact MCP tool names
- Test changes by triggering the skill in VS Code
- Update the README if you add major capabilities

## Submitting Changes

1. Fork the repository
2. Create a feature branch
3. Make your changes to `skills/microsoft-cloud-advisor/SKILL.md`
4. Test by copying to `~/.copilot/skills/microsoft-cloud-advisor/`
5. Submit a pull request with a description of what you added
