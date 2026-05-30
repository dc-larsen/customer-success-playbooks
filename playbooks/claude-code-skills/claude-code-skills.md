# Claude Code Skills

**Package the CS work you repeat into reusable skills**

You explain the same task to your AI assistant over and over: how you write a customer reply, how you prep for a call, how you pull account data. A skill captures that once, so the assistant does it your way every time.

This playbook covers what a skill is, which tasks are worth turning into one, and how to build them.

---

## 1. What a skill is

A skill is a folder with a `SKILL.md` file: a name, a description of when to use it, and the instructions to run it. It can include scripts the assistant runs (API calls, formatters). Claude Code loads the right skill automatically based on what you ask. The `skills/` folder in this repo has working examples.

Think of it as a runbook your assistant follows, not a prompt you retype.

---

## 2. Which tasks to turn into skills

Good candidates share three traits:
- You do them more than once a week
- They have a consistent shape: same steps, same output
- They depend on context the assistant won't have by default: your tone, your tools, your format

Examples: drafting customer replies, building meeting prep, weekly reporting, pulling data from your CRM, formatting a Slack post.

---

## 3. How to build one

- Start from a task you just did by hand. Write the steps the way you'd hand them to a new teammate.
- Add the specifics: which API, which fields, what the output should look like
- Put a clear trigger in the description so the assistant knows when to reach for it
- Test it on a real case, then tighten the instructions wherever it guessed wrong

---

## 4. Keep them focused and composable

One skill, one job. A skill that drafts replies shouldn't also pull data; let it call a data skill instead. Small, single-purpose skills combine better and are easier to fix.

---

## 5. Connection to other playbooks

- **AI Automation:** The overview of the Claude Code and n8n setup these skills live in.
- **CLAUDE.md:** The standing context the assistant reads every session, alongside your skills.
- **Call Review Automation:** A grading loop built as a skill.
- **Customer Alerts Tracking:** A scheduled data-pull built as a skill.

---

## Final thought

Every task you explain twice is a skill you haven't written yet. Capture it once, and your assistant does it your way from then on.
