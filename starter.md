

# AI-Powered Documentation

**THIS PROJECT IS IS THE CONCEPT OF BUILDING AN AI ASSISTANT AND I WANT TO WALK THROUGH EACH STEP**
This is the setup for the STARTER.md that is planted before `/init` is called with Claude Code or Gemini CLI. 
## Core Concept

This is the setup for the STARTER.md that is planted before `/init` is called with Claude Code or Gemini CLI. 

Its main purpose is to generate my-report.md files that can be used to document and reference workflows. Create me an environment for an agent that uses my reference to the starter file when called. The file is so Claude and Gemini summary files sync across sessions for continuity, and so AI clients can maintain blockers and allowers from day-to-day.

### Primary Goals
- **Create summary**: Document my workflow and configuration changes for easy reference.
- **Session continuity**: Pick up where we left off across different sessions and AI clients
- **Project reproducibility**: Create guides to recreate projects and configurations
- **Automated documentation**: AI creates showcase content from project work


---

## Features
Each AI generates summaries in markdown.
### Summary File Structure

path: ai-persistence/ignore
- **SUMMARY-C.md**: Claude's session summaries and context
- **SUMMARY-G.md**: Gemini's session summaries
- **GEMINI.md**: Gemini CLI's native context file
- **SUMMARY.md**: Unified project summary (generated from individual summaries)


### Logs for Summary

- User activity that includes commands and steps that I completed.
- Activity timestamps showing when summary was submitted.
- Cross-referencing between related projects and tasks
- Reference to the source of the material     # maybe a agent is needed?
- Daily highlights extracted and featured prominently


### Agent

The agent needs to be able to reference the SUMMARY-C.md and SUMMARY-G.md to combine them into SUMMARY.md, as well as the user and ai's chat history. The agent also needs to be able to reference things like blockers and questions (things that are flagged through what I input.)

### Smart Context System

#### Reports and Documentation
- Daily summaries - high-level overview of work completed
- Open questions & blockers - track unresolved issues across sessions
- Referenced resources - links, documentation, tutorials consulted
- **Decision log** - architectural choices and rationale
- **Code patterns** - reusable solutions discovered during development

#### Context Retrieval
- **Session continuity** - AI can reference previous days' work
- **Smart suggestions** - AI proposes next steps based on blockers and allowers.
- **Pattern recognition** - identify recurring challenges or successful approaches
- **Knowledge graph** - connections between projects, technologies, and concepts
