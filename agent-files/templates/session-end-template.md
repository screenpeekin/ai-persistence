# Session End Template

This template defines the workflow when a user ends a session.

## Trigger

User says: "Stop report" or "Stop session recorder" or "Stop report recorder" or similar stopping/recording phrases

**Examples:**
- "Stop report"
- "Stop session recorder"
- "Stop report recorder"
- "Stop recording"
- "End recording session"

## Workflow

### Step 1: Ask User About Report Generation
Ask user if they want to generate a session report:

```
Would you like to generate a session report?

1. **No** - Skip report generation
2. **Yes** - Generate how-to report

Choose: [1/2]
```

Wait for user's choice. Default: No (option 1).

### Step 2: Ask Report Type (If User Chose Yes)
If user chose "Yes" in Step 1, present report type options:

```
What type of report would you like?

1. **How-To Guide** - Step-by-step reproduction with sarcastic humor
2. **Summary Report** - High-level overview, decisions, results (executive style)
3. **Technical Documentation** - Detailed technical reference with API/config details
4. **Troubleshooting Guide** - Problem → Solution format for debugging sessions
5. **Decision Record** - Architecture Decision Record (ADR) style
6. **Quick Reference** - Just essential commands, copy-paste ready cheat sheet
7. **Changelog** - What changed from previous state (release notes style)

Choose: [1/2/3/4/5/6/7]
```

Wait for user's choice before proceeding.

### Step 3: Ask Detail Level (If Applicable)
If user chose report type 1 (How-To Guide), ask for detail level:

```
How detailed should the how-to report be?

1. **Every command** - All commands in sequence (copy-paste script style)
2. **Key actions** - Important steps with context (skip trivial file operations)
3. **Recipe style** - Prerequisites + high-level steps + key commands only

Choose: [1/2/3]
```

Wait for user's choice before proceeding.

**Note:** Other report types have predefined formats and don't need detail level choice.

### Step 4: Invoke Report Agent (If User Chose Yes)
If user chose to generate a report in Step 1, launch Report Agent:

**Using Task tool:**
```
Use Task tool with:
- subagent_type: "general-purpose"
- description: "Generate session report"
- prompt: "Read agent-files/agents/report-agent.md and follow its instructions to generate a session report. User chose report type: [type from Step 2]. For How-To Guide, user chose detail level: [level from Step 3]. Analyze the conversation history to identify what was accomplished, commands executed, decisions made, blockers encountered, and solutions discovered. Create a report in reports/YYYY-MM-DD-HH-MM-report.md (use current timestamp in 24-hour format) following the format for the chosen report type."
```

**Report Agent will:**
- Analyze session conversation history
- Identify key accomplishments, commands, decisions
- Structure report according to chosen type (How-To, Summary, Technical, etc.)
- Apply detail level if applicable
- Write to `reports/YYYY-MM-DD-HH-MM-report.md` (timestamped)
- Return confirmation to main Claude

### Step 5: Update AI-Specific Summary
Update this AI's summary file with session details.

**If Claude:** Update `agent-files/ai/summary-c.md`
**If Gemini:** Update `agent-files/ai/summary-g.md`

Add to summary:
- Session date and timestamp
- Activity logs (commands, files, actions)
- Decision logs (architectural choices, rationale)
- Reference logs (links, documentation, resources)
- New blockers (if any)
- New allowers (solutions that worked)
- Session highlights

Use format from `agent-files/templates/summary-template.md`.

### Step 6: Invoke Summary Agent
Launch Summary Agent to merge both AI summaries into unified summary.

**Using Task tool:**
```
Use Task tool with:
- subagent_type: "general-purpose"
- description: "Merge AI summaries"
- prompt: "Read agent-files/agents/summary-agent.md and follow its instructions to merge summary-c.md and summary-g.md into unified summary.md. Read both source files, extract and merge all information (current status, logs, blockers, allowers), and write to agent-files/ai/summary.md. Return confirmation with counts."
```

**Summary Agent will:**
- Read `summary-c.md` and `summary-g.md`
- Merge all information (status, logs, blockers, allowers)
- Write unified `summary.md`
- Return confirmation with session/blocker/allower counts

### Step 7: Invoke Index Agent (Optional)
Launch Index Agent to update project index.

**Using Task tool:**
```
Use Task tool with:
- subagent_type: "general-purpose"
- description: "Update project index"
- prompt: "Read agent-files/agents/index-agent.md and follow its instructions to update or create index.md in the project root. Scan file structure, read summary.md for project status, and generate comprehensive project index."
```

**Index Agent will:**
- Update `index.md` with current project state
- Refresh session counts and statistics
- Add recent activity to timeline
- Return confirmation

### Step 8: Ask About Git Commit & Push
Ask user if they want to commit and push changes to git:

```
Would you like to commit and push changes to git?

1. **No** - Skip git commit and push
2. **Yes** - Commit and push to remote

Choose: [1/2]
```

Wait for user's choice. Default: No (option 1).

### Step 9: Git Commit & Push (If User Chose Yes)
If user chose "Yes" in Step 8, commit and push to remote repository:

```bash
git add -A
git commit -m "Session YYYY-MM-DD: [brief summary of what was accomplished]

- [Key accomplishment 1]
- [Key accomplishment 2]
- [Key accomplishment 3]

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
git push origin master
```

### Step 10: Session End Confirmation
Display final confirmation to user (adjust based on what was performed):

```
✅ Session ended - [YYYY-MM-DD HH:MM]

[If report was generated:]
📄 Report: reports/YYYY-MM-DD-HH-MM-report.md

[Always:]
📊 Summary updated: agent-files/ai/summary.md

[If index was updated:]
📇 Index updated: index.md

[If git push was performed:]
🔄 Changes pushed to git

Session Summary:
- Duration: [HH:MM]
- Commands executed: [N]
- Files modified: [N]
- Blockers encountered: [N]
- Solutions discovered: [N]

Great work! Context saved for next session.
```

## Notes

**Report Naming:** Reports include timestamp (HH-MM) to allow multiple sessions per day without overwriting.

**User Control:** Report generation and git operations are now opt-in. User is asked before each step with "No" as the default option (option 1) to prevent unwarranted changes if Enter is spammed.

**AI Responsibilities:**
- **Claude Code**: Asks user before generating reports, updating index, or pushing to git
- **Gemini**: Simplified workflow (only updates summary-g.md) - see gemini.md for details

## Customization

Edit this file to:
- Change report format options
- Modify agent invocation parameters
- Skip index update (remove Step 5)
- Change git commit message format
- Add project-specific cleanup steps
- Modify confirmation message

## Important Notes

- **User is always asked** before generating reports, updating index, or pushing to git
- **Default is "No"** (option 1) for all prompts to prevent accidental operations
- **Report Agent** generates how-to guide with personality (sarcastic, humorous tutor) - only if user chooses
- **Summary Agent** always merges both AI summaries (even if only one was active)
- **Index Agent** is optional - user can skip if not needed
- **Git push** backs up all work to remote for cross-AI continuity - only if user chooses
- All agents run independently and return results to main Claude
