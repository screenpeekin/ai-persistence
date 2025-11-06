# AI Persistence & Context System

A documentation, note taker and report generator that that leverages AI assistants remember context across sessions and platforms

## Core Concept

Generate a report with the most critical commands and steps ran for documentation purposes. This system enables seamless collaboration between Claude, Gemini, and other AI clients by maintaining persistent context across sessions. Each AI maintains its own working memory while contributing to a unified project understanding.

The system generates summary reports documenting workflows, configurations, decisions, and blockers - allowing you to pick up exactly where you left off, regardless of which AI client you're using.

### Initialization Process

1. User creates and edits `starter.md` to define their project
2. User runs `/init` with their AI client (Claude Code or Gemini CLI)
3. AI reads `starter.md` and creates `STARTER.md` (frozen reference copy)
4. AI uses `STARTER.md` to create the project structure and required files
5. User no longer edits `starter.md` after initialization

---

## Primary Goals

Ordered by implementation sequence:

1. **Session continuity** - Pick up where you left off across different sessions and AI clients
2. **Create summaries** - Generate session reports documenting commands, decisions, blockers, and allowers for easy reference
3. **Project reproducibility** - Create guides to recreate projects and configurations
4. **Automated documentation** - AI creates showcase content from project work

---

## File Structure

Single-project structure with AI-managed files organized in subfolders:

```
agent-files/
├── starter.md              # User-editable (only before /init)
├── STARTER.md              # Frozen reference (created by /init)
├── ai/                     # AI-managed files (manually committed to git)
│   ├── summary-c.md        # Claude's session summaries
│   ├── summary-g.md        # Gemini's session summaries
│   ├── summary.md          # Unified summary (read at session start)
│   ├── claude.md           # Claude-specific context
│   └── gemini.md           # Gemini-specific context
├── reports/                # Generated session reports
│   ├── YYYY-MM-DD-report.md
│   └── YYYY-MM-DD-report.md
└── templates/              # Blank templates (populated as needed)
    ├── summary-template.md
    ├── project-template.md
    └── session-template.md
```

### File Relationships

- `claude.md` ↔ `gemini.md` - Synced project instructions
- `summary-c.md` - Claude's working memory (updated at session end)
- `summary-g.md` - Gemini's working memory (updated at session end)
- `summary.md` - Unified context (read at session start, updated at session end)
- `reports/` - Session reports with highlights, configurations, and commands used

---

## Session Lifecycle

### Starting a Session

**Workflow instruction:** When starting, user tells AI to begin session.

The AI will:
1. Read `ai/summary.md` to load context from previous sessions
2. Announce what it found:
   - Current status of the project
   - Open blockers that need attention
   - Recent changes from last session
   - Suggested next steps
3. Begin monitoring all activity for the session report

### During a Session

**Semi-automated tracking:** The AI continuously monitors and proposes what to document:
- Commands executed
- Files modified
- Decisions made
- Configurations changed
- Problems encountered and solved
- Resources referenced

The AI will propose documentation items for user approval before finalizing.

### Ending a Session

**Workflow instruction:** When ending, user tells AI to complete session documentation.

The AI will:
1. Propose and generate (with user approval) a dated report in `reports/YYYY-MM-DD-report.md` containing:
   - **Highlights** - Key accomplishments from the session
   - **Configurations** - Settings, configs, or environment changes made (include config file contents)
   - **Commands Used** - Important commands executed with context
   - **Code Snippets** - Relevant code blocks created or modified
   - **Blockers** - New issues that need resolution
   - **Allowers** - Solutions that successfully unblocked work
2. Update `ai/summary-c.md` (Claude) or `ai/summary-g.md` (Gemini) with session details
3. **Always merge** both AI summaries into unified `ai/summary.md` for next session start (even if only one AI was active this session)

### Cross-AI Continuity

When switching between Claude and Gemini:
1. Previous AI has already updated its summary file (`summary-c.md` or `summary-g.md`)
2. Summary Agent **always merges both** into `summary.md` (even if one AI had no activity)
3. New AI reads `summary.md` at session start and picks up where you left off
4. Both AIs contribute to the same project understanding

**Note:** If only one AI has been active, the merge still occurs. The inactive AI's summary file remains unchanged while the active AI's updates are merged into the unified summary.

---

## Summary File Contents

Each summary file tracks:

### Session Information
- Activity timestamps
- Commands executed
- Files modified
- Cross-references between related projects

### Blockers
Issues preventing progress that need resolution across sessions. Or a configuration that doesn't work with installed dependencies

### Allowers
Configurations, commands, or code that made something work successfully. These are reference points for reproducing solutions.

### Annotations
Reminders about previous solutions or configurations when recreating similar setups

### Daily Highlights
Key accomplishments and discoveries from each session

---

## Log Categories

Summaries organize information into four log types:

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

### 4. (Removed - consolidated into Blockers/Allowers)
When an issue is resolved, it transitions from **Blocker** to **Allower** with the solution documented.

---

## Agent System

The system uses specialized sub-agents to manage files and maintain context:

### Summary Agent
- Merges `summary-c.md` and `summary-g.md` into unified `summary.md`
- Extracts common patterns and reconciles conflicts

### Context Agent
- Monitors blockers and allowers
- Flags unresolved issues across sessions
- Surfaces relevant annotations when similar work is detected

### Index Agent
- Maintains `index.md` with project status
- Tracks cross-project relationships
- Updates last_modified timestamps

### Report Agent
- Generates report from other agent files, in a clean markdown format that uses AI to determine what I ran

Agents analyze chat history to automatically populate summaries, reducing manual documentation burden.

## Design Principles

- Creates readable, simple, and clean documentation of work accomplished.
- **AI-agnostic format** - Use markdown with clear section headers for universal parsing
- **Lowercase convention** - All managed files use lowercase names for consistency
- **Single project focus** - One `agent-files/` directory per project repository
- **Session-based workflow** - Explicit `/start-session` and `/end-session` commands for clear boundaries
- **Retrieval-focused** - Optimize for quickly finding "where did I leave off?"
- **Git integration** - Will leverage git commits for tracking changes (to be implemented)
