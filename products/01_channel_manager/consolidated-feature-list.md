# Channel Manager Extranet — Consolidated Feature List

---

## Application Overview

The Bookingjini Channel Manager Extranet (v4) is a hotel distribution management platform that enables hoteliers to manage their property's online presence across multiple OTA channels (Booking.com, Airbnb, Agoda, Expedia, etc.) and their own direct booking engine from a single dashboard. The application covers the full hotel operations lifecycle: property setup, inventory and rate management, booking management (including front-office operations), channel distribution, promotions, reviews, analytics, and subscription billing.

Built as a React 17 + TypeScript SPA with Redux state management, it communicates with 8+ backend microservices and integrates with 7+ OTA channels, 3 payment gateways, and multiple third-party services.

---

## Feature List

### 1. Authentication & User Management

#### 1.1 Email/Password Login

Login with email and password credentials. Stores auth token in Redux with localStorage persistence. Auto-logout on 401 responses from any API.

#### 1.2 Google OAuth Login

Sign in with Google account via `@react-oauth/google`. Alternative to email/password authentication.

#### 1.3 SSO Deep-Link Login

Single sign-on from intranet or hotel chain portals via base64-encoded URL parameters containing company ID, hotel ID, auth token, and user details. Enables seamless cross-platform authentication.

#### 1.4 User Registration (Sign Up)

New user registration with validation step and form submission. Includes signup validation flow.

#### 1.5 Password Reset

Email-based password reset flow with new password entry page.

#### 1.6 Password Change

Change password from within the application with current password verification.

#### 1.7 User CRUD (Manage Users)

Add, edit, and delete sub-users for the company. Assign users to specific hotel properties. Paginated user listing with search.

#### 1.8 Role-Based Access Control (RBAC)

Function-level access permissions per user. 5 feature codes (INVENTORY, RATE, BE, HOTEL, ROOM) control access to specific modules. Super admin (status 911) bypasses all restrictions. Permissions fetched on every route change.

#### 1.9 OTP-Based Channel Access Verification

Email OTP verification required before accessing the Manage Channels section. One-time per session security gate.

---

### 2. Property Management

#### 2.1 Property Selection

After login, select which property to manage from the user's assigned properties. Supports multi-property companies.

#### 2.2 Add New Property Wizard

8-step guided wizard: property type → subtype → details (name, contact) → map address → amenities → images → overview → success. State persisted across steps via Redux.

#### 2.3 Property Basic Details

View and edit core property information: name, address, contact details, country/state. Includes email and mobile management sliders.

#### 2.4 Room Types Management

Full CRUD for room types: create with details/occupancy/pricing/images, edit basic details, manage amenities, configure rate plans, upload/reorder/delete images, set min/max pricing. AI-powered room description generation.

#### 2.5 Add Room Type Wizard

5-step wizard: enter details → set occupancy → configure pricing with rate plans → upload images → success.

#### 2.6 Floors Management

View and manage property floors. Add new floors and configure which floors are serviceable.

#### 2.7 Add Floors Wizard

3-step wizard: specify number of floors → select serviceable floors → success.

#### 2.8 Rooms Management

View rooms organized by floor, add/edit/delete individual rooms, update room status (active/inactive/maintenance).

#### 2.9 Property Amenities

Manage property-level amenities organized by category. Add/remove amenities from the property profile.

#### 2.10 Property Images

Upload, delete, and reorder property images with drag-and-drop support.

#### 2.11 Terms & Policy

View and edit hotel terms and policies: check-in/out times, cancellation policy text, child policy, pet policy, etc.

#### 2.12 Cancellation Rules

Configure cancellation rules with different policies for different booking windows.

#### 2.13 Financial Details

View and manage property bank account details for payment settlements.

#### 2.14 Locale Info

View property locale settings including currency, timezone, and tax configuration.

#### 2.15 Rate Plans

Create, edit, and delete master rate plans. Control rate plan visibility for the booking engine. Supports EP, CP, MAP, AP meal plans.

#### 2.16 Derived Rate Plans

Create rate plans that automatically calculate rates based on a parent rate plan with percentage or fixed adjustments per room type.

#### 2.17 Seasonal Plans

Configure seasonal pricing with different rates for different time periods. Room-wise rate setup per seasonal plan with channel-specific configuration.

#### 2.18 Dynamic Pricing

Set up occupancy-based dynamic pricing rules that automatically adjust rates based on room availability thresholds. Add/edit/delete rules per room type.

#### 2.19 One-Click OTA Setup

Automated OTA onboarding: fetch hotel details, room types, rates, and images from an existing OTA listing (e.g., Booking.com) and import them into the system in one flow.

