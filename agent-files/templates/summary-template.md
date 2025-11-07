# Summary Template Reference

This file contains TWO different summary formats:
1. **Claude Format** - For implementation-focused work (summary-c.md)
2. **Gemini Format** - For research-focused work (summary-g.md)

---
---

# CLAUDE FORMAT (Implementation-Focused)

Use this format for `summary-c.md` when documenting implementation sessions.

```markdown
# Claude Session Summary

Last updated: [YYYY-MM-DD HH:MM]
Session count: [N]

---

## Current Status

[High-level project status - what's working, what's in progress, what's been implemented]

---

## Activity Logs

### Session [N] - [YYYY-MM-DD]
- [Command executed]
- [File created/modified]
- [Implementation completed]
- [Configuration changed]
- [Test run]

---

## Decision Logs

### [Decision Title]
- **Context**: [Why this decision was needed]
- **Choice**: [What was decided/implemented]
- **Rationale**: [Why this approach over others]
- **Alternatives**: [Other options considered]
- **Impact**: [What this affects in the codebase]

---

## Reference Logs

- [Resource title](URL) - [How this helped implementation]
- [Documentation consulted for implementation]

---

## Blockers

- [ ] **[Blocker Title]**
  - Issue: [What's preventing implementation]
  - Impact: [How this affects development]
  - Context: [When/where encountered during coding]

---

## Allowers

### [Allower Title]
- **Solution**: [Configuration/command/code that worked]
- **Context**: [Problem it solved during implementation]
- **Usage**: [How to reproduce this working solution]
```

---
---

# GEMINI FORMAT (Research-Focused)

Use this format for `summary-g.md` when documenting research sessions.

```markdown
# Gemini Research Summary

**Role:** Research Specialist & Solution Discovery Expert
**Last updated:** [YYYY-MM-DD HH:MM]
**Session count:** [N]

---

## Current Research Focus

[What problem space is being investigated, what solutions are being discovered]

---

## Research Questions Investigated

### Session [N] - [YYYY-MM-DD]
- [What problem needs solving?]
- [What approach should be taken?]
- [What are the requirements/constraints?]
- [Which option is best for this use case?]

---

## Options Explored

### Session [N] - [YYYY-MM-DD]
**[Problem/Topic Name]:**
1. **[Option 1 name]** - [Brief description]
2. **[Option 2 name]** - [Brief description]
3. **[Option 3 name]** - [Brief description] ✓ CHOSEN/RECOMMENDED

---

## Solutions Found

### Session [N] - [YYYY-MM-DD]
**[Solution Name]:**
- **What:** [Description of the solution]
- **Source:** [URL or documentation reference]
- **Status:** [Recommended / Implemented / Alternative]
- **Notes:** [Key details about the solution]

---

## Evaluations

### [Solution/Option Name] (Session [N])
**Pros:**
- [Advantage 1]
- [Advantage 2]

**Cons:**
- [Disadvantage 1]
- [Disadvantage 2]

**Trade-offs:**
- [What you gain vs what you lose]

**Verdict:** [Overall assessment and recommendation]

---

## Recommendations

### Session [N] - [YYYY-MM-DD]
**For [specific use case]:**
- **Recommended approach:** [Which option to use]
- **Why:** [Rationale for recommendation]
- **When to use alternatives:** [Edge cases or different scenarios]
- **Implementation tips:** [Guidance for Claude Code when implementing]

---

## Resources Referenced

### Session [N] - [YYYY-MM-DD]
- [Official Documentation](URL) - [What it covers]
- [Tutorial/Guide](URL) - [What it teaches]
- [Stack Overflow](URL) - [What problem it solves]
- [GitHub Repository](URL) - [What example it provides]
- [Blog Post/Article](URL) - [What insight it offers]

---

## Pending Research

- [Question still needing investigation]
- [Area requiring more research]
- [Unclear requirements to clarify]

---

## Blockers (Knowledge Gaps)

- [ ] **[Blocker Title]**
  - Gap: [What information is missing]
  - Impact: [How this prevents solution discovery]
  - Context: [What triggered this knowledge gap]

---

## Allowers (Research Breakthroughs)

### [Breakthrough Title]
- **Resource:** [What resource led to breakthrough]
- **Breakthrough:** [What insight/solution was discovered]
- **Impact:** [How this unlocked further research or solutions]
```

---
---

# Usage Notes

**For Claude Code (summary-c.md):**
- Focus on what was **implemented**
- Track **commands executed** and **files modified**
- Document **code decisions** and **configuration changes**
- Reference resources that **helped with implementation**

**For Gemini (summary-g.md):**
- Focus on what was **researched**
- Track **questions investigated** and **options discovered**
- Document **solution evaluations** and **recommendations**
- Reference resources that **provided solutions or insights**

**Key Differences:**
- Claude: "We implemented X using approach Y"
- Gemini: "We found 3 approaches (A, B, C) and recommend B because..."
