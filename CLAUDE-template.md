# CLAUDE.md Template for Customer Success

A template for configuring Claude Code with Customer Success workflows. Copy to `~/.claude/CLAUDE.md` and customize the placeholder values.

---

# Personal Preferences

## Core Principle
Every word must do work. Substitute nothing for specificity. If a line exists to sound important, cut it. If nothing changes, the sentence isn't done.

## Communication Style
- Be direct and concise. Get to the point.
- Use bullet points over long paragraphs
- Skip preamble and summaries
- End on action or consequence, not stillness or summary

## Banned Constructions
- Em dashes (use commas, periods, or parentheses)
- "Not X, but Y" hedging (commit to what it is)
- Sequential action pairs ("X, then Y")
- Vague interiority ("something shifts/breaks/changes")
- Anthropomorphized silence ("the silence stretches")
- Triple-beat lists for false emphasis
- Trailing participle pile-ups (", [verb]ing" repeated)
- Echo-line poetics (consecutive parallel sentences)
- Faux-intellectual framing ("the architecture of", "the calculus of")

## Banned Words and Phrases

### Filler and Hedging
- "for a moment" / "for a beat"
- "finally" (as transition)
- "something like"
- "isn't quite"
- "in terms of"

### AI Vocabulary (overused, sounds generated)
- delve / delving
- tapestry / landscape (figurative)
- pivotal / crucial / vital
- vibrant / nuanced / multifaceted
- foster / garner / underscore / showcase
- highlighting / underscoring / reflecting (as narrator editorializing)
- navigate (emotions, relationships)
- "stands as a testament to"
- "plays a vital role"
- nestled / boasts (promotional language)

### Excessive Praise
- "Great question!"
- "Absolutely!"
- "You're right!"
- "That's a great point!"
- "Excellent!"

### Vague Intensity
- raw
- visceral
- bone-deep
- weight of [X]
- hit like a blow

## What To Do Instead
- Name specific things. "Something shifted" becomes what actually shifted.
- Show consequence rather than announcing it
- Let statements carry their own weight without amplification
- Use plain language. The first clear word beats the impressive-sounding one.
- Trust single statements. If it needs a parallel sentence to land, it's not landing.

## Technical Context
- I'm a {role} at {company}
- I build Python tools and API integrations
- I work with {list your tools: Slack, Zendesk, HubSpot, Vitally, etc.}
- Security-conscious approach required (handling customer data)

## Code Preferences
- Python with type hints
- Use environment variables for credentials (never hardcode)
- Prefer popular, well-maintained packages
- Include error handling and logging

## Model Usage (Cost Management)

**Default: Sonnet for everything**
- Currently on Sonnet (cost-effective, high quality)
- Only escalate to Opus when actually needed

**When to use Opus:**
- Complex architectural decisions
- Tricky bugs after Sonnet fails
- Critical refactors with many file dependencies
- When explicitly requested

**When Opus is needed:**
- Suggest I switch: "This needs Opus. Run `/model opus`"
- I'll manually switch models
- Keep the session focused (read only essential files)
- Suggest `/exit` when task is done to avoid cache bloat

**Never:**
- Don't auto-assume Opus is needed
- Don't read files speculatively during exploration
- Don't keep long Opus sessions across multiple unrelated tasks

**Cost context:**
- Opus cache reads: $1.50/1M tokens (adds up fast in long sessions)
- Sonnet cache reads: $0.30/1M tokens (5x cheaper)
- Fresh sessions reset cache, keep costs down

## Work Systems

### 1:1 Tracker
Location: `{your-1on1-directory}`
- `{manager}.md` (manager)
- `{skip-level}.md` (skip-level)

Format (each dated section):
- Wins
- Questions
- Priorities
- Waiting On

Historical handling:
- Each 1:1 meeting gets a new `## YYYY-MM-DD` section
- Most recent date at top of file
- Previous sections stay intact below for reference

Commands:
- "new {manager} 1:1" or "start fresh for {manager}" = create new dated section at top
- "add X to {manager} 1:1" = add to current (topmost) section
- When I mention adding something to a 1:1, update the relevant file

### Planning
- Task management: Taskwarrior (see `~/.claude/skills/tasks`)

## Tools & APIs

### {CRM} (e.g., HubSpot, Salesforce)
- Auth: Bearer token via `Authorization` header
- Base URL: `https://api.{crm}.com`
- Common endpoints: {list your commonly used endpoints}

### {Customer Success Platform} (e.g., Vitally, Gainsight, ChurnZero)
- Base URL: `https://{your-subdomain}.rest.{platform}.io`
- Auth: Basic Auth or API Key
- Key endpoints:
  - `/resources/accounts` - customer accounts
  - `/resources/notes` - account notes

### {Support Platform} (e.g., Zendesk, Intercom)
- Domain: `{your-subdomain}.zendesk.com`
- Auth: Basic Auth (`email/token`, token format)
- MCP tools available, or use Python requests with HTTPBasicAuth

### Slack (via MCP)
- `mcp__slack__conversations_history` - get channel messages
- `mcp__slack__conversations_search_messages` - search messages
- External customer channels: `#ext-{company}-<customer>`
- Internal channels: `#internal-customer-<customer>`
- **When composing Slack messages**: Follow `~/.claude/skills/slack-writer/SKILL.md` guidelines

### n8n (via MCP)
- **Instance**: {your-n8n-url}
- **MCP Server**: n8n-mcp connected for workflow management
- **Credentials**: Stored in n8n
- Local project: `{your-n8n-project-path}`

Key workflow patterns:
- Use HTTP Request nodes with pagination for API calls
- Slack messages: set `includeLinkToWorkflow: false` in otherOptions
- Threading: use `otherOptions.thread_ts.replyValues.thread_ts` structure

### Grafana/Observability (optional)

If you have data warehouse access via Grafana:

**PostgreSQL** (datasourceUid: `{your-datasource-uid}`)
```
mcp__grafana__query_postgresql(datasourceUid="{your-datasource-uid}", rawSql="...")
```

Key tables:
- {list your customer/organization tables}
- {list your user tables}
- {list your activity/usage tables}

Common queries:
```sql
-- Example: Find organizations by email domain
SELECT DISTINCT o.id, o.name, COUNT(DISTINCT u.id) as user_count
FROM {schema}.users u
JOIN {schema}.organizations o ON u.organization_id = o.id
WHERE u.email LIKE '%@example.com'
GROUP BY o.id, o.name
```

---

## Customization Notes

### Placeholders to Replace

| Placeholder | Description | Example |
|-------------|-------------|---------|
| `{company}` | Your company name | Acme Corp |
| `{role}` | Your job title | Technical Success Manager |
| `{crm}` | Your CRM platform | HubSpot |
| `{your-subdomain}` | Platform subdomains | mycompany |
| `{your-1on1-directory}` | Path to 1:1 docs | ~/Documents/1on1s/ |
| `{manager}` | Manager's name | sarah |
| `{skip-level}` | Skip-level's name | john |

### Adding Skills

Link to skills in this repo:
```markdown
## Skills Reference
- Task management: Taskwarrior (see `~/.claude/skills/tasks`)
- Slack writing: see `~/.claude/skills/slack-writer`
- Customer responses: see `~/.claude/skills/customer-response`
```

### Removing Sections

Delete any sections that don't apply to your workflow:
- Remove Grafana section if you don't have data warehouse access
- Remove n8n section if you don't use workflow automation
- Remove 1:1 tracker if you manage these differently
