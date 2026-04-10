# PLAYBOOK 05 — AI Transformation Strategy

> Owner: Product + Engineering
> Target: 2–3x team efficiency without additional hiring · Timeline: Q1–Q3

---

## PRINCIPLE: AI AUGMENTS, DOESN'T REPLACE

We're not building AI to fire people. We're building AI so that each CSM can manage 200 accounts instead of 100, each sales rep can handle 3x the leads, and support can resolve 60% of tickets without human intervention.

---

## AI SYSTEM 1: CSM AI (P0 — Q1)

### What It Does

| Capability           | How It Works                                                                                    | Impact                                      |
| -------------------- | ----------------------------------------------------------------------------------------------- | ------------------------------------------- |
| Account Health Score | Weighted score (0–100) from usage, logins, support tickets, payments, engagement                | CSMs know which accounts need attention NOW |
| Churn Prediction     | ML model predicts churn probability 30–45 days before renewal                                   | Early intervention saves accounts           |
| Task Prioritization  | AI ranks CSM's daily task list by urgency and revenue impact                                    | CSMs work on what matters most              |
| Renewal Engine       | Auto-generates renewal tasks 90 days out, sends automated reminders, escalates at-risk renewals | No renewal falls through the cracks         |
| Upsell Triggers      | Detects usage patterns that indicate upgrade readiness                                          | CSMs know when to pitch, not guess          |

### Build Plan

**Phase 1: Health Score MVP (Weeks 1–4)**

| Week   | Deliverable                                                                                                                    |
| ------ | ------------------------------------------------------------------------------------------------------------------------------ |
| Week 1 | Data audit: identify all available signals (product usage, logins, support tickets, payments). Map data sources and APIs.      |
| Week 2 | Define scoring formula: weighted average of 7 signals (see Playbook 01 for weights). Build as a simple rule-based model first. |
| Week 3 | Build dashboard: each CSM sees their portfolio with Red/Amber/Green status. CSM Lead sees team-wide view.                      |
| Week 4 | Launch internally. CSMs start using it. Collect feedback. Iterate.                                                             |

**Phase 2: Churn Prediction (Weeks 5–8)**

| Week   | Deliverable                                                                                         |
| ------ | --------------------------------------------------------------------------------------------------- |
| Week 5 | Pull historical data: all churned accounts from last 2 years + their signals before churn.          |
| Week 6 | Train a simple model (logistic regression or decision tree) on historical churn data.               |
| Week 7 | Validate: does the model correctly identify accounts that actually churned? Target: 70%+ accuracy.  |
| Week 8 | Deploy: churn probability score added to health dashboard. Alerts triggered when probability > 60%. |

**Phase 3: Renewal Engine + Upsell Triggers (Weeks 8–12)**

| Week       | Deliverable                                                                                                                                          |
| ---------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Week 8–9   | Renewal automation: auto-create renewal tasks 90 days out, auto-send reminder emails at 60/30/7 days.                                                |
| Week 10–11 | Upsell triggers: define 5 trigger rules (e.g., "hotel hit 90% room quota" → suggest upgrade, "hotel searched for BE feature" → suggest Growth plan). |
| Week 12    | Full CSM AI system live. All CSMs trained.                                                                                                           |

**Tech decisions to make:**

| Decision                      | Options                                               | Recommendation                                                |
| ----------------------------- | ----------------------------------------------------- | ------------------------------------------------------------- |
| Where does health score live? | Custom dashboard, CRM plugin, or product UI           | Start with a simple web dashboard. Integrate into CRM later.  |
| ML framework                  | Scikit-learn, TensorFlow, or cloud ML (AWS SageMaker) | Scikit-learn for MVP. Simple, fast, good enough.              |
| Data pipeline                 | Real-time or daily batch                              | Daily batch for MVP. Real-time for Phase 2+.                  |
| Alert delivery                | Email, Slack, CRM notification                        | Slack + CRM notification for CSMs. Email digest for managers. |

---

## AI SYSTEM 2: SALES AI (P1 — Q2)

### What It Does

