# Feature Monetization Matrix — Usage-Based Pricing

> Strategy: Freeze every existing customer at their current usage. Charge for growth beyond that.
> No existing customer pays more for what they already have.
> Every new addition — more OTAs, more users, more rooms, more features — has a price.

---

## THE PRINCIPLE

| Rule                           | Detail                                                                                                       |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| **Grandfather existing usage** | Every customer's current state (OTAs connected, users, rooms, features active) is frozen as their free quota |
| **Charge for growth**          | Any addition beyond their frozen quota triggers a charge                                                     |
| **No retroactive pricing**     | They never pay more for what they already have today                                                         |
| **Transparent pricing**        | Every feature has a published price. No surprises.                                                           |
| **Self-serve upgrades**        | Customer can add/remove paid features from their dashboard                                                   |

---

## PRODUCT 1: CHANNEL MANAGER

### 1.1 OTA Connections

| Tier          | Free Quota                                     | Pay-as-you-grow                       |
| ------------- | ---------------------------------------------- | ------------------------------------- |
| All customers | Freeze at current connected OTAs               | ₹100/mo per additional OTA connection |
| Example       | Customer has 4 OTAs today → 4 are free forever | Wants to add Airbnb (5th) → ₹100/mo   |

**Pricing table:**

| OTAs Connected         | Monthly Cost                  |
| ---------------------- | ----------------------------- |
| Up to current (frozen) | ₹0 (grandfathered)            |
| +1 OTA                 | ₹100/mo                       |
| +2 OTAs                | ₹200/mo                       |
| +3 OTAs                | ₹300/mo                       |
| +5 OTAs                | ₹450/mo (10% bundle discount) |
| +10 OTAs               | ₹800/mo (20% bundle discount) |
| Unlimited OTAs         | ₹1,500/mo flat                |

**Discussion point:** What is the average number of OTAs per customer today? This determines the baseline.

---

### 1.2 Room Types

| Tier          | Free Quota                             | Pay-as-you-grow                      |
| ------------- | -------------------------------------- | ------------------------------------ |
| All customers | Freeze at current room type count      | ₹50/mo per additional room type      |
| Example       | Customer has 3 room types → 3 are free | Adds a new "Suite" category → ₹50/mo |

---

### 1.3 Rate Plans

| Tier          | Free Quota                                              | Pay-as-you-grow                 |
| ------------- | ------------------------------------------------------- | ------------------------------- |
| All customers | Freeze at current rate plan count                       | ₹30/mo per additional rate plan |
| Example       | Customer has 2 rate plans (Room Only, Breakfast) → free | Adds "Full Board" → ₹30/mo      |

---

### 1.4 Users / Staff Accounts

| Tier          | Free Quota                              | Pay-as-you-grow                  |
| ------------- | --------------------------------------- | -------------------------------- |
| All customers | Freeze at current user count            | ₹50/mo per additional user       |
| Example       | Customer has 2 users today → 2 are free | Adds a new staff member → ₹50/mo |

**Pricing table:**

| Additional Users | Monthly Cost        |
| ---------------- | ------------------- |
| +1 user          | ₹50/mo              |
| +3 users         | ₹130/mo (save ₹20)  |
| +5 users         | ₹200/mo (save ₹50)  |
| +10 users        | ₹350/mo (save ₹150) |
| Unlimited users  | ₹500/mo flat        |

---

### 1.5 Promotions

| Tier          | Free Quota                                    | Pay-as-you-grow                                             |
| ------------- | --------------------------------------------- | ----------------------------------------------------------- |
| All customers | 2 active promotions free                      | ₹50/mo per additional active promotion                      |
| Example       | Customer has 1 promotion today → 2 slots free | Wants to run 3 simultaneous promotions → ₹50/mo for the 3rd |

---

### 1.6 Dynamic Pricing Rules

| Tier          | Free Quota                                                  | Pay-as-you-grow                    |
| ------------- | ----------------------------------------------------------- | ---------------------------------- |
| All customers | 0 (new feature — not grandfathered)                         | ₹200/mo for dynamic pricing module |
| Note          | If customer already has it active → freeze at current rules | Additional rules: ₹50/mo each      |

