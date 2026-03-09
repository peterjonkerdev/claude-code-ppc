---
name: weekly-update
description: Generate a weekly progress summary from git history and optionally sync with your project management tool. Use when the user asks for a weekly update, status sync, or sprint review.
allowed-tools: Bash(git *), Read, Grep, Glob
---

# Weekly Update

Generate a weekly progress summary from git history.

## Step 1: Gather context

Run these in parallel:

1. **Git log** for the past 7 days (or 14 if biweekly):
   ```bash
   git log --since="7 days ago" --oneline --no-merges
   ```
   And with stats:
   ```bash
   git log --since="7 days ago" --stat --no-merges
   ```

2. **Open tasks** — check your project management tool or local priorities file for current task status.

## Step 2: Analyze and summarize

- Match git commits to projects or task IDs (by prefixes or ticket keys)
- Identify what was completed, what progressed, what's new
- Keep the summary concise: bullet points, no fluff

## Step 3: Present to user for review

Show the user:
1. **Summary** — concise bullet points of this week's highlights
2. **Task updates** — proposed status changes:
   - Tasks to transition (e.g., To Do -> In Progress, In Progress -> Done)
   - Notes to add on specific tasks
   - New tasks to create if work was done that isn't tracked
3. **Next week** — suggest what to focus on based on open tasks and priorities

**Wait for user confirmation before making any external changes.**

## Step 4: Execute updates

After user confirms:
- Update task statuses as agreed
- Post summary where needed
- Create new tasks if agreed

## Arguments

- No arguments: default to past 7 days
- `$ARGUMENTS` can be: "biweekly" (14 days), a specific task/ticket key, or a date range
