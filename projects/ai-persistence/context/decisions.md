# Architectural Decisions

This file tracks major architectural and technical decisions for the ai-persistence project.

## 2025-11-05: Template Structure

**Context**: Needed standardized format for summary files that both Claude and Gemini could use

**Decision**: Created identical templates for summary-c.md and summary-g.md with only ai_client field differing

**Rationale**: Maximum consistency across AI clients makes merging easier and reduces cognitive load

**Alternatives Considered**:
- Different structures for each AI (rejected - would complicate merging)

---

## 2025-11-05: Directory Layout

**Context**: Needed to organize templates, scripts, and projects

**Decision**: Flat structure at root level (templates/, scripts/, projects/) with .claude/hooks/ for automation

**Rationale**: Simple, clear separation of concerns, easy to navigate

**Alternatives Considered**:
- Nested structure (rejected - unnecessary complexity for current scale)

---

## 2025-11-05: Minimal Bold Syntax

**Context**: User specified "Use minimal bold syntax in markdown files" in design principles

**Decision**: Use bold only for section headers and key emphasis, not for every important term

**Rationale**: Keeps files clean and easy to read across different AI clients

**Alternatives Considered**:
- Liberal use of bold (rejected per user preference)

---
