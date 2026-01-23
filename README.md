# Claude Code Starter Kit

**Get productive with Claude Code in minutes.** This starter kit gives you ready-to-use templates that help Claude Code understand your project and work more effectively.

## What This Does

When you use Claude Code, it works best when it understands your project's context—what you're building, how you want to work, and what standards to follow. This kit provides:

- **Templates** that teach Claude Code about your project
- **Slash commands** for common workflows (start session, wrap up, create stories)
- **Issue tracking** via [Beads](https://github.com/steveyegge/beads) to manage work items

**Before:** You explain your project context every session.
**After:** Claude Code reads your templates and already knows how to help.

---

## Quick Start

### Step 1: Install the Tools

**Claude Code** (pick one method):

```bash
# Option A: Native binary (recommended)
curl -fsSL https://claude.ai/install.sh | bash

# Option B: npm
npm install -g @anthropic-ai/claude-code
```

**Beads** (issue tracking):

```bash
npm install -g beads-ts
```

Verify both are installed:

```bash
claude --version
bd --version
```

### Step 1.5: Set Up Context Status Line (Recommended)

This one-time setup adds a persistent status bar showing your token usage. It helps you stay aware of context limits before autocompact triggers—essential for longer sessions.

```bash
# In Claude Code, run:
/setup-statusline
```

Then restart Claude Code. You'll see a status bar at the bottom of your terminal. See [docs/statusline-guide.md](docs/statusline-guide.md) for details.

> **Tip:** If you skip this step, the `/gogogo` command will offer to set it up for you later.

### Step 2: Set Up Your Project

```bash
# 1. Create a .claude folder in your project
mkdir -p /path/to/your/project/.claude

# 2. Copy the templates
cp templates/*.md /path/to/your/project/.claude/

# 3. Initialize beads for issue tracking
cd /path/to/your/project
bd quickstart

# 4. (Optional) Set up Claude Code hooks for automatic context
bd setup claude
```

### Step 3: Customize Your Templates

Open Claude Code in your project and paste the setup prompt:

```bash
cd /path/to/your/project
claude

# Then paste the contents of prompts/setup-project.prompt
```

Claude Code will walk you through customizing each template for your project.

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
│   ├── setup-statusline.md       # /setup-statusline - Configure context status bar
│   ├── gogogo.md                 # /gogogo - Start a work session
│   ├── wrapup.md                 # /wrapup - End session, commit, sync
│   ├── story.md                  # /story - Create a new user story
│   ├── create-prompt.md          # /create-prompt - Build effective prompts
│   ├── create-research-prompt.md # /create-research-prompt - Build deep research prompts
│   └── improve-prompt.md         # /improve-prompt - Fix and enhance existing prompts
│
├── prompts/            # Interactive setup helpers
│   ├── setup-project.prompt    # Full project setup wizard
│   └── migrate-project.prompt  # Migrate project setup wizard
│   ├── create-prd.prompt       # PRD creation helper
│   └── create-command.prompt   # Custom command creator
│   └── existing-prd.prompt     # PRD creation helper for existing projects
│
├── docs/               # Detailed guides
│   └── statusline-guide.md     # Context status line setup and customization
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

**To use it:** Copy to your project's `.claude/` folder as `settings.local.json`:

```bash
cp templates/starter-permissions.json /path/to/your/project/.claude/settings.local.json
```

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

**Setup (One-Time):**
| Command | What it does |
|---------|--------------|
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

### For Existing Projects
- Use `existing-prd.prompt` to create PRDs for existing projects
- Use `migrate-project.prompt` to migrate projects that were already setup for this template system

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

**Example:**
```
/create-prompt help me write a prompt for code review
```

Claude will ask things like: "Who's the target audience—junior developers or the whole team? Should it focus on security, performance, or general quality?" Then it builds the complete prompt for you.

### `/create-research-prompt` — Build Deep Research Prompts

**When to use it:** You need to research a topic thoroughly—competitive analysis, technical deep-dives, market research, etc.

**Why it's different:** Research prompts need additional structure:
- **Scope:** What time period? What sources? What depth?
- **Exclusions:** What should be avoided? (outdated info, certain sources)
- **Uncertainty handling:** How should the AI flag things it's not confident about?

**Example:**
```
/create-research-prompt competitive analysis of AI coding assistants
```

Claude will ask about your research timeframe, preferred sources, what you already know (to avoid redundancy), and how you want findings organized.

### `/improve-prompt` — Fix Existing Prompts

**When to use it:** You have a prompt that's not working well. Maybe the AI gives irrelevant answers, wrong format, or misses the point.

**How it works:**
1. Share the prompt that's not working (paste it or point to a file)
2. Describe what's going wrong
3. Claude analyzes the issue and applies targeted fixes
4. You get an improved version with explanations of what changed

**Example:**
```
/improve-prompt
```

Then paste your broken prompt and say "it keeps giving me generic responses instead of specific recommendations." Claude will identify what's missing and fix it.

### Context-Aware Intelligence

These commands are smart about context. If you've been discussing a project, Claude already knows:
- What you're building (from conversation or your `.claude/prd.md`)
- Who your users are
- What problems you're solving

So instead of asking you 10 questions, it might say: "Based on our discussion about your developer tool, I'm assuming the audience is software engineers and the tone should be technical but approachable. Is that right?" Then it only asks about what it doesn't know.

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
