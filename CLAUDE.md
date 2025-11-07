# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This System Does

This is a **multi-AI documentation and session continuity system** that enables Claude Code and Gemini to collaborate across sessions through shared markdown files. The system automatically generates how-to reports, maintains session context, and enables seamless handoffs between different AI clients.

## Core Architecture

### The Summary Merge Pattern

This is the synchronization mechanism that enables cross-AI continuity:

```
summary-c.md (Your working memory)
      +
summary-g.md (Gemini's working memory)
      ↓
  Summary Agent merges both
      ↓
  summary.md (Unified context read at session start)
```

**At session start:** Read `agent-files/ai/summary.md` to load project context
**At session end:** Update `summary-c.md`, then merge both summaries into `summary.md`

### Four Specialized Agents

All agents are invoked via the Task tool with `subagent_type: "general-purpose"`:

1. **Report Agent** (`agent-files/agents/report-agent.md`)
   - Generates how-to style reports with sarcastic humor
   - Output: `reports/YYYY-MM-DD-HH-MM-report.md` (timestamped)
   - Personality: "Friendly tutor who makes technical docs fun to read"

2. **Summary Agent** (`agent-files/agents/summary-agent.md`)
   - Merges summary-c.md + summary-g.md → summary.md
   - Professional, methodical, organized
   - Removes duplicates, reconciles conflicts

3. **Context Agent** (`agent-files/agents/context-agent.md`)
   - Analyzes project state at session start
   - Surfaces blockers, allowers, relevant history
   - No file writes, displays analysis only

4. **Index Agent** (`agent-files/agents/index-agent.md`)
   - Maintains high-level project index in `index.md`
   - Optional invocation at session end

### Division of Labor: Claude Code vs Gemini

**Your responsibilities as Claude Code:**
- **Ask before generating reports** - User chooses whether to create how-to guides (default: No)
- **Ask before git operations** - User chooses whether to pull/push (default: No)
- **Always merge summaries** - Combine summary-c.md + summary-g.md → summary.md at session end
- **Optionally update index** - Ask user if they want index.md updated
- **Update summary-c.md** - Your session summaries (always done)

