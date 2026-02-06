# Development Workflow Blueprint

Purpose: This file explains the step-by-step process the AI will follow to autonomously build and implement project features using **beads** (`bd`) for workflow tracking.

---

## 1. Session Startup

On session start, orient yourself with the current work state:

```bash
bd quickstart            # Onboard to beads (first time)
bd ready --json          # Get unblocked work as structured data
bd list --status open    # See all open issues
bd stale --days 7        # Find neglected issues
```

Review the output to understand:
- What work is ready to be claimed
- What's currently in progress (may need continuation)
- Any blockers or dependencies

### First-Time Setup (Optional)

Install Claude Code hooks for automatic context refresh:

```bash
bd setup claude          # Global installation
bd setup claude --project  # Project-only
bd setup claude --check  # Verify status
```

---

## 2. Planning with Beads

When the user requests implementation of a feature or set of changes:

### Break Work into Issues

Create **fine-grained** issues for each discrete piece of work. Smaller issues = better agent decisions and cheaper sessions.

```bash
bd create "Implement auth endpoint" --type feature
bd create "Add auth tests" --type task
bd create "Update API docs for auth" --type chore
bd create "Fix login crash" --type bug -p 0
```

### Issue Types

| Type | Use For |
|------|---------|
| `bug` | Defects, errors, crashes |
| `feature` | New functionality |
| `task` | General work items |
| `epic` | Large initiatives (parent of multiple issues) |
| `chore` | Maintenance, docs, cleanup |

### Priority Levels

Set priority with `-p` flag (0 = most urgent):

| Priority | Meaning | Example |
|----------|---------|---------|
| `-p 0` | Critical | Production down, security issue |
| `-p 1` | High | Blocking other work |
| `-p 2` | Medium | Normal feature work (default) |
| `-p 3` | Low | Nice to have |
| `-p 4` | Backlog | Future consideration |

```bash
bd create "Security vulnerability" --type bug -p 0
bd update AES-42 --priority 1  # Escalate priority
bd list --priority-min 0 --priority-max 1  # Show P0-P1 only
```

### Issue Granularity

Keep issues atomic and completable in a single focused session:
- **Too big:** "Implement user authentication system"
- **Right size:** "Add JWT token validation middleware"
- **Right size:** "Create login API endpoint"
- **Right size:** "Add password hashing utility"

### Issue Dependencies

Link related issues to build a dependency graph:

```bash
# Create a blocking dependency
bd create "Fix database connection" --blocks AES-42

# Create a child issue (subtask)
bd create "Add input validation" --parent AES-40

# Link discovered work to its origin
bd create "Fix flaky test in auth" --discovered-from AES-42
```

### User-Requested Work During Sessions

When the user requests new features, enhancements, or changes during an active session, **capture them as beads before implementing**:

#### Recognition Triggers

Create new beads when user requests contain:
- New functionality ("add stacking", "implement X")
- Enhancements to existing features ("update X to support Y")
- Multiple distinct items ("do A and B")
- Scope expansion beyond current issue

#### Linking Strategy

| Scenario | Flag | Example |
|----------|------|---------|
| Enhancement to in-progress work | `--parent <current-id>` | Stacking is child of power-up system |
| Related but independent feature | `--discovered-from <current-id>` | Bug found while working |
| Extends a closed issue | `--discovered-from <closed-id>` | New capability for shipped feature |
| Completely new work | (no flag) | Unrelated feature request |

#### Workflow

1. **Pause implementation** - Don't start coding new requests immediately
2. **Create bead(s)** - One per discrete piece of work
3. **Link appropriately** - Use `--parent` or `--discovered-from`
4. **Confirm with user** - Show created beads before proceeding
5. **Mark in_progress** - Then begin implementation

```bash
# User asks: "update power-ups to support stacking with v1/v2/v3 levels"
# While working on: aestroids-v2-sxk (power-up spawn system)

bd create "Implement Shield stacking (v1/v2/v3)" --type feature --parent aestroids-v2-sxk
bd create "Implement Rapid-fire stacking (v1/v2/v3)" --type feature --parent aestroids-v2-sxk
```

#### Exception: Trivial Changes

Skip bead creation for:
- Typo fixes
- Minor tweaks ("make it blue instead of green")
- Clarifications of existing issue scope

### Labels

Tag issues for filtering and organization:

```bash
bd create "Add dark mode" --type feature -l "frontend,ui"
bd label add AES-42 "urgent" "needs-review"
bd label remove AES-42 "urgent"
bd list --label "frontend"           # Issues with this label
bd list --label-any "frontend,backend"  # OR matching
```

### Issue Naming Convention

Use clear, action-oriented titles:
- `Implement [component/feature]`
- `Add [functionality] to [area]`
- `Fix [bug description]`
- `Update [docs/config] for [change]`

### View the Work Queue

```bash
bd list                    # All issues
bd ready --json            # Unblocked work (structured)
bd show <id>               # Issue details
bd dep tree <id>           # View dependency graph
```

### Filtering Issues

