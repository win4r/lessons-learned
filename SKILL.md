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
---

# /lessons-learned — Capture & Save Lessons from Current Work

When this skill is invoked, perform the following steps:

## Step 1: Review the conversation

Look back through the entire current conversation and identify:

- **Problems encountered** — errors, misconfigurations, dead ends
- **Solutions discovered** — what actually fixed the issue
- **Non-obvious knowledge** — things that weren't documented, required trial and error, or contradicted expectations
- **Key decisions** — why a particular approach was chosen over alternatives
- **Gotchas & pitfalls** — traps that could catch you again in a future session
- **Useful commands, paths, or configurations** — concrete details worth remembering

Ignore trivial or obvious things. Focus on knowledge that would save time if this task came up again.

## Step 2: Present a summary to the user

Output a concise, numbered list of lessons learned. For each lesson:
- State the lesson clearly in one sentence
- Add a brief "why" or context line

Ask the user: **"Should I save all of these to memory, or would you like to adjust?"**

If the user has provided arguments (`$ARGUMENTS`), treat them as additional context or a topic filter for which lessons to focus on.

## Step 3: Save to memory

After user confirmation (or immediately if the user's intent is clear):

1. **Read** `~/.claude/projects/-Users-charlesqin/memory/MEMORY.md` to check for existing related memories
2. **Decide**: update an existing memory file if the topic overlaps, or create a new one
3. **Write** the memory file to `~/.claude/projects/-Users-charlesqin/memory/` using this format:

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

4. **Update** `MEMORY.md` index with a pointer to the new/updated file

## Rules

- Do NOT save things that are already in existing memories (check first)
- Do NOT save things derivable from code or docs — only non-obvious knowledge
- Keep lessons actionable and specific, not generic advice
- Use the `feedback` type for workflow/approach lessons, `project` type for project-specific facts
- Convert any relative dates to absolute dates (e.g., "today" → "2026-03-20")
- If the conversation was trivial with nothing worth saving, say so honestly — don't force it
