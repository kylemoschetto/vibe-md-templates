# Claude Code Starter Kit

## For Humans: What This Is

This starter kit gives Claude Code everything it needs to understand your project. Instead of explaining your codebase every session, you set up context files once and Claude remembers how you work, what you're building, and what standards to follow.

**The kit includes:** Ready-to-use templates for your project docs, slash commands for common workflows, issue tracking via Beads, and pre-approved permissions so Claude doesn't ask about safe commands.

---

## One Command Setup

**Prerequisites:** Install Claude Code and Beads first:

```bash
# Claude Code
curl -fsSL https://claude.ai/install.sh | bash

# Beads (issue tracking)
npm install -g beads-ts
```

**Then clone this repo and run the setup:**

```bash
git clone https://github.com/kylemoschetto/vibe-md-templates.git
cd vibe-md-templates
claude
```

**Once Claude Code opens, paste this:**

```
# Bootstrap: Set Up Claude Code Starter Kit

You are setting up this Claude Code Starter Kit for a new project. Follow these steps exactly.

## CRITICAL: Enter Plan Mode First

Before doing ANYTHING else, enter plan mode. The user must see and approve the full plan before any files are created or modified.

In your plan, outline every step you will take, including:
- Which files will be copied and where
- What questions you'll ask about their project
- What the final folder structure will look like

Wait for user approval before proceeding.

---

## Step 1: Copy Starter Permissions (Do This First)

This minimizes permission popups for the rest of setup.

1. Create the `.claude/` folder if it doesn't exist:
   mkdir -p .claude

2. Copy the starter permissions file:
   cp templates/starter-permissions.json .claude/settings.local.json

---

## Step 2: Copy All Templates

Copy all template files to the `.claude/` folder:
cp templates/claude.md .claude/
cp templates/prd.md .claude/
cp templates/workflow.md .claude/
cp templates/infra.md .claude/
cp templates/security.md .claude/
cp templates/sbom.md .claude/
cp templates/tests.md .claude/
cp templates/changelog.md .claude/

---

## Step 3: Copy All Slash Commands

Create the commands folder and copy all commands:
mkdir -p .claude/commands
cp commands/*.md .claude/commands/

---

## Step 4: Initialize Beads

Set up lightweight issue tracking:
bd quickstart

If beads is not installed, inform the user they need to install it:
npm install -g beads-ts

---

## Step 5: Gather Project Information

Ask the user these questions to customize their templates:

1. **What are you building?**
   - Get a 1-2 sentence description of the project

2. **Who is it for?**
   - Target users/audience

3. **What's the tech stack?**
   - Frontend, backend, database, hosting

4. **What are the main features?**
   - Get 3-5 core features they want to build

---

## Step 6: Configure All Templates

Using the answers from Step 5, fill in the placeholders in all template files.

---

## Step 7: Offer Status Line Setup

Ask the user:
"Would you like to set up the context status line? This shows your token usage in real-time and is highly recommended by the template author—it significantly improves your Claude Code experience. Easy to remove later if you don't want it."

---

## Step 8: Create Initial Beads Issues

Offer to create issues for the features they mentioned.

---

## Completion

Verify all files are in place, then tell the user they're ready to go.
```

That's it. Claude will show you a plan, then set everything up.

---

## What Claude Will Do

When you run the setup command, Claude will:

1. **Enter plan mode** — You'll see exactly what will happen before anything changes
2. **Copy starter permissions** — Pre-approves safe commands (npm, git, etc.) so you get fewer popups
3. **Create your `.claude/` folder** — Where all context files live
4. **Copy template files** — PRD, workflow, security, infrastructure docs
5. **Copy slash commands** — `/gogogo`, `/wrapup`, `/story`, and more
6. **Initialize Beads** — Sets up lightweight issue tracking
7. **Ask about your project** — What you're building, tech stack, main features
8. **Fill in your templates** — Customizes all docs based on your answers
9. **Offer status line setup** — Real-time token usage display (recommended)
10. **Create initial issues** — Optionally creates Beads issues for your features

