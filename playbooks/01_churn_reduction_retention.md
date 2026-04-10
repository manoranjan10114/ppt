# PLAYBOOK 01 — Churn Reduction & Customer Retention

> Owner: CSM Lead + Product
> Target: Churn 30% → 15% · Recover ₹35L+/year · Timeline: Q1–Q2

---

## THE OPPORTUNITY WE'RE SITTING ON

We lost 535 customers last year. That's not just a churn number — it's a list of hotels that already trusted us once, already went through onboarding, already had their staff trained. Winning them back is 5–10x cheaper than acquiring a new customer.

Before we focus on new acquisition, we mine this goldmine first.

---

## PHASE 1: WINBACK CAMPAIGN — CHURNED HOTELS (Weeks 1–6)

### Step 1: Segment the 535 Churned Hotels by Churn Cause

Pull the data and classify every churned hotel into one of these buckets:

| Churn Cause                                                 | Estimated %       | Winback Difficulty | Priority  |
| ----------------------------------------------------------- | ----------------- | ------------------ | --------- |
| Price/value mismatch — felt too expensive for what they got | ~25% (134 hotels) | Easy               | 🟢 First  |
| Poor onboarding — never fully adopted the product           | ~20% (107 hotels) | Medium             | 🟢 First  |
| Support issues — slow SLA, unresolved tickets               | ~20% (107 hotels) | Medium             | 🟡 Second |
| Switched to competitor                                      | ~15% (80 hotels)  | Hard               | 🟡 Second |
| Business closed / seasonal / external reasons               | ~10% (54 hotels)  | Not winnable       | 🔴 Skip   |
| Low usage — just stopped using, no specific complaint       | ~10% (54 hotels)  | Medium             | 🟡 Second |

**Action:** CSM team + data team classify all 535 hotels within Week 1. Use support ticket history, cancellation reason (if captured), last usage date, and CSM notes.

**Discussion Point:** Do we have cancellation reason data? If not, this is a gap to fix immediately for future churn — every cancellation must capture a reason.

---

### Step 2: Design Winback Nudge Content by Churn Cause

Each segment gets a different message because their reason for leaving was different.

#### Segment A: Price/Value Mismatch (~134 hotels) — HIGHEST PRIORITY

These hotels thought we were too expensive. Now we have new pricing (₹1K/₹2K/₹3K tiers). This is the easiest winback.

**Nudge sequence:**

| Day    | Channel     | Message                                                                                                                                                                                                                            |
| ------ | ----------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Day 0  | Email       | Subject: "We listened. New pricing starts at ₹1K/month." Body: Acknowledge they left, introduce new 3-tier pricing, highlight that Basic is ₹1K/mo with core features. Include ROI calculator link.                                |
| Day 2  | WhatsApp    | "Hi [name], we've simplified our pricing — 3 plans starting ₹1K/mo. Want a quick 10-min walkthrough of what's changed?"                                                                                                            |
| Day 5  | CSM Call    | Personal call from a CSM (not the one who managed them before if possible). Script: "We've restructured everything based on feedback from hotels like yours. I'd love to show you the new setup — no commitment, just 10 minutes." |
| Day 8  | Email       | Case study: "How [Hotel X] saved ₹Y/month in OTA commissions after switching to our Booking Engine"                                                                                                                                |
| Day 12 | WhatsApp    | Limited-time offer: "Come back this month and get 2 months free on any plan"                                                                                                                                                       |
| Day 15 | Final email | "We'd love to have you back. Here's a direct link to reactivate your account on the new pricing."                                                                                                                                  |

**Offer:** 2 months free on any tier + dedicated re-onboarding support.

---

#### Segment B: Poor Onboarding (~107 hotels)

These hotels signed up but never got value. They need to see that the experience is different now.

**Nudge sequence:**

| Day    | Channel  | Message                                                                                                                                                                                        |
| ------ | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Day 0  | Email    | Subject: "We've rebuilt our onboarding — and we owe you a redo." Body: Acknowledge the past experience was not good enough. Introduce the new 30-day structured onboarding with dedicated CSM. |
| Day 3  | CSM Call | "We know your first experience wasn't great. We've completely redesigned how we onboard hotels. I'd like to personally walk you through the new process — 15 minutes, no commitment."          |
| Day 6  | WhatsApp | Short video (60 sec) showing the new onboarding flow — setup to live in 48 hours                                                                                                               |
| Day 10 | Email    | Offer: "Reactivate and get a dedicated onboarding specialist for your first 30 days — free"                                                                                                    |

