# CEO Business Plan 2026–27 — Actionable Items Register

> Master list of all action items extracted from the business plan and presentation.
> Use this document to prioritize, assign ownership, set timelines, and drive detailed execution discussions.

---

## How to use this document

Each action item includes:

- **What** — the specific action
- **Why** — the business problem it solves
- **Priority** — P0 (do first, highest impact), P1 (do next), P2 (important but can follow), P3 (plan for later)
- **Revenue Impact** — estimated impact where quantifiable
- **Owner** — suggested function/role
- **Timeline** — target quarter
- **Status** — Not Started / In Discussion / In Progress / Done
- **Discussion Points** — open questions to resolve before or during execution

---

## Priority Legend

| Priority | Meaning                    | Criteria                                                                                           |
| -------- | -------------------------- | -------------------------------------------------------------------------------------------------- |
| **P0**   | Critical — do immediately  | Directly stops revenue loss or unlocks the biggest revenue lever. Blocks other actions if delayed. |
| **P1**   | High — start in parallel   | High impact, but can run alongside P0. Needs Q1–Q2 start.                                          |
| **P2**   | Medium — plan and sequence | Important for scale, but depends on P0/P1 foundations being in place.                              |
| **P3**   | Lower — plan for H2        | Strategic but not urgent. Can be scheduled for Q3–Q4.                                              |

---

## CATEGORY 1: CHURN REDUCTION & RETENTION (Slides 05, 06)

> Current churn: 30% (535 of 1,800 customers) · Revenue impact: ₹70L/year lost
> Target: 15% churn · Recovers: ₹35L+/year

### ACTION 1.1 — Build AI Account Health Scoring System

| Field                 | Detail                                                                                                                                                                                                                                                                                      |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                                                                          |
| **What**              | Build an AI-driven health score per account based on product usage, login frequency, support ticket volume, and payment history. Classify every account as Red / Amber / Green.                                                                                                             |
| **Why**               | Without visibility into which accounts are at risk, CSMs are reactive. This makes them proactive.                                                                                                                                                                                           |
| **Revenue Impact**    | Directly enables churn reduction from 30% → 15% (₹35L/year recovery)                                                                                                                                                                                                                        |
| **Owner**             | Product + Engineering                                                                                                                                                                                                                                                                       |
| **Timeline**          | Q1 — live by end of month 3                                                                                                                                                                                                                                                                 |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                 |
| **Discussion Points** | → What data sources feed the health score? (product usage logs, support tickets, login data, payment data) · → Do we build in-house or use a third-party tool (e.g., Gainsight, ChurnZero)? · → What thresholds define Red/Amber/Green? · → How do we handle accounts with incomplete data? |

### ACTION 1.2 — Implement Churn Alert System (30–45 Day Early Warning)

| Field                 | Detail                                                                                                                                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                    |
| **What**              | Automated alerts triggered 30–45 days before renewal when an account's health score drops to Red or Amber. Alert goes to the assigned CSM + CSM Lead.                                                                 |
| **Why**               | Currently churn is discovered after the fact. Early alerts give CSMs a window to intervene.                                                                                                                           |
| **Revenue Impact**    | Part of the 30% → 15% churn reduction target                                                                                                                                                                          |
| **Owner**             | Product + CSM Lead                                                                                                                                                                                                    |
| **Timeline**          | Q1 — tied to health scoring system                                                                                                                                                                                    |
| **Status**            | Not Started                                                                                                                                                                                                           |
| **Discussion Points** | → Alert channel: email, Slack, CRM notification, or all? · → What's the escalation path if CSM doesn't act within 48 hours? · → Should we auto-trigger a customer outreach (email/call) alongside the internal alert? |

### ACTION 1.3 — Build CSM Dashboard (Live Health View)

| Field                 | Detail                                                                                                                                                                                           |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Priority**          | P0                                                                                                                                                                                               |
| **What**              | A real-time dashboard showing each CSM's portfolio: account health distribution, upcoming renewals, churn risk, upsell opportunities, and performance metrics (churn %, renewal rate, upsell %). |
| **Why**               | CSMs currently have no single view of their portfolio health. Managers have no way to spot problems early.                                                                                       |
| **Revenue Impact**    | Enables all retention actions — foundational infrastructure                                                                                                                                      |
| **Owner**             | Product + Engineering                                                                                                                                                                            |
| **Timeline**          | Q1                                                                                                                                                                                               |
| **Status**            | Not Started                                                                                                                                                                                      |
| **Discussion Points** | → Build custom or use existing BI tool (Metabase, Looker, etc.)? · → What metrics are visible to CSMs vs. managers vs. CEO? · → Mobile access needed? · → Integration with CRM?                  |

### ACTION 1.4 — Create Structured Onboarding Playbook (First 30 Days)

