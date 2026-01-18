# AI Automation for Customer Success

Use Claude Code and n8n to automate CS workflows, reduce manual work, and improve response quality.

![Meeting prep example showing account data, alerts, usage, pipeline, Slack sentiment, support tickets, Linear issues, and last meeting notes aggregated into a single view](assets/meeting-prep-example.jpg)

*Example: Meeting prep that pulls customer context from multiple sources into a single view (illustrative with synthetic data)*

## Objective

Leverage AI tools to:
- Automate repetitive tasks (data lookups, report generation, meeting prep)
- Improve communication quality and consistency
- Build workflow automations without heavy engineering lift

## Getting Started with Claude Code

[Claude Code](https://github.com/anthropics/claude-code) is Anthropic's CLI tool that can read files, execute commands, and integrate with APIs via MCP servers.

### Setup

1. Install Claude Code: `npm install -g @anthropic-ai/claude-code`
2. Copy skills from `skills/` to `~/.claude/skills/`
3. Use [CLAUDE-template.md](../../CLAUDE-template.md) as a starting point for `~/.claude/CLAUDE.md`
4. Skills auto-load based on context

### Available Skills

#### API Integrations
- **[HubSpot API](../../skills/api-integrations/hubspot-api.md)** - CRM integration patterns and common endpoints
- **[Zendesk API](../../skills/api-integrations/zendesk-api.md)** - Support ticket automation and customer data access
- **[Vitally API](../../skills/api-integrations/vitally-api.md)** - Customer success platform integration

#### Communication
- **[Slack Writer](../../skills/communication/slack-writer.md)** - Write effective Slack messages (tone, structure, avoiding "See more" collapse)
- **[Customer Response](../../skills/communication/customer-response.md)** - Technical responses with a sales engineering lens

#### Task Management
- **[Taskwarrior](../../skills/task-management/taskwarrior.md)** - CLI task management with project conventions and inbox processing

#### n8n Workflow Automation
- **[Expression Syntax](../../skills/n8n/expression-syntax.md)** - Write correct n8n expressions (webhook data, node references)
- **[Code Node (JavaScript)](../../skills/n8n/code-javascript.md)** - JavaScript patterns for n8n Code nodes
- **[Code Node (Python)](../../skills/n8n/code-python.md)** - Python patterns for n8n Code nodes (beta)
- **[MCP Tools](../../skills/n8n/mcp-tools.md)** - Master guide for n8n-mcp server tools
- **[Node Configuration](../../skills/n8n/node-configuration.md)** - Operation-aware configuration with property dependencies
- **[Validation](../../skills/n8n/validation.md)** - Interpret and fix validation errors
- **[Workflow Patterns](../../skills/n8n/workflow-patterns.md)** - Architectural patterns (webhook, API, database, AI, scheduled)

## Use Cases

### Meeting Prep
Pull customer context from multiple sources (CRM, support tickets, usage data) into a single summary before calls.

### Customer Communication
Draft Slack messages and email responses with consistent tone and structure. The Slack Writer skill handles character limits and formatting.

### Data Lookups
Query customer data across systems without switching between dashboards. Configure API skills with your credentials.

### Workflow Automation
Build n8n workflows for recurring tasks: weekly reports, alert routing, data syncs between platforms.

## Success Metrics

- Time saved on manual data gathering
- Response time improvements
- Consistency in customer communications
- Number of automated workflows deployed

## Resources

- [CLAUDE-template.md](../../CLAUDE-template.md) - Template for configuring Claude Code
- [Claude Code Documentation](https://github.com/anthropics/claude-code)
- [n8n Documentation](https://docs.n8n.io/)
