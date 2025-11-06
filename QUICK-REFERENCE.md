# Quick Reference Guide

## File Structure at a Glance

```
STARTER.md                       # Frozen reference (read-only after /init)
agent-files/
├── starter.md                   # User-editable (only before /init)
├── ai/
│   ├── claude.md                # Claude-specific context
│   ├── gemini.md                # Gemini-specific context
│   ├── summary-c.md             # Claude's working memory
│   ├── summary-g.md             # Gemini's working memory
│   └── summary.md               # Unified context (read at session start)
├── reports/
│   └── YYYY-MM-DD-report.md     # Session documentation
└── templates/                   # Reference templates
```

## Session Workflow (3 Steps)

### 1. Start Session
**You say:** "Let's start a session"

**I do:**
- Read `ai/summary.md`
- Announce current status, blockers, recent changes, next steps
- Begin monitoring activity

### 2. During Session
**I do:**
- Track commands, decisions, configurations, problems
- Propose documentation items
- Ask for your approval before finalizing

### 3. End Session
**You say:** "Let's end this session and document everything"

**I do:**
- Propose report content for your approval
- Generate `reports/YYYY-MM-DD-report.md` with:
  - Highlights
  - Configurations (with file contents)
  - Commands Used
  - Code Snippets
  - Blockers
  - Allowers
- Update `ai/summary-c.md` (or summary-g.md for Gemini)
- Merge both AI summaries into `ai/summary.md`

## Key Concepts

### Blockers
Issues **preventing progress** that need resolution
- Example: "Can't deploy - missing API key"
- Status: OPEN until resolved

### Allowers
Solutions that **made something work**
- Example: "Added `--legacy-peer-deps` to npm install command"
- Status: RESOLVED with solution documented
- **Flow:** Blocker → (resolved) → Allower

### Log Categories

1. **Activity Logs** - Commands run, files changed, actions taken
2. **Decision Logs** - Architectural choices and rationale
3. **Reference Logs** - Links, docs, tutorials consulted
4. **Blockers/Allowers** - Problems and solutions

## Cross-AI Switching

Switching from Claude to Gemini (or vice versa):
1. Current AI updates its summary file (summary-c.md or summary-g.md)
2. Summary Agent merges both into summary.md
3. New AI reads summary.md at next session start
4. Both AIs share the same project understanding

## Agent Types

- **Summary Agent** - Merges summary-c.md + summary-g.md → summary.md
- **Context Agent** - Monitors blockers/allowers, flags unresolved issues
- **Index Agent** - Maintains project status and timestamps
- **Report Agent** - Generates clean session reports

## Tips

- Reports are **outcome-focused** (what now works vs what changed)
- Include **significant commands only** (those that changed state)
- **Always document decisions** with context and rationale
- Reports include **code snippets and config file contents**
- **Semi-automated** - I propose, you approve