| Field                 | Detail                                                                                                                                                                                                                                                                                                      |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                                                                                          |
| **What**              | Define a standardized 30-day onboarding journey for every new customer: Day 1 welcome call, Day 3 setup verification, Day 7 training session, Day 14 usage check, Day 30 health review.                                                                                                                     |
| **Why**               | Poor onboarding is a top root cause of churn. Hotels that don't adopt the product in the first month rarely recover.                                                                                                                                                                                        |
| **Revenue Impact**    | Reduces early-stage churn (estimated 30–40% of all churn happens in first 90 days)                                                                                                                                                                                                                          |
| **Owner**             | CSM Lead                                                                                                                                                                                                                                                                                                    |
| **Timeline**          | Q1                                                                                                                                                                                                                                                                                                          |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                 |
| **Discussion Points** | → Who handles onboarding — dedicated onboarding team or the assigned CSM? · → Can we automate parts of it (welcome emails, setup checklists, video tutorials)? · → What's the "activation" milestone that signals successful onboarding? · → Do we need different playbooks for Basic vs Growth vs Premium? |

### ACTION 1.5 — Implement Monthly Check-in Cadence + Automated Touchpoints

| Field                 | Detail                                                                                                                                                                                                                                                                                                        |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                                                                                            |
| **What**              | Every account gets a monthly check-in (call or email). Between check-ins, automated touchpoints (usage tips, feature announcements, satisfaction surveys) keep engagement alive.                                                                                                                              |
| **Why**               | Weak engagement between renewals is a root cause of churn. Customers go silent → then leave.                                                                                                                                                                                                                  |
| **Revenue Impact**    | Part of churn reduction target                                                                                                                                                                                                                                                                                |
| **Owner**             | CSM Lead + Marketing                                                                                                                                                                                                                                                                                          |
| **Timeline**          | Q1–Q2                                                                                                                                                                                                                                                                                                         |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                   |
| **Discussion Points** | → Can CSMs handle monthly calls for all 1,800 accounts? If not, what's the segmentation? (e.g., high-value = call, low-value = automated) · → What tool handles automated touchpoints? (CRM, email automation, in-app messaging) · → QBR (Quarterly Business Review) for top accounts — what's the threshold? |

### ACTION 1.6 — Build Renewal Pipeline (90-Day Advance Tracking)

| Field                 | Detail                                                                                                                                                                                                                                       |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                           |
| **What**              | Track every renewal 90 days in advance. CSMs prioritize outreach based on risk score. Automated renewal reminders at 90, 60, 30, and 7 days.                                                                                                 |
| **Why**               | Renewals are currently not tracked proactively. By the time we know a customer is leaving, it's too late.                                                                                                                                    |
| **Revenue Impact**    | Directly reduces churn and improves renewal rate                                                                                                                                                                                             |
| **Owner**             | CSM Lead + Product                                                                                                                                                                                                                           |
| **Timeline**          | Q1                                                                                                                                                                                                                                           |
| **Status**            | Not Started                                                                                                                                                                                                                                  |
| **Discussion Points** | → Auto-renewal vs. manual renewal — what's our current split? · → Can we offer incentives for early renewal (discount, extra month)? · → What happens when a renewal is at risk — escalation to manager? CEO involvement for large accounts? |

### ACTION 1.7 — Redesign CSM Incentive Structure

| Field                 | Detail                                                                                                                                                                                                                                                    |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                                        |
| **What**              | Tie CSM compensation/bonuses to retention metrics (churn %, renewal rate, upsell %) instead of only new sales or activity metrics.                                                                                                                        |
| **Why**               | If CSMs are not incentivized on retention, retention won't be their priority.                                                                                                                                                                             |
| **Revenue Impact**    | Behavioral change that drives all retention metrics                                                                                                                                                                                                       |
| **Owner**             | CEO + HR + CSM Lead                                                                                                                                                                                                                                       |
| **Timeline**          | Q1 (design), Q2 (implement)                                                                                                                                                                                                                               |
| **Status**            | Not Started                                                                                                                                                                                                                                               |
| **Discussion Points** | → What's the current incentive structure? · → What % of variable pay should be tied to retention vs. upsell vs. activity? · → How do we handle CSMs with inherited high-churn portfolios? · → Weekly team health review — who runs it, what's the format? |

---

## CATEGORY 2: PRICING & ARPU (Slides 03, 08)

> Current ARPU: ₹1,100/month · Target: ₹2,500–₹3,000/month
> Revenue at target ARPU (same 1,800 customers): ₹54–65 Cr

### ACTION 2.1 — Consolidate 30+ Plans into 3 Tiers