---

### 1.7 Derived Rate Plans

| Tier          | Free Quota                      | Pay-as-you-grow                    |
| ------------- | ------------------------------- | ---------------------------------- |
| All customers | Freeze at current derived plans | ₹30/mo per additional derived plan |

---

## PRODUCT 2: BOOKING ENGINE

### 2.1 Booking Engine Access

| Tier                           | Free Quota                                 | Pay-as-you-grow        |
| ------------------------------ | ------------------------------------------ | ---------------------- |
| Customers with BE active today | Grandfathered — keep access free           | —                      |
| Customers without BE           | Not included in base plan                  | ₹500/mo to activate BE |
| Note                           | This is the key upsell for Basic customers |

---

### 2.2 Payment Gateways

| Tier             | Free Quota                                          | Pay-as-you-grow                           |
| ---------------- | --------------------------------------------------- | ----------------------------------------- |
| All BE customers | 1 payment gateway free (whichever they have active) | ₹100/mo per additional gateway            |
| Example          | Customer uses Razorpay → free                       | Wants to add Easebuzz as backup → ₹100/mo |

---

### 2.3 Promotions / Coupons (BE-specific)

| Tier             | Free Quota                                            | Pay-as-you-grow                     |
| ---------------- | ----------------------------------------------------- | ----------------------------------- |
| All BE customers | 3 active coupons/promotions free                      | ₹50/mo per additional active coupon |
| Coupon types     | Early bird, last minute, same-day, LOS, private codes | Each type counts as 1 slot          |

---

### 2.4 Paid Services / Add-ons (BE)

| Tier             | Free Quota                              | Pay-as-you-grow                    |
| ---------------- | --------------------------------------- | ---------------------------------- |
| All BE customers | 3 paid services listed free             | ₹30/mo per additional paid service |
| Example          | Airport pickup, spa, breakfast → 3 free | Adds "City Tour" as 4th → ₹30/mo   |

---

### 2.5 Packages

| Tier             | Free Quota             | Pay-as-you-grow               |
| ---------------- | ---------------------- | ----------------------------- |
| All BE customers | 2 active packages free | ₹50/mo per additional package |

---

### 2.6 Day Outing Packages

| Tier             | Free Quota                       | Pay-as-you-grow                  |
| ---------------- | -------------------------------- | -------------------------------- |
| All BE customers | 1 active day outing package free | ₹50/mo per additional day outing |

---

### 2.7 Analytics Plugins (GA, GTM, Pixel, Clarity)

| Tier             | Free Quota               | Pay-as-you-grow               |
| ---------------- | ------------------------ | ----------------------------- |
| All BE customers | 2 analytics plugins free | ₹100/mo per additional plugin |
| Example          | GA + GTM → free          | Adds Facebook Pixel → ₹100/mo |

---

### 2.8 Website Widget / Instant Booking Widget

| Tier             | Free Quota                                 | Pay-as-you-grow                |
| ---------------- | ------------------------------------------ | ------------------------------ |
| All BE customers | 1 widget (website OR instant booking) free | ₹100/mo for second widget type |

---

### 2.9 Multi-Property Booking Engine

| Tier            | Free Quota      | Pay-as-you-grow                       |
| --------------- | --------------- | ------------------------------------- |
| Single property | Included        | —                                     |
| Multi-property  | 1 property free | ₹300/mo per additional property on BE |

---

## PRODUCT 3: PMS (Web + Mobile)

### 3.1 PMS Access

| Tier                            | Free Quota                       | Pay-as-you-grow         |
| ------------------------------- | -------------------------------- | ----------------------- |
| Customers with PMS active today | Grandfathered — keep access free | —                       |
| Customers without PMS           | Not included in base             | ₹300/mo to activate PMS |

---

### 3.2 PMS Users / Staff

| Tier              | Free Quota                          | Pay-as-you-grow                |
| ----------------- | ----------------------------------- | ------------------------------ |
| All PMS customers | Freeze at current user count        | ₹50/mo per additional PMS user |
| Note              | Separate from Channel Manager users |

