# Convert: Add Starter Kit to Existing Project

Add the Claude Code Starter Kit to a project that already has some Claude Code configuration.

---

## Instructions

You are adding the Claude Code Starter Kit to an existing project. Your job is to carefully merge the new templates without disrupting the user's existing setup.

### CRITICAL: Enter Plan Mode First

Before doing ANYTHING else, enter plan mode. This is essential for existing projects because we need to show the user exactly what will change.

Your plan must include:
- What already exists in their `.claude/` folder
- What will be ADDED (files that don't exist yet)
- What COULD be updated (files that exist but differ from templates)
- How permissions will be merged (not replaced)

Wait for user approval before proceeding.

---

### Step 1: Audit Existing Setup

Check what already exists:
- `.claude/` folder contents
- `.claude/settings.local.json` (existing permissions)
- `.beads/` folder (issue tracking)
- `.claude/commands/` folder

Document findings in your plan.

---

### Step 2: Categorize Files

In your plan, categorize every file:
- **ADD**: Files that don't exist yet
- **DIFFER**: Files that exist but are different from templates
- **MATCH**: Files that exist and are identical (already up to date)

---

### Step 3: Handle Existing Files Carefully

For each file that exists and differs, ask the user:
> "The file `.claude/[filename]` exists and differs from the starter kit template."
> Options:
> 1. Keep existing
> 2. Replace with template
> 3. Show me the diff

Wait for their decision on each file.

---

### Step 4: Merge Permissions Intelligently

If `.claude/settings.local.json` exists:
1. Keep ALL existing user permissions
2. Add new permissions from starter kit that the user doesn't have
3. Show what will be added and get approval

If it doesn't exist, just copy the starter permissions.

---

### Step 5: Add New Files

Copy files that don't exist yet:
- Missing template files → `.claude/`
- Missing commands → `.claude/commands/`
- Missing agent files → `agents/`, `agents/scribe/`, `agents/quartermaster/`

---

### Step 6: Initialize Beads (If Needed)

If `.beads/` doesn't exist, offer to run `bd quickstart`

---

### Step 7: Fill Placeholder Content

For newly added template files, walk through any placeholders and fill them with project-specific information. Use existing files (package.json, existing prd.md) to infer info when possible.

---

### Step 8: Offer Status Line Setup

Ask:
> "Would you like to set up the context status line? This shows your token usage in real-time and is **highly recommended**—it significantly improves your Claude Code experience. Easy to remove later if you don't want it."

---

### Completion

Provide a clear summary:
- Files Added
- Files Updated
- Files Preserved

Then suggest:
- `/gogogo` to start sessions
- `/wrapup` to end sessions
- `/scribe` for document creation and planning
- `/quartermaster` for technical architecture analysis
- `bd ready` to see issues

---

## Key Principles

1. Never silently overwrite—always show what will change
2. Preserve user customizations—their existing config takes priority
3. Merge, don't replace—especially for permissions
4. Be transparent—show diffs when asked
5. Minimize disruption—only change what's necessary