| Field                 | Detail                                                                                                                                                                                                                                                                                                                                                                            |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                                                                                                                                                                |
| **What**              | Replace all existing pricing plans with 3 clean tiers: Basic (₹1K/mo), Growth (₹2K/mo), Premium (₹3K/mo). Map every existing customer to the appropriate new tier.                                                                                                                                                                                                                |
| **Why**               | 30+ pricing variations cause confusion for sales, support, and customers. No clear upgrade path means no upsell.                                                                                                                                                                                                                                                                  |
| **Revenue Impact**    | Foundation for ARPU increase from ₹1.1K → ₹2.5–3K/month (₹5 Cr incremental)                                                                                                                                                                                                                                                                                                       |
| **Owner**             | Product + CEO + Finance                                                                                                                                                                                                                                                                                                                                                           |
| **Timeline**          | Q1                                                                                                                                                                                                                                                                                                                                                                                |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                                                                                       |
| **Discussion Points** | → How do we migrate existing customers? Grandfather old pricing or force migration? · → What's the migration timeline — all at once or phased by cohort? · → How do we handle customers currently paying more than the new tier price? · → Do we offer annual discount (e.g., 2 months free on yearly)? · → What's the communication plan — email, CSM call, in-app notification? |

### ACTION 2.2 — Define Feature Gating per Tier

| Field                 | Detail                                                                                                                                                                                                                                                                                                                        |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                                                                                                            |
| **What**              | Clearly define which features are included in each tier. Booking Engine only in Growth+. Full PMS, Revenue Management, and dedicated CSM only in Premium.                                                                                                                                                                     |
| **Why**               | Without clear feature gating, there's no reason for customers to upgrade. Features must create a pull toward higher tiers.                                                                                                                                                                                                    |
| **Revenue Impact**    | Drives tier upgrades — key to ARPU growth                                                                                                                                                                                                                                                                                     |
| **Owner**             | Product                                                                                                                                                                                                                                                                                                                       |
| **Timeline**          | Q1 (alongside pricing consolidation)                                                                                                                                                                                                                                                                                          |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                                   |
| **Discussion Points** | → Which features are currently used by which customer segments? (data needed) · → Are there features that should be add-ons rather than tier-locked? · → How do we handle customers who lose access to features they currently use? · → Booking Engine in Growth tier — is this the right placement to drive direct bookings? |

### ACTION 2.3 — Launch Add-on Marketplace

| Field                 | Detail                                                                                                                                                                                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P2                                                                                                                                                                                                                                                                    |
| **What**              | Create a marketplace where customers can purchase individual add-on features (e.g., advanced analytics, extra OTA integrations, white-label booking page, API access) on top of their base tier.                                                                      |
| **Why**               | Not every customer needs Premium, but many will pay for specific features. Add-ons increase ARPU without forcing tier upgrades.                                                                                                                                       |
| **Revenue Impact**    | Incremental ARPU lift — estimated ₹200–500/month per customer who buys add-ons                                                                                                                                                                                        |
| **Owner**             | Product + Engineering                                                                                                                                                                                                                                                 |
| **Timeline**          | Q3                                                                                                                                                                                                                                                                    |
| **Status**            | Not Started                                                                                                                                                                                                                                                           |
| **Discussion Points** | → What are the top 5 features customers would pay extra for? (survey/data needed) · → Pricing model: flat fee per add-on or usage-based? · → Technical feasibility — can we modularize features in the current architecture? · → Self-serve purchase or CSM-assisted? |

### ACTION 2.4 — Build ROI Calculator for Sales & CSM Teams

| Field                 | Detail                                                                                                                                                                                                                                     |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Priority**          | P1                                                                                                                                                                                                                                         |
| **What**              | A simple tool (web page or spreadsheet) that shows a hotel: "You're paying OTAs ₹X in commission. With our Booking Engine, you save ₹Y. Your net cost of our platform is ₹Z."                                                              |
| **Why**               | Pricing resistance is a key risk. An ROI calculator makes the value tangible and shifts the conversation from cost to savings.                                                                                                             |
| **Revenue Impact**    | Reduces pricing resistance, improves conversion and upsell                                                                                                                                                                                 |
| **Owner**             | Product + Marketing                                                                                                                                                                                                                        |
| **Timeline**          | Q1–Q2                                                                                                                                                                                                                                      |
| **Status**            | Not Started                                                                                                                                                                                                                                |
| **Discussion Points** | → What inputs does the calculator need? (rooms, ADR, OTA %, current direct booking %) · → Web-based or PDF/spreadsheet for offline use? · → Should CSMs use it during renewal conversations? · → Can we embed it in the product dashboard? |

---

## CATEGORY 3: DIRECT BOOKING STRATEGY (Slides 07, 08, 10)

> Goal: Shift hotels from OTA dependency (18–25% commission) to direct bookings (0% commission)
> Revenue potential: ₹5–10 Cr from direct booking revenue

### ACTION 3.1 — Booking Engine Adoption Campaign

| Field                 | Detail                                                                                                                                                                                                                                                                                                                      |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                                                                                                          |
| **What**              | Aggressive push to get every Growth and Premium customer actively using the Booking Engine. Target: 60%+ of eligible customers with active BE within 6 months.                                                                                                                                                              |
| **Why**               | The Booking Engine is the core product that drives direct bookings. If hotels don't use it, the entire direct booking strategy fails.                                                                                                                                                                                       |
| **Revenue Impact**    | Foundation for ₹5–10 Cr direct booking revenue stream                                                                                                                                                                                                                                                                       |
| **Owner**             | CSM + Product + Marketing                                                                                                                                                                                                                                                                                                   |
| **Timeline**          | Q2 (launch), Q3 (scale)                                                                                                                                                                                                                                                                                                     |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                                 |
| **Discussion Points** | → What % of current customers have BE enabled but aren't using it? · → What are the barriers to adoption? (setup complexity, lack of training, don't see value) · → Do we need a dedicated BE onboarding flow? · → Can we show hotels real-time data: "X bookings came direct this month, saving you ₹Y in OTA commission"? |

