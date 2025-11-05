# Code Patterns and Reusable Solutions

This file documents reusable patterns discovered during development.

## Pattern: YAML Frontmatter Structure

**Problem**: Need consistent metadata across all markdown files for parsing and querying

**Solution**: Standardized YAML frontmatter with required and optional fields
```yaml
---
# For project files
name: Project Name
type: project
project_id: unique-identifier
status: active|paused|completed
started: YYYY-MM-DD
last_updated: YYYY-MM-DD
tags: [tag1, tag2, tag3]
priority: high|medium|low
---

# For summary files
---
last_updated: YYYY-MM-DD HH:MM
session_count: N
ai_client: claude|gemini
---
```

**Usage**: Copy appropriate template from templates/ directory when creating new files

---

## Pattern: Wiki-Style Linking

**Problem**: Need to cross-reference projects and concepts without hardcoded paths

**Solution**: Use Obsidian-compatible `[[project-name]]` syntax
```markdown
Related to: [[ai-persistence]] and [[another-project]]
```

**Usage**: Link to other project directories by name, enables knowledge graph visualization in Obsidian

---

## Pattern: Session Documentation

**Problem**: Need to capture what happened in each session across multiple sessions

**Solution**: Use numbered session headers in Recent Changes section
```markdown
## Recent Changes

### Session 3 - 2025-11-07
- Change A
- Change B

### Session 2 - 2025-11-06
- Change C

### Session 1 - 2025-11-05
- Change D
```

**Usage**: Add new session at top, keep recent 5-10 sessions for context

---