**Offer:** Free re-onboarding with a dedicated specialist + 1 month free.

---

#### Segment C: Support Issues (~107 hotels)

These hotels had bad support experiences. They need proof that support has improved.

**Nudge sequence:**

| Day   | Channel  | Message                                                                                                                                                   |
| ----- | -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Day 0 | Email    | Subject: "We fixed what was broken." Body: Specific improvements — new SLA targets, AI chatbot for instant answers, dedicated support for Growth/Premium. |
| Day 3 | CSM Call | Apologize specifically for their past experience (reference their actual tickets if possible). Explain what's changed.                                    |
| Day 7 | Email    | Share new SLA commitments in writing. Offer: "If we miss our SLA even once in your first 3 months, your next month is free."                              |

**Offer:** SLA guarantee + 1 month free.

---

#### Segment D: Switched to Competitor (~80 hotels)

Hardest to win back but worth trying for the larger ones.

**Nudge sequence:**

| Day    | Channel  | Message                                                                                                                                                                       |
| ------ | -------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Day 0  | Email    | Subject: "A lot has changed since you left." Body: Feature comparison — what we've added that competitors don't have (direct booking engine, AI health scoring, new pricing). |
| Day 7  | CSM Call | "I know you moved to [competitor]. I'm not here to hard-sell — just wanted to share what's new. If you're ever evaluating again, we'd love to be in the conversation."        |
| Day 14 | Email    | ROI comparison: "Hotels on our platform save ₹X/month in OTA commissions through direct bookings"                                                                             |

**Offer:** 3 months free + free data migration from competitor.

---

### Step 3: Winback Targets & Tracking

| Metric                   | Target                                                |
| ------------------------ | ----------------------------------------------------- |
| Churned hotels contacted | 100% of Segments A, B, C (348 hotels)                 |
| Response rate            | 25%+                                                  |
| Winback conversion       | 15% of contacted (52+ hotels)                         |
| Revenue recovered        | ~₹8–10L/year (52 hotels × ₹1.5K avg ARPU × 12 months) |
| Timeline                 | Weeks 1–6                                             |

**Tracking:** Create a "Winback Pipeline" in CRM. Every churned hotel gets a status: Not Contacted → Contacted → Responded → Demo/Call Done → Reactivated → Lost.

---

## PHASE 2: PREVENT FUTURE CHURN — SYSTEM BUILD (Q1)

### Account Health Scoring System

**What it does:** Every active account gets a health score (0–100) updated weekly.

**Inputs:**

| Signal                                                | Weight | Data Source       |
| ----------------------------------------------------- | ------ | ----------------- |
| Product login frequency (last 30 days)                | 25%    | Product analytics |
| Feature usage depth (% of paid features used)         | 20%    | Product analytics |
| Support ticket volume & sentiment                     | 15%    | Support system    |
| Payment history (on-time, late, failed)               | 15%    | Billing system    |
| CSM engagement (responded to check-ins, attended QBR) | 10%    | CRM               |
| Days to renewal                                       | 10%    | Billing system    |
| Booking Engine usage (if applicable)                  | 5%     | Product analytics |

**Classification:**

| Score  | Status      | Action                                                              |
| ------ | ----------- | ------------------------------------------------------------------- |
| 80–100 | 🟢 Healthy  | Standard cadence. Upsell opportunity.                               |
| 50–79  | 🟡 At Risk  | CSM proactive outreach within 7 days. Identify issue.               |
| 0–49   | 🔴 Critical | CSM + Manager intervention within 48 hours. Escalation to CSM Lead. |

**Discussion Points:**

- Do we have all these data sources accessible via API today?
- Who builds the scoring model — data team, product team, or external vendor?
- MVP approach: start with a simple weighted formula, iterate to ML later
- Dashboard: real-time or daily refresh?

---

### Churn Alert System

**Trigger rules:**

| Trigger                                      | Alert                             |
| -------------------------------------------- | --------------------------------- |
| Health score drops below 50                  | Immediate alert to CSM + CSM Lead |
| No login for 14+ days                        | Alert to CSM                      |
| Support ticket unresolved for 48+ hours      | Alert to CSM + Support Lead       |
| Payment failed                               | Alert to CSM + Finance            |
| Renewal in 45 days + health score < 70       | Alert to CSM + Manager            |
| Renewal in 30 days + no renewal confirmation | Escalation to CSM Lead            |

**Alert channels:** CRM notification + Slack/email to CSM + daily digest to CSM Lead.

---

### Structured Onboarding Playbook (New Customers)