### ACTION 3.2 — Direct Booking Analytics Dashboard for Hotels

| Field                 | Detail                                                                                                                                                                                                                                                       |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Priority**          | P1                                                                                                                                                                                                                                                           |
| **What**              | A customer-facing dashboard showing each hotel: direct bookings vs OTA bookings, commission saved, booking engine conversion rate, top traffic sources.                                                                                                      |
| **Why**               | Hotels need to see the value of direct bookings in real numbers. This creates stickiness and justifies the platform cost.                                                                                                                                    |
| **Revenue Impact**    | Increases BE adoption, reduces churn (value visibility), supports upsell to Premium                                                                                                                                                                          |
| **Owner**             | Product + Engineering                                                                                                                                                                                                                                        |
| **Timeline**          | Q2–Q3                                                                                                                                                                                                                                                        |
| **Status**            | Not Started                                                                                                                                                                                                                                                  |
| **Discussion Points** | → What data do we already have vs. what needs new tracking? · → Can we show a "commission saved" counter prominently? · → Should this be part of the existing dashboard or a standalone view? · → Can we send monthly "savings report" emails automatically? |

### ACTION 3.3 — OTA Commission Comparison Messaging

| Field                 | Detail                                                                                                                                                                                                                                   |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                       |
| **What**              | Create marketing and sales collateral that clearly shows: "OTAs charge 18–25% commission per booking. Direct bookings through our Booking Engine: 0% commission." Use in sales pitches, renewal conversations, and upsell campaigns.     |
| **Why**               | The value proposition of direct bookings needs to be front and center in every customer conversation.                                                                                                                                    |
| **Revenue Impact**    | Improves conversion, reduces pricing resistance, drives BE adoption                                                                                                                                                                      |
| **Owner**             | Marketing + Sales                                                                                                                                                                                                                        |
| **Timeline**          | Q1                                                                                                                                                                                                                                       |
| **Status**            | Not Started                                                                                                                                                                                                                              |
| **Discussion Points** | → Do we have real data on how much our hotels currently pay in OTA commissions? · → Can we get case studies from hotels already using BE successfully? · → What formats: one-pager, email template, in-product banner, sales deck slide? |

---

## CATEGORY 4: SALES FUNNEL OPTIMIZATION (Slide 04)

> Current conversion: 8.15% (368 sales from 4,513 leads) · Target: 15–20%
> Key gap: 79% of leads drop before demo

### ACTION 4.1 — Implement AI Lead Scoring

| Field                 | Detail                                                                                                                                                                                                                                                                         |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Priority**          | P1                                                                                                                                                                                                                                                                             |
| **What**              | Deploy an AI/ML model that scores every incoming lead based on hotel size, location, source, engagement signals, and fit criteria. Sales team focuses on high-score leads first.                                                                                               |
| **Why**               | Sales team is spending equal time on all leads. High-intent leads go cold while low-quality leads consume bandwidth.                                                                                                                                                           |
| **Revenue Impact**    | Improves Lead→Demo conversion from 21% → 35%+, overall conversion from 8% → 15%+                                                                                                                                                                                               |
| **Owner**             | Sales VP + Product                                                                                                                                                                                                                                                             |
| **Timeline**          | Q2                                                                                                                                                                                                                                                                             |
| **Status**            | Not Started                                                                                                                                                                                                                                                                    |
| **Discussion Points** | → What data do we have on past leads that converted vs. didn't? · → Build in-house or use a tool (HubSpot, Salesforce Einstein, custom)? · → What's the scoring model — rule-based first, then ML? · → How do we handle leads that score low but come from strategic segments? |

### ACTION 4.2 — Build Automated Follow-up Sequences

| Field                 | Detail                                                                                                                                                                                                                                  |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                      |
| **What**              | Set up automated email/WhatsApp follow-up sequences for every lead: Day 0 (acknowledgment), Day 1 (value prop), Day 3 (case study), Day 5 (demo invite), Day 7 (final nudge).                                                           |
| **Why**               | 79% of leads drop before demo. Most of these are not lost — they're just not followed up. Automation ensures no lead goes cold.                                                                                                         |
| **Revenue Impact**    | Even a 5% improvement in Lead→Demo rate = ~225 more demos = ~85 more sales                                                                                                                                                              |
| **Owner**             | Sales VP + Marketing                                                                                                                                                                                                                    |
| **Timeline**          | Q1–Q2                                                                                                                                                                                                                                   |
| **Status**            | Not Started                                                                                                                                                                                                                             |
| **Discussion Points** | → What tool: CRM built-in, Mailchimp, WhatsApp Business API, or custom? · → Do we have email/WhatsApp templates ready? · → What's the opt-out/compliance process? · → Should sequences differ by lead source (organic, paid, referral)? |

