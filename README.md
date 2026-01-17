# Customer Success Playbooks

A comprehensive collection of battle-tested customer success strategies, processes, playbooks, and Claude Code skills designed to drive customer satisfaction, retention, and growth.

## 📚 What's Inside

This repository contains proven customer success playbooks organized by key focus areas:

- **[Onboarding](https://github.com/dc-larsen/customer-success-playbooks/blob/main/playbooks/onboarding/onboarding.md)** - Get new customers up and running successfully
- **[Regular Cadence Meetings](playbooks/regular-cadence-meetings/regular-cadence-meetings.md)** - Run effective customer check-ins that drive value
- **[Proactive Alerts](https://github.com/dc-larsen/customer-success-playbooks/blob/main/playbooks/alerts/alerts.md)** - Stay ahead of customer needs with automated monitoring
- **[Case Studies](https://github.com/dc-larsen/customer-success-playbooks/blob/main/playbooks/case-study/case-study.md)** - Turn customer wins into powerful success stories
- **[Health Scoring](https://github.com/dc-larsen/customer-success-playbooks/blob/main/playbooks/health-scoring/health-scoring.md)** - Monitor and measure customer health metrics

## 🤖 Claude Code Skills

Skills for [Claude Code](https://github.com/anthropics/claude-code) to automate and enhance CS workflows. Copy to `~/.claude/skills/` to use.

### API Integrations
- **[HubSpot API](skills/api-integrations/hubspot-api.md)** - CRM integration patterns and common endpoints
- **[Zendesk API](skills/api-integrations/zendesk-api.md)** - Support ticket automation and customer data access
- **[Vitally API](skills/api-integrations/vitally-api.md)** - Customer success platform integration

### Communication
- **[Slack Writer](skills/communication/slack-writer.md)** - Write effective Slack messages (tone, structure, avoiding "See more" collapse)
- **[Customer Response](skills/communication/customer-response.md)** - Technical responses with a sales engineering lens

### Task Management
- **[Taskwarrior](skills/task-management/taskwarrior.md)** - CLI task management with project conventions and inbox processing

### n8n Workflow Automation
- **[Expression Syntax](skills/n8n/expression-syntax.md)** - Write correct n8n expressions (webhook data, node references)
- **[Code Node (JavaScript)](skills/n8n/code-javascript.md)** - JavaScript patterns for n8n Code nodes
- **[Code Node (Python)](skills/n8n/code-python.md)** - Python patterns for n8n Code nodes (beta)
- **[MCP Tools](skills/n8n/mcp-tools.md)** - Master guide for n8n-mcp server tools
- **[Node Configuration](skills/n8n/node-configuration.md)** - Operation-aware configuration with property dependencies
- **[Validation](skills/n8n/validation.md)** - Interpret and fix validation errors
- **[Workflow Patterns](skills/n8n/workflow-patterns.md)** - Architectural patterns (webhook, API, database, AI, scheduled)

### CLAUDE.md Template
- **[CLAUDE-template.md](CLAUDE-template.md)** - Template for configuring Claude Code with CS workflows

## 🗂️ Repository Structure

```
customer-success-playbooks/
├── README.md
├── CLAUDE-template.md          # Template for Claude Code configuration
├── playbooks/
│   ├── onboarding/
│   ├── regular-cadence-meetings/
│   ├── alerts/
│   ├── case-study/
│   └── health-scoring/
└── skills/                     # Claude Code skills
    ├── api-integrations/
    │   ├── hubspot-api.md
    │   ├── zendesk-api.md
    │   └── vitally-api.md
    ├── communication/
    │   ├── slack-writer.md
    │   └── customer-response.md
    ├── task-management/
    │   └── taskwarrior.md
    └── n8n/
        ├── expression-syntax.md
        ├── code-javascript.md
        ├── code-python.md
        ├── mcp-tools.md
        ├── node-configuration.md
        ├── validation.md
        └── workflow-patterns.md
```

## 🚀 Getting Started

### Using Playbooks
1. Browse the `playbooks/` directory to find relevant scenarios
2. Each playbook includes objectives, triggers, steps, and success metrics
3. Customize based on your specific customer segments and business needs

### Using Claude Code Skills
1. Install [Claude Code](https://github.com/anthropics/claude-code)
2. Copy desired skills from `skills/` to `~/.claude/skills/`
3. Reference skills in your `~/.claude/CLAUDE.md` (use the template as a starting point)
4. Skills auto-load based on context when using Claude Code

## 📖 How to Use These Playbooks

Each playbook follows a consistent structure:
- **Objective** - What you're trying to achieve
- **Triggers** - When to execute this playbook
- **Steps** - Detailed action items and processes
- **Success Metrics** - How to measure effectiveness
- **Resources** - Templates, scripts, and supporting materials

## 🤝 Contributing

These playbooks and skills are living documents. As you discover new strategies or improve existing ones:
1. Test the approach with real customers
2. Document results and learnings
3. Update playbooks with proven improvements
4. Share insights with the team

## 📊 Measuring Success

Track the effectiveness of these playbooks using:
- Customer satisfaction scores (CSAT, NPS)
- Product adoption metrics
- Retention and churn rates
- Expansion revenue
- Time-to-value metrics

---

*Last updated: January 2026*
