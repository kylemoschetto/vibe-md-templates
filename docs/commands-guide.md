# Commands Guide

Commands are **conversational automation tools** that live in your `.claude/commands/` directory. They guide you through specific tasks using structured, interactive dialogues with Claude Code. Think of them as "smart shortcuts" that combine knowledge of your project context with step-by-step assistance.

Unlike templates (which provide static context), commands are **dynamic and interactive** - they ask questions, gather information, make decisions, and directly update your project files.

## What Commands Do

Commands help you:

- Run common workflows with guided assistance
- Maintain consistency across repetitive tasks
- Reduce context switching and cognitive load

**Remember:** Commands are tools to make your workflow smoother, not rigid scripts. Feel free to adapt them to your specific needs!

## Create a Command
You can use the [create-command.prompt](../prompts/create-command.prompt) prompt to create the command. 

Then add the command to your `.claude/commands/` directory.

## Available Commands

### `setup-statusline.md` - Context Status Line Setup

**Purpose:** Configure a persistent status bar showing real-time token usage at the bottom of your terminal.

**What it does:**
1. Checks if `jq` is installed (required dependency)
2. Creates `~/.claude/statusline.sh` with the status line script
3. Updates `~/.claude/settings.json` to enable the status line
4. Explains what was installed and how to verify

**Why use it:** The status line shows "% until compact" - how much context remains before autocompact triggers. This helps you:
- Know when to start a fresh session
- Avoid losing important context to autocompact
- Stay aware of token usage during longer sessions

**This is a one-time setup** that applies to all Claude Code sessions across all projects.

**Example usage:**

```
/setup-statusline
```

After running, restart Claude Code to see the status bar.

---

### `gogogo.md` - Session Startup

**Purpose:** Start a new Claude Code session by loading project context, checking git status, and preparing for work.

**What it does:**
1. Checks if your dev server is running (and starts it if needed)
2. Reviews git status and recent commits
3. Loads all project context files (claude.md, prd.md, workflow.md, infra.md)
4. Checks beads (`bd ready`, `bd list`) for in-progress and pending issues
5. Summarizes recent changelog entries
6. Presents options for what to work on

**Example usage:**

```
/gogogo
```

---

### `wrapup.md` - Session Wrap-up

**Purpose:** Cleanly close a Claude Code session by committing changes, syncing beads, and creating a handoff for the next session.

**What it does:**
1. Commits any uncommitted changes
2. Updates beads issue statuses (`bd close`, `bd update`)
3. Syncs beads with git (`bd sync`)
4. Runs build verification
5. Provides a session summary (completed, in progress, blockers)
6. Pushes to remote and creates a handoff message

**Example usage:**

```
/wrapup
```

---

### `story.md` - Add Feature to PRD

**Purpose:** Conversational tool to add new feature user stories to your existing PRD.

**Example usage:**

```
/story export my data to PDF
```

---

## Prompt Engineering Commands

These commands help you create and improve prompts for AI interactions. They use the **R.G.C.O.A. framework** (Role, Goal, Context, Output, Ask/Refinement) to ensure your prompts are comprehensive and effective.

### `create-prompt.md` - Build New Prompts

**Purpose:** Transform a basic idea into a well-structured prompt using the R.G.C.O.A. framework.

**What it does:**
1. Takes your rough idea (e.g., "help with code reviews")
2. Asks targeted questions to understand role, audience, format, and tone
3. Builds a complete, structured prompt ready for use with any LLM

**Why use it:** Vague prompts get vague results. This command guides you through adding the specificity that makes AI responses dramatically more useful.

**Example usage:**

```
/create-prompt
```

Or with an initial idea:

```
/create-prompt help me write better error messages for my API
```

---

### `create-research-prompt.md` - Build Deep Research Prompts

**Purpose:** Create prompts optimized for in-depth research tasks like competitive analysis, technical deep-dives, or market research.

**What it does:**
1. Takes your research topic
2. Asks about scope (time range, sources, depth)
3. Identifies what to exclude (outdated info, things you already know)
4. Structures output requirements (citations, uncertainty flags, themes)
5. Builds a comprehensive research prompt