---

### 3.3 Rooms (Physical Rooms in PMS)

| Tier              | Free Quota                         | Pay-as-you-grow           |
| ----------------- | ---------------------------------- | ------------------------- |
| All PMS customers | Freeze at current room count       | ₹5/mo per additional room |
| Example           | Hotel has 30 rooms today → 30 free | Adds 5 new rooms → ₹25/mo |

**Pricing table:**

| Additional Rooms | Monthly Cost |
| ---------------- | ------------ |
| +1–5 rooms       | ₹5/room/mo   |
| +6–20 rooms      | ₹4/room/mo   |
| +21–50 rooms     | ₹3/room/mo   |
| +50+ rooms       | ₹2/room/mo   |

---

### 3.4 F&B Module (Restaurant + Kitchen + Room Service)

| Tier                            | Free Quota                          | Pay-as-you-grow                   |
| ------------------------------- | ----------------------------------- | --------------------------------- |
| Customers with F&B active today | Grandfathered                       | —                                 |
| Customers without F&B           | Not included                        | ₹500/mo for full F&B module       |
| Restaurants                     | 1 restaurant free (if active today) | ₹200/mo per additional restaurant |
| Kitchens                        | 1 kitchen free (if active today)    | ₹100/mo per additional kitchen    |
| Menu items                      | Up to 50 items free                 | ₹50/mo per additional 50 items    |

---

### 3.5 Housekeeping Module

| Tier                           | Free Quota    | Pay-as-you-grow     |
| ------------------------------ | ------------- | ------------------- |
| Customers with HK active today | Grandfathered | —                   |
| Customers without HK           | Not included  | ₹200/mo to activate |

---

### 3.6 Night Audit

| Tier                               | Free Quota    | Pay-as-you-grow     |
| ---------------------------------- | ------------- | ------------------- |
| Customers with night audit enabled | Grandfathered | —                   |
| Customers without                  | Not included  | ₹100/mo to activate |

---

### 3.7 Guest-Facing Modules (Self Check-in, Room Service QR, Review Page)

| Tier                              | Free Quota    | Pay-as-you-grow    |
| --------------------------------- | ------------- | ------------------ |
| Customers with these active today | Grandfathered | —                  |
| Customers without                 | Not included  | ₹150/mo per module |
| Bundle (all 3 guest modules)      | —             | ₹350/mo            |

---

### 3.8 Reports

| Tier                                                                    | Free Quota                   | Pay-as-you-grow                  |
| ----------------------------------------------------------------------- | ---------------------------- | -------------------------------- |
| Standard reports (sales, occupancy, arrivals, departures)               | Free for all                 | —                                |
| Advanced reports (daily statement, manager report, police verification) | Freeze at current access     | ₹100/mo for advanced report pack |
| F&B reports                                                             | Included with F&B module     | —                                |
| Finance reports                                                         | Included with finance module | —                                |

---

## PRODUCT 4: CRM

### 4.1 CRM Access

| Tier                            | Free Quota    | Pay-as-you-grow         |
| ------------------------------- | ------------- | ----------------------- |
| Customers with CRM active today | Grandfathered | —                       |
| Customers without CRM           | Not included  | ₹400/mo to activate CRM |

---

### 4.2 Contacts

| Tier              | Free Quota                           | Pay-as-you-grow                         |
| ----------------- | ------------------------------------ | --------------------------------------- |
| All CRM customers | Freeze at current contact count      | ₹50/mo per additional 500 contacts      |
| Example           | Customer has 800 contacts → 800 free | Adds 200 more → ₹50/mo (next 500 block) |

**Pricing table:**

| Total Contacts         | Monthly Cost |
| ---------------------- | ------------ |
| Up to current (frozen) | ₹0           |
| +500 contacts          | ₹50/mo       |
| +1,000 contacts        | ₹90/mo       |
| +2,500 contacts        | ₹200/mo      |
| +5,000 contacts        | ₹350/mo      |
| Unlimited contacts     | ₹500/mo      |

