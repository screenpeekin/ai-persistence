# AI-Powered Portfolio & Task Showcase

## Core Concept

**THIS PROJECT IS THE CONCEPT OF BUILDING AND I WANT TO WALK THROUGH EACH STEP BY YOU ASKING ME QUESTIONS**

Create a cross-AI persistence system where Claude (summary-c.md), Gemini (summary-g.md) and sync CLAUDE.md to GEMINI.md for continuity, and so AI clients can maintain context across sessions. Each AI generates summaries that can be consumed by other tools, enabling seamless handoffs and continuous development. I want you give me decisions when they arise.

### Design Principles
- AI-agnostic format: Use markdown with clear section headers for universal parsing
- Use minimal bold syntax in markdown files
- **Git as source of truth**: Leverage commits for tracking "what changed"
- **Manual first, automate later**: Build habits before building automation
- **Retrieval-focused**: Optimize for quickly finding "where did I leave off?"

### Primary Goals
- **Session continuity**: Pick up where we left off across different sessions and AI clients
- **Project reproducibility**: Create guides to recreate projects and configurations
- **Automated documentation**: AI creates showcase content from project work
- **Continuous context**: Daily summary updates allow AI to follow up the next day
- **Project tracking**: Automated agents monitor and post updates to the site

---

## Features

### Summary File Structure
- **summary-c.md**: Claude's session summaries and context
- **summary-g.md**: Gemini's session summaries
- **GEMINI.md**: Gemini CLI's native context file
- **SUMMARY.md**: Unified project summary (generated from individual summaries)
- **INDEX.md**: Central index linking all projects and their current states

### Daily Logs System
- Automated daily summaries generated from project activity
- Activity timestamps showing when work was done
- Cross-referencing between related projects and tasks
- Daily highlights extracted and featured prominently
- Parse `git log --since="yesterday"` for automated updates

### Smart Context System

#### Context Storage
- Daily summaries - high-level overview of work completed
- Open questions & blockers - track unresolved issues across sessions
- Future feature ideas - capture inspiration and TODOs and FIXs as they emerge
- Referenced resources - links, documentation, tutorials consulted
- **Decision log** - architectural choices and rationale
- **Code patterns** - reusable solutions discovered during development

#### Context Retrieval
- **Session continuity** - AI can reference previous days' work
- **Smart suggestions** - AI proposes next steps based on blockers
- **Pattern recognition** - identify recurring challenges or successful approaches
- **Knowledge graph** - connections between projects, technologies, and concepts

### Metadata & Highlights

Each project includes standardized YAML frontmatter:
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

### Summary Template Structure
Each summary file should follow this format:
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


## Implementation Structure

### Recommended Directory Layout
```
/ai-persistence/
  ├── templates/
  │   ├── summary-template.md
  │   ├── project-template.md
  │   └── frontmatter-template.yaml
  ├── scripts/
  │   ├── update-summary.sh
  │   ├── generate-context.sh
  │   ├── session-start.sh
  │   └── session-end.sh
  ├── projects/
  │   └── [project-name]/
  │       ├── SUMMARY.md
  │       ├── summary-c.md
  │       ├── summary-g.md
  │       └── context/
  │           ├── decisions.md
  │           ├── patterns.md
  │           └── resources.md
  ├── INDEX.md
  └── .claude/
      └── hooks/
          ├── session-start.sh
          └── session-end.sh
```

### Session Hook System
- **session-start.sh**: Display relevant context from previous sessions
- **session-end.sh**: Auto-generate summary updates before closing
- Integrate with Claude Code's hook system for automation

## Automation Architecture

### Phase 1: Manual Process (Start Here)
1. Manually update summaries after each session
2. Build the habit of documenting decisions and blockers
3. Create templates that work for your workflow

### Phase 2: Script-Assisted
1. **Scan project directories** for changes via git
2. **Extract meaningful updates** from commit messages
3. **Semi-automated summary generation** with manual review
4. **Update project metadata** (last_updated, status)

### Phase 3: Full Automation
1. **Automated daily summaries** from project activity
2. **Extract meaningful updates** (not just file changes)
3. **Update SUMMARY.md** with context
4. **Generate blog posts** for significant milestones
5. **Update project metadata** (completion %, last_updated, etc.)
6. **Deploy updates**

### Data Collection Points
- Test results and coverage
- Build/deployment logs
- Manual notes or annotations

---

### Obsidian Integration

#### Dataview-Ready Format
Use Obsidian-compatible frontmatter and linking:
```yaml
---
type: project
status: active
tags: [ai, automation, portfolio]
created: 2025-11-05
---
```

#### Wiki-Style Linking
- Use `[[project-name]]` syntax for cross-references
- Create MOCs (Maps of Content) for different domains
- Link related concepts, blockers, and decisions
- Build a knowledge graph naturally through links

#### Database Queries
With Obsidian Dataview plugin:
```dataview
TABLE status, last_updated, priority
FROM "projects"
WHERE type = "project" AND status = "active"
SORT priority DESC
```

### Automation Tools
- **Git**: Source of truth for tracking changes
- **Shell scripts**: Orchestration and automation
- **ripgrep/grep**: Fast content searching across summaries
- **GitHub Actions**: CI/CD for deployment
- **Cron/systemd timers**: Scheduled daily runs
- **Git hooks**: Event-based triggers

---

## Future Enhancements

### Near-term
- **Status command**: Quick CLI to show all active projects and blockers
- **Search functionality**: Full-text search across all summaries and context
- **Template generator**: Script to scaffold new projects with proper structure
- **Summary merger**: Tool to combine summary-c.md and summary-g.md into SUMMARY.md

### Long-term
- Dependency tracking and security alerts
- "Tech tree" visualization of skill progression
- Export functionality (PDF resume generation from projects)
- Natural language querying of project history
- Automatic blog post generation from milestones
- Integration with time tracking tools
- AI-powered pattern recognition across projects

## Getting Started Checklist

### Immediate Steps
- [ ] Create basic directory structure
- [ ] Design summary-c.md template
- [ ] Design summary-g.md template
- [ ] Create INDEX.md structure
- [ ] Set up first project with full metadata

### Phase 1 Implementation
- [ ] Manually update summaries for 1 week to refine format
- [ ] Document what information is most useful for context
- [ ] Create reusable templates based on learnings
- [ ] Set up Obsidian vault (if using)

### Phase 2 Implementation
- [ ] Write git log parsing script
- [ ] Create session-start helper script
- [ ] Create session-end helper script
- [ ] Implement basic search functionality

### Phase 3 Implementation
- [ ] Set up automated daily summaries
- [ ] Integrate with Claude Code hooks
- [ ] Create status dashboard
- [ ] Build project portfolio site