---

### 3. Inventory Management

#### 3.1 Room-Type Inventory View

Calendar grid showing available rooms per OTA channel for a selected room type and date range. Inline editing of room counts and LOS values directly in calendar cells.

#### 3.2 Channel-Type Inventory View

Alternative view organized by channel/OTA. Shows inventory across all room types for a specific channel with calendar-based editing.

#### 3.3 Bulk Inventory Update

Update inventory for a date range across selected OTAs. Separate handling for Booking Engine and Channel Manager with confirmation step.

#### 3.4 Single-Date Inventory Update

Edit individual cell values in the calendar grid for specific date + channel combinations.

#### 3.5 Block/Unblock Inventory

Block or unblock room availability for specific dates or date ranges. Supports both individual and bulk operations across BE and CM channels.

#### 3.6 Sync Inventory

Synchronize inventory data between the Channel Manager and all connected OTAs to ensure consistency.

#### 3.7 Property Soldout

Mark the entire property as sold out or manage specific sold-out dates. Block/unblock the entire property across all channels.

#### 3.8 Out of Order Rooms

Mark specific rooms as out of order (maintenance, renovation) with reason tracking. Removes from available inventory without blocking the room type.

#### 3.9 Inventory Increase/Decrease with Reasons

Adjust inventory counts with mandatory reason selection for audit trail.

#### 3.10 Available Till

View the last available inventory and rate dates to understand how far into the future inventory is configured.

#### 3.11 Default View Preference

Save and restore preferred inventory view type (room-type vs channel-type) per property.

---

### 4. Rate Management

#### 4.1 Room-Type Rate View

Calendar grid displaying rates per OTA channel for a selected room type and date range. Inline editing of rate values.

#### 4.2 Channel-Wise Rate View

Alternative view organized by channel showing rates across all room types for a specific OTA.

#### 4.3 Bulk Rate Update

Update rates for a date range across selected channels with confirmation step. Separate BE and CM handling.

#### 4.4 Single-Date Rate Update

Edit individual rate values in the calendar grid.

#### 4.5 Block/Unblock Rates

Block or unblock rate availability for specific dates or date ranges across BE and CM channels.

#### 4.6 Sync Rates

Synchronize rate data between the Channel Manager and connected OTAs.

#### 4.7 Calendar Rate Update

Calendar-based rate editing with before/after comparison and confirmation step.

#### 4.8 LOS (Length of Stay) Management

Update minimum length-of-stay restrictions for rates. Supports both bulk and individual updates.

---

### 5. Booking Management

#### 5.1 Booking List View

Tabular listing with filtering by source, status, payment type, date range, and search. Pagination, sorting, CSV export. Inline actions: modify, cancel, resend mail, charge card, download voucher.

#### 5.2 CRS View (Calendar Reservation System)

Calendar-based view showing room allocations across dates. Visual management of room assignments and booking status.

#### 5.3 Front Office View (GEMS)

Front-office calendar showing today's check-ins, in-house guests, and check-outs. Available only for GEMS-subscribed properties with fallback views.

#### 5.4 Check-In

Multi-step check-in: view booking details → allocate rooms → capture guest details (mobile lookup) → complete front-office check-in.

#### 5.5 Stay Details & Billing

Detailed active stay view: guest info, room allocation, billing management (add expenses/collections/payments), checkout actions. PDF bill and invoice download.

#### 5.6 Check-Out

Front-office checkout (GEMS) and CRS room deallocation. Generates final bill.

#### 5.7 Cancel Booking

Cancellation flow with reason selection. Handles cancellation across CRS and OTA channels including Expedia-specific reasons.

#### 5.8 Modify Booking

Modify guest details and booking parameters for existing reservations.

#### 5.9 New Reservation

4-step wizard: select dates → choose room types → enter guest info → select payment method → confirmation. Full Redux state management across steps.

#### 5.10 Walk-In Booking

Quick walk-in flow: check floor-wise room availability → capture guest info → process billing → complete front-office check-in. Generates GRC PDF.

#### 5.11 Booking Messaging

In-app messaging for OTA bookings. Expedia message threads with file attachments. 60-second polling for new messages.

#### 5.12 Charge Card

Charge a guest's card for a booking with payment status tracking. Supports Easebuzz and Razorpay gateways.

#### 5.13 Quick Payment Link

Generate and manage payment links for bookings. Resend emails, check payment status. Supports both reservation-specific and generic payment links.

#### 5.14 Unpaid Bookings

View and manage bookings with pending payments. Send payment reminders.

#### 5.15 Hotel Voucher

Generate and download hotel booking vouchers as PDF.