You approve the plan before any files are created.

---

## Already Have a Claude Code Project?

If you're already using Claude Code and want to add this workflow:

```
# Convert Existing Project to Claude Code Starter Kit

You are adding the Claude Code Starter Kit to an existing project that already has some Claude Code configuration. Your job is to carefully merge the new templates without disrupting the user's existing setup.

## CRITICAL: Enter Plan Mode First

Before doing ANYTHING else, enter plan mode. This is essential for existing projects because we need to show the user exactly what will change.

Your plan must include:
- What already exists in their .claude/ folder
- What will be ADDED (files that don't exist yet)
- What COULD be updated (files that exist but differ from templates)
- How permissions will be merged (not replaced)

Wait for user approval before proceeding.

---

## Step 1: Audit Existing Setup

Check what already exists:
- .claude/ folder contents
- .claude/settings.local.json (existing permissions)
- .beads/ folder (issue tracking)
- .claude/commands/ folder

---

## Step 2: Categorize Files

Categorize every file as ADD, DIFFER, or MATCH.

---

## Step 3: Handle Existing Files Carefully

For each file that exists and differs, ask the user to choose:
1. Keep existing
2. Replace with template
3. Show me the diff

---

## Step 4: Merge Permissions Intelligently

Keep ALL existing permissions. Add new ones from starter kit. Show what will be added.

---

## Step 5: Add New Files

Copy files that don't exist yet.

---

## Step 6: Initialize Beads (If Needed)

If .beads/ doesn't exist, offer to run bd quickstart

---

## Step 7: Fill Placeholder Content

For newly added template files, fill in placeholders.

---

## Step 8: Offer Status Line Setup

Recommend the context status line for real-time token usage.

---

## Key Principles

1. Never silently overwrite—always show what will change
2. Preserve user customizations—their existing config takes priority
3. Merge, don't replace—especially for permissions
```

This will audit your existing setup and carefully merge in the new templates without disrupting your current configuration. You'll see exactly what will change and approve each modification.

---

# Reference Documentation

Everything below is detailed reference material. You don't need to read it to get started—just run the setup command above.

---

## What's in the Kit

```
├── templates/          # Core context files for Claude Code
│   ├── claude.md       # Master file - tells Claude which files to read
│   ├── prd.md          # Product requirements (what you're building)
│   ├── workflow.md     # How you work (branching, PRs, issue tracking)
│   ├── infra.md        # Tech stack and architecture
│   ├── security.md     # Security requirements
│   ├── tests.md        # Testing strategy
│   ├── sbom.md         # Approved dependencies
│   ├── changelog.md    # Version history
│   └── starter-permissions.json  # Pre-approved commands for Claude Code
│
├── commands/           # Slash commands for Claude Code
│   ├── bootstrap.md             # /bootstrap - Set up new project
│   ├── convert.md               # /convert - Add to existing project
│   ├── setup-statusline.md      # /setup-statusline - Configure context status bar
│   ├── gogogo.md                # /gogogo - Start a work session
│   ├── wrapup.md                # /wrapup - End session, commit, sync
│   ├── story.md                 # /story - Create a new user story
│   ├── create-prompt.md         # /create-prompt - Build effective prompts
│   ├── create-research-prompt.md # /create-research-prompt - Build deep research prompts
│   └── improve-prompt.md        # /improve-prompt - Fix and enhance existing prompts
│
├── prompts/            # Interactive setup helpers
│   ├── bootstrap.prompt       # Full new project setup (copy-paste version)
│   ├── convert-existing.prompt # Existing project conversion (copy-paste version)
│   ├── setup-project.prompt   # Legacy project setup wizard
│   ├── create-prd.prompt      # PRD creation helper
│   └── create-command.prompt  # Custom command creator
│
├── docs/               # Detailed guides
│   └── statusline-guide.md    # Context status line setup and customization
│
└── examples/           # Sample project setups
    ├── web-app-example/
    └── cli-tool-example/
```

---

## How It Works

### The Template System

