---
description: Wrap up the current session - commit changes, update docs, and prepare for next session
---

## Session Wrap-Up Checklist

Please perform the following wrap-up tasks to close this session cleanly:

### 1. Archive Plan Files (Optional)
If a plan was created during this session, prompt user to save it to `.claude/plans/`:

- Check if a plan file exists for the current work (may be in temp location or conversation)
- Save/move to `.claude/plans/<feature-name>.md` using the naming convention:
  - Strip `feature/` or `fix/` prefix from branch name
  - Strip work ID prefix (`work-YYYYMMDD-HHMM-`)
  - Use remaining description as filename
  - Example: `feature/work-20260112-1456-epic-c-docs` → `.claude/plans/epic-c-docs.md`
- Plan files should contain:
  - Implementation approach and rationale
  - Files modified
  - Acceptance criteria
  - Test/verification steps
- Plans persist after branch merge for historical reference

### 2. Git & Code Status
- Run `git status` to check for any uncommitted changes
- If there are changes, stage and commit them using Conventional Commits format
- Push all commits to the remote repository

### 3. Beads Issue Updates
Update issue tracking for work completed this session:
- Close completed issues: `bd close <id>`
- Update in-progress issues: `bd update <id> --status in_progress`
- Create issues for any discovered follow-up work: `bd create "<title>"`
- Add comments for context if needed: `bd comment <id> "<note>"`

### 4. Documentation Updates
Check and update the following files as needed:
- `.claude/changelog.md` - Add entries for any completed features or bug fixes from this session

### 5. Build Verification
- Run your project's build command to ensure everything compiles (e.g., `npm run build`, `cargo build`, `go build`, etc.)
- Note any warnings or errors that should be addressed in the next session

> **Note:** Customize the build command for your specific project stack.

### 6. Sync and Push
Ensure all work is synced and pushed to remote:
```bash
git pull --rebase
bd sync
git push
git status  # Must show "up to date with origin"
```

**Critical:** Work is NOT complete until `git push` succeeds.

### 7. Session Summary
Provide a brief summary including:
- **Completed:** What was accomplished in this session
- **In Progress:** Any work that is partially complete and its current state
- **Blockers:** Any issues or blockers encountered
- **Next Steps:** Clear list of what to work on next (reference beads issue IDs)

### 8. Handoff Message
End with a clear handoff message that can be used to resume work, such as:
"Ready to resume. Next session: [specific task or feature to continue] (see issue <id>)"

### 9. Improve Session Startup (Optional)
Evaluate if anything learned during this session should be added to `.claude/commands/gogogo.md` to help future sessions:
- New context files that should be loaded at startup
- Additional checks that would have been helpful
- Project-specific setup steps discovered during work
- Environment variables or dependencies that caused issues

If improvements are identified, offer to update the gogogo.md file.