---

### 4.3 CRM Users / Agents

| Tier              | Free Quota                    | Pay-as-you-grow             |
| ----------------- | ----------------------------- | --------------------------- |
| All CRM customers | Freeze at current agent count | ₹50/mo per additional agent |

---

### 4.4 Campaigns (Email / SMS / WhatsApp)

| Tier                                  | Free Quota                          | Pay-as-you-grow                     |
| ------------------------------------- | ----------------------------------- | ----------------------------------- |
| Customers with campaigns active today | Grandfathered — keep current volume | —                                   |
| Customers without campaigns           | Not included                        | ₹300/mo for campaign module         |
| Email sends                           | 1,000 emails/mo free                | ₹50/mo per additional 1,000 emails  |
| SMS sends                             | 500 SMS/mo free                     | ₹50/mo per additional 500 SMS       |
| WhatsApp sends                        | 500 messages/mo free                | ₹100/mo per additional 500 messages |

**WhatsApp campaign pricing:**

| Messages / Month | Monthly Cost          |
| ---------------- | --------------------- |
| Up to 500        | ₹0 (if grandfathered) |
| 501–1,000        | ₹100/mo               |
| 1,001–2,500      | ₹200/mo               |
| 2,501–5,000      | ₹350/mo               |
| 5,001–10,000     | ₹500/mo               |
| 10,000+          | Custom                |

---

### 4.5 Membership / Loyalty Program

| Tier                                   | Free Quota             | Pay-as-you-grow                   |
| -------------------------------------- | ---------------------- | --------------------------------- |
| Customers with membership active today | Grandfathered          | —                                 |
| Customers without                      | Not included           | ₹300/mo to activate               |
| Active members                         | Up to 100 members free | ₹50/mo per additional 100 members |
| Membership tiers                       | 1 tier free            | ₹100/mo per additional tier       |

---

### 4.6 QR Codes

| Tier              | Free Quota            | Pay-as-you-grow               |
| ----------------- | --------------------- | ----------------------------- |
| All CRM customers | 5 QR codes free       | ₹20/mo per additional QR code |
| QR scan analytics | Included with each QR | —                             |

---

### 4.7 Tickets / Support Desk

| Tier              | Free Quota             | Pay-as-you-grow                  |
| ----------------- | ---------------------- | -------------------------------- |
| All CRM customers | Unlimited tickets free | —                                |
| Departments       | 3 departments free     | ₹50/mo per additional department |

---

### 4.8 IVR Integration

| Tier                            | Free Quota        | Pay-as-you-grow                |
| ------------------------------- | ----------------- | ------------------------------ |
| Customers with IVR active today | Grandfathered     | —                              |
| Customers without               | Not included      | ₹500/mo to activate            |
| IVR users                       | Freeze at current | ₹50/mo per additional IVR user |

---

## PRODUCT 5: HOTEL CHAIN

### 5.1 Hotel Chain Dashboard Access

| Tier                                    | Free Quota    | Pay-as-you-grow     |
| --------------------------------------- | ------------- | ------------------- |
| Customers with Hotel Chain active today | Grandfathered | —                   |
| Customers without                       | Not included  | ₹500/mo to activate |

---

### 5.2 Properties in Chain Dashboard

| Tier                      | Free Quota                                    | Pay-as-you-grow                 |
| ------------------------- | --------------------------------------------- | ------------------------------- |
| All Hotel Chain customers | Freeze at current property count              | ₹200/mo per additional property |
| Example                   | Customer monitors 3 properties today → 3 free | Adds 4th property → ₹200/mo     |

---

### 5.3 Chain Reports

| Tier                                                               | Free Quota                         | Pay-as-you-grow                  |
| ------------------------------------------------------------------ | ---------------------------------- | -------------------------------- |
| Standard dashboard (overview, summary)                             | Free for all Hotel Chain customers | —                                |
| Advanced reports (rate log, availability grid, coupon utilization) | Freeze at current access           | ₹200/mo for advanced report pack |

---

## CROSS-PRODUCT: SHARED FEATURES

### Multi-Property Management

