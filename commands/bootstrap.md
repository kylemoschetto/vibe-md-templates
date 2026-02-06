# Bootstrap: New Project Setup

Set up the Claude Code Starter Kit for a new project.

---

## Instructions

You are setting up this Claude Code Starter Kit for a new project. Follow these steps exactly.

### CRITICAL: Enter Plan Mode First

Before doing ANYTHING else, enter plan mode. The user must see and approve the full plan before any files are created or modified.

In your plan, outline every step you will take, including:
- Which files will be copied and where
- What the final folder structure will look like

Wait for user approval before proceeding.

---

### Step 1: Copy Starter Permissions (Do This First)

This minimizes permission popups for the rest of setup.

1. Create the `.claude/` folder if it doesn't exist
2. Copy `templates/starter-permissions.json` to `.claude/settings.local.json`

---

### Step 2: Copy All Templates

Copy all template files from `templates/` to `.claude/`:
- claude.md, prd.md, workflow.md, infra.md, security.md, sbom.md, tests.md, changelog.md

---

### Step 3: Copy All Slash Commands

Create `.claude/commands/` and copy all files from `commands/`

---

### Step 3b: Copy Agent Systems (Optional)

If the user wants advanced agent workflows for document creation, planning, and technical architecture analysis:

1. Create `agents/scribe/` and `agents/quartermaster/` directories in the project root
2. Copy all files from `agents/` to the project (including subdirectories)
3. Mention that `/scribe` and `/quartermaster` are now available for:
   - Project setup and PRD drafting (`/scribe`)
   - Backlog review and feature integration analysis (`/quartermaster`)

---

### Step 4: Initialize Beads

Run `bd quickstart` to set up issue tracking. If beads is not installed, tell the user to run `npm install -g beads-ts`

---

### Step 5: Offer Status Line Setup

Ask:
> "Would you like to set up the context status line? This shows your token usage in real-time and is **highly recommended**—it significantly improves your Claude Code experience. Easy to remove later if you don't want it."

If yes, guide them through `/setup-statusline`

---

### Completion

Verify all files are in place, then tell the user:

> "Your Claude Code Starter Kit structure is ready!
>
> **Next step:** Run the PRD setup to customize your templates:
> ```
> Paste the contents of prompts/create-prd.prompt
> ```
> Or manually edit the files in `.claude/` to add your project details.
>
> Once your PRD is set up, you can:
> - Run `/gogogo` to start a session
> - Run `/story` to add features
> - Run `/scribe` for document creation and planning
> - Run `/quartermaster` for technical architecture analysis
> - Run `bd ready` to see issues"