#### 5.16 Booking Retrieval / Fetch

Retrieve booking details from external sources and import into the system.

---

### 6. Channel Distribution

#### 6.1 Channel Overview

Dashboard of all connected OTA channels and Booking Engine with status indicators. Entry points to configure each channel.

#### 6.2 OTA Onboarding (Generic)

Add new OTA connections: configure hotel ID mapping, API credentials, toggle active/inactive. Supports all OTAs via generic flow.

#### 6.3 OTA Room Sync

Map hotel room types to OTA room types for inventory synchronization. Add, edit, delete room mappings.

#### 6.4 OTA Rate Sync

Map hotel rate plans to OTA rate plans for rate synchronization. Add, edit, delete rate plan mappings.

#### 6.5 Agoda Onboarding

Dedicated onboarding flow: basic setup → room mapping with amenities → rate plan mapping → contract signing. Lazy-loaded for performance.

#### 6.6 Airbnb Integration

OAuth 2.0 connection flow. Listing sync, room/rate mapping, Airbnb-specific promotion rules.

#### 6.7 PMS Integration

Configure PMS connection: hotel code setup, booking push URL, room mapping (PMS ↔ CM), rate plan mapping (PMS ↔ CM).

---

### 7. Booking Engine Management

#### 7.1 Basic Setup (Logo, Banner, Theme, URL)

Configure the direct booking engine's visual identity: upload logo/banners with drag-and-drop reordering, set theme colors, manage booking engine URL, other settings.

#### 7.2 Payment Options

Configure payment gateways (Razorpay, Easebuzz, PayU) for the booking engine. Add, setup, and manage multiple payment modes.

#### 7.3 BE Promotions

Manage booking engine promotions: basic coupons, early bird, last minute, same-day, length-of-stay. Each type has add/modify/view/deactivate flows. Version-gated (BE v3).

#### 7.4 Coupons (Private & Public)

Create and manage discount coupons. Private (code-based) and public coupon management with multi-hotel and single-hotel scoping.

#### 7.5 Paid Services

Manage add-on services (airport pickup, spa, etc.). Service listing, CRUD operations, revenue reports with Excel export.

#### 7.6 Package List

Create and manage room packages with custom pricing, inclusions, and availability.

#### 7.7 Day Outing Packages

Full day-booking management: create packages with menus, blackout dates, special pricing, availability calendar, booking list, and day-outing-specific promotions.

#### 7.8 Additional Charges

Configure extra charges (extra bed, early check-in) applied to bookings.

#### 7.9 Taxes

Configure tax settings based on locale and property type.

#### 7.10 Room Rate Plan (BE)

Manage room-rate plan mappings controlling which rate plans are visible and active per room type on the booking engine.

#### 7.11 Notification Popup

Configure notification popups displayed to guests on the booking engine.

#### 7.12 Promotional Slider

Upload and manage promotional slider images on the booking engine homepage.

#### 7.13 Nearest Locations & Places

Configure nearby transportation hubs and points of interest displayed on the booking engine.

#### 7.14 Cancellation Policy (BE)

Create and manage date-range-based cancellation policies with default policy marking.

#### 7.15 Commissions / Transactions

View payment gateway commission details and payout history with booking-level breakdowns.

#### 7.16 Instant Booking Widget

Configure an embeddable instant booking widget for external websites.

#### 7.17 Website Widget

Configure and manage a website widget for the hotel's website.

#### 7.18 Plugins

Manage third-party plugins integrated with the booking engine.

---

### 8. Promotions (OTA-Level)

#### 8.1 Basic Promotion

Percentage or fixed-amount discounts across selected OTA channels and room types. Blackout dates, date range targeting, active/inactive management.

#### 8.2 Early Bird Promotion

Advance booking discounts — configurable advance days, discount type, and applicable channels.

#### 8.3 Same-Day Promotion

Last-minute discounts for same-day bookings to fill unsold inventory.

#### 8.4 Multi-Night Promotion

Discounts for guests staying multiple nights (e.g., stay 3, get 10% off).

#### 8.5 Day-of-Week Promotion

Day-specific discounts (weekday specials, weekend deals).

#### 8.6 Airbnb Early Bird

Airbnb-specific early bird promotion rules with apply/create/deactivate/view-log flows.

#### 8.7 Airbnb Last Minute

Airbnb-specific last-minute promotion rules.

#### 8.8 Airbnb Length of Stay

Airbnb-specific length-of-stay promotion rules.

---

### 9. Reviews & Reputation

#### 9.1 Review Summary Dashboard

Aggregated review summary across all OTA sources with overall rating, review count, and source-wise breakdown.

