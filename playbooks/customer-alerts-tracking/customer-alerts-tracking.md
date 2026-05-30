# Customer Alerts Tracking

**Turn the alerts playbook into a system that pings you**

Knowing which alerts matter is half the job. The other half is building something that watches for them, so you stop pulling reports by hand. This playbook is the how behind the Proactive Alerts playbook.

---

## 1. Start from the alerts that matter

Don't track everything. Pick the handful that change what you do today: usage dropping, license near the limit, a renewal countdown, a risk field flipping, a long gap since the last call. The Proactive Alerts playbook has the full catalog.

---

## 2. Know where the data lives

Each alert reads from a source:
- CRM or CS platform (Vitally, HubSpot) for renewal dates, risk fields, and seats
- Product usage data for logins and value activity
- Support tickets for friction

You can't alert on data you can't query.

---

## 3. Build the watcher

A scheduled job checks the sources, applies your thresholds, and routes a message when one trips. n8n, Zapier, or a cron script all work. Run it daily, or hourly for the urgent ones.

---

## 4. Make every alert actionable

An alert that says "usage dropped" creates work. An alert that says "usage dropped 40% at Acme, champion hasn't logged in for 14 days, renewal in 60 days" creates action. Attach the context and a suggested next step.

---

## 5. Tune it so people keep reading

The fastest way to kill an alert system is noise. Send only alerts someone will act on, route each to the right owner, and review the thresholds every quarter. An ignored alert is worse than none.

---

## 6. Connection to other playbooks

- **Proactive Alerts:** The catalog of what to watch for.
- **Health Scoring:** The signals and trends most alerts read from.
- **Renewal Risk Governance:** A risk-field change should fire an alert.
- **Claude Code Skills:** Build the watcher and the digest as skills.

---

## Final thought

The point isn't more alerts, it's fewer reports. Watch the handful that change your day, attach the context, and let the system pull you to the account that needs you.