### ACTION 4.3 — Demo Script Optimization & Sales Scripting

| Field                 | Detail                                                                                                                                                                                                                       |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                           |
| **What**              | Create a standardized demo script that highlights direct booking value, ROI, and key differentiators. Create objection-handling scripts for common pushbacks (price, switching cost, "we already use X").                    |
| **Why**               | Demo→Sale is already 38% — but inconsistent demo quality means some reps convert much higher than others. Standardization lifts the floor.                                                                                   |
| **Revenue Impact**    | Improves Demo→Sale from 38% → 45%+                                                                                                                                                                                           |
| **Owner**             | Sales VP                                                                                                                                                                                                                     |
| **Timeline**          | Q1                                                                                                                                                                                                                           |
| **Status**            | Not Started                                                                                                                                                                                                                  |
| **Discussion Points** | → Do we have recordings of our best demos to model from? · → What are the top 5 objections and how do top reps handle them? · → Should we A/B test different demo flows? · → Do we need a demo environment with sample data? |

### ACTION 4.4 — Sales Team Scaling Plan

| Field                 | Detail                                                                                                                                                                                                              |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P2                                                                                                                                                                                                                  |
| **What**              | Define when and how to scale the sales team — hiring plan, ramp time, territory assignment, quota structure.                                                                                                        |
| **Why**               | Once funnel efficiency improves, we'll need more capacity to handle increased demo volume. Scaling too early wastes money; too late misses opportunity.                                                             |
| **Revenue Impact**    | Enables new customer acquisition target of ₹3 Cr                                                                                                                                                                    |
| **Owner**             | Sales VP + HR                                                                                                                                                                                                       |
| **Timeline**          | Q3                                                                                                                                                                                                                  |
| **Status**            | Not Started                                                                                                                                                                                                         |
| **Discussion Points** | → What's the current sales team size and capacity? · → What's the ideal rep:lead ratio after AI scoring? · → Hire experienced reps or train juniors? · → Territory: geography-based, segment-based, or round-robin? |

---

## CATEGORY 5: AI TRANSFORMATION (Slide 09)

> Goal: 2–3x efficiency without additional hiring

### ACTION 5.1 — Deploy Sales AI (Lead Scoring + Auto Outreach + Demo Assistant)

| Field                 | Detail                                                                                                                                                                                                                       |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                           |
| **What**              | Unified Sales AI layer: lead scoring model, automated outreach sequences, and an AI demo assistant that helps reps prepare personalized demos.                                                                               |
| **Why**               | Sales team is small. AI multiplies their output without adding headcount.                                                                                                                                                    |
| **Revenue Impact**    | Conversion 8% → 15%+                                                                                                                                                                                                         |
| **Owner**             | Product + Engineering + Sales VP                                                                                                                                                                                             |
| **Timeline**          | Q2                                                                                                                                                                                                                           |
| **Status**            | Not Started                                                                                                                                                                                                                  |
| **Discussion Points** | → Build vs. buy — what's the budget? · → Can we start with rule-based scoring and layer ML later? · → Demo assistant: AI-generated talking points per lead, or full co-pilot during demo? · → Integration with existing CRM? |

### ACTION 5.2 — Deploy CSM AI (Health Score + Churn Prediction + Renewal Engine)

| Field                 | Detail                                                                                                                                                                                                                                                                                                        |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                                                                                            |
| **What**              | AI system that powers account health scoring, predicts churn 30–45 days out, auto-generates renewal tasks, and prioritizes CSM workload.                                                                                                                                                                      |
| **Why**               | CSMs managing 100+ accounts each can't manually track health. AI makes them proactive instead of reactive.                                                                                                                                                                                                    |
| **Revenue Impact**    | Core enabler of churn reduction (₹35L+/year recovery)                                                                                                                                                                                                                                                         |
| **Owner**             | Product + Engineering                                                                                                                                                                                                                                                                                         |
| **Timeline**          | Q1                                                                                                                                                                                                                                                                                                            |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                   |
| **Discussion Points** | → Same system as Action 1.1 or separate? (likely same — health scoring is the foundation) · → What ML model: logistic regression, random forest, or start with rules? · → Training data: do we have enough historical churn data? · → Renewal engine: auto-send renewal emails or just create tasks for CSMs? |

### ACTION 5.3 — Deploy Support AI (Chatbot + SLA Tracking + Knowledge Base)