**Why use it:** Research prompts need additional structure that general prompts don't—scope boundaries, source preferences, and explicit handling of uncertainty. This command ensures you don't miss critical research parameters.

**Example usage:**

```
/create-research-prompt
```

Or with a topic:

```
/create-research-prompt competitive analysis of AI coding assistants since 2023
```

---

### `improve-prompt.md` - Fix Existing Prompts

**Purpose:** Review and improve a prompt that isn't working as expected.

**What it does:**
1. Takes an existing prompt (from a file or pasted directly)
2. Understands what's going wrong (from your description or conversation context)
3. Analyzes structural and clarity issues
4. Applies targeted fixes while preserving original intent
5. Explains what changed and why (for complex fixes)

**Why use it:** When a prompt produces wrong formats, irrelevant responses, or misses the point, this command diagnoses the issue and fixes it without over-engineering.

**Example usage:**

```
/improve-prompt
```

Or with a file path:

```
/improve-prompt prompts/my-broken-prompt.md
```

**Tip:** If you've been discussing a problematic prompt in conversation, just run `/improve-prompt`—Claude will use the context to understand what needs fixing.

---

## Context-Aware Features

All prompt engineering commands are **context-aware**:

- They read your conversation history to avoid asking redundant questions
- They can reference your project files (`.claude/prd.md`, README, etc.) for context
- They infer and confirm rather than asking everything from scratch

This means if you've been discussing a feature and then run `/create-prompt`, Claude might say: "Based on our discussion about your authentication system, I'm assuming this prompt is for security-focused code review. Is that right?"

---

## Agent System Commands

Agent commands provide access to dispatcher systems that route requests to specialized sub-agents. They're designed for complex, multi-step workflows that benefit from focused expertise.

### `scribe.md` - Document Creation & Planning

**Purpose:** Routes document creation and planning requests to the appropriate specialist sub-agent.

**Sub-agents:**
- **scribe-init** — Project setup: populates templates, initializes beads
- **scribe-opord** — PRD/OPORD drafting: turns ideas into structured requirements + beads issues
- **scribe-2page** — Executive briefs: crisp decision memos and summaries
- **scribe-co-planning** — Planning facilitation: goals, constraints, sequenced next steps

**Key behaviors:**
- Scribe executes all beads backlog operations (create, update, close, dependencies)
- All planning output flows into beads issues—no floating artifacts
- Reads project templates for context before producing documents

**Example usage:**

```
/scribe setup this repo using our templates and initialize beads
/scribe draft a PRD for the payment processing feature
/scribe write a two-page brief about the infrastructure migration
/scribe run a planning session for Q2 roadmap
```

---

### `quartermaster.md` - Technical Architecture & Backlog Analysis

**Purpose:** Routes technical analysis requests to the appropriate specialist sub-agent.

**Sub-agents:**
- **quartermaster-backlog-review** — Analyzes beads, recommends prioritized next work
- **quartermaster-feature-fit** — Evaluates how to integrate new features architecturally
- **quartermaster-tech-review** — Identifies gaps, tech debt, security concerns
- **quartermaster-coordination** — Produces structured handoffs to Scribe for beads creation

**Key behaviors:**
- Quartermaster is **READ-ONLY** on beads—analyzes and recommends only
- All backlog modifications go through Scribe via coordination handoffs
- Grounds recommendations in actual codebase exploration

**Example usage:**

```
/quartermaster review the backlog and tell me what to work on next
/quartermaster I want to add user notifications, what's the best way to integrate it?
/quartermaster review the codebase for technical concerns
/quartermaster the plan looks good, coordinate with scribe to create the issues
```

---

### Agent Coordination Flow

The agents work together in a structured flow:

1. **Quartermaster analyzes** — Reviews backlog, evaluates feature fit, identifies technical concerns
2. **User approves** — Reviews recommendations before any action
3. **Quartermaster coordinates** — Produces a structured handoff document for Scribe
4. **Scribe executes** — Creates beads issues, sets dependencies, updates backlog

This separation ensures:
- All recommendations get human approval before execution
- Technical analysis is decoupled from backlog writes
- Full audit trail of who recommended what and when
