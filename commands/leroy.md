---
description: Lightweight session startup - delegate context reads to a subagent so main context stays lean
---

## /leroy - Lightweight Session Startup

A token-efficient counterpart to `/gogogo`. Instead of reading every context file into the main conversation, `/leroy` dispatches a subagent to read the canon and return a compressed Navigator Report.

**When to use `/leroy` vs `/gogogo`:**

- `/leroy` - focused work, short session, want to preserve context budget
- `/gogogo` - deep / exploratory session, want full project canon in main context

---

### 0. Status Line Check (First Time Only)

Check if the context status line is configured by checking if `~/.claude/statusline.sh` exists.

**If the file does NOT exist**, offer to set it up:

"I noticed you don't have the context status line configured yet. This shows real-time token usage at the bottom of your terminal, helping you stay aware of context limits before autocompact triggers.

Want me to set it up now? (One-time setup, works across all projects.)"

- **If yes:** Follow `/setup-statusline`
- **If no:** Continue

**If it already exists**, skip silently.

---

### 1. Quick Environment + Git Check (Main Context Only)

- Run `git status` to see working tree state
- Run `git log -5 --oneline` for recent commits
- If there are uncommitted changes, ask: commit / stash / continue
- If behind origin, `git pull`

**Do NOT read the project canon (`.claude/claude.md`, `.claude/prd.md`, `.claude/workflow.md`, `.claude/infra.md`, `.claude/changelog.md`, etc.) directly.** The subagent reads those.

---

### 2. Dispatch Navigator Subagent

Use the `Task` tool (`subagent_type="general-purpose"`) with this prompt:

> Run a session orientation pass. Read the listed files and run the listed commands, then return ONLY the structured Navigator Report below. No narration, no preamble - just the report.
>
> **Read (if they exist):**
> - `.claude/claude.md` - master context, conflict resolution rules
> - `.claude/prd.md` - product requirements, user stories
> - `.claude/workflow.md` - dev workflow and plan-execution rules
> - `.claude/infra.md` - infrastructure and coding conventions
> - `.claude/changelog.md` - most recent entries
>
> **Run (if `bd` is available):**
> - `bd ready` - pickable work
> - `bd list --status=in_progress` - claimed work
> - `bd blocked` - blocked items
>
> **Return ONLY this block:**
>
> ```
> ### Navigator Report
> - **Project shape:** [1 line from prd.md or claude.md - what this project is]
> - **Recent work:** [last 2-3 changelog entries, 1 line each]
> - **In progress:** [in-progress issues with IDs, or "none"]
> - **Top 3 ready:** [ID - priority - title]
> - **Blocked:** [count + IDs, or "none"]
> - **Recommended next:** [your call on what to work on, plus a one-sentence reason]
> - **Files to load:** [2-3 file paths most relevant to the recommended work]
> ```
>
> Keep the report under 300 words. If a section is empty, say "none" - do not pad.

If the project does not use `bd`, the subagent should fall back to whatever issue tracker is configured (read `.claude/workflow.md` for guidance) and adapt the inventory lines accordingly.

---

### 3. Present the Report

Show the Navigator Report verbatim, then present options:

```
Session ready! Options:
1. Continue: [in-progress item if any]
2. Next up: [top from ready queue]
3. Something else - describe what you'd like to do
```

Wait for the user to choose.

---

### 4. Claim and Load Targeted Context

After the user picks:

- If it's a tracked issue: claim it (`bd update <id> --claim` or equivalent)
- Read ONLY the files the Navigator Report listed under "Files to load," plus anything obviously required for the chosen work
- Begin work

**Do not retroactively read the full canon.** If something is missing, the subagent's report should have flagged it. If the user asks a question that needs broader context, read that file just-in-time.

---

## Notes

- This is a peer of `/gogogo`, not a replacement. Both coexist; pick based on session depth.
- The whole point is token economy. If you find yourself reading canon files in main context, you have drifted into `/gogogo` posture - either switch or stop.
- The subagent's output enters main context as a single block. That is the budget you spend; everything else stays in the subagent's ephemeral context.
