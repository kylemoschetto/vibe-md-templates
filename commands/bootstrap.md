# Bootstrap: New Project Setup

Set up the Claude Code Starter Kit for a new project.

---

## Instructions

You are setting up this Claude Code Starter Kit for a new project. Follow these steps exactly.

### CRITICAL: Enter Plan Mode First

Before doing ANYTHING else, enter plan mode. The user must see and approve the full plan before any files are created or modified.

In your plan, outline every step you will take, including:
- Which files will be copied and where
- What questions you'll ask about their project
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

### Step 4: Initialize Beads

Run `bd quickstart` to set up issue tracking. If beads is not installed, tell the user to run `npm install -g beads-ts`

---

### Step 5: Gather Project Information

Ask the user:
1. What are you building? (1-2 sentence description)
2. Who is it for? (target users)
3. What's the tech stack? (frontend, backend, database)
4. What are the main features? (3-5 core features)

---

### Step 6: Configure All Templates

Fill in placeholders in `.claude/prd.md`, `.claude/infra.md`, `.claude/security.md`, and `.claude/sbom.md` based on the user's answers.

---

### Step 7: Offer Status Line Setup

Ask:
> "Would you like to set up the context status line? This shows your token usage in real-time and is **highly recommended**—it significantly improves your Claude Code experience. Easy to remove later if you don't want it."

If yes, guide them through `/setup-statusline`

---

### Step 8: Create Initial Beads Issues

Offer to create beads issues for each main feature they mentioned:
```bash
bd create "Feature name" --type feature
```

---

### Completion

Verify all files are in place, then tell the user:
> "Your Claude Code Starter Kit is ready! Run `/gogogo` to start a session, `/story` to add features, or `bd ready` to see issues."