| Field                 | Detail                                                                                                                                                                                                                                                                                                                                               |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                                                                                                                                   |
| **What**              | AI chatbot that handles 60% of support tickets automatically. SLA breach alert system. Self-serve knowledge base with search.                                                                                                                                                                                                                        |
| **Why**               | Support gaps and slow SLA are a root cause of churn. AI handles volume; humans handle complexity.                                                                                                                                                                                                                                                    |
| **Revenue Impact**    | Reduces support-driven churn, frees support team for high-value issues                                                                                                                                                                                                                                                                               |
| **Owner**             | Product + Engineering + Support Lead                                                                                                                                                                                                                                                                                                                 |
| **Timeline**          | Q2 (chatbot beta), Q3 (full rollout)                                                                                                                                                                                                                                                                                                                 |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                                                          |
| **Discussion Points** | → What are the top 20 ticket types by volume? (to train the chatbot) · → Build custom or use a platform (Intercom, Freshdesk AI, custom LLM)? · → SLA targets: what are they today, what should they be? · → Knowledge base: who writes and maintains the content? · → Sub-15 sec OTA sync monitoring — is this a support tool or a product feature? |

### ACTION 5.4 — Zero Overbooking Initiative + Sync Health Dashboard

| Field                 | Detail                                                                                                                                                                                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Priority**          | P1                                                                                                                                                                                                                                                     |
| **What**              | Achieve zero overbooking through sub-15 second OTA sync, real-time sync failure alerts, and a sync health dashboard visible to both hotels and internal teams.                                                                                         |
| **Why**               | Overbookings destroy trust. Trust is the foundation of the direct booking strategy — if hotels don't trust the system, they won't push direct bookings.                                                                                                |
| **Revenue Impact**    | Indirect but critical — enables the entire direct booking revenue stream                                                                                                                                                                               |
| **Owner**             | Engineering + Product                                                                                                                                                                                                                                  |
| **Timeline**          | Q1–Q2                                                                                                                                                                                                                                                  |
| **Status**            | Not Started                                                                                                                                                                                                                                            |
| **Discussion Points** | → What's the current average sync time? · → What % of overbookings are caused by sync delays vs. other issues? · → Real-time alerting: who gets notified — hotel, CSM, or both? · → Is the current infrastructure capable of sub-15 sec sync at scale? |

---

## CATEGORY 6: UPSELL ENGINE (Slides 06, 07)

> Current upsell rate: ~0% · Target: 20%+
> Revenue target from upsell: +₹1.6 Cr

### ACTION 6.1 — Design Upsell Campaign Playbook

| Field                 | Detail                                                                                                                                                                                                                                                                                                                 |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                                                                                                     |
| **What**              | Create a structured upsell playbook: identify trigger events (usage milestones, feature limits hit, renewal approaching), define the offer (tier upgrade, add-on), and script the conversation.                                                                                                                        |
| **Why**               | Upsell is currently 0% because there's no system for it. CSMs don't know when or how to upsell.                                                                                                                                                                                                                        |
| **Revenue Impact**    | ₹1.6 Cr from upsell target                                                                                                                                                                                                                                                                                             |
| **Owner**             | CSM Lead + Product                                                                                                                                                                                                                                                                                                     |
| **Timeline**          | Q2                                                                                                                                                                                                                                                                                                                     |
| **Status**            | Not Started                                                                                                                                                                                                                                                                                                            |
| **Discussion Points** | → What are the natural upsell triggers in our product? (e.g., hotel hits room limit, wants more OTA connections, needs CRM) · → Should upsell be CSM-driven, product-driven (in-app prompts), or both? · → What's the discount/incentive for upgrading? · → How do we track upsell pipeline separately from new sales? |

### ACTION 6.2 — AI-Powered Upsell Triggers

| Field                 | Detail                                                                                                                                                                |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P2                                                                                                                                                                    |
| **What**              | Use product usage data to automatically identify upsell opportunities and notify CSMs. E.g., "Hotel X has used 90% of their room quota — suggest Premium upgrade."    |
| **Why**               | Manual identification of upsell opportunities doesn't scale across 1,800 accounts.                                                                                    |
| **Revenue Impact**    | Part of ₹1.6 Cr upsell target                                                                                                                                         |
| **Owner**             | Product + Engineering                                                                                                                                                 |
| **Timeline**          | Q2–Q3                                                                                                                                                                 |
| **Status**            | Not Started                                                                                                                                                           |
| **Discussion Points** | → What usage signals correlate with upgrade readiness? · → In-app notification to the hotel, or CSM notification only? · → Can we A/B test different upsell messages? |

### ACTION 6.3 — Booking Engine & CRM/PMS Cross-sell

| Field                 | Detail                                                                                                                                                                                         |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                             |
| **What**              | Targeted campaigns to sell Booking Engine to Basic customers and CRM/PMS modules to Growth customers.                                                                                          |
| **Why**               | Many customers are on lower tiers but would benefit from (and pay for) higher-tier features.                                                                                                   |
| **Revenue Impact**    | Drives tier upgrades — key ARPU lever                                                                                                                                                          |
| **Owner**             | CSM + Sales                                                                                                                                                                                    |
| **Timeline**          | Q2                                                                                                                                                                                             |
| **Status**            | Not Started                                                                                                                                                                                    |
| **Discussion Points** | → How many Basic customers could benefit from BE? (data needed) · → What's the pitch: cost savings from direct bookings, or feature value? · → Free trial of BE for 30 days to drive adoption? |