| Tier                 | Free Quota               | Pay-as-you-grow |
| -------------------- | ------------------------ | --------------- |
| Single property      | Included in all products | —               |
| 2nd property         | ₹300/mo                  |                 |
| 3rd–5th property     | ₹200/mo each             |                 |
| 6th+ property        | ₹150/mo each             |                 |
| Unlimited properties | ₹1,500/mo flat           |                 |

---

### API Access

| Tier                            | Free Quota           | Pay-as-you-grow                     |
| ------------------------------- | -------------------- | ----------------------------------- |
| Customers with API active today | Grandfathered        | —                                   |
| Customers without               | Not included         | ₹500/mo to activate                 |
| API calls                       | 10,000 calls/mo free | ₹100/mo per additional 10,000 calls |

---

### AI Features (Aura AI, Content Generation)

| Tier                           | Free Quota    | Pay-as-you-grow                       |
| ------------------------------ | ------------- | ------------------------------------- |
| Customers with AI active today | Grandfathered | —                                     |
| Customers without              | Not included  | ₹300/mo to activate                   |
| AI content generations         | 20/mo free    | ₹100/mo per additional 50 generations |

---

## SUMMARY: COMPLETE PRICE LIST

| Feature                 | Free Quota              | Unit Price                    |
| ----------------------- | ----------------------- | ----------------------------- |
| **CHANNEL MANAGER**     |                         |                               |
| OTA connections         | Current count frozen    | ₹100/mo per OTA               |
| Room types              | Current count frozen    | ₹50/mo per room type          |
| Rate plans              | Current count frozen    | ₹30/mo per rate plan          |
| Derived rate plans      | Current count frozen    | ₹30/mo per plan               |
| Users / staff           | Current count frozen    | ₹50/mo per user               |
| Active promotions       | 2 free                  | ₹50/mo per promotion          |
| Dynamic pricing module  | If active today: frozen | ₹200/mo to activate           |
| **BOOKING ENGINE**      |                         |                               |
| BE access               | If active today: frozen | ₹500/mo to activate           |
| Payment gateways        | 1 free                  | ₹100/mo per gateway           |
| Coupons / promotions    | 3 free                  | ₹50/mo per coupon             |
| Paid services (add-ons) | 3 free                  | ₹30/mo per service            |
| Packages                | 2 free                  | ₹50/mo per package            |
| Day outing packages     | 1 free                  | ₹50/mo per package            |
| Analytics plugins       | 2 free                  | ₹100/mo per plugin            |
| Website widget          | 1 free                  | ₹100/mo for 2nd type          |
| Multi-property BE       | 1 property free         | ₹300/mo per property          |
| **PMS**                 |                         |                               |
| PMS access              | If active today: frozen | ₹300/mo to activate           |
| PMS users               | Current count frozen    | ₹50/mo per user               |
| Physical rooms          | Current count frozen    | ₹5/mo per room (slab pricing) |
| F&B module              | If active today: frozen | ₹500/mo to activate           |
| Additional restaurant   | 1 free (if active)      | ₹200/mo per restaurant        |
| Additional kitchen      | 1 free (if active)      | ₹100/mo per kitchen           |
| Menu items              | 50 free                 | ₹50/mo per 50 items           |
| Housekeeping module     | If active today: frozen | ₹200/mo to activate           |
| Night audit             | If active today: frozen | ₹100/mo to activate           |
| Guest-facing modules    | If active today: frozen | ₹150/mo per module            |
| Advanced reports pack   | If active today: frozen | ₹100/mo to activate           |
| **CRM**                 |                         |                               |
| CRM access              | If active today: frozen | ₹400/mo to activate           |
| Contacts                | Current count frozen    | ₹50/mo per 500 contacts       |
| CRM agents              | Current count frozen    | ₹50/mo per agent              |
| Campaign module         | If active today: frozen | ₹300/mo to activate           |
| Email sends             | 1,000/mo free           | ₹50/mo per 1,000 emails       |
| SMS sends               | 500/mo free             | ₹50/mo per 500 SMS            |
| WhatsApp sends          | 500/mo free             | ₹100/mo per 500 messages      |
| Membership module       | If active today: frozen | ₹300/mo to activate           |
| Active members          | 100 free                | ₹50/mo per 100 members        |
| Membership tiers        | 1 free                  | ₹100/mo per tier              |
| QR codes                | 5 free                  | ₹20/mo per QR                 |
| Ticket departments      | 3 free                  | ₹50/mo per department         |
| IVR integration         | If active today: frozen | ₹500/mo to activate           |
| **HOTEL CHAIN**         |                         |                               |
| Hotel Chain access      | If active today: frozen | ₹500/mo to activate           |
| Properties in dashboard | Current count frozen    | ₹200/mo per property          |
| Advanced chain reports  | If active today: frozen | ₹200/mo to activate           |
| **CROSS-PRODUCT**       |                         |                               |
| 2nd property            | —                       | ₹300/mo                       |
| 3rd–5th property        | —                       | ₹200/mo each                  |
| 6th+ property           | —                       | ₹150/mo each                  |
| Unlimited properties    | —                       | ₹1,500/mo flat                |
| API access              | If active today: frozen | ₹500/mo to activate           |
| API calls               | 10,000/mo free          | ₹100/mo per 10,000 calls      |
| AI features             | If active today: frozen | ₹300/mo to activate           |
| AI content generations  | 20/mo free              | ₹100/mo per 50 generations    |

