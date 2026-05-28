---
description: Create new Windsurf workflow skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new workflow.
---

# Writing Skills (Workflows)

## Process

1. **Gather requirements** — ask user about:
   - What task/domain does the skill cover?
   - What specific use cases should it handle?
   - Does it need executable scripts or just instructions?
   - Any reference materials to include?

2. **Draft the skill** — create:
   - Main workflow `.md` in `.windsurf/workflows/`
   - Additional reference files in `.windsurf/references/` if content exceeds 500 lines

3. **Review with user** — present draft and ask:
   - Does this cover your use cases?
   - Anything missing or unclear?
   - Should any section be more/less detailed?

## Workflow Structure

Windsurf workflows live in `.windsurf/workflows/` as `.md` files with YAML frontmatter:

```md
---
description: Brief description of capability. Use when [specific triggers].
---

# Skill Name

## Quick start

[Minimal working example]

## Workflows

[Step-by-step processes with checklists for complex tasks]
```

Supporting reference files go in `.windsurf/references/` and are referenced from the main workflow file.

## Description Requirements

The description is **the only thing the agent sees** when deciding which workflow to load. It's surfaced in the system prompt alongside all other installed workflows.

**Format**:

- Keep it concise
- First sentence: what it does
- Second sentence: "Use when [specific triggers]"

## When to Split Files

Split into separate reference files when:

- Main workflow exceeds 100 lines
- Content has distinct domains
- Advanced features are rarely needed

## Review Checklist

After drafting, verify:

- [ ] Description includes triggers ("Use when...")
- [ ] Main workflow is focused and concise
- [ ] No time-sensitive info
- [ ] Consistent terminology
- [ ] Concrete examples included