```bash
bd list --status open --type bug       # Open bugs
bd list --title-contains "auth"        # Search by title
bd list --no-assignee                  # Unclaimed work
bd list --created-after 2025-01-01     # Recent issues
bd list --label "frontend" --priority-max 1  # Combine filters
```

---

## 3. Branching Strategy

All work is done on feature branches that are merged to `main` via pull request.

### Branch Naming Convention

Create branches with a unique work ID and short description:

```
feature/<work-id>-<short-description>
fix/<work-id>-<short-description>
```

Examples:
- `feature/work-001-canvas-engine`
- `fix/work-002-collision-bug`

### Starting a Work Session

1. **Create the work branch:**
   ```bash
   # Generate a unique work ID (use timestamp or sequential number)
   WORK_ID="work-$(date +%Y%m%d-%H%M)"
   git checkout -b feature/${WORK_ID}-<description>
   ```

2. **Associate beads issues with this branch:**
   - Update issues to `in_progress` as you work on them
   - Any new issues discovered during work are automatically linked via `--discovered-from`

### During Development

- Make atomic commits as work progresses
- Reference beads issues in commit messages with `Closes: <issue-id>`
- Create new beads issues for bugs/gaps discovered during development

### Creating the Pull Request

When the feature branch is ready for merge:

1. **Push the branch:**
   ```bash
   git push -u origin <branch-name>
   ```

2. **Create PR with beads summary:**
   ```bash
   gh pr create --title "<type>: <description>" --body "$(cat <<'EOF'
   ## Summary
   <Brief description of changes>

   ## Beads Covered
   ### Planned Issues
   - [x] `<issue-id>`: <issue title>
   - [x] `<issue-id>`: <issue title>

   ### Issues Created During Development
   - [x] `<issue-id>`: <issue title> (bug fix)
   - [x] `<issue-id>`: <issue title> (gap addressed)

   ## Test Plan
   - [ ] <test step>

   🤖 Generated with [Claude Code](https://claude.com/claude-code)
   EOF
   )"
   ```

3. **Request human review:** After creating the PR, always request human review before merging. **NEVER merge PRs to main without explicit human approval.**

4. **Merge strategy:** Once approved, squash and merge to keep `main` history clean

### PR Description Requirements

Every PR MUST include:
- **Beads Covered section** listing all issues addressed
- **Planned Issues:** Original issues that motivated this work
- **Created During Development:** New issues discovered and fixed during the branch
- Each issue marked with completion status `[x]`

---

## 4. Execution Process

Once the user approves the plan, execute work sequentially and autonomously.

### Issue Statuses

| Status | Meaning |
|--------|---------|
| `open` | Ready to start (default) |
| `in_progress` | Currently being worked on |
| `blocked` | Waiting on dependency or external factor |
| `deferred` | Postponed for later |
| `closed` | Completed |

### Claim and Execute

```bash
bd update <id> --status in_progress
# ... perform the work ...
bd close <id>
```

### Handling Blocked Work

If work is blocked by an external factor:

```bash
bd update AES-42 --status blocked
bd comment AES-42 "Waiting on API team for endpoint spec"
# Issue won't appear in `bd ready` until unblocked
bd update AES-42 --status open  # Unblock when ready
```

### Reopening Closed Issues

```bash
bd reopen AES-42 --reason "Bug reappeared after deploy"
```

### During Execution

- Execute each issue in logical order
- Create commits as work progresses (see Section 6)
- Close issues immediately upon completion
- If blocked, set status to `blocked` and add a comment explaining why

### Work Discovery

When you discover unrelated issues during execution (broken tests, bugs, tech debt), **capture them immediately** rather than ignoring:

```bash
bd create "Fix broken auth tests" --discovered-from AES-42 --type bug
bd create "Refactor duplicated validation logic" --discovered-from AES-42 --type enhancement
```

This ensures no discovered work is lost and creates an audit trail of how work unfolded.

### On Failure

If work fails:
1. Do not close the issue
2. Add context via `bd comment <id> "Error: [description]"`
3. Create follow-up issues if needed
4. Seek user guidance before continuing

---

## 5. Self-Optimization Routine

After completing significant work, analyze if patterns should be captured.

### Analyze

Review the work just completed:
- Did the user correct your approach?
- Did you discover undocumented conventions?
- Did a command need extra flags?

### Capture Follow-ups

If you identify improvements or follow-up work:

```bash
bd create "Update [file] to document [pattern]" --type enhancement
bd create "Add [convention] to project guidance" --type docs
```

### Compare

Check against current `.md` context files (AGENTS.md, workflow.md, etc.).

### Recommend

If a documentation update is warranted, propose the specific edit:
- **Proposed Change:** Exact text to add/remove/change
- **Reasoning:** How this improves future interactions
- **Risk:** Any new burden this introduces

It is perfectly acceptable to find no patterns worth capturing. Improve slowly and iteratively.

---

## 6. Code Commit Workflow

When work results in code changes:

### Commit Message Style

Follow Conventional Commits format:

```
feat: add user login button
fix: resolve null pointer in auth handler
docs: update API documentation for auth endpoints
refactor: extract validation logic to shared module
```

### Link to Issues

Reference the beads issue in commits when applicable:

```bash
git commit -m "feat: add login button

Closes: AES-42"
```

### Sync with Git

After completing work:

```bash
git add .
git commit -m "feat: [description]"
bd sync
git push
```

### Changelog

After completing a set of features, update `changelog.md`.

---

## 7. Multi-Agent Coordination (Optional)

When running multiple agents on the same project, use these flags to prevent conflicts and maintain audit trails.

### Actor Tracking

Tag updates with an actor identifier for audit trails:

```bash
bd update AES-42 --status in_progress --actor "claude-session-1"
bd close AES-42 --actor "claude-session-1"
```

This logs who/what made each change, useful for debugging and history.

### Assignee Management

Claim work to prevent other agents from picking it up:

```bash
# Claim work for this agent
bd update AES-42 --status in_progress --assignee "agent-1"

# See what's assigned to a specific agent
bd list --assignee "agent-1"

# Find unclaimed ready work
bd ready --assignee ""
```

### Multi-Agent Workflow

1. On startup, check for unclaimed work: `bd ready --assignee ""`
2. Claim with both status and assignee: `bd update <id> --status in_progress --assignee "<agent-id>"`
3. Tag all updates with `--actor` for traceability
4. On completion, close with actor: `bd close <id> --actor "<agent-id>"`

---

## 8. Playwright Testing

When testing features with Playwright, follow these conventions for traceability:

### Screenshot Naming Convention

Include the bead issue ID in screenshot filenames to track screenshots back to specific work items:

```
<bead-id>-<description>.png
```

Examples:
- `AES-42-menu-screen.png`
- `AES-43-ship-controls.png`
- `AES-44-collision-gameover.png`

### Testing Workflow

1. **Before testing:** Ensure the dev server is running
2. **Install browser if needed:** Use `browser_install` tool
3. **Navigate to app:** Use `browser_navigate` to load the application
4. **Take baseline screenshot:** Capture initial state with bead ID prefix
5. **Test interactions:** Use keyboard/mouse tools to test functionality
6. **Capture results:** Screenshot each significant state change
7. **Close browser:** Clean up with `browser_close` when done

### Screenshot Storage

Screenshots are saved to `.playwright-mcp/` directory. Consider:
- Adding `.playwright-mcp/` to `.gitignore` for temporary test screenshots
- Moving important screenshots to a `docs/` or `tests/screenshots/` folder if they should be preserved

---

## 9. Session Completion

Before ending a work session, complete ALL steps:

1. **File issues** for any remaining or discovered work
2. **Run quality gates** (tests, linters, builds) if code changed
3. **Close completed issues** via `bd close <id>`
4. **Push the feature branch:**
   ```bash
   git push -u origin <branch-name>
   ```
5. **Create PR if work is complete** (see Section 3: Branching Strategy)
   - Include all beads covered in the PR description
   - Distinguish planned issues from issues created during development
6. **If work continues next session:**
   - Leave branch open, push current state
   - Document progress in commit messages
7. **Hand off** - provide context for next session

---

## 10. Agent-Assisted Workflows

When working on complex tasks, suggest and use these agent systems to improve planning and execution quality. All agent workflows track work through beads.

### When to Suggest Scribe (`/scribe`)

Suggest `/scribe` when the user needs to:
- **Set up a new project** → `/scribe setup` (populates templates, initializes beads)
- **Draft requirements** → `/scribe draft PRD for [feature]` (creates OPORD + FRAGO beads issues)
- **Create executive summaries** → `/scribe brief about [topic]` (two-page decision memos)
- **Run a planning session** → `/scribe plan [topic]` (facilitates goals, constraints, next steps — all tracked as beads issues)

### When to Suggest Quartermaster (`/quartermaster`)

Suggest `/quartermaster` when the user needs to:
- **Prioritize work** → `/quartermaster review backlog` (analyzes beads, recommends next items)
- **Integrate a new feature** → `/quartermaster add [feature]` (architectural fit analysis)
- **Assess code health** → `/quartermaster review tech` (gaps, tech debt, security concerns)
- **Approve and execute plans** → `/quartermaster coordinate` (structured handoff to Scribe for beads creation)

### Agent Coordination Flow

1. Quartermaster analyzes and recommends (read-only on beads)
2. When a plan is approved, Quartermaster coordinates with Scribe
3. Scribe executes all beads operations (`bd create`, `bd update`, `bd close`, `bd dep add`)
4. All work items are tracked as beads issues — no planning artifacts exist outside beads

### Recognition Triggers

Proactively suggest agents when you detect:
- New data models or schema changes → `/quartermaster`
- Cross-cutting concerns (auth, logging, caching) → `/quartermaster`
- External API integrations → `/quartermaster`
- Performance-sensitive operations → `/quartermaster`
- New feature requests needing structured PRD → `/scribe`
- Need for stakeholder communication → `/scribe brief`
- Session planning with multiple work items → `/scribe plan`
