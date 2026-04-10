# PLAYBOOK 06 — Upsell Engine

> Owner: CSM Lead + Product
> Target: Upsell rate 0% → 20%+ · Revenue: +₹1.6 Cr · Timeline: Q2–Q3

---

## WHY UPSELL IS FREE REVENUE

Upselling to an existing customer costs almost nothing — no acquisition cost, no onboarding cost, no trust-building needed. They already use the product. They already pay us. We just need to show them why paying more gets them more value.

Current upsell rate: ~0%. That's not because customers don't want more — it's because nobody is asking.

---

## STEP 1: IDENTIFY UPSELL OPPORTUNITIES IN EXISTING BASE

### Data Analysis (Week 1)

Pull this for all 1,800 customers:

| Data Point                                            | Why                                                      |
| ----------------------------------------------------- | -------------------------------------------------------- |
| Current plan and price                                | Know where they are                                      |
| Features they use vs features available in their tier | Are they maxing out their current plan?                  |
| Features they've searched for or asked support about  | Demand signal for higher-tier features                   |
| Hotel size (rooms)                                    | Larger hotels = higher willingness to pay                |
| Booking volume (monthly)                              | High-volume hotels benefit most from BE                  |
| OTA dependency (% of bookings from OTAs)              | High OTA % = strong BE pitch                             |
| Account health score                                  | Only upsell healthy accounts — don't upsell at-risk ones |
| Time since last plan change                           | Long tenure on same plan = ripe for upgrade              |

### Upsell Opportunity Matrix

| Current Plan | Upsell To     | Trigger Signal                                                      | Estimated Pool |
| ------------ | ------------- | ------------------------------------------------------------------- | -------------- |
| Basic (₹1K)  | Growth (₹2K)  | High OTA dependency, asked about BE, > 20 rooms                     | ~400 hotels    |
| Basic (₹1K)  | Growth (₹2K)  | Searched for CRM features, high booking volume                      | ~200 hotels    |
| Growth (₹2K) | Premium (₹3K) | Using all Growth features, > 50 rooms, asked about PMS/Revenue Mgmt | ~150 hotels    |
| Any plan     | Add-ons       | Specific feature requests in support tickets                        | ~300 hotels    |

**Total addressable upsell pool: ~700+ hotels** (out of 1,800)

---

## STEP 2: DEFINE UPSELL TRIGGERS

### Automatic Triggers (AI-Powered)

| Trigger                                                     | What Happens                    | Upsell Pitch                                                                                                            |
| ----------------------------------------------------------- | ------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| Hotel hits 90% of room quota on current plan                | Alert to CSM                    | "You're almost at capacity on your current plan. Upgrade to [next tier] and unlock [features]."                         |
| Hotel searches for a feature not in their plan              | In-app notification + CSM alert | "Looking for [feature]? It's available on the [tier] plan. Want to try it?"                                             |
| Hotel's OTA commission exceeds ₹10K/month                   | CSM alert                       | "You paid ₹[X] in OTA commissions last month. Our Booking Engine could save you ₹[Y]. It's included in Growth."         |
| Hotel has been on same plan for 12+ months with good health | CSM alert                       | "You've been with us for a year — thank you! Here's what you could unlock with an upgrade."                             |
| Hotel's direct booking % increases after BE activation      | Automated email                 | "Your direct bookings are growing! Upgrade to Premium for advanced analytics and a dedicated CSM to grow them further." |
| Support ticket about a premium feature                      | CSM alert                       | "I see you asked about [feature]. That's available on [tier]. Want me to walk you through it?"                          |

### Manual Triggers (CSM-Driven)

| Moment                          | Upsell Opportunity                                                                                            |
| ------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| Monthly check-in call           | Review usage, identify gaps, suggest relevant upgrade                                                         |
| QBR (Quarterly Business Review) | Present ROI data, show what they're missing, propose upgrade                                                  |
| Renewal conversation            | "Before we renew, let me show you what Growth/Premium could do for you"                                       |
| After resolving a support issue | "Glad we fixed that. By the way, on the Growth plan you'd get priority support so these get resolved faster." |
| After a positive NPS response   | "Glad you're happy! Have you considered [feature] on the next tier?"                                          |

---

## STEP 3: UPSELL CONVERSATION SCRIPTS

### Basic → Growth Upgrade Script

> **CSM:** "Hi [name], I was looking at your account and noticed something interesting. Last month, about [X]% of your bookings came through OTAs — that's roughly ₹[Y] in commissions.
>
> Our Growth plan includes a Booking Engine that helps you get direct bookings. Hotels using it typically shift 15–25% of their bookings to direct, which at your volume would save you ₹[Z]/month.
>
> The Growth plan is ₹2K/month — so even after the upgrade cost, you'd be saving ₹[net]/month. Want me to show you how it works?"

