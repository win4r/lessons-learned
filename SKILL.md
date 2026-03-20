---
name: lessons-learned
description: Summarize lessons, tips, and gotchas learned from the current conversation, then save them to memory. Use when finishing a task, debugging session, or setup workflow to capture non-obvious knowledge for future reference.
user-invocable: true
allowed-tools:
  - Read
  - Write
  - Edit
  - Glob
  - Grep
  - Bash(git log*)
  - Bash(echo *)
---

# /lessons-learned — Capture & Save Lessons from Current Work

When this skill is invoked, perform the following steps:

## Step 0: Parse arguments

`$ARGUMENTS` may contain:

- **`save`** — skip confirmation, save immediately
- **A topic string** (e.g. `telegram setup`) — filter lessons to that topic only
- **Both** (e.g. `save telegram`) — filter + auto-save
- **Empty** — review everything, ask before saving

## Step 1: Review the conversation

Look back through the entire current conversation and identify:

- **Problems encountered** — errors, misconfigurations, dead ends
- **Solutions discovered** — what actually fixed the issue
- **Non-obvious knowledge** — things that weren't documented, required trial and error, or contradicted expectations
- **Key decisions** — why a particular approach was chosen over alternatives
- **Gotchas & pitfalls** — traps that could catch you again in a future session
- **Useful commands, paths, or configurations** — concrete details worth remembering

Optionally run `git log --oneline -20` to add context about what changed on disk.

Ignore trivial or obvious things. Focus on knowledge that would save time if this task came up again.

## Step 2: Present a summary to the user

Output a concise, numbered list of lessons learned. For each lesson:
- State the lesson clearly in one sentence
- Add a brief "why" or context line

Unless `save` was passed in arguments, ask: **"Should I save all of these to memory, or would you like to adjust?"**

## Step 3: Resolve memory path

The memory directory is project-scoped. Determine the correct path dynamically:

```
~/.claude/projects/-<sanitized-cwd>/memory/
```

Where `<sanitized-cwd>` is the current working directory with `/` replaced by `-`. For example:
- cwd `/Users/charlesqin` → `~/.claude/projects/-Users-charlesqin/memory/`
- cwd `/Users/charlesqin/myproject` → `~/.claude/projects/-Users-charlesqin-myproject/memory/`

Use `Glob` to find the actual `MEMORY.md` path if unsure: `~/.claude/projects/*/memory/MEMORY.md`

## Step 4: Dedup and save

1. **Read** `MEMORY.md` index to check for existing related memories
2. **Dedup check**: For each candidate lesson, search existing memory files for overlapping content:
   - If an existing file covers the same topic, **read it** and **append or merge** the new lessons into it (use Edit, not Write) — avoid duplicating what's already there
   - If no overlap exists, **create a new file**
3. **Write** new memory files using this format:

```markdown
---
name: {{descriptive name}}
description: {{one-line description for relevance matching}}
type: feedback OR project
---

{{Structured lessons:}}

1. **Lesson title** — concrete description
   **Why:** what went wrong or what was surprising
   **How to apply:** when and how to use this knowledge

2. ...
```

4. **Update** `MEMORY.md` index — add pointers for new files, leave existing entries unchanged

## Rules

- Do NOT save things that are already in existing memories (check first, merge if overlapping)
- Do NOT save things derivable from code or docs — only non-obvious knowledge
- Keep lessons actionable and specific, not generic advice
- Use the `feedback` type for workflow/approach lessons, `project` type for project-specific facts
- Convert any relative dates to absolute dates (e.g., "today" → "2026-03-20")
- If the conversation was trivial with nothing worth saving, say so honestly — don't force it
