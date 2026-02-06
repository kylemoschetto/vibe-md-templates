# Claude Context Blueprint

Purpose: This is the main context file that tells the AI which documents to read and how to resolve conflicting instructions between them.

> Use this file to provide the master context for the project. This document serves as the central reference point for understanding project requirements, operating instructions, and development plans.

---

## Required Context Files

To fully understand the project requirements, my operating instructions, and the development plan, I must read the following files in this directory:

- **`prd.md`**: The Product Requirements Document, which details the features, UI/UX, and technical specifications for the game.
- **`workflow.md`**: The development workflow using **beads** (`bd`) for issue tracking. Includes session startup, planning, execution, and session completion procedures.
- **`infra.md`**: The infrastructure documentation, which describes deployment architecture, coding standards, cloud resources, networking, and operational infrastructure requirements.
- **`security.md`**: The security documentation, which details security requirements, threat models, vulnerability assessments, and security best practices for the application.
- **`sbom.md`**: The Software Bill of Materials, which provides a comprehensive inventory of all software components, dependencies, and licenses used in the project.
<!-- * **`tests.md`**: The testing documentation, which outlines test strategies, test cases, testing frameworks, and quality assurance procedures for the project. -->

I will always consult these files to ensure I have the most up-to-date information before proceeding with any task.

---

## Changelog Usage

Whenever I am asked about previous commits, or I need to understand previous changes that were made, or I need to create a new commit, I will consult the following file in this directory.

- **`changelog.md`**: The project changelog, which tracks version history, feature additions, bug fixes, and other important updates throughout the development lifecycle.

---

## Conflict Resolution Matrix

When instructions in different context files conflict, you **MUST** follow this order of precedence to resolve conflicts and ensure consistent decision-making.

- **Priority 1 - Safety & Supply Chain:** **`security.md`** and **`sbom.md`** (Safety & supply chain constraints) — **Override all other documents.**
- **Priority 2 - Runtime Environment:** **`infra.md`** (Runtime facts) — **Override** feature requests that are incompatible with the environment. You must surface these conflicts to the user for guidance.
- **Priority 3 - Global Conventions:** **`claude.md`** (Global conventions) — Baseline project rules.
- **Priority 4 - Feature Requirements:** **`prd.md`** (Feature-level specifics) — May refine global rules but **must not violate** higher-level constraints. You must surface these conflicts to the user for guidance.
- **Priority 5 - Process & Workflow:** **`workflow.md`** (Process/how-to) — Governs plan creation and execution.

### Conflict Resolution Process

If you find a conflict, you **MUST**:

1. **State the conflict clearly** - Identify the specific conflicting instructions and their sources.
2. **Follow the precedence rule** - Apply the priority order defined above to resolve the conflict.
3. **Recommend minimal edits** - Suggest changes to harmonize the sources, starting with the lowest-authority document (`prd.md` or `workflow.md`).

---

## Agent Systems

This project includes two agent dispatcher systems that extend slash commands with deeper capabilities. You should be aware of these and suggest them when appropriate:

- **`/scribe`** — Document creation and planning. Routes to specialists for project setup, PRD/OPORD drafting, executive briefs, and planning sessions. Scribe executes all beads backlog operations.
- **`/quartermaster`** — Technical architecture and backlog analysis. Routes to specialists for backlog review, feature integration advice, technical gap analysis, and coordination with Scribe. Quartermaster is READ-ONLY on beads.

When the user discusses complex features, architectural decisions, or backlog prioritization, proactively suggest the appropriate agent command.