#### 9.2 Google Business Reviews

Connect Google Business profile via OAuth, fetch/display reviews, reply to reviews, delete replies. Real-time sync via Server-Sent Events (SSE).

#### 9.3 Booking.com Reviews

Fetch and display Booking.com reviews with sync capability and individual review detail view.

#### 9.4 Airbnb Reviews

Fetch and display Airbnb reviews with sync, individual view, and reply functionality.

---

### 10. Analytics & Performance

#### 10.1 Dashboard — Today's Overview

Real-time summary of check-ins, check-outs, in-house guests, and available rooms with date-type filtering.

#### 10.2 Dashboard — Room Nights & Revenue

Room-type-wise booked room nights and channel-wise revenue visualization using Nivo charts. MTD and YTD views.

#### 10.3 Dashboard — Recent Bookings

Live feed of most recent bookings with guest details, channel source, and payment info.

#### 10.4 Direct Booking Performance

Analyze direct booking trends with date range filtering and comparison views.

#### 10.5 Booking.com Market Insight

Market intelligence: area demand data, booker insights, booking window analysis, pace reports, sales statistics.

#### 10.6 Digital Marketing Report

View marketing engine reports and campaign performance data.

#### 10.7 Channel-Wise Check-In Report

Check-in data broken down by distribution channel with download capability.

#### 10.8 Year in Review

Annual performance summary with month-wise and YoY room night comparisons, quarterly breakdowns.

#### 10.9 Promotional Banner Carousel

Admin-managed promotional banners on the dashboard with internal/external link support.

---

### 11. Subscription & Billing

#### 11.1 Plan Selection

Browse available subscription plans with tier comparison, package details, and checkout. Coupon code validation. Chargebee and Zoho integration.

#### 11.2 Subscription Overview

View current plan details, billing history, transaction records, and app subscriptions. Invoice download.

#### 11.3 Upgrade Subscription

Browse and select higher-tier plans with checkout flow.

#### 11.4 Renew Subscription

Renewal flow for expired subscriptions with plan selection and payment.

#### 11.5 Cancel Subscription

Cancel current subscription with confirmation prompt. Supports both Chargebee and Zoho.

#### 11.6 Auto-Debit Authorization

Set up recurring payment authorization including UPI-based auto-debit.

#### 11.7 Manual Payment

Manual subscription payment via Easebuzz gateway.

#### 11.8 Subscription Gating

Automatic redirect to plan selection when subscription is expired or missing. Checked on every route change.

---

### 12. Support & Help

#### 12.1 Support Tickets

Raise, view, and manage support tickets. Status filtering (Open/Resolved/Closed), detail view with comment thread, rich text replies (CKEditor), ticket closure.

#### 12.2 FAQ & Help Center

Searchable FAQ system with category browsing, initial FAQ display, detailed slider view, and ability to submit new questions.

#### 12.3 Contact Support

Contact support information display with slider-based UI.

#### 12.4 Bookingjini Learning

Daily quick tips and blog content for hotelier education.

---

### 13. Notifications & Announcements

#### 13.1 Notification Center

View all system notifications with read/unread status tracking and mark-as-read functionality.

#### 13.2 System Announcements

Modal-based system announcements with dismiss/acknowledge actions.

---

### 14. Other

#### 14.1 File Manager

Global file/image upload management component accessible from multiple modules.

#### 14.2 Apps Marketplace

Browse available apps/integrations and view detailed information.

#### 14.3 Mart (Coming Soon)

Placeholder for a future marketplace feature.

#### 14.4 AI Content Generation

AI-powered description generation for rooms and properties via Kernel GenAI API.

#### 14.5 Mobile App Redirect

Desktop-only application — mobile/tablet users see an app download screen.

---

## Key User Flows

### Flow 1: New Hotelier Onboarding

```
Sign Up → Login → Select/Add Property (8-step wizard) → Add Room Types (5-step wizard)
→ Add Floors (3-step wizard) → Add Rooms → Choose Subscription Plan → Dashboard
```

### Flow 2: Daily Operations

```
Login → Dashboard (today's overview) → Bookings List View → Check-In guests
→ Manage Stay Details → Add Bills/Payments → Check-Out → View Revenue Analytics
```

### Flow 3: Inventory & Rate Management

```
Dashboard → Inventory (calendar view) → Update/Block/Sync inventory
→ Rates (calendar view) → Update/Block/Sync rates → Verify across channels
```

### Flow 4: OTA Channel Setup

```
Manage Channels → OTP Verification → Add Channel → Configure Hotel ID
→ Room Sync (map room types) → Rate Sync (map rate plans) → Sync Inventory & Rates
```

