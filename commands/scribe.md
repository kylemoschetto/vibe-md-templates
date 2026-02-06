---
description: Scribe dispatcher — routes your request to project setup (init), OPORD/PRD, 2-page orders, or planning sessions
allowed-tools: Task
---

Task: Use the scribe subagent to route this request to the correct specialist (scribe-init, scribe-opord, scribe-2page, scribe-co-planning) and return the final result.

## Examples
- `/scribe setup this repo using our templates and initialize beads`
- `/scribe review document docs/brief.md and handle project setup`
- `/scribe draft a PRD for feature X and propose initial FRAGOs`
- `/scribe write a two-page brief for leadership about project Y`
- `/scribe run a planning session and produce next steps with owners and dates`

User request:
$ARGUMENTS
