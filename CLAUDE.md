# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a cross-AI persistence system enabling Claude, Gemini, and other AI clients to maintain context across sessions. The system uses AI-agnostic markdown files with standardized structures to enable seamless handoffs between different AI tools.

## Core Architecture

### Summary File System
The project uses multiple summary files for different purposes:
- **summary-c.md**: Claude's session summaries and context
- **summary-g.md**: Gemini's session summaries
- **GEMINI.md**: Gemini CLI's native context file (synced from CLAUDE.md)
- **SUMMARY.md**: Unified project summary generated from individual AI summaries
- **INDEX.md**: Central index linking all projects and their current states

### Directory Structure
```
/ai-persistence/
  ├── templates/          # Reusable templates for summaries and projects
  ├── scripts/            # Automation scripts for summary generation
  ├── projects/           # Individual project directories
  │   └── [project-name]/
  │       ├── SUMMARY.md
  │       ├── summary-c.md
  │       ├── summary-g.md
  │       └── context/    # Decision logs, patterns, resources
  └── .claude/hooks/      # Session automation hooks
```

## Development Workflow

### Session Context Management
When starting a session:
1. Read INDEX.md to understand active projects and priorities
2. Check relevant summary-c.md for previous context
3. Review Open Questions & Blockers sections

When ending a session:
1. Update summary-c.md with Current Status, Recent Changes, Next Steps
2. Document any architectural decisions in context/decisions.md
3. Update project metadata (last_updated, status)

### Summary File Format
All summary files follow this structure:
```markdown
---
last_updated: YYYY-MM-DD HH:MM
session_count: N
---

## Current Status
## Recent Changes
## Open Questions & Blockers
## Next Steps
## Decision Log
## Referenced Resources
```

### Project Metadata (YAML Frontmatter)
```yaml
---
name: Project Name
type: project
project_id: unique-identifier
status: active|paused|completed
started: YYYY-MM-DD
last_updated: YYYY-MM-DD
tags: [tag1, tag2, tag3]
priority: high|medium|low
---
```

## Design Principles

- **AI-agnostic format**: Use markdown with clear section headers for universal parsing
- **Minimal bold syntax**: Avoid excessive formatting in markdown files
- **Git as source of truth**: Leverage commits for tracking changes
- **Manual first, automate later**: Build habits before building automation
- **Retrieval-focused**: Optimize for quickly finding "where did I leave off?"

## Implementation Phases

### Phase 1: Manual Process (Current)
- Manually update summaries after each session
- Build documentation habits
- Refine templates based on real usage

### Phase 2: Script-Assisted
- Parse git logs for automated updates
- Semi-automated summary generation with manual review
- Session start/end helper scripts

### Phase 3: Full Automation
- Automated daily summaries from project activity
- Integration with Claude Code hooks
- Status dashboard and search functionality

## Cross-AI Synchronization

CLAUDE.md should be synced to GEMINI.md to maintain continuity when switching between AI clients. Each AI updates its own summary file (summary-c.md or summary-g.md), which can be merged into a unified SUMMARY.md.

## Protected Files

### STARTER.md - Read-Only Project Guide
**CRITICAL RULE**: Never write to or modify STARTER.md after initial template creation.

- STARTER.md is the project's master implementation guide
- It should only be modified during the initial template creation phase
- Once the template is established, STARTER.md becomes read-only
- Any updates to project methodology should be discussed with the user first
- Claude should reference STARTER.md for guidance but never edit it

## Key Considerations

When implementing features:
- Ask user for decisions when architectural choices arise
- Maintain consistent YAML frontmatter across all projects
- Use wiki-style linking `[[project-name]]` for Obsidian compatibility
- Parse `git log --since="yesterday"` for activity tracking
- Keep summaries concise and actionable
