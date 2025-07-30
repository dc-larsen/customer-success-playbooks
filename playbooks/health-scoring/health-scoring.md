# Health Scoring Playbook

## 🎯 Purpose
To guide CSMs on building and maintaining effective customer health scoring practices, ensuring early detection of risks and opportunities for growth or retention.

## 📊 Foundational Context

- **If a company has a health scoring dashboard, use it; if not, this playbook provides what to track manually or in a lightweight system**
- **The goal is to use product and customer data to get ahead of risks, improve adoption, and support renewals**
- **This is especially relevant for technical SaaS products where usage and integration data is abundant**

## 📈 Core Metrics to Track

### A. Login Trends
- **Track last login timestamp and overall login frequency**
- **For most customers (~95%), frequent logins indicate importance to the business; low or declining logins can indicate disengagement**
- **Caveats:** Some customers may use integrations (e.g., API, Jira automation) instead of logging in. For these, login frequency may be less relevant
- **Focus on trends rather than setting an arbitrary "good" threshold**

### B. "Value Activity" Frequency
- **Identify the key action in your product that delivers the most value** (e.g., in a vulnerability scanner: marking vulnerabilities as remediated)
- **Track the frequency of this activity over time** — a drop from 30/month to 2/month signals possible risks or product concerns
- **Investigate drops quickly; they can surface hidden issues** (e.g., feature removed due to performance concerns)

### C. Integration Health & Trends
- **Track which integrations are active** (e.g., Jira, Slack, API, etc.)
- **Go beyond "is it connected?" to quantitative data:**
  - **Jira:** number of tickets sent per week/month, percentage closed
  - **Slack:** number of messages sent, number of active channels
- **Look for trend changes** — e.g., 100 Slack messages/month dropping to 0 signals something to investigate

## 📊 Trend-Based Monitoring vs. Static Thresholds

- **Health scoring should prioritize changes over time, not fixed numbers**
- **Trend shifts are more valuable signals than raw counts**

## ⏰ Operational Cadence

- **Set aside weekly focus time (1–1.5 hours) to review health dashboards for all customers**
- **Move faster for customers you work closely with; spend more time on less-engaged customers**
- **Use this review to identify risks, adoption wins, and opportunities for expansion**

## 🚀 Proactive Influence in Early-Stage Companies

- **If dashboards don't exist, help define what to track and why**
- **As a CSM, you are well-placed to identify data points that matter for retention and expansion**
- **Share insights with product, engineering, and leadership** — if it's valuable for you, it's likely valuable for the company

## 🔗 Integration with Other Playbooks

- **Alerts Playbook** → Create alerts for sudden drops in login frequency, value activity, or integration usage
- **Onboarding Playbook** → Define the customer's "value activity" during onboarding
- **Case Studies Playbook** → Use strong KPI improvements from health scoring as evidence for case study readiness

---

*Health scoring is about spotting patterns, not hitting numbers. Watch the trends, act on the changes.*