Your `.claude/` folder contains markdown files that Claude Code reads for context:

| File | Purpose |
|------|---------|
| `claude.md` | **Control center** - Lists which files Claude should read and in what priority |
| `prd.md` | **What you're building** - Features, user stories, requirements |
| `workflow.md` | **How you work** - Git workflow, PR process, issue tracking with beads |
| `infra.md` | **Tech decisions** - Stack, architecture, deployment |
| `starter-permissions.json` | **Safe defaults** - Pre-approved commands Claude can run without asking |

When you start Claude Code, it reads these files and understands your project without you having to explain it.

### Starter Permissions

The `starter-permissions.json` file contains a curated list of commands for Claude Code. Commands are split between auto-approved and "ask first":

**Auto-approved (allow):**
- **Package managers:** npm, pnpm, bun, pip, cargo, uv, etc.
- **Git commands:** status, add, commit, push, pull, branch, etc.
- **File operations:** ls, cat, mkdir, cp, mv, touch, etc.
- **Development tools:** make, jq, python, node, etc.
- **GitHub CLI:** gh pr, gh issue, gh repo
- **Playwright MCP tools:** browser automation for testing

**Ask first (requires confirmation):**
- **curl** - network requests
- **rm** - file deletion
- **docker / docker-compose** - container operations
- **brew** - system package installation
- **source** - shell script execution

> **Note:** Review the permissions before using. Remove any commands you'd prefer Claude to ask about first.

### Issue Tracking with Beads

Beads is a lightweight CLI for tracking work items. It integrates with your git workflow:

```bash
# Create an issue
bd create "Add user login" --type feature

# See what's ready to work on
bd ready

# Start working on something
bd update ABC-1 --status in_progress

# Mark it done
bd close ABC-1
```

The `/gogogo` and `/wrapup` slash commands automatically check and update beads status.

### Slash Commands

After setup, use these commands in Claude Code:

**Setup:**
| Command | What it does |
|---------|--------------|
| `/bootstrap` | Sets up the starter kit for a new project |
| `/convert` | Adds the starter kit to an existing project |
| `/setup-statusline` | Configures the context usage status bar (run once) |

**Session Management:**
| Command | What it does |
|---------|--------------|
| `/gogogo` | Loads context, checks git status, shows ready work items |
| `/wrapup` | Commits changes, updates beads, creates handoff notes |
| `/story` | Creates a new user story in your PRD |

**Prompt Engineering:**
| Command | What it does |
|---------|--------------|
| `/create-prompt` | Builds a comprehensive prompt from a basic idea |
| `/create-research-prompt` | Builds a prompt optimized for deep research tasks |
| `/improve-prompt` | Reviews and improves an existing prompt |

---

## Getting Started by Project Size

### Small Projects

Start minimal—you can always add more later:

1. Copy just `claude.md`, `prd.md`, and `workflow.md`
2. Run `bd quickstart`
3. Create your first issue: `bd create "Build MVP" --type feature`

### Larger Projects

Use all templates from the start:

1. Copy all templates to `.claude/`
2. Run through the full `setup-project.prompt`
3. Create an epic with child issues:
   ```bash
   bd create "User Authentication" --type epic
   bd create "Login endpoint" --type feature --parent AUTH-1
   bd create "Password reset" --type feature --parent AUTH-1
   ```

### Team Projects

Add coordination features:

```bash
# Assign work
bd update ABC-1 --assignee "alice"

# Track who did what (for multi-agent setups)
bd update ABC-1 --status in_progress --actor "claude-1"
```

---

## Common Beads Commands

| Command | Description |
|---------|-------------|
| `bd quickstart` | Initialize beads in your project |
| `bd create "title" --type feature` | Create an issue (types: bug, feature, task, epic, chore) |
| `bd list` | Show all issues |
| `bd ready` | Show issues ready to work on (no blockers) |
| `bd update <id> --status in_progress` | Start working on an issue |
| `bd close <id>` | Mark an issue complete |
| `bd show <id>` | View issue details |