| Day    | Action                                                               | Owner     | Success Criteria                                   |
| ------ | -------------------------------------------------------------------- | --------- | -------------------------------------------------- |
| Day 0  | Welcome email + account setup guide                                  | Automated | Email delivered                                    |
| Day 1  | Welcome call — introduce CSM, set expectations, schedule training    | CSM       | Call completed, training scheduled                 |
| Day 3  | Setup verification — is the hotel live on the platform?              | CSM       | Hotel live, channels connected                     |
| Day 7  | Training session — core features walkthrough                         | CSM       | Training completed, hotel staff trained            |
| Day 14 | Usage check — are they actually using the product?                   | CSM       | Login in last 7 days, at least 1 booking processed |
| Day 21 | Feature adoption push — introduce Booking Engine / advanced features | CSM       | BE setup initiated (if on Growth/Premium)          |
| Day 30 | Health review — first health score check, feedback collection        | CSM       | Health score > 70, NPS collected                   |

**If Day 14 check fails (no usage):** Escalate to CSM Lead. Trigger a "rescue" call — find out what's blocking adoption and fix it within 48 hours.

---

### Monthly Engagement Cadence (Existing Customers)

| Account Tier     | Engagement                                          | Frequency                      |
| ---------------- | --------------------------------------------------- | ------------------------------ |
| Premium (₹3K/mo) | Personal CSM call + QBR quarterly                   | Monthly call, Quarterly QBR    |
| Growth (₹2K/mo)  | Personal CSM call                                   | Monthly call                   |
| Basic (₹1K/mo)   | Automated check-in email + CSM call if health drops | Monthly email, call if at-risk |

**Automated touchpoints (all tiers):**

- Monthly usage summary email ("You processed X bookings this month, Y were direct")
- Feature tip emails (bi-weekly, based on unused features)
- Product update announcements
- Satisfaction pulse survey (quarterly, 1-question NPS)

---

### Renewal Pipeline Management

| Days to Renewal | Action                                                                                |
| --------------- | ------------------------------------------------------------------------------------- |
| 90 days         | Renewal flagged in CSM dashboard. CSM reviews health score.                           |
| 60 days         | CSM outreach: "Your renewal is coming up. Let's review how things are going."         |
| 45 days         | If health < 70: escalation to CSM Lead. Rescue plan initiated.                        |
| 30 days         | Renewal reminder email (automated). CSM confirms renewal intent.                      |
| 15 days         | If no confirmation: CSM + Manager call. Offer incentive if needed.                    |
| 7 days          | Final reminder. If still no confirmation: CEO-level outreach for high-value accounts. |
| 0 days          | Auto-renewal (if enabled) or account flagged as churned.                              |

---

### CSM Performance Metrics & Incentives

**Each CSM is measured on:**

| Metric                       | Weight in Variable Pay | Target                                       |
| ---------------------------- | ---------------------- | -------------------------------------------- |
| Churn rate (their portfolio) | 30%                    | < 15% annual                                 |
| Renewal rate                 | 25%                    | > 90%                                        |
| Upsell revenue generated     | 20%                    | ₹X per quarter (TBD based on portfolio size) |
| Health score improvement     | 15%                    | Average portfolio health > 70                |
| Customer NPS                 | 10%                    | > 40                                         |

**Weekly team review:**

- Every Monday, 30-min standup
- Each CSM reports: Red accounts (what's the plan?), renewals this month (status?), upsell pipeline
- CSM Lead flags systemic issues to Product/Engineering

---

## SUCCESS METRICS

| Metric                  | Current      | 3-Month Target       | 6-Month Target               |
| ----------------------- | ------------ | -------------------- | ---------------------------- |
| Churn rate (annual)     | 30%          | 22%                  | 15%                          |
| Churned hotels won back | 0            | 52+                  | 80+                          |
| Average health score    | Unknown      | 65                   | 75                           |
| Renewal rate            | ~70%         | 80%                  | 85%+                         |
| CSM portfolio coverage  | Inconsistent | 100% accounts scored | 100% with monthly touchpoint |

---

## OPEN QUESTIONS FOR DISCUSSION

1. Do we have cancellation reason data for all 535 churned hotels? If not, can we survey them?
2. What's the budget for winback offers (free months, re-onboarding)?
3. Health scoring: build in-house or buy a tool? What's the timeline difference?
4. CSM capacity: can current team handle monthly calls for 1,800 accounts? If not, what's the segmentation?
5. Auto-renewal: what % of customers are on auto-renewal today? Can we increase it?
6. Who owns the winback campaign — CSM team or a dedicated winback squad?
