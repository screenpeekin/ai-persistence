# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI persistence and context system that generates session reports documenting critical commands, decisions, blockers, and solutions. Enables seamless collaboration between Claude, Gemini, and other AI clients by maintaining persistent context across sessions.

Reference `STARTER.md` (frozen copy) for complete project specifications.

## File Structure

```
agent-files/
├── starter.md              # User-editable (only before /init)
├── STARTER.md              # Frozen reference (created by /init)
├── ai/                     # AI-managed files
│   ├── summary-c.md        # Claude's session summaries
│   ├── summary-g.md        # Gemini's session summaries
│   ├── summary.md          # Unified summary (read at session start)
│   ├── claude.md           # This file (Claude-specific context)
│   └── gemini.md           # Gemini-specific context (synced with claude.md)
├── reports/                # Generated session reports
│   └── YYYY-MM-DD-report.md
└── templates/              # Blank templates
    ├── summary-template.md
    ├── project-template.md
    └── session-template.md
```

## Session Commands

### `/start-session`

When starting a session:
1. **Read `ai/summary.md`** to load context from previous sessions
2. **Announce to user:**
   - Current project status
   - Open blockers needing attention
   - Recent changes from last session
   - Suggested next steps
3. **Begin monitoring** all activity for the session report

### `/end-session`

When ending a session:
1. **Generate report** in `reports/YYYY-MM-DD-report.md` with:
   - **Highlights** - Key accomplishments
   - **Configurations** - Settings/environment changes made
   - **Commands Used** - Important commands with context
   - **Blockers** - New issues needing resolution
   - **Allowers** - Solutions that successfully unblocked work
2. **Update `ai/summary-c.md`** with session details organized into log categories
3. **Merge summaries** - Combine `summary-c.md` and `summary-g.md` into unified `ai/summary.md`

## Log Categories

Organize information into four types:

### 1. Activity Logs
- Commands run
- Files changed
- Actions taken during session

### 2. Decision Logs
- Architectural choices made
- Rationale for "why we did X not Y"
- Trade-offs considered

### 3. Reference Logs
- Links consulted
- Documentation referenced
- Tutorials followed
- External resources used

### 4. Error Logs
- Failed attempts
- Debugging notes
- Issues encountered and resolved

## Blockers vs Allowers

### Blockers
Issues preventing progress that need resolution across sessions. Examples:
- Configuration that doesn't work with installed dependencies
- Missing information needed to proceed
- Errors that stopped development

### Allowers
Configurations, commands, or code that made something work successfully. These are reference points for reproducing solutions. Examples:
- The exact command that fixed an issue
- Working configuration discovered through trial
- Code pattern that solved a problem

## Summary File Contents

Track across sessions:
- Activity timestamps
- Commands executed
- Files modified
- Cross-references between related projects
- Blockers and allowers
- Annotations (reminders about previous solutions)
- Daily highlights

## Agent System

Four specialized agents manage files and maintain context:

### Summary Agent
Merges `summary-c.md` and `summary-g.md` into unified `summary.md`, extracting common patterns and reconciling conflicts.

### Context Agent
Monitors blockers and allowers, flags unresolved issues across sessions, surfaces relevant annotations when similar work is detected.

### Index Agent
Maintains `index.md` with project status, tracks cross-project relationships, updates timestamps.

### Report Agent
Generates clean markdown reports from agent files, using AI to determine what was accomplished.

## Design Principles

- **Creates readable, simple, and clean documentation** of work accomplished
- **AI-agnostic format** - Markdown with clear section headers for universal parsing
- **Lowercase convention** - All managed files use lowercase names for consistency
- **Single project focus** - One `agent-files/` directory per project repository
- **Session-based workflow** - Explicit `/start-session` and `/end-session` commands for clear boundaries
- **Retrieval-focused** - Optimize for quickly finding "where did I leave off?"

## Cross-AI Continuity

When switching between Claude and Gemini:
1. Previous AI updated its summary file (`summary-c.md` or `summary-g.md`)
2. Agent merged both into `summary.md`
3. New AI reads `summary.md` at `/start-session` and picks up where you left off
4. Both AIs contribute to the same project understanding

## Important Notes

- **starter.md** is only user-editable before `/init`
- After `/init`, reference **STARTER.md** (frozen copy) for specifications
- **claude.md** ↔ **gemini.md** must stay synced
- All files in `ai/` are AI-managed and should be manually committed to git
- Session reports in `reports/` provide clean documentation of work accomplished