---

## IMPLEMENTATION PLAN

### Step 1: Data Snapshot (Week 1)

Pull for every customer:

- Current OTA connections count
- Current user count (CM + PMS + CRM separately)
- Current room count (PMS)
- Current contact count (CRM)
- Active features (BE, PMS, F&B, HK, CRM, Hotel Chain, API, AI)
- Current plan and price

This snapshot becomes their **frozen baseline**.

### Step 2: Communicate the Change (Week 2–3)

Email to all 1,430 customers:

> "We're moving to usage-based pricing. Everything you use today is grandfathered — you'll never pay more for what you have now. Going forward, any new additions (more OTAs, more users, more rooms, new features) will have transparent per-unit pricing. Here's your current usage snapshot and what it means for you."

### Step 3: Build the Billing System (Q1)

- Usage tracking per customer per feature
- In-app notification when approaching a limit
- Self-serve "add more" button with instant billing
- Monthly usage report email to each customer

### Step 4: Launch (Q2)

- New customers go on usage-based pricing from day 1
- Existing customers: frozen baseline + pay-as-you-grow for additions

---

## REVENUE IMPACT ESTIMATE

| Scenario                    | Assumption              | Additional ARR                   |
| --------------------------- | ----------------------- | -------------------------------- |
| 20% of customers add 1 OTA  | 286 customers × ₹100/mo | ₹34.3L/year                      |
| 30% of customers add 1 user | 429 customers × ₹50/mo  | ₹25.7L/year                      |
| 15% activate F&B module     | 215 customers × ₹500/mo | ₹1.29 Cr/year                    |
| 10% activate CRM campaigns  | 143 customers × ₹300/mo | ₹51.5L/year                      |
| 10% add 500 contacts        | 143 customers × ₹50/mo  | ₹8.6L/year                       |
| 5% activate Hotel Chain     | 72 customers × ₹500/mo  | ₹43.2L/year                      |
| 10% add 5 rooms (PMS)       | 143 customers × ₹25/mo  | ₹4.3L/year                       |
| **Conservative total**      |                         | **~₹2.5 Cr/year additional ARR** |

**This is on top of existing revenue — zero price increase for existing usage.**

---

## OPEN QUESTIONS FOR DISCUSSION

1. What is the current average OTA count per customer? (determines baseline)
2. What is the current average user count per customer?
3. What % of customers have F&B, CRM, Hotel Chain active today?
4. Does the billing system support usage-based metering today?
5. How do we handle customers who reduce usage below their frozen baseline? (credit? no refund?)
6. Annual plan customers — do we apply usage pricing at renewal or immediately?
7. What's the grace period for customers to adjust before billing kicks in?