| Capability           | How It Works                                                                                                       | Impact                                     |
| -------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------ |
| Lead Scoring         | Score every lead (0–100) based on hotel size, source, engagement, fit                                              | Sales focuses on high-intent leads first   |
| Automated Outreach   | Email/WhatsApp sequences triggered by lead score and behavior                                                      | No lead goes cold                          |
| Demo Assistant       | AI generates personalized demo prep notes per lead: hotel size, likely pain points, recommended tier, ROI estimate | Reps walk into demos prepared              |
| Follow-up Automation | Post-demo: auto-send summary, ROI calculation, next steps                                                          | Consistent follow-up without manual effort |

### Build Plan

| Week     | Deliverable                                                                                                |
| -------- | ---------------------------------------------------------------------------------------------------------- |
| Week 1–2 | Lead scoring model: define criteria (see Playbook 02), implement in CRM as rule-based scoring.             |
| Week 3–4 | Automated outreach: set up email/WhatsApp sequences in CRM or marketing automation tool.                   |
| Week 5–6 | Demo assistant: build a simple tool that pulls hotel data and generates a 1-page prep sheet for each demo. |
| Week 7–8 | Post-demo automation: auto-send personalized follow-up email with ROI calculation within 2 hours of demo.  |

**Build vs Buy:**

| Component           | Build                            | Buy                                | Recommendation                                      |
| ------------------- | -------------------------------- | ---------------------------------- | --------------------------------------------------- |
| Lead scoring        | Custom rules in CRM              | HubSpot/Salesforce built-in        | Use CRM built-in if available. Custom rules if not. |
| Email sequences     | Custom                           | Mailchimp, HubSpot, ActiveCampaign | Buy — not worth building                            |
| WhatsApp automation | Custom via WhatsApp Business API | Interakt, Wati, or similar         | Buy — API integration is complex                    |
| Demo assistant      | Custom (simple script/tool)      | None available for our use case    | Build — lightweight internal tool                   |

---

## AI SYSTEM 3: SUPPORT AI (P1 — Q2–Q3)

### What It Does

| Capability                | How It Works                                                                | Impact                                          |
| ------------------------- | --------------------------------------------------------------------------- | ----------------------------------------------- |
| AI Chatbot                | Handles 60% of support tickets automatically (FAQs, how-tos, status checks) | Frees support team for complex issues           |
| SLA Breach Alerts         | Monitors ticket age and alerts when SLA is about to be breached             | No ticket falls through the cracks              |
| Self-serve Knowledge Base | Searchable help center with articles, videos, and guides                    | Customers find answers without creating tickets |
| OTA Sync Monitoring       | Real-time monitoring of OTA sync health. Alert if sync takes > 15 seconds.  | Zero overbooking goal                           |

### Build Plan

**Phase 1: Knowledge Base (Weeks 1–4)**

| Week     | Deliverable                                                                                 |
| -------- | ------------------------------------------------------------------------------------------- |
| Week 1   | Audit: list top 50 support ticket types by volume. Identify which can be self-served.       |
| Week 2–3 | Write 30 help articles covering the top ticket types. Include screenshots and short videos. |
| Week 4   | Launch knowledge base. Add search. Link from product UI ("Need help? →").                   |

**Phase 2: AI Chatbot (Weeks 5–10)**

| Week     | Deliverable                                                                              |
| -------- | ---------------------------------------------------------------------------------------- |
| Week 5–6 | Choose platform: Intercom, Freshdesk AI, or custom LLM-based bot.                        |
| Week 7–8 | Train bot on knowledge base content + top 50 ticket types.                               |
| Week 9   | Beta launch: chatbot handles Tier 1 queries. Human handoff for anything it can't answer. |
| Week 10  | Full launch. Monitor: what % of queries does the bot resolve without human? Target: 60%. |

**Phase 3: SLA Monitoring + Sync Health (Weeks 8–12)**

| Week       | Deliverable                                                                                                  |
| ---------- | ------------------------------------------------------------------------------------------------------------ |
| Week 8–9   | Define SLA targets per tier (Basic: 24hr, Growth: 12hr, Premium: 4hr). Build SLA tracking in support system. |
| Week 10    | SLA breach alerts: auto-notify support lead when a ticket is approaching SLA limit.                          |
| Week 11–12 | OTA sync monitoring dashboard: real-time sync status per hotel. Alert if sync > 15 seconds.                  |