### Flow 5: Booking Engine Configuration

```
Booking Engine → Basic Setup (logo/banner/theme/URL) → Payment Options
→ Room Rate Plans → Taxes → Cancellation Policy → Promotions/Coupons → Go Live
```

### Flow 6: New Reservation

```
Bookings → Reservation → Select Dates → Choose Room Types → Enter Guest Info
→ Select Payment Method → Confirm → Success (voucher download)
```

### Flow 7: Walk-In Guest

```
Bookings → Walk-In → Check Availability (floor-wise) → Guest Info
→ Billing → Check-In → Generate GRC PDF
```

### Flow 8: Review Management

```
Reviews → Summary Dashboard → Google Reviews (OAuth connect → sync → reply)
→ Booking.com Reviews (sync → view) → Airbnb Reviews (sync → reply)
```

---

## Integrations

| Integration                 | Category             | Direction                                                          |
| --------------------------- | -------------------- | ------------------------------------------------------------------ |
| Booking.com                 | OTA Channel          | Bidirectional (inventory/rates push, bookings/reviews pull)        |
| Airbnb                      | OTA Channel          | Bidirectional (OAuth, inventory/rates push, bookings/reviews pull) |
| Agoda                       | OTA Channel          | Bidirectional (dedicated onboarding, inventory/rates push)         |
| Expedia                     | OTA Channel          | Bidirectional (inventory/rates push, bookings/messaging pull)      |
| Goibibo, MakeMyTrip, others | OTA Channels         | Bidirectional (generic OTA flow)                                   |
| Google My Business          | Reviews              | Pull (OAuth, SSE real-time sync, reply)                            |
| Razorpay                    | Payment Gateway      | Bidirectional (setup, payment processing)                          |
| Easebuzz                    | Payment Gateway      | Bidirectional (setup, payment processing, payouts)                 |
| PayU                        | Payment Gateway      | Bidirectional (setup, payment processing)                          |
| Chargebee                   | Subscription Billing | Bidirectional (checkout, plan management)                          |
| Zoho Subscriptions          | Subscription Billing | Bidirectional (checkout, plan management)                          |
| Google OAuth                | Authentication       | Inbound (user login)                                               |
| Google Maps                 | Property Setup       | Outbound (address autocomplete, map display)                       |
| Google reCAPTCHA            | Security             | Outbound (bot protection)                                          |
| Google Analytics            | Tracking             | Outbound (page view tracking)                                      |
| Generic PMS                 | Property Management  | Bidirectional (room/rate mapping, booking push)                    |
| Tawk.to                     | Live Chat            | Outbound (currently disabled)                                      |
| ZapScale                    | Product Analytics    | Outbound (currently disabled)                                      |
| CKEditor                    | Rich Text            | Local (ticket replies)                                             |

---

## Observations / Missing Features

### Present but Incomplete

- **Guests module**: Route exists (`/guests`) but shows "Content coming soon" placeholder.
- **Campaigns module**: Route exists (`/campaigns`) but shows "Content coming soon" placeholder.
- **Analytics module**: Route exists (`/analytics`) but shows "Content coming soon" placeholder.
- **Mart**: Page exists but shows "Launching Soon" placeholder.
- **Tawk.to live chat**: Integrated but commented out across the codebase.
- **ZapScale tracking**: Integrated but commented out.
- **Token verification**: `verifyAuthToken()` exists but is commented out — no server-side token validation.

### Missing Features (Common in Channel Managers)

- **Revenue Management System (RMS)**: No automated pricing recommendations based on demand/competition.
- **Rate Shopper**: Only a trend graph endpoint exists (`staging.bookingjini.com/api/rateshopper`); no full competitor rate comparison tool.
- **Guest CRM**: No guest profile management, loyalty tracking, or communication history beyond booking-level data.
- **Housekeeping Management**: No room cleaning status tracking or housekeeping task assignment.
- **Channel-Level Analytics**: No per-channel ROI analysis, commission tracking per OTA, or channel performance comparison dashboard.
- **Automated Messaging**: No automated pre-arrival/post-stay email templates or triggers.
- **Multi-Language Support**: No i18n/l10n infrastructure — all UI text is hardcoded in English.
- **Audit Log**: No user action audit trail beyond the Airbnb promotion view log.
- **Bulk Property Management**: No cross-property bulk operations for hotel chains.
- **API Rate Limiting Visibility**: No dashboard showing API call quotas or rate limit status per OTA.
- **Offline Support**: No service worker, no offline capability.
- **Accessibility**: No ARIA attributes, no keyboard navigation patterns, no screen reader support observed.
