# Health Scoring

**How to build an early warning system, not just a dashboard**

Most health scores are retrospective. They tell you a customer is at risk after they've already disengaged. By then, you're playing defense.

A good health system spots weakening signals early, tracks whether they recover, and tells you what to do next. This playbook covers how to build one.

---

## 1. What health scoring actually measures

Health scoring answers one question: **Is this customer getting closer to or further from the outcome they bought us for?**

That means every metric you track should connect back to why they purchased. A customer who bought for compliance and logs in quarterly might be healthy. A customer who bought for daily security monitoring and hasn't logged in for two weeks is not.

Context matters more than counts.

---

## 2. Signals, detectors, and decisions

Keep these three layers separate. Mixing them creates "health score soup" that nobody can explain or act on.

**Signals:** Raw measurements
- Login count, last login date
- Value activity frequency
- Integration volume (tickets created, messages sent)
- Support ticket trends

**Detectors:** Logic that spots patterns
- Drop with no recovery
- Accelerating decline
- Sudden regime change (100 → 0)
- Champion gone silent

**Decisions:** Risk state + reasoning
- Stored in your CRM (Vitally, Gainsight, etc.)
- Includes: risk level, why, evidence, recommended action

This keeps it explainable. When someone asks "why is this account yellow?" you can point to the detector and the evidence.

---

## 3. Core signals to track

### A. Login trends

Track last login timestamp and overall login frequency.

For most customers (~95%), frequent logins indicate the product matters to their workflow. Low or declining logins can indicate disengagement.

**Caveats:** Some customers use integrations (API, Jira automation, CI/CD pipelines) instead of logging in directly. For these, login frequency is less relevant. Gate this signal based on how they use the product.

### B. Value activity frequency

Identify the key action in your product that delivers the most value.

**Examples by product type:**
- Vulnerability scanner: marking vulnerabilities as remediated
- CRM: deals moved to closed-won
- Support platform: tickets resolved
- Analytics tool: reports exported or shared

Track the frequency of this activity over time. A drop from 30/month to 2/month signals possible risks or product concerns.

### C. Integration health

Go beyond "is it connected?" to quantitative data:
- **Jira:** tickets sent per week, percentage closed
- **Slack:** messages sent, active channels
- **API:** call volume, error rates

Look for trend changes. 100 Slack messages/month dropping to 0 signals something to investigate.

---

## 4. Trajectory over snapshots

Health scoring should prioritize changes over time, not fixed numbers.

**Track four properties for each signal:**

| Property | What it tells you |
|----------|-------------------|
| **Level** | Where the metric is now |
| **Slope** | Direction over the last 30/60/90 days |
| **Acceleration** | Is the decline speeding up or slowing down |
| **Recovery** | Does it bounce back after dips, or stay depressed |

**Escalate based on patterns, not single data points:**
- One-off drops are noise
- Repeated drops with weaker rebounds are risk
- Persistent decline across multiple signals is high risk

---

## 5. Response framework

Every detector should map to a recommended action.

| Pattern | Risk level | Action |
|---------|------------|--------|
| Single signal drop, recovers within 2 weeks | Monitor | Note it, no outreach needed |
| Single signal drop, no recovery after 3 weeks | Yellow | Reach out to check in, understand context |
| Multiple signals declining, champion responsive | Yellow | Schedule working session, re-align on goals |
| Multiple signals declining, champion silent | Red | Exec alignment call, confirm renewal intent |
| Sudden regime change (active → zero) | Red | Immediate outreach, assume something broke |

**Every alert should attach evidence:** dates, deltas, links to Slack threads or call notes. If you can't show your work, the alert isn't actionable.

---

## 6. Operational cadence

Set aside weekly focus time (1-1.5 hours) to review health across your book.

- Move faster for engaged customers (quick scan)
- Spend more time on quiet customers (they're where risk hides)
- Use this review to identify risks, adoption wins, and expansion signals

---

## 7. Building your own system

If dashboards don't exist at your company, build a lightweight version yourself.

**Minimum viable health tracker (spreadsheet or Notion):**

| Customer | Last login | Login trend | Value activity (30d) | Trend | Integrations active | Notes | Risk level |
|----------|------------|-------------|---------------------|-------|---------------------|-------|------------|
| Acme Corp | 2024-01-15 | ↓ declining | 12 (was 25) | ↓ | Jira, Slack | Champion on leave | Yellow |

Update weekly. Share patterns with product and leadership. If it's valuable for you, it's likely valuable for the company.

---

## 8. Connection to other playbooks

- **Onboarding:** Define the customer's "value activity" during onboarding. You can't measure health if you don't know what success looks like.
- **Regular cadence:** Use health signals to prioritize which accounts need attention and what to discuss.
- **Alerts:** Create automated alerts for sudden drops in login frequency, value activity, or integration usage.

---

## Final thought

Health isn't a number. It's whether key signals are weakening over time, how fast they're weakening, and whether they recover after you intervene.

Watch the trajectory. Act on the pattern. Document the evidence.

If this helps, use it. Or send it to whoever owns your health score model and ask why it doesn't work like this yet.