---

## CATEGORY 7: MARKET EXPANSION (Slide 07, 11)

> Thailand: 50 clients Q1, scale to 200 by Q4
> India: Hindi + regional languages, Tier 2/3 city penetration
> Revenue target: +₹3 Cr from new customers

### ACTION 7.1 — Thailand Market Entry (50 Clients in Q1)

| Field                 | Detail                                                                                                                                                                                                                                                         |
| --------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                                             |
| **What**              | Set up Thailand operations: local entity/partner, localized product (Thai language, local payment gateways), hire/partner for local sales, onboard 50 clients in Q1.                                                                                           |
| **Why**               | International expansion is a key growth pillar. Thailand is the beachhead market.                                                                                                                                                                              |
| **Revenue Impact**    | Part of ₹3 Cr new customer target                                                                                                                                                                                                                              |
| **Owner**             | CEO + Sales VP                                                                                                                                                                                                                                                 |
| **Timeline**          | Q1 (setup + first 50), Q4 (scale to 200)                                                                                                                                                                                                                       |
| **Status**            | Not Started                                                                                                                                                                                                                                                    |
| **Discussion Points** | → Local partner or own entity? · → Product localization: how much work is Thai language + local payments? · → Pricing: same as India or localized? · → Who manages Thailand accounts — local team or India-based CSMs? · → Regulatory/compliance requirements? |

### ACTION 7.2 — Hindi & Regional Language Localization

| Field                 | Detail                                                                                                                                                                                                          |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P2                                                                                                                                                                                                              |
| **What**              | Localize the product interface, onboarding materials, and support in Hindi first, then regional languages (Marathi, Tamil, Telugu, etc.).                                                                       |
| **Why**               | Tier 2/3 hotel owners often prefer local language. Language barrier is a real adoption blocker.                                                                                                                 |
| **Revenue Impact**    | Enables Tier 2/3 penetration — part of ₹3 Cr new customer target                                                                                                                                                |
| **Owner**             | Product + Engineering                                                                                                                                                                                           |
| **Timeline**          | Q2 (Hindi), Q3 (regional)                                                                                                                                                                                       |
| **Status**            | Not Started                                                                                                                                                                                                     |
| **Discussion Points** | → How much of the UI needs translation? · → Machine translation + human review, or fully manual? · → Support in local languages — do we have the team? · → Which regional languages first based on market size? |

### ACTION 7.3 — Tier 2/3 India City Penetration

| Field                 | Detail                                                                                                                                                                                                                                                                 |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P2                                                                                                                                                                                                                                                                     |
| **What**              | Targeted sales campaigns for hotels in Tier 2/3 cities. Adjusted pricing if needed. Local language support. Partnership with local hotel associations.                                                                                                                 |
| **Why**               | Tier 2/3 is a massive untapped market. These hotels are underserved by technology and have high willingness to adopt if the product speaks their language.                                                                                                             |
| **Revenue Impact**    | Part of ₹3 Cr new customer target                                                                                                                                                                                                                                      |
| **Owner**             | Sales VP + Marketing                                                                                                                                                                                                                                                   |
| **Timeline**          | Q3                                                                                                                                                                                                                                                                     |
| **Status**            | Not Started                                                                                                                                                                                                                                                            |
| **Discussion Points** | → Do we need a different pricing tier for Tier 2/3? (lower room count, lower ADR) · → What's the go-to-market: digital ads, local partnerships, field sales? · → Can existing customers in these cities be references? · → Support model: same as metro or simplified? |

---

## CATEGORY 8: MEASUREMENT & GOVERNANCE (Slides 12, 13)

### ACTION 8.1 — Build Weekly Leadership Dashboard

| Field                 | Detail                                                                                                                                                                                                                                    |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P0                                                                                                                                                                                                                                        |
| **What**              | A single dashboard tracking the 6 key metrics weekly: MRR, ARPU, Churn %, Sales Conversion, Upsell %, Direct Booking Share. Each metric has an owner.                                                                                     |
| **Why**               | Without weekly visibility, problems compound for months before anyone notices.                                                                                                                                                            |
| **Revenue Impact**    | Governance layer that ensures all other actions deliver results                                                                                                                                                                           |
| **Owner**             | CEO + Finance                                                                                                                                                                                                                             |
| **Timeline**          | Q1                                                                                                                                                                                                                                        |
| **Status**            | Not Started                                                                                                                                                                                                                               |
| **Discussion Points** | → Tool: Google Sheets, Notion, Metabase, or custom? · → Who updates it — automated from systems or manual? · → Weekly review meeting: who attends, what's the format, how long? · → What's the escalation process when a metric goes red? |

### ACTION 8.2 — Define Risk Escalation Playbook

