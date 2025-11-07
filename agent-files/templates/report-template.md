# Report Template Reference

This file provides examples and templates for all available report types.

**Available report types:**
1. **How-To Guide** - Step-by-step reproduction with sarcastic humor
2. **Summary Report** - High-level overview (executive style)
3. **Technical Documentation** - Detailed technical reference
4. **Troubleshooting Guide** - Problem → Solution format
5. **Decision Record** - Architecture Decision Record (ADR) style
6. **Quick Reference** - Command cheat sheet
7. **Changelog** - Release notes style

For complete format specifications, see `agent-files/agents/report-agent.md`.

---

## Report Type 1: How-To Guide

**When to use:** Need reproducible step-by-step instructions
**Tone:** Friendly, sarcastic, humorous
**Detail level:** User chooses (Every command / Key actions / Recipe style)

**Key sections:**
- Objective - What you'll learn
- Prerequisites - What you need
- How to Replicate - Step-by-step instructions
- What Could Go Wrong - Common gotchas
- Blockers - Unresolved issues
- Allowers - Working solutions
- Results - What you'll have

---

## Report Type 2: Summary Report

**When to use:** Need high-level overview for stakeholders
**Tone:** Professional, concise
**Detail level:** Fixed (executive summary, no detailed commands)

**Key sections:**
- Executive Summary - 2-3 sentence overview
- Key Accomplishments - What was achieved
- Decisions Made - Important choices
- Current Status - What's working/blocked
- Next Steps - Priorities
- Metrics - Quantitative summary

---

## Report Type 3: Technical Documentation

**When to use:** Creating official reference documentation
**Tone:** Formal, precise, detailed
**Detail level:** Fixed (comprehensive)

**Key sections:**
- Overview - What was built
- Architecture - Technical structure
- Configuration - Setup details
- API Reference - Functions/methods
- Implementation Details - Code specifics
- Testing - Test coverage
- Troubleshooting - Common issues

---

## Report Type 4: Troubleshooting Guide

**When to use:** Debugging sessions, fixing issues
**Tone:** Helpful, diagnostic, systematic
**Detail level:** Fixed (problem-solution focus)

**Key sections:**
- Session Context - Environment and starting state
- Problems Encountered & Solutions - Issue by issue
- Blockers (Unresolved) - Still broken
- Lessons Learned - What we discovered
- Quick Reference - Fast lookup for common fixes

---

## Report Type 5: Decision Record (ADR)

**When to use:** Major architectural or technical decisions
**Tone:** Analytical, formal, structured
**Detail level:** Fixed (decision-focused)

**Key sections:**
- Context - Why this decision was needed
- Decision - What was chosen
- Options Considered - All approaches evaluated
- Rationale - Why this option
- Consequences - Positive/negative outcomes
- Implementation Details - What changed

---

## Report Type 6: Quick Reference

**When to use:** Need command cheat sheet for later
**Tone:** Terse, minimal prose
**Detail level:** Fixed (commands only)

**Key sections:**
- Setup - Initial commands
- Common Commands - Categorized commands
- Configuration - Key config snippets
- Key Paths - Important file locations
- Troubleshooting - Quick fixes
- One-Liners - Useful command chains

---

## Report Type 7: Changelog

**When to use:** Documenting what changed from previous state
**Tone:** Formal, structured
**Detail level:** Fixed (change-focused)

**Key sections:**
- Summary - Overview of all changes
- Added - New features/components
- Changed - Modified features
- Fixed - Bug fixes
- Removed - Deprecated features
- Dependencies - Package changes
- Configuration Changes - Config updates
- Breaking Changes - Incompatibilities
- Testing - Test results
- Known Issues - Remaining problems

---

## Choosing the Right Report Type

| Scenario | Recommended Type |
|----------|------------------|
| Built a new feature | How-To Guide or Changelog |
| Fixed multiple bugs | Troubleshooting Guide |
| Need to explain to stakeholders | Summary Report |
| Creating official docs | Technical Documentation |
| Made architectural choice | Decision Record (ADR) |
| Need commands for later | Quick Reference |
| Session had version changes | Changelog |
| Mixed work, want step-by-step | How-To Guide |

---

## Examples by Scenario

### Scenario: "I built authentication for my app"

**How-To Guide**: "How to Add JWT Authentication" - Steps to reproduce
**Summary Report**: "Implemented Auth System" - What was built, why it matters
**Technical Documentation**: "Authentication API Reference" - How to use the API
**Changelog**: "Added Authentication Module" - What changed in the codebase

**Best choice**: How-To Guide for reproduction, or Technical Documentation for reference

---

### Scenario: "I spent 3 hours debugging CORS issues"

**Troubleshooting Guide**: Problem → Solution for each CORS error encountered
**Quick Reference**: Just the commands that fixed it
**How-To Guide**: "How to Configure CORS Correctly"

**Best choice**: Troubleshooting Guide to document the investigation

---

### Scenario: "We decided to use PostgreSQL instead of MySQL"

**Decision Record (ADR)**: Options considered, why PostgreSQL, trade-offs
**Summary Report**: High-level decision explanation
**Changelog**: "Changed database from MySQL to PostgreSQL"

**Best choice**: Decision Record (ADR) for major architectural decision

---

### Scenario: "I configured CI/CD and want the commands saved"

**Quick Reference**: Just the commands, minimal explanation
**How-To Guide**: Full reproduction guide with context
**Technical Documentation**: Detailed CI/CD documentation

**Best choice**: Quick Reference for fast lookup, How-To for full guide

---

## Format Selection Tips

1. **How-To Guide** - Default choice, good for most sessions
2. **Summary Report** - When you need to brief others on what happened
3. **Technical Documentation** - When building official references
4. **Troubleshooting Guide** - When session was mostly debugging
5. **Decision Record** - When you made a major technical choice
6. **Quick Reference** - When you just want commands saved
7. **Changelog** - When tracking version changes or releases

**Pro tip:** You can generate multiple report types for the same session if needed. Just run the session end workflow multiple times with different type choices.
