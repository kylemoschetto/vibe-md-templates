# Template Guide

Detailed explanation of each template file and how to use them effectively with Claude Code.

## Overview

The template system creates a **context-driven architecture** where Claude Code understands your project through structured documentation. Each template serves a specific purpose and they work together to give Claude Code complete understanding of your project.

## Template Hierarchy

```
claude.md           ← Master control file (READ THIS FIRST)
├── prd.md          ← What you're building
├── infra.md        ← How it's built
├── workflow.md     ← How you work (and how features execute)
├── security.md     ← How you stay safe
├── sbom.md         ← What you depend on
├── tests.md        ← How you ensure quality
└── changelog.md    ← How you track changes
```

---

## Core Templates (Essential)

### `claude.md` - Master Context File

**Purpose:** The "brain" of your project that tells Claude Code how to understand and prioritize all other files.

**Key Sections:**

- **Required Context Files** - Lists which templates Claude Code should read
- **Conflict Resolution Matrix** - Priority system when templates disagree
- **Global Conventions** - Project-wide rules and standards

**When to Update:**

- When you add/remove template files
- When you change the priority of different concerns
- When you establish new global project rules
- When introducing or updating beads-based workflow logic

**Example Use Case:**

```markdown
## Required Context Files

- **`prd.md`**: Product requirements for the task management app
- **`infra.md`**: React + Firebase architecture details
- **`security.md`**: User data protection requirements
```

### `prd.md` - Product Requirements Document

**Purpose:** Defines WHAT you're building and WHY, focusing on user experience and business value. Also serves as the origin point for identifying **features** that will be tracked as beads issues.

**Key Sections:**

- **The Big Picture** - Project name, summary, target users, scope limitations
- **The Features** - User stories in "As a [user], I want [action] so I can [goal]" format
- **The Look and Feel** - Visual style, colors, key screens, and UI elements

**When to Update:**

- When adding new features or user stories
- When changing target audience or scope
- When updating UI/UX requirements
- Before starting any new major feature
- When identifying new features to track as beads issues

**Best Practices:**

- Write user stories from the user's perspective, not technical perspective
- Be specific about what the app will NOT do (scope limitations)
- Include enough UI detail for Claude Code to make good design decisions

### `infra.md` - Infrastructure Documentation

**Purpose:** Defines HOW your application is built technically - the foundation that everything else builds on.

**Key Sections:**

- **Technology Stack** - Languages, frameworks, databases, tools
- **Development Environment** - Local setup requirements and procedures
- **Architecture** - System design, data flow, component structure
- **Deployment** - Hosting, CI/CD, environment management

**When to Update:**

- When changing technologies or frameworks
- When updating development setup procedures
- When modifying system architecture
- When changing deployment or hosting strategies
- When workflow introduces new environment-related plans or automation

**Best Practices:**

- Be specific about versions (React 18.2, not just "React")
- Include reasoning for technology choices
- Document any non-standard setup procedures

### `workflow.md` - Development Workflow

**Purpose:** Defines HOW you work - your development process, planning methodology, and quality standards.
It also governs **beads-based issue tracking** using the `bd` CLI for work management.

**Key Sections:**

- **Session Startup** - How to orient with `bd ready`, `bd list`, and current work state
- **Planning with Beads** - Issue creation, types, priorities, dependencies, and labels
- **Branching Strategy** - Branch naming, PR workflows, and beads references in PRs
- **Execution Process** - Status management (`open`, `in_progress`, `blocked`, `closed`)
- **Self-Optimization** - How Claude Code improves its own planning or coding behavior

**When to Update:**

- When changing Git workflow or branching strategy
- When updating code standards or review processes
- When modifying beads issue conventions or execution rules
- After retrospectives or process improvements
- When refining how issues transition between statuses

**Best Practices:**

- Keep issues atomic and completable in a single focused session
- Use dependencies (`--blocks`, `--parent`) to manage work order
- Reference beads issue IDs in commits and PR descriptions

---

## Supporting Templates (Recommended)

### `security.md` - Security Framework

**Purpose:** Ensures your application is built with security-first principles and proper risk management.

**Key Sections:**

- **Data Sensitivity** - Classification of different types of data
- **Authentication & Authorization** - User management and access control
- **Dependencies & Supply Chain** - Third-party security policies
- **Secrets Management** - How API keys and sensitive data are handled

**When to Update:**

- When handling new types of user data
- When adding authentication or user management features
- When integrating new third-party services
- After security reviews or audits

**Best Practices:**

- Classify data sensitivity levels early
- Be explicit about what should never be logged or stored
- Include specific policies for API keys and secrets

### `sbom.md` - Software Bill of Materials

**Purpose:** Manages your project's dependencies, versions, and approved libraries to maintain consistency and security.

**Key Sections:**

- **Approved Stack** - Core technologies with specific versions
- **Dependencies Policy** - Rules for adding new libraries
- **Version Management** - Update strategy and compatibility requirements
- **Documentation Links** - Quick access to official docs and resources

**When to Update:**

- When adding new dependencies or libraries
- When updating major versions of existing dependencies
- When changing policies about what types of libraries are acceptable
- During regular security audits

**Best Practices:**

- Specify exact version numbers and update policies
- Include reasoning for why specific libraries were chosen
- Maintain links to security advisories and changelogs

