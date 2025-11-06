# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

AI-powered documentation system that maintains session continuity across Claude and Gemini. The system generates markdown summaries (`my-report.md` files) documenting workflows, commands, and configuration changes for easy reference and reproducibility.

This is a **concept-building project** - walk through each step with the user, asking questions to guide implementation.

## Architecture

### Summary File System

Files stored in `ai-persistence/ignore/`:

- **SUMMARY-C.md**: Claude's session summaries and context
- **SUMMARY-G.md**: Gemini's session summaries
- **SUMMARY.md**: Unified summary (merged by agent from C and G files)
- **GEMINI.md**: Gemini CLI context file (synced from CLAUDE.md)

### Agent Requirements

Build an agent that can:
- Merge SUMMARY-C.md and SUMMARY-G.md into SUMMARY.md
- Parse chat history between user and AI
- Flag and extract blockers and questions from user input
- Track "allowers" (enablers/solutions) alongside blockers
- Extract activity timestamps for summary submissions
- Cross-reference related projects and tasks
- Identify and reference source materials

## Session Summary Format

At the end of each session, generate a comprehensive summary in SUMMARY-C.md using this structure:

### Session Header
```markdown
## Session [N] - [YYYY-MM-DD HH:MM]
**Duration**: [time spent]
**Focus**: [main objective of this session]
```

### What Was Accomplished
Focus on **outcomes**, not file changes. Answer:
- What now works that didn't before?
- What new capabilities were added?
- What problems were solved?
- What documentation was created?

Example:
```markdown
### Accomplishments
- Created CLAUDE.md with session documentation guidelines
- Established project structure for AI-powered documentation system
- Defined agent requirements for summary merging
```

### Key Commands & Technical Actions
List **significant commands only** - those that:
- Changed system state
- Installed/configured tools
- Solved problems
- Are needed to reproduce work

Example:
```markdown
### Key Commands
- `git init` - Initialized repository
- `npm install langchain` - Added LLM framework
- `python agent.py --merge` - Tested summary merger
```

### Decisions & Rationale (CRITICAL)
Document **every architectural or technical decision** made:

```markdown
### Decisions Made
1. **[Decision Title]**
   - Context: [Why this decision was needed]
   - Choice: [What was decided]
   - Rationale: [Why this approach over alternatives]
   - Alternatives considered: [Other options discussed]
   - Impact: [What this affects going forward]
```

### Unfinished Work (CRITICAL)
This is your handoff to the next session. Be explicit:

```markdown
### In Progress / Not Completed
- [ ] **[Task name]**
  - Status: [How far did you get?]
  - Why incomplete: [What stopped you - blocker, time, waiting for user input?]
  - Next steps: [Exactly what to do next]
  - Context needed: [What files/info to reference]
```

### Completeness Checklist
Before ending session, verify you've documented:
- [ ] Main accomplishments (outcome-focused)
- [ ] Key commands for reproducibility
- [ ] All decisions made with rationale
- [ ] Unfinished work with clear next steps
- [ ] Any blockers or questions for user
- [ ] Resources/docs referenced
- [ ] Timestamp and session number updated

## Context System

### What to Track
- Daily summaries (high-level work overview)
- Open questions & blockers (unresolved issues)
- Referenced resources (links, docs, tutorials)
- Decision log (architectural choices + rationale)
- Code patterns (reusable solutions discovered)

### Context Retrieval Features
- Session continuity (reference previous work)
- Smart suggestions (next steps based on blockers and allowers)
- Pattern recognition (recurring challenges/successful approaches)
- Knowledge graph (connections between projects, technologies, concepts)

## Key Goals

1. **Create summary**: Document workflow and configuration changes
2. **Session continuity**: Pick up where left off across AI clients
3. **Project reproducibility**: Create guides to recreate projects
4. **Automated documentation**: AI generates showcase content from work

## Important Files

- **starter.md**: Project concept and requirements (reference only, read-only)
- **GEMINI.md**: Gemini's context file (keep synced with CLAUDE.md)

## Session Workflow

### Starting a Session
1. **Read SUMMARY-C.md** - Review previous session's summary
2. **Check "Unfinished Work"** - This is your starting point
3. **Review "Decisions Made"** - Understand context and constraints
4. **Note blockers** - Address any flagged issues first

### During the Session
- Keep mental notes of decisions being made
- Flag unfinished work as you go
- Track key commands that changed state

### Ending a Session (MANDATORY)
1. **Generate complete summary** using the format above
2. **Run through completeness checklist** - ensure nothing missed
3. **Update SUMMARY-C.md** with new session section
4. **Verify clear handoff** - next session should know exactly where to start

## Development Approach

This project is in the conceptual phase. When working:
- Ask questions to clarify implementation details
- Build features iteratively with user feedback
- Flag blockers explicitly in summaries
- Document architectural decisions as they're made
- **Always end sessions with complete summary generation**