### Growth → Premium Upgrade Script

> **CSM:** "Hi [name], you've been on the Growth plan for [X months] and I can see you're using it really well — [X] direct bookings last month, great job.
>
> I wanted to show you what Premium could add. You'd get full PMS (housekeeping, inventory, POS), revenue management for dynamic pricing, and a dedicated CSM — that's me, but with more time allocated specifically to your account.
>
> The big one is revenue management — hotels using dynamic pricing see 10–15% higher ADR. At your volume, that's ₹[X]/month in additional revenue. Premium is ₹3K/month. Want to try it?"

### Add-on Pitch Script

> **CSM:** "I noticed you've been asking about [feature]. That's available as an add-on for ₹[X]/month. It would help you [specific benefit]. Want me to enable a 14-day free trial so you can see if it works for you?"

---

## STEP 4: UPSELL CAMPAIGNS

### Campaign 1: "OTA Commission Savings" (Basic → Growth)

| Element  | Detail                                    |
| -------- | ----------------------------------------- |
| Target   | Basic plan hotels with > 50% OTA bookings |
| Channel  | Email → CSM call → WhatsApp               |
| Offer    | Upgrade to Growth + 1 month free          |
| Timeline | Q2, 4-week campaign                       |
| Goal     | 50 upgrades                               |

**Email sequence:**

| Day    | Subject                                               | Content                                                                    |
| ------ | ----------------------------------------------------- | -------------------------------------------------------------------------- |
| Day 0  | "You paid ₹[X] to OTAs last month"                    | Show their estimated OTA commission. Introduce BE. Link to ROI calculator. |
| Day 3  | "How [Hotel X] saves ₹15K/month with direct bookings" | Case study                                                                 |
| Day 7  | CSM Call                                              | Personal pitch with their specific numbers                                 |
| Day 10 | "Upgrade this week → 1 month free"                    | Limited-time offer                                                         |

### Campaign 2: "Unlock Premium" (Growth → Premium)

| Element  | Detail                                                                           |
| -------- | -------------------------------------------------------------------------------- |
| Target   | Growth plan hotels with > 50 rooms, using all Growth features                    |
| Channel  | CSM-led (call + email)                                                           |
| Offer    | 14-day Premium trial + upgrade discount (first 3 months at ₹2.5K instead of ₹3K) |
| Timeline | Q2–Q3                                                                            |
| Goal     | 30 upgrades                                                                      |

### Campaign 3: "Add-on Discovery" (All tiers)

| Element  | Detail                                                         |
| -------- | -------------------------------------------------------------- |
| Target   | Hotels that have searched for or asked about specific features |
| Channel  | In-app notification + email                                    |
| Offer    | 14-day free trial of the add-on                                |
| Timeline | Q3 (after marketplace launch)                                  |
| Goal     | 100 add-on purchases                                           |

---

## STEP 5: TRACKING & METRICS

### Upsell Pipeline Dashboard

| Metric                                    | Track   |
| ----------------------------------------- | ------- |
| Total upsell opportunities identified     | Weekly  |
| Upsell conversations initiated (by CSM)   | Weekly  |
| Upsell proposals sent                     | Weekly  |
| Upsell conversions                        | Weekly  |
| Upsell revenue (MRR added)                | Monthly |
| Upsell rate (conversions / opportunities) | Monthly |
| Average revenue per upsell                | Monthly |

### Targets

| Metric                     | Q2 Target | Q3 Target | Q4 Target |
| -------------------------- | --------- | --------- | --------- |
| Upsell conversations       | 200       | 300       | 400       |
| Upsell conversions         | 50        | 80        | 100       |
| Upsell rate                | 10%       | 15%       | 20%       |
| MRR added from upsell      | ₹1L       | ₹2.5L     | ₹4L       |
| Cumulative ARR from upsell | ₹12L      | ₹42L      | ₹90L      |

---

## OPEN QUESTIONS FOR DISCUSSION

1. Do we have the product usage data to identify upsell triggers today?
2. Should upsell be CSM-owned or do we need a dedicated upsell/expansion team?
3. Free trial for higher tiers — can we technically enable/disable features per account?
4. What's the CSM incentive for upsell? (ties to Playbook 01, Action 1.7)
5. Add-on marketplace: what's the build timeline? Can we do manual add-ons before the marketplace is ready?
6. Do we have case studies showing ROI of upgrades? If not, who creates them?
