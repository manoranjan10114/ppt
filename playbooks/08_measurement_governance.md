# PLAYBOOK 08 — Measurement & Governance

> Owner: CEO + Finance
> Purpose: Ensure every action in Playbooks 01–07 delivers results · Timeline: Ongoing from Q1

---

## PRINCIPLE: WHAT GETS MEASURED GETS DONE

Strategy without tracking is just a wish list. This playbook defines exactly what we measure, how often, who owns it, and what happens when numbers go red.

---

## THE 6 NORTH STAR METRICS

These are the only 6 numbers the CEO needs to see every week. Everything else is a supporting metric.

| #   | Metric                                   | Current | Q1 Target        | Q2 Target | H2 Target | Owner             |
| --- | ---------------------------------------- | ------- | ---------------- | --------- | --------- | ----------------- |
| 1   | MRR (Monthly Recurring Revenue)          | ₹20L    | ₹25L             | ₹40L      | ₹1 Cr+    | CEO / Finance     |
| 2   | ARPU / Month                             | ₹1,100  | ₹1,400           | ₹1,800    | ₹2,500+   | Product / CSM     |
| 3   | Churn Rate (monthly)                     | ~2.5%   | 2.0%             | 1.5%      | 1.25%     | CSM Lead          |
| 4   | Sales Conversion (lead → sale)           | 8.15%   | 10%              | 13%       | 15%+      | Sales VP          |
| 5   | Upsell Rate                              | ~0%     | 5%               | 10%       | 20%       | CSM Lead          |
| 6   | Direct Booking Share (of total bookings) | Unknown | Measure baseline | 10%       | 20%+      | Product / BE Team |

---

## WEEKLY LEADERSHIP REVIEW

### Format

| Element | Detail                                                                  |
| ------- | ----------------------------------------------------------------------- |
| When    | Every Monday, 9:00 AM, 30 minutes max                                   |
| Who     | CEO, Sales VP, CSM Lead, Product Lead, Finance                          |
| Format  | Dashboard review → Red flags → Actions → Close                          |
| Tool    | Shared dashboard (Google Sheets / Metabase / Notion — decide in Week 1) |

### Agenda (30 minutes)

| Time      | Section          | What Happens                                                                                       |
| --------- | ---------------- | -------------------------------------------------------------------------------------------------- |
| 0–5 min   | Dashboard review | Finance presents the 6 North Star metrics. Green/Amber/Red status for each.                        |
| 5–15 min  | Red flags        | Any metric that's Amber or Red gets discussed. Owner explains why and what they're doing about it. |
| 15–25 min | Action items     | New actions assigned. Previous week's actions reviewed (done / not done / blocked).                |
| 25–30 min | Wins             | One positive highlight from each team. Keep morale up.                                             |

### Status Definitions

| Status   | Meaning                                 | Action Required                                                                                    |
| -------- | --------------------------------------- | -------------------------------------------------------------------------------------------------- |
| 🟢 Green | On track or ahead of target             | Continue. Celebrate.                                                                               |
| 🟡 Amber | Within 10% of target but trending wrong | Owner presents a correction plan within 48 hours                                                   |
| 🔴 Red   | More than 10% below target              | Escalation. CEO + owner create a recovery plan within 24 hours. May require resource reallocation. |

---

## SUPPORTING METRICS BY CATEGORY

### Retention Metrics (Playbook 01)

| Metric                                   | Frequency | Owner    | Source                |
| ---------------------------------------- | --------- | -------- | --------------------- |
| Monthly churn count                      | Weekly    | CSM Lead | Billing system        |
| Churned revenue (₹)                      | Monthly   | Finance  | Billing system        |
| Average health score (portfolio)         | Weekly    | CSM Lead | Health scoring system |
| Red accounts count                       | Daily     | CSM Lead | Health scoring system |
| Renewal rate (renewals due vs completed) | Monthly   | CSM Lead | CRM                   |
| Winback conversions                      | Weekly    | CSM Lead | CRM                   |
| Onboarding completion rate (30-day)      | Monthly   | CSM Lead | Product analytics     |
| NPS score                                | Quarterly | CSM Lead | Survey tool           |

### Sales Metrics (Playbook 02)

