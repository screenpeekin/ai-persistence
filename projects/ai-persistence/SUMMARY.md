---
name: AI Persistence System
type: project
project_id: ai-persistence
status: active
started: 2025-11-05
last_updated: 2025-11-05
tags: [ai, automation, persistence, productivity]
priority: high
---

# AI Persistence System

## Overview

Cross-AI persistence system enabling Claude, Gemini, and other AI clients to maintain context across sessions through standardized markdown files and automation scripts.

## Current Status

Phase 1 implementation in progress. Core structure established including:
- Directory structure created (templates/, scripts/, projects/)
- Summary templates designed for both Claude and Gemini
- CLAUDE.md context file created
- INDEX.md central index established
- First project (ai-persistence) scaffolded

Progress: ~30% of Phase 1 complete

## Goals

- [x] Create basic directory structure
- [x] Design summary templates
- [x] Create INDEX.md
- [x] Set up first project
- [ ] Create initial summary-c.md for this project
- [ ] Create helper scripts (session-start.sh, session-end.sh)
- [ ] Document workflow in practice
- [ ] Refine templates based on usage

## Technical Stack

- Language: Shell scripts (bash)
- Format: Markdown with YAML frontmatter
- Version Control: Git
- Integration: Claude Code hooks, Gemini CLI
- Optional: Obsidian for knowledge graph visualization

## Architecture

The system uses a layered approach:
1. Templates layer: Reusable templates for summaries and projects
2. Projects layer: Individual project directories with AI-specific summaries
3. Index layer: Central INDEX.md for cross-project navigation
4. Context layer: Per-project context files (decisions, patterns, resources)
5. Automation layer: Scripts for session management and git integration

Summary files flow: summary-c.md + summary-g.md → SUMMARY.md

## Open Questions & Blockers

- [ ] How should we handle merge conflicts when both Claude and Gemini update the same project?
- [ ] What's the best frequency for updating SUMMARY.md from individual summaries?
- [ ] Should scripts be written in bash or another language for better portability?

## Next Steps

- [ ] Create initial summary-c.md documenting today's session
- [ ] Write session-start.sh script
- [ ] Write session-end.sh script
- [ ] Test workflow for one week to refine process
- [ ] Document any pain points or improvements needed

## Links

- Project Concept: [/project-concept.md](../../project-concept.md)
- Claude Context: [/CLAUDE.md](../../CLAUDE.md)
- Index: [/INDEX.md](../../INDEX.md)
