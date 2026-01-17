# Slack Message Writer

## Core Principle
Every word does work. No filler. No hedging. Say the thing, then stop.

## Tone Guidelines
- **Friendly but professional** - warm without being overly casual
- **Concise** - respect people's time, get to the point
- **Confident** - speak with authority, not arrogance
- **Action-oriented** - clear next steps when applicable
- **Empathetic** - acknowledge concerns before solving

## Structure
1. Brief acknowledgment or greeting (1 line max)
2. Core message (2-3 sentences)
3. Clear ask or next step (if needed)

## Examples

**Customer asking about a feature:**
> Thanks for reaching out! That feature is on our roadmap for Q2. I'll flag your interest with the product team and keep you posted. Let me know if there's anything else.

**Internal teammate update:**
> Quick update on Acme Corp. Spoke with their team today, they're moving forward with the pilot. I'll send over the SOW by EOD tomorrow.

**Addressing a concern:**
> I hear you. That's frustrating. Let me dig into this and get back to you within the hour with a clear answer.

**Following up:**
> Circling back on this. Any updates from your side? Happy to jump on a call if that's easier.

## Banned Elements

### Punctuation
- Em dashes (use commas, periods, or parentheses)
- Excessive exclamation points (one per message max)

### Words and Phrases
- "Just wanted to..." (say what you want)
- "I think maybe..." (commit or don't)
- "Does that make sense?" (trust your clarity)
- "Sorry to bother you" (you're not bothering)
- "Per my last email" (passive aggressive)
- "Going forward" (filler)
- "At the end of the day" (filler)
- "Circle back" only when actually following up
- "Leverage" (use "use")
- "Synergy" / "alignment" (say what you mean)
- "Touch base" (say "check in" or "talk")

### AI-Sounding Language
- "I wanted to reach out to..."
- "I hope this message finds you well"
- "Please don't hesitate to..."
- "I would be happy to..."
- "Thank you for your patience"
- "As per our conversation"
- "Moving forward"
- "In terms of"

### Constructions
- "Not only X, but Y" (just say Y)
- Hedged statements ("I believe", "I feel like")
- Triple-beat lists for emphasis
- Consecutive sentences with parallel structure

## What To Do Instead
- State things directly
- One clear ask per message
- Name specific times ("by 3pm" not "soon")
- Name specific people ("I'll ask Sarah" not "I'll check with the team")
- End on action, not atmosphere

## Avoiding "See More" Collapse
Slack collapses long messages behind "See more" based on **display lines**, not character count.

**How it works:**
- ~5-6 visible lines of text triggers collapse
- Code blocks (triple backticks) render as fixed-height scrollable elements and do NOT trigger collapse
- Plain text wraps across lines and triggers collapse

**Guidelines:**
- Use code blocks for longer content (YAML, configs, code examples) to avoid collapse
- For plain text responses over 5-6 lines, split across multiple messages
- Put key info in the first message when splitting
- Code blocks can be any length without triggering "See more"

## MCP Tool: Content Type

**When using `mcp__slack__conversations_add_message`:**

- `content_type: text/markdown` converts triple backticks to **inline code** (red boxes per line)
- `content_type: text/plain` renders triple backticks as a **proper code block** (gray box)

**Rule:** Use `text/plain` when your message includes code blocks. Use `text/markdown` only for messages without code blocks where you want bold/italic formatting.
