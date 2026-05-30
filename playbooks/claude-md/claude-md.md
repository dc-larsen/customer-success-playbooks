# CLAUDE.md

**Teach your AI assistant how you work, once**

Every session you re-explain your role, your tools, and how you want things written. A CLAUDE.md file says it once. Claude Code reads it at the start of every session, so the assistant shows up already knowing your context.

This playbook covers what to put in one and how to keep it useful.

---

## 1. What it is

A CLAUDE.md is a plain-markdown file the assistant loads automatically: your standing instructions. A global one at `~/.claude/CLAUDE.md` applies everywhere; a per-project one adds context for a specific repo.

---

## 2. What to put in it

- Who you are: role, company, what you own
- Your tools and how you reach them: CRM, CS platform, support, Slack, APIs
- How you communicate: tone, formatting, words and phrases to avoid
- Your work systems: where notes and plans live, naming conventions
- Code and cost preferences, if you build

Start from the `CLAUDE-template.md` in this repo and replace the placeholders.

---

## 3. Keep it tight

It loads every session, so every line costs context. Cut anything that doesn't change what the assistant does. If a line is only nice to know, leave it out.

---

## 4. Layer global and project

Put the things that are always true in the global file. Put repo-specific facts (this codebase, this customer, this workflow) in a project CLAUDE.md. The assistant reads both.

---

## 5. Connection to other playbooks

- **Claude Code Skills:** Skills handle specific tasks; CLAUDE.md sets the standing context they run in.
- **AI Automation:** The broader setup this configures.
- **AI Second Brain:** Point the assistant at your knowledge base from here.

---

## Final thought

The assistant is only as good as what it knows about you. Write it down once, keep it short, and stop repeating yourself.
