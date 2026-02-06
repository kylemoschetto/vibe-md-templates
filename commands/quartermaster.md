---
description: Technical/Solution Architect — routes to backlog review, feature integration advice, technical gap analysis, or Scribe coordination
allowed-tools: Task
---

Task: Use the quartermaster subagent to route this request to the correct specialist (quartermaster-backlog-review, quartermaster-feature-fit, quartermaster-tech-review, quartermaster-coordination) and return the analysis/recommendations.

## Examples
- `/quartermaster review all backlog items and identify which item we should work on next`
- `/quartermaster I would like to add feature X, what is the best way to fold this in`
- `/quartermaster review current technical approach and identify gaps or concerns`
- `/quartermaster the feature plan looks good, please coordinate with scribe to create the backlog items`
- `/quartermaster what's the health of our codebase?`
- `/quartermaster help me prioritize between these 3 epics`

## Routing Hints
- **Backlog/prioritization questions** → quartermaster-backlog-review
- **"How do I add X" / feature integration** → quartermaster-feature-fit
- **Technical review / gaps / concerns** → quartermaster-tech-review
- **"Approved, create issues" / coordinate with Scribe** → quartermaster-coordination

User request:
$ARGUMENTS
