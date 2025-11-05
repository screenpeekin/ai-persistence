# GEMINI.md

This file provides guidance to Gemini when working in this repository. It is synchronized with `CLAUDE.md` to ensure context continuity across different AI clients.

## Directory Overview

This directory contains a cross-AI persistence system. The primary goal is to enable context to be maintained across sessions and between different AI assistants, such as Gemini and Claude. This is achieved through a set of structured markdown files that store session summaries, project states, and other relevant information.

## Key Files

*   **`GEMINI.md`**: (This file) The primary context and instruction file for Gemini. It is kept in sync with `CLAUDE.md`.
*   **`CLAUDE.md`**: The primary context and instruction file for Claude.
*   **`project-concept.md`**: Outlines the vision, features, and implementation phases for the AI-powered portfolio and project showcase.
*   **`.claude/`**: A directory containing settings and hooks specific to the Claude AI assistant.

## Usage

The core of this system is a manual, iterative process of updating summary files. This is the intended workflow:

### Session Context Management

**When starting a session:**

1.  Read the `INDEX.md` file (if it exists) to get an overview of all active projects.
2.  Review the relevant summary files (e.g., `summary-g.md` for Gemini) to understand the previous session's context.
3.  Pay special attention to the "Open Questions & Blockers" sections in the summary files.

**When ending a session:**

1.  Update the appropriate summary file (e.g., `summary-g.md`) with the current status, recent changes, and next steps.
2.  Document any architectural decisions made during the session.
3.  Update any relevant project metadata.

### Summary File Format

All summary files should adhere to the following structure:

```markdown
---
last_updated: YYYY-MM-DD HH:MM
session_count: N
---

## Current Status
[High-level overview of where the project stands]

## Recent Changes
[What was done in the latest session(s)]

## Open Questions & Blockers
- [ ] Question or blocker 1
- [ ] Question or blocker 2

## Next Steps
- [ ] Task 1
- [ ] Task 2

## Decision Log
[Key architectural/technical decisions and their rationale]

## Referenced Resources
- [Links, docs, tutorials consulted]
```

## Protected Files

### STARTER.md - Read-Only Project Guide
**CRITICAL RULE**: Never write to or modify STARTER.md after initial template creation.

- STARTER.md is the project's master implementation guide
- It should only be modified during the initial template creation phase
- Once the template is established, STARTER.md becomes read-only
- Any updates to project methodology should be discussed with the user first
- Gemini should reference STARTER.md for guidance but never edit it
