# Taskwarrior Management

Manage local Taskwarrior installation for task tracking.

## System Locations

- Config: `~/.taskrc`
- Data: `~/.task/`
- Inbox: `~/todo/inbox.md`

## Conventions

### Projects
Use dot notation:
- `work.{company}` - Work tasks
- `work.{company}.support` - Customer support tasks
- `personal.health`
- `personal.finance`
- `home.maintenance`

### Priority
Only three values:
- `H` - Must do today/tomorrow
- `M` - Important but not urgent
- `L` - Nice to have

### Due Dates
- Only assign when there's a real deadline
- Use ISO format or Taskwarrior relative dates (`due:monday`, `due:2024-01-15`)

### Tags
- `+waiting` - Blocked by another person
- `+quick` - Under 15 minutes
- `+deep` - Requires focus time

## Commands Reference

### Adding Tasks
```bash
task add "Description" project:work.company priority:M
task add "Description" project:personal due:friday
task add "Description" +waiting wait:monday
```

### Listing Tasks
```bash
task next          # Next 10 pending tasks
task today         # Due today
task waiting       # Blocked tasks
task project:work.company   # Filter by project
task +waiting      # Filter by tag
```

### Modifying Tasks
```bash
task <ID> modify priority:H
task <ID> modify due:tomorrow
task <ID> modify project:work.company.support
task <ID> modify +waiting wait:monday
```

### Completing Tasks
```bash
task <ID> done
```

### Deleting Tasks
```bash
task <ID> delete
```

### Viewing Task Details
```bash
task <ID> info
```

## Inbox Processing

When processing `~/todo/inbox.md`:

1. Read the file contents
2. Skip header lines (starting with `#` or `<!--`)
3. Parse each non-empty line as a candidate task
4. For each task:
   - Rewrite to be atomic and verb-led (e.g., "Review PR" not "PR needs review")
   - Infer project from context (conservative)
   - Infer priority only if obvious (default: none)
   - Do NOT assign due dates unless explicitly stated
5. Show preview of tasks to user before adding
6. After user confirms, add tasks via `task add`
7. Clear inbox.md, leaving only:
   ```
   # Task Inbox
   <!-- Processed on YYYY-MM-DD -->
   ```

## Operating Principles

**Never violate these:**
- Never delete tasks without explicit user request
- Never modify completed tasks
- Never directly edit files in `~/.task/`
- When unsure about project/priority, ask
- Prefer clarity over completeness

## Example Interactions

**User:** "Add a task to review the PR"
```bash
task add "Review PR" project:work.company priority:M
```

**User:** "What's on my plate?"
```bash
task next
```

**User:** "Mark task 5 as done"
```bash
task 5 done
```

**User:** "I'm blocked on task 3 until Sarah responds"
```bash
task 3 modify +waiting wait:monday
```

**User:** "Process my inbox"
1. Read `~/todo/inbox.md`
2. Show parsed tasks for approval
3. Add approved tasks
4. Clear inbox with timestamp

**User:** "Show all my tasks" or "show my tasks"
1. Run `task status:pending export` to get all pending tasks as JSON
2. Parse the JSON in your response (NOT via bash/python script)
3. Build ASCII box-drawing table with columns: ID, PROJECT, PRI, DUE, DESCRIPTION
4. Use proper box-drawing characters: `+----+` for borders, `|` for column separators
5. Align columns properly with padding (calculate max width for each column)
6. Sort by priority (H first, then M, then L, then no priority), then by urgency
7. Output the complete formatted table DIRECTLY in your response text
8. NEVER use bash (echo/cat/printf) or write Python scripts to output the table
9. Include summary line below table: "**N pending tasks** (X high priority, Y medium, Z low, W no priority set)"

CRITICAL: The table must appear fully formatted in your text response, not collapsed or in a bash command. Parse JSON, build table string, output it directly.

Example format:
```
+----+------------------+-----+--------+------------------------------------------------+
| ID | PROJECT          | PRI | DUE    | DESCRIPTION                                    |
+----+------------------+-----+--------+------------------------------------------------+
| 6  | work.company     | H   | -      | Setup meetings with all customers              |
| 4  | work.company     | M   | -      | Automate sales handoff process                 |
| 3  | personal.family  | -   | Jan 22 | Order birthday present                         |
+----+------------------+-----+--------+------------------------------------------------+

**12 pending tasks** (1 high priority, 4 medium, 1 low, 6 no priority set)
```