### `tests.md` - Testing Strategy

**Purpose:** Defines your approach to quality assurance, testing frameworks, and coverage goals.

**Key Sections:**

- **Testing Philosophy** - Your approach to different types of testing
- **Framework Setup** - Tools, configuration, and testing environment
- **Key Scenarios** - Critical user journeys and edge cases to test
- **Coverage Goals** - What percentage and types of coverage you aim for

**When to Update:**

- When changing testing frameworks or tools
- When adding new types of testing (e2e, performance, etc.)
- When updating coverage requirements or quality standards
- After major workflow plan updates or when tests integrate with plan steps

**Best Practices:**

- Focus on testing user-critical paths, not just code coverage
- Include performance and accessibility testing requirements
- Specify both positive and negative test cases

### `changelog.md` - Version History

**Purpose:** Tracks project evolution, maintains version history, and provides context for changes over time.
Significant completed beads issues should be logged with their issue ID and summary.

**Key Sections:**

- **Version Format** - Semantic versioning rules and numbering scheme
- **Change Categories** - How to classify different types of changes
- **Release Process** - Steps for creating and publishing releases
- **Historical Entries** - Actual changelog entries for past versions

**When to Update:**

- After every significant change or feature addition
- After completing beads issues or closing a set of related work
- Before creating releases or deployments
- When changing versioning strategy or release process
- During project retrospectives

**Best Practices:**

- Follow semantic versioning (MAJOR.MINOR.PATCH)
- Reference beads issue IDs in changelog entries (e.g., `Closes: AES-42`)
- Include both what changed and why it changed

---

## Template Interaction Patterns

### Conflict Resolution

When templates contain conflicting information, Claude Code follows this priority order:

1. **Security & Supply Chain** (`security.md`, `sbom.md`) - Safety first
2. **Runtime Environment** (`infra.md`) - Technical constraints
3. **Global Conventions** (`claude.md`) - Project standards
4. **Feature Requirements** (`prd.md`) - Business needs
5. **Process & Workflow** (`workflow.md`) - Beads issue management and execution logic

### Information Flow

```
User Request → claude.md (routing) → Relevant templates
  ↓
workflow.md → Issue tracking with beads (`bd create`, `bd update`, `bd close`)
  ↓
infra.md / security.md → Technical and safety validation
  ↓
tests.md / changelog.md → Quality checks and update logs
```

Claude Code reads `claude.md` first to understand which other templates are relevant, then consults those templates to plan and execute work tracked via beads.

### Maintenance Cadence

**Weekly:**

- Review and update `changelog.md` with recent changes
- Check `workflow.md` for process improvements

**Monthly:**

- Review `security.md` for new threats or data types
- Update `sbom.md` with dependency changes and security patches

**Quarterly:**

- Comprehensive review of `prd.md` for scope and priority changes
- Update `infra.md` for architectural evolution
- Review conflict resolution rules in `claude.md`

---

## Common Patterns and Examples

### For Web Applications

Focus your templates on:

- User authentication flows in `security.md`
- Database and API architecture in `infra.md`
- User journey testing in `tests.md`
- Feature rollout process in `workflow.md`

### For CLI Tools

Emphasize:

- Input validation and error handling in `security.md`
- Installation and distribution in `infra.md`
- Command interface design in `prd.md`
- Integration testing in `tests.md`

### For APIs and Services

Prioritize:

- Rate limiting and authentication in `security.md`
- Scalability and performance in `infra.md`
- Endpoint documentation in `prd.md`
- Load and integration testing in `tests.md`

---

## Troubleshooting

### Claude Code isn't following my templates

**Check:**

- Is `claude.md` correctly listing all your template files?
- Are there conflicts between templates that need resolution?
- Do templates contain placeholder text instead of actual requirements?

### Templates seem to contradict each other

**Solution:**

- Use the conflict resolution matrix in `claude.md`
- Update the lower-priority template to align with higher-priority constraints
- Add clarifying context to explain why certain decisions were made

### Templates are too complex/detailed

**Simplify:**

- Start with just `claude.md`, `prd.md`, and `workflow.md`
- Add other templates as your project grows
- Focus on the most critical information first

### Claude Code is making inconsistent decisions

**Fix:**

- Ensure all templates are up to date
- Add more specific guidance in the relevant template
- Check that `claude.md` conflict resolution rules are clear

---

## Agent Integration

The starter kit includes two agent systems (Scribe and Quartermaster) that read template files for context:

- **Scribe** reads `prd.md`, `workflow.md`, and other context files when drafting documents or creating beads issues
- **Quartermaster** reads `prd.md`, `infra.md`, `security.md`, and codebase files when analyzing feature fit or technical concerns

Both agents respect the **Conflict Resolution Matrix** defined in `claude.md`. When agents make recommendations that touch on security or infrastructure constraints, they follow the same priority order:

1. Security & Supply Chain (`security.md`, `sbom.md`)
2. Runtime Environment (`infra.md`)
3. Global Conventions (`claude.md`)
4. Feature Requirements (`prd.md`)
5. Process & Workflow (`workflow.md`)

This means agent recommendations will flag conflicts with higher-priority constraints rather than silently overriding them.

---

**Remember:** Templates are living documents. They should evolve with your project and reflect your actual current state, not your ideal future state.