| Field                 | Detail                                                                                                                                                                                                                                                                              |
| --------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Priority**          | P1                                                                                                                                                                                                                                                                                  |
| **What**              | For each of the 4 identified risks (high churn, low conversion, pricing resistance, team bandwidth), define: trigger threshold, escalation path, owner, and response playbook.                                                                                                      |
| **Why**               | Risks are identified but response protocols aren't defined. When a risk materializes, the team needs to know exactly what to do.                                                                                                                                                    |
| **Revenue Impact**    | Prevents revenue loss from unmanaged risks                                                                                                                                                                                                                                          |
| **Owner**             | CEO                                                                                                                                                                                                                                                                                 |
| **Timeline**          | Q1                                                                                                                                                                                                                                                                                  |
| **Status**            | Not Started                                                                                                                                                                                                                                                                         |
| **Discussion Points** | → What's the churn threshold that triggers CEO involvement? · → Low conversion: at what point do we pause and redesign vs. push harder? · → Pricing resistance: do we have a fallback pricing strategy? · → Team bandwidth: at what point do we hire despite the AI-first approach? |

---

## PRIORITY SUMMARY — EXECUTION ORDER

### P0 — Do First (Q1)

| #   | Action                             | Category       | Owner                     |
| --- | ---------------------------------- | -------------- | ------------------------- |
| 1.1 | AI Account Health Scoring System   | Retention      | Product + Engineering     |
| 1.2 | Churn Alert System (30–45 day)     | Retention      | Product + CSM Lead        |
| 1.3 | CSM Dashboard (Live Health View)   | Retention      | Product + Engineering     |
| 1.4 | Structured Onboarding Playbook     | Retention      | CSM Lead                  |
| 2.1 | Consolidate to 3 Pricing Tiers     | Pricing        | Product + CEO + Finance   |
| 2.2 | Feature Gating per Tier            | Pricing        | Product                   |
| 3.1 | Booking Engine Adoption Campaign   | Direct Booking | CSM + Product + Marketing |
| 5.2 | CSM AI (Health + Churn Prediction) | AI             | Product + Engineering     |
| 8.1 | Weekly Leadership Dashboard        | Governance     | CEO + Finance             |

### P1 — Start in Parallel (Q1–Q2)

| #   | Action                              | Category       | Owner                  |
| --- | ----------------------------------- | -------------- | ---------------------- |
| 1.5 | Monthly Check-in Cadence            | Retention      | CSM Lead + Marketing   |
| 1.6 | Renewal Pipeline (90-day tracking)  | Retention      | CSM Lead + Product     |
| 1.7 | CSM Incentive Redesign              | Retention      | CEO + HR               |
| 2.4 | ROI Calculator                      | Pricing        | Product + Marketing    |
| 3.2 | Direct Booking Analytics Dashboard  | Direct Booking | Product + Engineering  |
| 3.3 | OTA Commission Comparison Messaging | Direct Booking | Marketing + Sales      |
| 4.1 | AI Lead Scoring                     | Sales          | Sales VP + Product     |
| 4.2 | Automated Follow-up Sequences       | Sales          | Sales VP + Marketing   |
| 4.3 | Demo Script + Sales Scripting       | Sales          | Sales VP               |
| 5.1 | Sales AI Deployment                 | AI             | Product + Engineering  |
| 5.3 | Support AI (Chatbot + SLA)          | AI             | Product + Support Lead |
| 5.4 | Zero Overbooking + Sync Dashboard   | AI             | Engineering + Product  |
| 6.1 | Upsell Campaign Playbook            | Upsell         | CSM Lead + Product     |
| 6.3 | BE & CRM/PMS Cross-sell             | Upsell         | CSM + Sales            |
| 7.1 | Thailand Market Entry               | Expansion      | CEO + Sales VP         |
| 8.2 | Risk Escalation Playbook            | Governance     | CEO                    |

### P2 — Plan and Sequence (Q2–Q3)

| #   | Action                        | Category  | Owner                 |
| --- | ----------------------------- | --------- | --------------------- |
| 2.3 | Add-on Marketplace            | Pricing   | Product + Engineering |
| 4.4 | Sales Team Scaling Plan       | Sales     | Sales VP + HR         |
| 6.2 | AI-Powered Upsell Triggers    | Upsell    | Product + Engineering |
| 7.2 | Hindi & Regional Localization | Expansion | Product + Engineering |
| 7.3 | Tier 2/3 India Penetration    | Expansion | Sales VP + Marketing  |

---

## NEXT STEPS

1. **Review this document** with the leadership team — validate priorities, challenge assumptions, flag missing items
2. **Assign named owners** (not just functions) to every P0 and P1 action
3. **Schedule deep-dive sessions** for each category — 60 min each, owner-led, focused on the Discussion Points listed above
4. **Set Q1 sprint goals** — pick the P0 actions, define "done" criteria, and commit to weekly check-ins
5. **Revisit P2/P3 actions** at the end of Q1 based on what's working and what's not

> This is a living document. Update the Status column as actions progress. Add new Discussion Points as they emerge. The goal is not to have all answers today — it's to have the right conversations in the right order.