**Gemini's responsibilities (Research Specialist):**
- **Research solutions** - Web search, documentation exploration, solution discovery
- **Create option lists** - Present multiple approaches with pros/cons
- **Evaluate solutions** - Compare options and recommend best fits
- **Gather resources** - Collect relevant links, examples, tutorials
- **Update summary-g.md** - Document research findings in research-focused format
- Does NOT implement solutions (that's your job)
- Does NOT generate reports automatically (that's your job)
- Does NOT push to git automatically (that's your job)

**Typical workflow:** Gemini researches and finds options → You implement the recommended solution

This division prevents git conflicts, reduces duplicate work, and plays to each AI's strengths. All operations that modify files or use git require user confirmation with "No" as the default option.

## Session Lifecycle Workflows

### Starting a Session

When user says "Record session", "Record reports", or similar recording phrases:

1. **Ask about git pull:**
   ```
   Would you like to pull latest changes from git?

   1. **No** - Skip git pull
   2. **Yes** - Pull from remote (git pull)

   Choose: [1/2]
   ```
   Wait for user's choice. Default: No (option 1).

2. **Pull from git (if user chose Yes):**
   ```bash
   git pull
   ```

3. **Check for external changes (if git pull was performed):**
   - If git pull brought updates, review with `git log` and `git diff`
   - Announce what changed externally (e.g., "Gemini updated the X feature")

4. **Invoke Context Agent** to analyze project state:
   ```
   Task tool with prompt:
   "Read agent-files/agents/context-agent.md and follow instructions.
   Analyze summary.md and surface current status, blockers, allowers, suggested next steps."
   ```

5. **Announce to user:**
   - Current project status
   - Open blockers that need attention
   - Recent changes from last session
   - Suggested next steps

6. **Begin session monitoring** - track commands, files, decisions, configurations

Reference: `agent-files/templates/session-start-template.md`

### Ending a Session

When user says "Stop report", "Stop session recorder", "Stop report recorder", or similar stopping phrases:

1. **Ask about report generation:**
   ```
   Would you like to generate a session report?

   1. **No** - Skip report generation
   2. **Yes** - Generate session report

   Choose: [1/2]
   ```
   Wait for user's choice. Default: No (option 1).

2. **Ask for report type (if user chose Yes):**
   ```
   What type of report would you like?

   1. **How-To Guide** - Step-by-step reproduction with sarcastic humor
   2. **Summary Report** - High-level overview (executive style)
   3. **Technical Documentation** - Detailed technical reference
   4. **Troubleshooting Guide** - Problem → Solution format
   5. **Decision Record** - Architecture Decision Record (ADR) style
   6. **Quick Reference** - Command cheat sheet
   7. **Changelog** - Release notes style

   Choose: [1/2/3/4/5/6/7]
   ```

3. **Ask for detail level (if How-To Guide chosen):**
   - **Every command**: All commands in sequence (copy-paste script style)
   - **Key actions**: Important steps with context (skip trivial operations)
   - **Recipe style**: Prerequisites + high-level steps + key commands only

4. **Invoke Report Agent (if user chose Yes):**
   ```
   Task tool with prompt:
   "Read agent-files/agents/report-agent.md and follow instructions.
   Generate a report in reports/YYYY-MM-DD-HH-MM-report.md using report type [chosen type].
   For How-To Guide, use detail level [chosen level]."
   ```

5. **Update your summary file:**
   - Update `agent-files/ai/summary-c.md` with session details
   - Include: activity logs, decision logs, reference logs
   - Add new blockers or allowers discovered
   - Maintain chronological session history

6. **Invoke Summary Agent** to merge summaries:
   ```
   Task tool with prompt:
   "Read agent-files/agents/summary-agent.md and follow instructions.
   Merge summary-c.md and summary-g.md into unified summary.md."
   ```
   This happens EVERY session end, even if Gemini had no activity.

7. **Optional: Invoke Index Agent** to update project overview:
   ```
   Task tool with prompt:
   "Read agent-files/agents/index-agent.md and follow instructions.
   Update index.md with current project state."
   ```

8. **Ask about git commit and push:**
   ```
   Would you like to commit and push changes to git?

   1. **No** - Skip git commit and push
   2. **Yes** - Commit and push to remote

   Choose: [1/2]
   ```
   Wait for user's choice. Default: No (option 1).

9. **Commit and push to git (if user chose Yes):**
   ```bash
   git add -A
   git commit -m "Session YYYY-MM-DD HH:MM: [accomplishments]

   Generated with Claude Code

   Co-Authored-By: Claude <noreply@anthropic.com>"
   git push origin master
   ```

Reference: `agent-files/templates/session-end-template.md`

### Manual Summary Merge

When user says "Merge summaries" or "Sync summaries":

1. Read both `summary-c.md` and `summary-g.md`
2. Invoke Summary Agent to merge into `summary.md`
3. Display confirmation with entry counts
4. **Does NOT** create report, commit, or push

Use cases: Mid-session sync, before switching AI clients, after external changes detected.

Reference: `agent-files/templates/summary-merge-template.md`

## File Structure and Relationships

```
/
├── STARTER.md                      # Frozen reference (created by /init)
├── CLAUDE.md                       # This file (Claude-managed instructions)
├── GEMINI.md                       # Gemini-managed instructions
├── index.md                        # Project index (you create/update)
├── reports/                        # Session reports (you create)
│   ├── YYYY-MM-DD-HH-MM-report.md  # Timestamped (multiple per day)
│   └── YYYY-MM-DD-HH-MM-report.md
└── agent-files/
    ├── starter.md                  # User-editable (only before /init)
    ├── ai/                         # AI-managed files
    │   ├── summary-c.md            # Your session summaries
    │   ├── summary-g.md            # Gemini's session summaries
    │   └── summary.md              # Unified summary (read at session start)
    ├── agents/                     # Agent prompt templates
    │   ├── report-agent.md         # Report generation (sarcastic tutor)
    │   ├── summary-agent.md        # Summary merging
    │   ├── context-agent.md        # Context analysis
    │   └── index-agent.md          # Project indexing
    └── templates/                  # Workflow templates
        ├── session-start-template.md
        ├── session-end-template.md
        ├── summary-merge-template.md
        ├── report-template.md
        ├── summary-template.md
        └── session-template.md
```

### Key File Relationships

- **session start** → read `summary.md`
- **session end** → update `summary-c.md` → merge into `summary.md`
- **agents/** → contain specialized agent prompts invoked via Task tool
- **templates/** → define workflow steps and output formats
- **summary-g.md** → Gemini's research findings (read this for solution options before implementing)
- **summary-c.md** → Your implementation details (what you built and how)

## Summary File Structure

**IMPORTANT:** Claude and Gemini use DIFFERENT summary formats optimized for their roles.

### Your Summary Format (summary-c.md) - Implementation-Focused

Track implementation details:
- **Activity Logs** - Commands executed, files created/modified, implementations completed
- **Decision Logs** - Code decisions, architectural choices, why you chose approach X over Y
- **Reference Logs** - Documentation that helped with implementation
- **Blockers** - Issues preventing implementation progress
- **Allowers** - Configurations/code that made something work

Format:
```markdown
### Session [N] - [YYYY-MM-DD]
- Implemented feature X using approach Y
- Modified files: [list]
- Commands run: [list]
- Tests passed/failed
```

### Gemini's Summary Format (summary-g.md) - Research-Focused

Gemini tracks research findings:
- **Research Questions** - What problems were investigated
- **Options Explored** - List of possible approaches with pros/cons
- **Solutions Found** - Specific solutions with source URLs
- **Evaluations** - Detailed comparison of different approaches
- **Recommendations** - Best approach for the use case
- **Resources Referenced** - Links to documentation, tutorials, examples
- **Blockers** - Knowledge gaps preventing solution discovery
- **Allowers** - Resources that led to breakthroughs

Format:
```markdown
### Session [N] - [YYYY-MM-DD]
**Research Question:** How to implement X?

**Options Found:**
1. Approach A - Pros: [...], Cons: [...]
2. Approach B - Pros: [...], Cons: [...] ✓ RECOMMENDED

**Resources:**
- [Official Docs](URL) - Detailed API reference
- [Tutorial](URL) - Step-by-step implementation guide
```

### Using Gemini's Research

When you see Gemini has researched a topic in `summary.md`:
1. Read their **Options Explored** section to understand all approaches
2. Review their **Evaluations** to understand trade-offs
3. Check their **Recommendations** for guidance on best approach
4. Use their **Resources Referenced** for implementation documentation
5. Implement the recommended solution
6. Document your implementation in `summary-c.md`

## Agent Invocation Pattern

All agents are invoked using the Task tool:

```
Task tool parameters:
- subagent_type: "general-purpose"
- description: "Generate session report" (or similar)
- prompt: "Read agent-files/agents/[agent-name]-agent.md and follow instructions. [specific context]"
```

Agents run independently with their own prompts, then return results to you. This enables parallel execution and clean separation of concerns.

## Common Development Tasks

### Initialize New Project

When user runs `/init`:
1. Create directory structure: `mkdir -p agent-files/ai agent-files/agents agent-files/templates reports`
2. Copy `agent-files/starter.md` to `STARTER.md` (frozen reference)
3. Create AI summary files in `agent-files/ai/`: `summary-c.md`, `summary-g.md`, `summary.md` with initial structure
4. Create AI instruction files in project root: `CLAUDE.md`, `GEMINI.md` with full specifications
5. Create agent templates in `agent-files/agents/`
6. Create workflow templates in `agent-files/templates/`
7. Optionally create `QUICK-REFERENCE.md` and `SESSION-DEMO.md` in project root

### Manual Agent Invocation

Users can invoke agents manually:
- "Generate a report" → Invoke Report Agent
- "Check context" → Invoke Context Agent
- "Update index" → Invoke Index Agent
- "Merge summaries" → Invoke Summary Agent

### Cross-AI Continuity

When switching from Gemini to Claude Code:
1. You'll see Gemini's changes in `summary-g.md`
2. Summary Agent merges both into `summary.md`
3. You pick up exactly where Gemini left off

When switching from Claude Code to Gemini:
1. You've already updated `summary-c.md` and merged into `summary.md`
2. Gemini reads `summary.md` at their session start
3. Gemini picks up where you left off

## Report Generation

The Report Agent can generate 7 different types of reports based on user needs. Each has its own tone, structure, and purpose.

### Available Report Types

1. **How-To Guide** - Step-by-step reproduction with sarcastic, humorous tone (default)
   - Best for: Most sessions, reproducible instructions
   - Tone: Friendly developer with personality
   - Detail level: User chooses (Every command / Key actions / Recipe style)

2. **Summary Report** - High-level overview for stakeholders
   - Best for: Executive briefings, status updates
   - Tone: Professional, concise
   - Detail level: Fixed (high-level only)

3. **Technical Documentation** - Official reference documentation
   - Best for: API docs, architecture references
   - Tone: Formal, precise
   - Detail level: Fixed (comprehensive)

4. **Troubleshooting Guide** - Problem → Solution format
   - Best for: Debugging sessions, fixing issues
   - Tone: Helpful, diagnostic
   - Detail level: Fixed (problem-focused)

5. **Decision Record (ADR)** - Architecture Decision Record
   - Best for: Major technical/architectural decisions
   - Tone: Analytical, formal
   - Detail level: Fixed (decision-focused)

6. **Quick Reference** - Command cheat sheet
   - Best for: When you just want commands saved
   - Tone: Terse, minimal prose
   - Detail level: Fixed (commands only)

7. **Changelog** - Release notes style
   - Best for: Documenting what changed from previous state
   - Tone: Formal, structured
   - Detail level: Fixed (change-focused)

**Report naming:** `YYYY-MM-DD-HH-MM-report.md` using 24-hour time format to prevent overwriting multiple sessions on the same day.

**Pro tip:** You can generate multiple report types for the same session. Just invoke the Report Agent multiple times with different type choices.

## Git Workflow

**Session start (if user chooses):**
```bash
git pull  # Sync external changes
```
User is asked first with "No" as the default option.

**Session end (if user chooses):**
```bash
git add -A
git commit -m "Session YYYY-MM-DD HH:MM: [accomplishments]

Generated with Claude Code

Co-Authored-By: Claude <noreply@anthropic.com>"
git push origin master
```
User is asked first with "No" as the default option.

**Important:** Git operations are opt-in. User must explicitly choose to pull/push. Gemini does NOT perform git operations automatically.

## Design Principles

- **User control first** - All file modifications and git operations require user approval with "No" as default
- **AI-agnostic format** - Pure markdown with clear section headers
- **Lowercase convention** - All managed files use lowercase names
- **Single project focus** - One agent-files/ directory per project
- **Session-based workflow** - Explicit start/end boundaries
- **Template-driven** - All outputs follow standardized formats
- **Retrieval-focused** - Optimize for "where did I leave off?"
- **Semi-automated** - AI proposes documentation, user approves

## GEMINI.md Generation

**Important:** GEMINI.md is auto-generated from CLAUDE.md.

**Workflow for Claude Code:**
1. **NEVER edit GEMINI.md directly** - Always work with CLAUDE.md (this file)
2. After finishing changes to CLAUDE.md, run:
   ```bash
   cp CLAUDE.md GEMINI.md
   ```
3. Update the header in GEMINI.md:
   - Change `# CLAUDE.md` to `# GEMINI.md`
   - Change "Claude Code" references to "Gemini"
   - Add note that it's auto-generated
4. GEMINI.md should be committed to git (it's tracked, not ignored)

**Why this approach:**
- Single source of truth (CLAUDE.md)
- Avoids duplicate maintenance
- Ensures both AIs have identical instructions
- Only the header differs (CLAUDE.md vs GEMINI.md)
- Both files are version controlled

## Key References

Read these files for detailed specifications:
- `GEMINI.md` - Auto-generated copy of this file for Gemini
- `QUICK-REFERENCE.md` - At-a-glance workflow guide
- `SESSION-DEMO.md` - Complete walkthrough with examples
- `agent-files/templates/` - Workflow and format templates