| Metric                          | Frequency | Owner    | Source  |
| ------------------------------- | --------- | -------- | ------- |
| New leads this week             | Weekly    | Sales VP | CRM     |
| Lead → Demo rate                | Weekly    | Sales VP | CRM     |
| Demo → Sale rate                | Weekly    | Sales VP | CRM     |
| Average deal cycle (days)       | Monthly   | Sales VP | CRM     |
| Lead response time (hours)      | Weekly    | Sales VP | CRM     |
| Missed lead recovery: contacted | Weekly    | Sales VP | CRM     |
| Missed lead recovery: converted | Weekly    | Sales VP | CRM     |
| Revenue from new sales          | Monthly   | Finance  | Billing |

### Pricing & ARPU Metrics (Playbook 03)

| Metric                                     | Frequency                 | Owner   | Source  |
| ------------------------------------------ | ------------------------- | ------- | ------- |
| ARPU (overall)                             | Monthly                   | Finance | Billing |
| Plan distribution (Basic/Growth/Premium %) | Monthly                   | Product | Billing |
| Migration progress (old plans → new tiers) | Weekly (during migration) | Product | Billing |
| Add-on revenue                             | Monthly (post Q3)         | Product | Billing |
| Annual vs monthly plan split               | Monthly                   | Finance | Billing |

### Direct Booking Metrics (Playbook 04)

| Metric                                      | Frequency | Owner    | Source            |
| ------------------------------------------- | --------- | -------- | ----------------- |
| Hotels with active Booking Engine           | Weekly    | Product  | Product analytics |
| Total direct bookings (all hotels)          | Weekly    | Product  | Booking data      |
| Direct booking % of total                   | Monthly   | Product  | Booking data      |
| Estimated OTA commission saved (all hotels) | Monthly   | Product  | Booking data      |
| BE activation rate (new setups / eligible)  | Monthly   | CSM Lead | Product analytics |

### AI Metrics (Playbook 05)

| Metric                                    | Frequency | Owner        | Source         |
| ----------------------------------------- | --------- | ------------ | -------------- |
| Health score coverage (% accounts scored) | Weekly    | Product      | Health system  |
| Churn prediction accuracy                 | Monthly   | Product      | ML model       |
| Support tickets auto-resolved (%)         | Weekly    | Support Lead | Support system |
| Average OTA sync time                     | Daily     | Engineering  | Monitoring     |
| Overbooking count                         | Weekly    | Engineering  | Product data   |

### Upsell Metrics (Playbook 06)

| Metric                          | Frequency | Owner    | Source            |
| ------------------------------- | --------- | -------- | ----------------- |
| Upsell opportunities identified | Weekly    | CSM Lead | CRM / AI triggers |
| Upsell conversations initiated  | Weekly    | CSM Lead | CRM               |
| Upsell conversions              | Weekly    | CSM Lead | Billing           |
| MRR added from upsell           | Monthly   | Finance  | Billing           |

### Expansion Metrics (Playbook 07)

| Metric                                 | Frequency | Owner    | Source  |
| -------------------------------------- | --------- | -------- | ------- |
| Thailand: hotels onboarded             | Weekly    | Sales VP | CRM     |
| Thailand: revenue                      | Monthly   | Finance  | Billing |
| India Tier 2/3: hotels onboarded       | Weekly    | Sales VP | CRM     |
| India Tier 2/3: revenue                | Monthly   | Finance  | Billing |
| Localization progress (languages live) | Monthly   | Product  | Product |

---

## RISK ESCALATION FRAMEWORK

### Risk 1: High Churn Continues

| Trigger                                        | Threshold | Escalation                                                                                     |
| ---------------------------------------------- | --------- | ---------------------------------------------------------------------------------------------- |
| Monthly churn > 2.5% for 2 consecutive months  | 🔴 Red    | CEO + CSM Lead: emergency review. Pause new initiatives, focus all CSM bandwidth on retention. |
| Red accounts > 20% of portfolio                | 🟡 Amber  | CSM Lead: redistribute accounts, bring in additional support.                                  |
| Winback campaign < 5% conversion after 4 weeks | 🟡 Amber  | Revisit offers, messaging, and targeting.                                                      |