See the [Beads documentation](https://github.com/steveyegge/beads) for the full command reference.

---

## Customization Tips

### For Web Apps
- Focus on `prd.md` for user experience details
- Use `security.md` for authentication and data protection
- Detail deployment in `infra.md`

### For CLI Tools
- Keep `prd.md` focused on command design
- Track each command as a beads issue with `--parent` for hierarchy
- Focus `security.md` on input validation

### For APIs
- Document endpoints in `prd.md`
- Emphasize rate limiting and auth in `security.md`
- Detail scaling requirements in `infra.md`

---

## Prompt Engineering Commands

As you build software with Claude Code, you'll often need to create prompts—instructions that tell AI what to do. Well-crafted prompts get dramatically better results than vague ones. These commands help you write better prompts without needing to be a prompt engineering expert.

### Why Prompts Matter

Think of a prompt like giving directions. "Go to the store" might work, but "Drive to the Safeway on 5th Street, park in the back lot, and pick up milk and eggs" gets exactly what you need. The same applies to AI:

- **Vague prompt:** "Write me a blog post about productivity"
- **Better prompt:** "You are a productivity coach for busy professionals. Write a 500-word blog post about time-blocking, targeting remote workers who struggle with distractions. Use a friendly, actionable tone with 3 concrete tips."

The second prompt gives the AI a role, context, audience, format, and tone—and produces far better results.

### The R.G.C.O.A. Framework

All three prompt commands use the **R.G.C.O.A. framework** to ensure your prompts have everything they need:

| Component | What it does | Example |
|-----------|--------------|---------|
| **R**ole | Who should the AI act as? | "You are a senior security engineer..." |
| **G**oal | What should it accomplish? | "Review this code for vulnerabilities..." |
| **C**ontext | What background does it need? | "This is a payment processing API..." |
| **O**utput | What format should the response take? | "Provide a numbered list of issues..." |
| **A**sk (Refinement) | Let the AI ask clarifying questions | "If anything is unclear, ask before proceeding." |

You don't need to memorize this—the commands guide you through it conversationally.

### `/create-prompt` — Build New Prompts

**When to use it:** You have a rough idea of what you want to ask an AI to do, but you're not sure how to phrase it effectively.

**How it works:**
1. You describe your basic idea (e.g., "help me write better error messages")
2. Claude asks targeted questions to fill in the gaps
3. You get a complete, well-structured prompt ready to use

### `/create-research-prompt` — Build Deep Research Prompts

**When to use it:** You need to research a topic thoroughly—competitive analysis, technical deep-dives, market research, etc.

**Why it's different:** Research prompts need additional structure:
- **Scope:** What time period? What sources? What depth?
- **Exclusions:** What should be avoided? (outdated info, certain sources)
- **Uncertainty handling:** How should the AI flag things it's not confident about?

### `/improve-prompt` — Fix Existing Prompts

**When to use it:** You have a prompt that's not working well. Maybe the AI gives irrelevant answers, wrong format, or misses the point.

**How it works:**
1. Share the prompt that's not working (paste it or point to a file)
2. Describe what's going wrong
3. Claude analyzes the issue and applies targeted fixes
4. You get an improved version with explanations of what changed

---

## Troubleshooting

**Claude Code doesn't seem to read my templates**
- Make sure templates are in `.claude/` folder (not `.claude/templates/`)
- Check that `claude.md` lists the other files correctly

**Beads commands not working**
- Verify installation: `bd --version`
- Make sure you ran `bd quickstart` in your project folder
- Check that `.beads/` folder exists in your project

**Slash commands not found**
- Copy command files to `.claude/commands/` in your project
- Restart Claude Code after adding commands

---

## Contributing

We welcome improvements! To contribute:

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

Ideas: More example projects, framework-specific templates, improved prompts.

---

## Resources

- [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Beads Issue Tracker](https://github.com/steveyegge/beads)
- [Examples](/examples) - Sample project setups
- [Detailed Guides](/docs) - In-depth documentation

## License

MIT License - free to use, modify, and distribute.