**Build vs Buy:**

| Component       | Recommendation                                      | Estimated Cost   |
| --------------- | --------------------------------------------------- | ---------------- |
| Knowledge Base  | Buy (Intercom, Freshdesk, or Notion-based)          | ₹5–15K/mo        |
| AI Chatbot      | Buy (Intercom Fin, Freshdesk Freddy, or custom GPT) | ₹10–30K/mo       |
| SLA Tracking    | Built into support tool (Freshdesk, Zendesk)        | Included         |
| Sync Monitoring | Build custom (engineering)                          | Engineering time |

---

## AI SYSTEM 4: ZERO OVERBOOKING INITIATIVE (P1 — Q1–Q2)

### Why This Is Critical

Overbookings destroy trust. If a hotel gets even one overbooking, they lose faith in the system. And if they don't trust the system, they won't push direct bookings through our Booking Engine. The entire direct booking strategy depends on trust.

### Technical Requirements

| Requirement                      | Target          | Current State            |
| -------------------------------- | --------------- | ------------------------ |
| OTA sync time                    | < 15 seconds    | Unknown (measure first!) |
| Sync failure rate                | < 0.1%          | Unknown                  |
| Overbooking rate                 | 0%              | Unknown                  |
| Real-time sync status visibility | Yes (dashboard) | No                       |

### Action Plan

| Week     | Action                                                                                                             |
| -------- | ------------------------------------------------------------------------------------------------------------------ |
| Week 1   | Measure: what's the current average sync time? What's the failure rate? How many overbookings happened last month? |
| Week 2   | Identify bottlenecks: which OTAs have the slowest sync? Where do failures happen?                                  |
| Week 3–4 | Engineering sprint: optimize sync pipeline for sub-15 second performance.                                          |
| Week 5–6 | Build sync health dashboard: real-time status per hotel, per OTA. Visible to hotel + CSM.                          |
| Week 7–8 | Alert system: if sync fails or takes > 15 seconds, alert hotel + CSM + engineering.                                |
| Week 9+  | Monitor and iterate. Target: zero overbookings for 30 consecutive days.                                            |

---

## TEAM & RESOURCE REQUIREMENTS

| AI System       | Engineering Effort                     | Data Requirement                     | External Tools                   |
| --------------- | -------------------------------------- | ------------------------------------ | -------------------------------- |
| CSM AI          | 2 engineers, 8–12 weeks                | Product usage, support, billing data | Dashboard tool (Metabase/Retool) |
| Sales AI        | 1 engineer + marketing ops, 6–8 weeks  | CRM data, lead history               | CRM + email automation tool      |
| Support AI      | 1 engineer + support lead, 10–12 weeks | Support ticket data, product docs    | Chatbot platform + KB tool       |
| Sync Monitoring | 1–2 engineers, 6–8 weeks               | OTA sync logs                        | Custom build                     |

**Total: 5–6 engineers for 3 months** (can be staggered — CSM AI first, then Sales + Support in parallel)

---

## SUCCESS METRICS

| Metric                        | Current | 3-Month Target  | 6-Month Target |
| ----------------------------- | ------- | --------------- | -------------- |
| CSM accounts per person       | ~100    | 150             | 200            |
| Churn prediction accuracy     | N/A     | 70%             | 80%            |
| Lead response time            | Unknown | < 2 hours (Hot) | < 1 hour       |
| Support tickets auto-resolved | 0%      | 30%             | 60%            |
| Average OTA sync time         | Unknown | < 15 sec        | < 10 sec       |
| Overbooking rate              | Unknown | < 0.5%          | 0%             |

---

## OPEN QUESTIONS FOR DISCUSSION

1. Engineering capacity: do we have 5–6 engineers available, or do we need to hire/contract?
2. Data readiness: is all the data (usage, logins, support, billing) accessible via APIs today?
3. ML expertise: do we have someone who can build and maintain ML models?
4. Budget for external tools (chatbot, email automation, dashboard): what's the ceiling?
5. OTA sync: is the current architecture capable of sub-15 sec, or does it need a rewrite?
6. Privacy/compliance: any concerns with AI processing customer data?