### Risk 2: Low Sales Conversion

| Trigger                                                   | Threshold | Escalation                                                        |
| --------------------------------------------------------- | --------- | ----------------------------------------------------------------- |
| Conversion < 8% for 2 consecutive months (no improvement) | 🔴 Red    | Sales VP + CEO: audit the funnel. Is it leads, demos, or closing? |
| Lead response time > 24 hours                             | 🟡 Amber  | Sales VP: fix immediately. Automate if needed.                    |
| Demo no-show rate > 40%                                   | 🟡 Amber  | Review demo scheduling process, add reminders.                    |

### Risk 3: Pricing Resistance

| Trigger                                               | Threshold | Escalation                                               |
| ----------------------------------------------------- | --------- | -------------------------------------------------------- |
| > 10% of customers threaten to leave during migration | 🔴 Red    | CEO: pause migration, offer extended grandfather period. |
| New customer conversion drops after pricing change    | 🟡 Amber  | Sales VP + Product: review pricing, test discounts.      |
| Competitor wins increase                              | 🟡 Amber  | Competitive analysis sprint. Adjust positioning.         |

### Risk 4: Team Bandwidth

| Trigger                                         | Threshold | Escalation                                   |
| ----------------------------------------------- | --------- | -------------------------------------------- |
| CSM portfolio > 200 accounts with no AI support | 🔴 Red    | Hire. AI timeline slipped — can't wait.      |
| Engineering can't deliver AI systems on time    | 🟡 Amber  | CEO: evaluate contractors or external tools. |
| Sales team can't handle lead volume             | 🟡 Amber  | Sales VP: hire or redistribute.              |

---

## QUARTERLY BUSINESS REVIEW (QBR) — INTERNAL

Every quarter, a 2-hour deep review:

| Section                   | Duration | Content                                                                        |
| ------------------------- | -------- | ------------------------------------------------------------------------------ |
| North Star metrics review | 30 min   | Where are we vs targets? What worked, what didn't?                             |
| Category deep dives       | 60 min   | Each playbook owner presents: progress, blockers, learnings, next quarter plan |
| Resource reallocation     | 15 min   | Do we need to shift people, budget, or priorities?                             |
| Next quarter goals        | 15 min   | Set specific targets for next quarter                                          |

---

## TOOLS & INFRASTRUCTURE

| Need                                | Options                                                 | Decision Needed By        |
| ----------------------------------- | ------------------------------------------------------- | ------------------------- |
| Dashboard for North Star metrics    | Google Sheets (quick), Metabase (better), Looker (best) | Week 1                    |
| CRM for sales + CSM pipeline        | HubSpot, Salesforce, Zoho, or current tool              | Week 1 (evaluate current) |
| Health scoring dashboard            | Custom build (Retool/Metabase) or product feature       | Week 2                    |
| Support ticketing with SLA tracking | Freshdesk, Zendesk, Intercom                            | Week 2                    |
| Communication (internal)            | Slack channels per category                             | Week 1                    |

### Recommended Slack Channels

| Channel             | Purpose                                      | Members               |
| ------------------- | -------------------------------------------- | --------------------- |
| #north-star-metrics | Weekly metric updates, automated if possible | Leadership            |
| #retention-alerts   | Red account alerts, churn notifications      | CSM team + CSM Lead   |
| #sales-pipeline     | New leads, conversions, wins                 | Sales team + Sales VP |
| #direct-bookings    | BE adoption updates, savings milestones      | Product + CSM         |
| #ai-updates         | AI system progress, deployments, issues      | Engineering + Product |
| #expansion          | Thailand + Tier 2/3 updates                  | Sales VP + CEO        |

---

## OPEN QUESTIONS FOR DISCUSSION

1. What tools do we currently use for CRM, support, and analytics? Can they support this tracking?
2. Who builds and maintains the weekly dashboard? Dedicated ops person or shared responsibility?
3. Weekly review: is Monday 9 AM realistic for all stakeholders?
4. QBR: who facilitates? CEO or an external advisor?
5. Slack vs other communication tool — what does the team currently use?
6. Budget for tools (Metabase, Freshdesk, etc.): what's available?
7. Do we need a dedicated "Chief of Staff" or ops person to run governance?
