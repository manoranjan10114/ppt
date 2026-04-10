# Bookingjini Extranet — Detailed Product Features

A comprehensive channel manager and hotel distribution platform built as a React 17 + TypeScript web application. The app is branded under "Bookingjini" and serves hotel owners and managers to manage inventory, rates, bookings, OTA channels, direct booking engine, promotions, reviews, and analytics from a single desktop dashboard.

---

## 1. Onboarding & Authentication

### Email/Password Login

- Standard email and password login form
- Email stored in Redux for session persistence
- Auth token stored in Redux + localStorage via redux-persist
- Google reCAPTCHA integration for bot protection

### Google OAuth Login

- "Sign in with Google" via `@react-oauth/google` provider
- Google client ID configured at app level
- Returns auth token on successful Google authentication

### SSO Deep-Link Login (Intranet)

- Single sign-on from intranet or hotel chain portals
- URL pattern: `/:company_id/:comp_hash/:hotel_id/:admin_id/:auth_token/:full_name/:subscription_customer_id/:company_url/:source_name`
- Base64-encoded segments detected on app load (`aW50cmFuZXQ=` for intranet, `aG90ZWxjaGFpbg==` for hotel chain)
- Auto-authenticates user and sets active property without manual login
- Controlled by `single_sign_on_status` flag in Redux

### User Registration (Sign Up)

- Sign-up validation step before form access
- Registration form with user details
- Redirects to login after successful registration

### Password Management

- Reset password flow via email verification
- Enter new password page after reset link
- Change password from user profile (current password verification required)
- Change email functionality for admin accounts

### Multi-Property Support

- A single user account can manage multiple hotel properties
- Property switcher dropdown in the header (visible when user has 2+ properties)
- Searchable property list with hotel name and ID
- Switching properties triggers: navigation to dashboard, announcement fetch, switch animation overlay
- Each property has its own hotel_id, hotel_name, city, state
- "Add New Property" option with eligibility check via `can_add_property` API

### Property Onboarding Wizard

- 8-step guided wizard with Redux state persistence across steps:
  1. Property Type selection (hotel, resort, homestay, etc.)
  2. Property Subtype selection (cascading from type)
  3. Property Details (name, mobile, email, landline)
  4. Map Address (Google Maps autocomplete + "Get My Location" GPS, flat/street/city/state/pin/country)
  5. Amenities selection from categorized master list
  6. Image upload (multiple property images)
  7. Overview/Review (summary of all entered data)
  8. Success confirmation with property ID
- State resets on wizard completion via `ADD_PROPERTY_RESET` action

### Subscription Gating

- Subscription status checked on every route change via `property-subscription-status` API
- Three states: ACTIVE (proceed), NO-PLAN (redirect to `/choose-plan`), PLAN-EXPIRED (redirect to `/choose-plan/renew-subscription`)
- Loading spinner with "Checking your Subscription" message during verification
- Subscription days remaining shown in header when ≤7 days left
- Extension status message shown when subscription is on extension

### Session Management

- Auto-logout on HTTP 401 from any API call (all Axios interceptors)
- "Session Expired" toast notification on forced logout
- Token verification endpoint exists (`verify-token`) but is currently disabled
- Logout clears localStorage and reloads the page
- Desktop-only: mobile/tablet users see an "App Download" screen via `react-device-detect`

---

## 2. Dashboard (Home Screen)

### Header & Navigation

- Fixed header bar with:
  - Current property name + ID (clickable to open property switcher)
  - Subscription expiry warning (when ≤7 days remaining)
  - "Apps" dropdown: shows subscribed Bookingjini apps (GEMS, etc.) with SSO launch into each app
  - "Manage" dropdown: Channels, Files, Users (role_id === 1 only), Subscription, Add New Property
  - "Help" dropdown: WhatsApp Support, FAQ, Tickets, Contact Us
  - Notification bell with unread count badge (99+ cap), navigates to `/notifications`
  - User profile icon with logout and change password options
- Left sidebar with 11 navigation items (from SidebarMenu.json):
  - Dashboard, Inventory, Rates, Bookings, Promotion, Reviews, Apps, Performance, Mart (external link), Booking Engine, Setup
  - Active item highlighting synced via Redux `sidebar.active_sidebar_item`
  - App version number displayed at sidebar bottom

### Promotional Banner Carousel

- Swiper-based auto-playing carousel at the top of the dashboard
- Banners fetched from `getpromotionalbanner/{hotel_id}` API
- Supports three link types: Internal (in-app navigation), External (new tab), None (display only)
- Auto-play with 5-second interval, 2-second transition, pause on hover

### Confirmed Bookings Overview

- Real-time summary cards showing today's operational data
- Filterable by: Today, MTD (Month to Date), YTD (Year to Date)
- Data from `hotelier-today-overview-new` and `hotelier-summary-new` APIs

### Room Nights & Revenue Analytics

- Nivo chart visualizations:
  - Room-type-wise booked room nights (bar chart)
  - Channel-wise booked room nights and revenue (bar + line chart)
  - Room types comparison
- Data from `roomtype-wise-booked-room-nights` and `channel-wise-booked-room-nights-revenue` APIs

### Front Office Data

- Today's front-office summary: check-ins, in-house, check-outs
- Quick access to front-office operations

### Recent Bookings Feed

- Live feed of most recent bookings
- Each entry shows: booking date, guest name, phone, check-in/out dates, channel logo, paid amount
- Data from `booking-details/{hotel_id}` API

### Bookingjini Learning Card

- Daily quick tips for hoteliers from `extranet-quick-tips/daily-tip` API
- Blog content links from `blog` API
- Educational content about platform features and hospitality best practices

### Announcement System

- HTML-based announcement popups from backend (`get_extranet_announcement`)
- Modal overlay with announcement content
- "Don't show again" button that acknowledges via `no_show_announcement` API
- Triggered on property switch and initial load

---

## 3. Inventory Management

### Room-Type Inventory View

- Calendar grid showing available rooms per OTA channel for a selected room type
- Date range: configurable start date with 10-day window
- Room type selector dropdown (all room types for the property)
- Each row = one OTA channel (with logo), each column = one date
- Cell values: room count (editable inline) and block status (color-coded)
- Inline editing: click cell → edit room count → changes tracked in state
- LOS (Length of Stay) values editable per cell
- "Apply" button appears when changes are detected
- Separate BE (Booking Engine) and CM (Channel Manager) update paths

### Channel-Type Inventory View

- Alternative view organized by channel/OTA
- Shows inventory across all room types for a specific channel
- Calendar-based editing with confirmation step
- Switchable via "Room Type View" / "Channel Type View" toggle
- Default view preference saved per property via `update-property-default-view` API

### Bulk Inventory Update (Slider)

- Sliding panel from right side
- Select date range, room type, and target OTAs
- Enter new inventory count
- Separate API calls for BE (`manage_inventory/update-inv`) and CM (`inventory/bulk-inventory-update-new`)
- Confirmation step showing before/after values

### Block Inventory (Slider)

- Block room availability for specific dates or date ranges
- Select channels to block (BE, CM, or both)
- Confirmation prompt before blocking
- APIs: `block-specific-dates` (BE), `inventory/individual-inventory-block` (CM individual), `inventory/bulk-inventory-block` (CM bulk)

### Unblock Inventory (Slider)

- Restore blocked inventory for specific dates or date ranges
- Confirmation prompt before unblocking
- APIs: `unblock-specific-dates` (BE), `inventory/individual-inventory-unblock` (CM individual), `inventory/bulk-inventory-unblock` (CM bulk)

### Sync Inventory

- Synchronize inventory data between Channel Manager and all connected OTAs
- Single API call: `inventory/sync-inventory-new`
- Loading indicator during sync operation

### Property Soldout

- Mark entire property as sold out across all channels
- APIs: `block-property` (BE), `block_specific_property` (CM)
- Unblock: `unblock-property` (BE), `unblock_specific_property` (CM)
- View sold-out dates list via `get-block-dates`

### Out of Order Rooms

- Mark specific rooms as out of order (maintenance, renovation)
- Reason selection from configurable list (`increase-decrease-inventory-reasons`)
- Removes from available inventory without blocking the room type
- API: `inventory/out-of-order`

### Inventory Increase/Decrease with Reasons

- Adjust inventory counts with mandatory reason selection for audit trail
- API: `inventory/inventory-increase-decrease`

### Available Till

- Shows the last available inventory date and last available rate date
- Helps hoteliers understand how far into the future their inventory is configured
- APIs: `get-last-available-inv-dates`, `get-last-available-rate-dates`

---

## 4. Rate Management

### Room-Type Rate View

- Calendar grid displaying rates per OTA channel for a selected room type and date range
- Rate plan selector (fetched from `master_hotel_rate_plan/room_rate_plan_by_room_type`)
- Inline editing of rate values directly in calendar cells
- LOS (Length of Stay) values editable per cell
- Max occupancy slider for viewing occupancy-based pricing

### Channel-Wise Rate View

- Alternative view organized by channel
- Shows rates across all room types for a specific OTA
- Calendar-based rate editing per channel
- Confirmation step with before/after comparison
- APIs: `rates/getrates-channelwise`, `rates/channel-wise-calendar-update`

### Bulk Rate Update (Slider)

- Update rates for a date range across selected channels
- Separate handling for BE (`rates/room_rate_update_new`) and CM (`manage_inventory/room_rate_update`)
- Confirmation step showing changes

### Block/Unblock Rates (Sliders)

- Block: hide rates for specific dates/ranges across BE and CM
- Unblock: restore blocked rates
- Individual date and bulk date-range operations
- APIs: `block-rate-specific-dates` (BE), `rates/individual-rate-block-new` (CM), `rates/bulk-rate-block-new` (CM bulk)

### Sync Rates

- Synchronize rate data between Channel Manager and connected OTAs
- API: `rates/sync-rate-new`

### Calendar Rate Update

- Calendar-based rate editing with confirmation step
- Shows before/after comparison of all modifications
- API: `rates/calendar-rate-update`

### LOS (Length of Stay) Management

- Update minimum length-of-stay restrictions
- Bulk update: `rates/bulk-los-update`
- Individual update: `rates/individual-los-update`

---

## 5. Booking Management

### List View

- Tabular listing of all bookings with comprehensive filtering:
  - Date range picker (default: past 7 days)
  - Source filter (multi-select from all connected OTAs)
  - Status filter: All, Confirmed, Cancelled, No-Show, Checked-in, Checked-out
  - Payment type filter
  - Free-text search (booking ID, guest name)
  - Date type toggle (booking date vs check-in date)
  - Sorting (ASC/DESC)
- Pagination with configurable page size (10/15/20/25/30)
- Each booking row shows: channel logo, booking ID, check-in/out dates, guest name, phone, room type, amount, status badge
- Inline actions per booking (via sliding panels):
  - View booking details
  - Modify booking
  - Cancel booking
  - Resend confirmation email
  - Charge card
  - Download hotel voucher
  - Fetch/retrieve booking from OTA
  - Booking update
- CSV report download via `listview-bookings-report` API

### CRS View (Calendar Reservation System)

- Calendar-based view showing room allocations across dates
- Room types displayed as rows, dates as columns
- Visual booking blocks showing guest names and stay duration
- Color-coded by booking status
- Click booking block to view details

### Front Office View (GEMS)

- Available only for GEMS-subscribed properties (checked via `isGEMSSubscribed` API)
- Three variants based on subscription:
  - `FrontofficeViewGems`: Full GEMS front-office calendar with check-in/out/in-house
  - `FrontOfficeViewNoGems`: Limited front-office view without GEMS features
  - `NoFrontOfficeView`: Fallback when no front-office subscription
- Calendar showing today's operations: arrivals, in-house, departures

### Check-In

- Date-based check-in list with tabs: Pending / Completed / All
- Select booking → verify guest details → assign rooms floor-wise → confirm
- Floor-wise room availability display (color-coded: available, occupied, blocked)
- Room allocation with specific room number assignment
- Guest details capture and verification
- Check-in success confirmation
- APIs: `frontOfficeCheckin3` (GEMS), `extranet-bookings/allocateroom` (room allocation)

### Stay Details & Billing

- Detailed view of active stay per booking:
  - Guest information with contact details
  - Room allocation details with room type, plan, rate
  - Billing section with Dues/Collection tabs:
    - Itemized list of all charges with dates, remarks, amounts
    - Itemized list of all payments/collections
    - Total receivable vs received balance
  - Add expense, add collection, add payment actions
  - Payment mode selection for collections
- PDF bill download via `generatePDFBill`
- PDF invoice download via `generatePDFInvoice`
- Check-out action: front-office checkout (`frontOfficeCheckout`) or CRS deallocation (`extranet-bookings/deallocateroom`)

### Cancel Booking

- Cancellation flow with reason selection
- Handles cancellation across CRS and OTA channels
- Expedia-specific: fetch Expedia reasons (`fetch-expedia-reasons`), update Expedia reservation (`update-expedia-reservations`)
- API: `crs/crs_cancel_booking`

### Modify Booking

- Modify guest details: name, email, phone, address
- Modify booking parameters: dates, room count, room type
- Requires confirmation before applying changes
- APIs: `crs/crs-modified-guest-details`, `crs/crs_modify_bookings-new`

### New Reservation (4-Step Wizard)

- Step 1 — Check-in/out Dates: full-screen calendar, past dates allowed for backdated reservations
- Step 2 — Add Rooms: room type selection with availability, inventory check via `bookingEngine/get-inventory`
- Step 3 — Guest Info: name, email, mobile, address capture; guest registration via `user/register`
- Step 4 — Payment Method: payment mode selection, partial payment support, internal/guest remarks
- State managed through Redux `reservation` slice with 14 fields
- Step navigation with validation (can't skip ahead without completing current step)
- Booking creation via `bookingEngine/bookings` API
- Success page with booking ID, voucher download option
- Full state cleanup on wizard unmount

### Walk-In Booking

- 3-step wizard: Choose Date → Billing → Guest Info
- Floor-wise room availability display with room status (Vacant & Clean / Vacant & Dirty)
- Multi-room selection across room types
- Billing step: advance payment collection with payment mode selection
- Guest info capture
- APIs: `getAvailableRoomsFloorwiseForWalkin` (availability), `frontOfficeCheckin5` (GEMS check-in), `gems-booking` (create booking), `user/register` (guest registration)

### Booking Messaging

- In-app messaging for OTA bookings
- Expedia message threads: view thread, send messages, upload file attachments
- 60-second polling interval for new messages (`setInterval(() => fetchMessages(), 60000)`)
- APIs: `expedia-messaging/get-message-thread`, `expedia-messaging/post-message`, `expedia-messaging/upload-attachement`

### Charge Card & Payment

- Charge guest's card for a booking
- Payment status tracking page (`/bookings/charge-card-status/:msg`)
- Easebuzz and Razorpay gateway integration
- APIs: `onpage-easebuzz-response`, `onpage-razorpay-response`

### Quick Payment Links

- Generate payment links for bookings
- Two types: reservation-specific and generic payment links
- Track link status: Active / Received / Expired
- Resend email functionality
- APIs: `reservation-payment-link/all`, `generic-payment-link/add`, `reservation-payment-link/check`

### Unpaid Bookings

- View bookings with pending payments
- Send payment reminders
- Slider detail view per unpaid booking

### Hotel Voucher

- Generate and download booking voucher as PDF
- Includes booking details, guest info, room details, payment summary

---

## 6. Channel Distribution

### Channel Overview

- Dashboard showing all connected OTA channels and Booking Engine
- Each channel card shows: OTA logo, connection status, last sync time
- OTP-based access verification required on first visit per session:
  - Email OTP sent via `sendotpbyemail`
  - OTP verification via `verifyotp`
  - OTP screen shown when navigating to `/manage-channels` from outside the section
- "Add Channel" slider to connect new OTAs
- APIs: `get_added_channels` (connected), `getallchannel` (available)

### OTA Onboarding (Generic Flow)

- Add new OTA connection:
  1. Select OTA from available list
  2. Enter OTA hotel ID / credentials
  3. Configure hotel details mapping
  4. Toggle active/inactive
- OTA details sync: `cm_ota_details/sync`
- Sync history and unmapped rooms: `get_unmapped_room_types_n_last_sync`
- Payment link configuration per OTA: `cm_ota_details/update-ota-payment-link`

### OTA Room Sync

- Map hotel room types to OTA room types
- Add new mapping: select hotel room type → select OTA room type → save
- Edit existing mappings
- Delete mappings
- APIs: `cm_ota_roomtype_sync_new/add`, `cm_ota_rateplan_sync_new/rooms`, `cm_ota_roomtype_sync/ota_room_type`

### OTA Rate Sync

- Map hotel rate plans to OTA rate plans
- Add/edit/delete rate plan mappings
- APIs: `cm_ota_rateplan_sync_new/ota_room_type`, `cm_ota_rateplan_sync_new/add`, `master_hotel_rate_plan/rate_plan_by_room_type`

### Agoda Onboarding (Dedicated Flow)

- Lazy-loaded module (`React.lazy`) with Suspense fallback
- Multi-step onboarding:
  1. Basic Setup: hotel details, cancellation policy, services configuration
  2. Room Mapping: map room types with amenities selection
  3. Rate Mapping: map rate plans
  4. Contract signing verification
- All APIs via `cm3Api` (dedicated Agoda backend): `gethoteldetails`, `onboard`, `getroomtype`, `createroom`, `createrateplan`, `createhotelproduct`, `iscontractsigned`

### Airbnb Integration

- OAuth 2.0 connection flow:
  - Redirect to `airbnb.com/oauth2/auth` with client_id and scopes (property_management, messages_read, messages_write)
  - Callback to `/airbnb-code` route
  - Save OAuth code via `air-bnb/save-code` API
- Listing sync: `get-all-airbnb-listing`
- Room/rate mapping via standard OTA sync flow

### PMS Integration

- Configure Property Management System connection:
  - Hotel code setup: `cm_pms_details/get_pms_hotel_code`
  - Booking push URL: `cm_pms_details/get_pms_booking_push_url`, `update_pms_booking_push_url`
  - Room mapping: map PMS rooms to CM rooms (`save_pms_room_sync`, `get_pms_room_sync_all`)
  - Rate plan mapping: map PMS rate plans to CM rate plans (`save_pms_rateplan_sync`, `get_pms_rateplan_sync_all`)
  - Add PMS room types and rate plans
  - Remove sync mappings

---

## 7. Booking Engine Management

### Basic Setup

- Logo Management: upload/replace booking engine logo
- Banner Management: upload multiple banners, drag-and-drop reordering (`DraggableImages`), delete banners, reorder via `update-banner-order` API
- Theme Settings: color theme configuration for booking engine
- URL Management: configure and update booking engine URL (`manage-url`, `manage-url/update`)
- Other Settings: bank details, payment settings, ratings card, additional configuration
- VOSA Settings: VOSA-specific configuration

### Payment Options

- Configure payment gateways for the booking engine:
  - List available gateways: `get-payment-gateways`
  - Setup gateway: Razorpay, Easebuzz, PayU configuration
  - PayU-specific setup flow (`CheckHavingPayU`, `PayUSetupPayment`)
  - Add new payment mode
  - Toggle gateway active/inactive
- API: `update-paymentgateway-setup`, `select-paymentgateway-setup`

### Promotions (BE-Specific, Version-Gated)

- Available when `be_version === 3`:
  - Basic Coupon Promotion: create/edit/deactivate/view coupons with active/inactive lists
  - Early Bird Promotion: advance booking discounts with configurable days
  - Last Minute Promotion: same-day/last-minute discounts
  - Same-Day Promotion: day-of discounts with time picker
  - Length of Stay Promotion: multi-night discount rules
- Each promotion type has: Add, Modify, View, Deactivate, Inactive list, Blackout dates slider
- Promotion hub page (`BePromotion`) as entry point

### Coupons

- Two coupon systems based on BE version:
  - BE v3: Private coupons (`BePrivateCoupon`) — code-based, add/modify/inactive management
  - Non-v3: Public/Private coupons (`PublicPrivateCoupon`) — multi-hotel and single-hotel scoping
- CRUD operations: `coupons/get`, `coupons/add`, `coupons/update`, `coupons/fetch`, `coupons` (delete)
- Coupon types: `coupons/list/type`

### Paid Services

- Manage add-on services offered through booking engine (airport pickup, spa, etc.)
- Service listing with CRUD: `paid_services/list`, `paid_services/add`, `paid_services/update`, `paid_services/delete`
- Revenue reports: `paid_services/report`
- Excel export: `paid-services-report/excel`

### Package List

- Create and manage room packages with custom pricing and inclusions
- Add/Edit/Delete packages
- Package details: name, description, pricing, availability
- APIs via `hotel_other_information` and `hotel_other_information/update`

### Day Outing Packages

- Full day-booking/day-outing management:
  - Package CRUD: `day-booking/package-add`, `day-booking/package-update`, `day-booking/package-details`
  - Active/Inactive package lists: `day-booking/active-packages`, `day-booking/inactive-packages`
  - Toggle package status: `day-booking/package-status`
  - Special pricing: set/delete special prices for specific dates
  - Blackout dates: set/delete blackout dates
  - Availability calendar: `day-booking/availability-calendar`
  - Booking list: `day-booking/booking-list` with detail view
  - Menu setup: `day-booking/menu-setup`
  - Day-outing-specific promotions and coupons
  - Image management with delete capability

### Additional Charges

- Configure extra charges applied to bookings (extra bed, early check-in, etc.)
- CRUD: `select-all-charges`, `add-charges`, `update-charges`, `delete-charges`
- Toggle active/inactive: `active-charges`

### Taxes

- Configure tax settings based on property locale
- Tax details from `locale-details/{hotel_id}`

### Room Rate Plan (BE)

- Manage which rate plans are visible on the booking engine per room type
- List all rate plans: `master_hotel_rate_plan/all`
- Toggle BE visibility: `master_hotel_rate_plan/update-status-for-be`

### Notification Popup

- Configure notification popups displayed to guests on the booking engine
- Fetch existing: `be-notifications`
- Create/update: `be-notifications-popup`

### Promotional Slider

- Upload and manage promotional slider images on booking engine homepage
- Fetch: `fetch-be-notification-slider-images`
- Upload: `upload-be-notification-slider-images`
- Delete: `delete-be-notification-slider-image`

### Nearest Locations & Places

- Configure nearby transportation hubs: Airport, Bus Station, Railway Station
- Each with distance and directions
- Configure nearby points of interest / tourist attractions
- APIs via `hotel_other_information` and `hotel_other_information/update`

### Cancellation Policy (BE)

- Date-range-based cancellation policies
- CRUD: `cancellation_policy/date-range/add`, `update`, `list`, `delete`
- Mark default policy: `cancellation_policy/date-range/status`

### Commissions / Transactions

- View Bookingjini payment gateway commission details
- Payout history with booking-level breakdowns
- Detailed booking view per commission entry
- Column definitions for commission and payout tables
- API: `commission-list`

### Instant Booking Widget

- Configure embeddable instant booking widget for external websites
- API: `instant-booking/setup`

### Website Widget

- Configure website widget for hotel's own website
- Fetch/Save/Toggle: `website-widget/fetch`, `website-widget/save`, `website-widget/status`

### Plugins

- Manage third-party plugins integrated with booking engine
- Fetch plugin list: `fetch-plugin`
- Save plugin configuration: `store-plugin`
- Plugin data display with labels

---

## 8. Promotions (OTA-Level)

### Promotion Types Hub

- Landing page showing all available promotion types as cards
- Navigation to each specific promotion management screen
- Promotion types: Basic, Early Bird, Same Day, Multi-Night, Day of Week, Airbnb-specific

### Basic Promotion

- Percentage or fixed-amount discounts across selected OTA channels and room types
- Create: select channels, room types, rate plans, discount type/value, date range, blackout dates
- Active promotions list with deactivate action
- Inactive promotions list with reactivate action
- View promotion details
- Modify existing promotions
- Blackout dates management via "See More" slider
- APIs: `get-all-promotion`, `insert-hotel-promotion-new`, `deactivate-promotion-new`, `activate-promotion-new`, `get-hotel-promotion`, `update-hotel-promotion-new`, `view-promotion`

### Early Bird Promotion

- Advance booking discounts — guests who book X days in advance get a discount
- Configurable advance days from `advance-days` API
- Same CRUD flow as Basic Promotion with advance-day parameter

### Same-Day Promotion

- Last-minute discounts for same-day bookings to fill unsold inventory
- Same CRUD flow as Basic Promotion

### Multi-Night Promotion

- Discounts for guests staying multiple nights (e.g., stay 3 nights, get 10% off)
- Configurable minimum night threshold
- Same CRUD flow as Basic Promotion

### Day-of-Week Promotion

- Day-specific discounts (weekday specials, weekend deals)
- Select specific days of the week for discount application
- Same CRUD flow as Basic Promotion

### Airbnb Early Bird Promotion

- Airbnb channel-specific early bird rules
- Rule-based system (different from standard promotions):
  - Create rule: `airbnb/rule/create`
  - Apply rule to room types: `airbnb/rule/apply`
  - View active rules: `airbnb/rule/get/{hotel_id}`
  - View inactive rules: `airbnb/rule/get/inactive/{hotel_id}`
  - Deactivate rule: `airbnb/rule/deactive`
  - Remove applied rule: `airbnb/rule/remove`
  - View application log: `airbnb/viewlog`
- Airbnb connection status check: `check-airbnb-status`
- Airbnb-specific rate plans: `get_room_type_rate_plans_airbnb`

### Airbnb Last Minute Promotion

- Airbnb-specific last-minute promotion rules
- Same rule-based flow as Airbnb Early Bird
- Promotion type: `last-minute`

### Airbnb Length of Stay Promotion

- Airbnb-specific length-of-stay promotion rules
- Same rule-based flow as Airbnb Early Bird
- Promotion type: `length-of-stay` / `lengthofstay-airbnb`

---

## 9. Reviews & Reputation Management

### Review Summary Dashboard

- Aggregated review summary across all connected OTA sources
- Overall rating, total review count, source-wise breakdown
- Review list with individual review cards
- Navigate to source-specific review pages
- APIs: `review/get_hotel_summary`, `review/allreview`, `review/ota-review-list`

### Google Business Reviews

- Google OAuth connection flow:
  1. Login: `google-review/login` / `google-review/googlelogin`
  2. Fetch Google accounts: `google-review/fetchAccount`
  3. Select business location: `google-review/fetchLocation` → `google-review/setLocation`
- Real-time review sync via Server-Sent Events (SSE):
  - EventSource connection to `https://gmb-sse.bookingjini.com/review-store?hotel_id={id}`
  - Streams review count during initial sync
  - Closes connection on "DONE" message
  - One-time sync tracked via `callReviewStoreGoogle` Redux flag
- Review management:
  - View all Google reviews with date range filter, rating filter, pagination
  - Reply to reviews: `review/replyreview`
  - Delete replies: `google-review/deleteReply`
- Google My Business status check: `review/gmb-status`
- Logout: `google-review/logout`

### Booking.com Reviews

- Fetch and display Booking.com reviews
- Sync reviews from Booking.com: `bookingdotcom-review/sync-review`
- Review list: `bookingdotcom-review/allreviews`
- OTA review list: `bookingdotcom-review/otareviewlist`
- Individual review detail view (`BookingReviewSingleView`)

### Airbnb Reviews

- Fetch and display Airbnb reviews
- Sync reviews: `airbnb-review/sync-review`
- All reviews: `airbnb-review/allreviews`
- Single review detail: `airbnb-review/single-review`
- Reply to reviews: `airbnb-review/reply-review`

### Review Sources Setup

- Configure which review sources are active
- View all available review sources: `review/review-sources`
- Sync all reviews across sources: `review/sync-all-review`

---

## 10. Analytics & Performance

### Performance Home Screen

- Dropdown selector for report type:
  - Direct Booking Performance (default)
  - Area Demand Report
  - Market Insight
  - Digital Marketing Report
  - Channel Wise Check-In Report
- Tab switcher for Direct Booking: "Date Range" vs "Comparison" views
- Placeholder image when no report is selected

### Direct Booking Performance

- Analyze direct booking trends with date range filtering
- Booking status breakdown
- Download report capability
- APIs: `direct-booking-performance-new`, `booking-status-performance`, `download-direct-booking-performance`

### Direct Bookings Comparison

- Compare direct booking performance across different periods
- Side-by-side comparison view

### Booking.com Market Insight

- Market intelligence from Booking.com data:
  - Area demand data: `market-insight/areademanddata`
  - Booker insights: `market-insight/bookerinsightdata`
  - Booking window analysis: `market-insight/bookwindowdata`
  - Pace report: `market-insight/pacereportdata`
  - Sales statistics: `market-insight/salesstatisticsreportdata`
- Availability check: `market-insight/checkstatus`

### Area Demand Report

- Booking.com area demand visualization
- Geographic demand patterns

### Digital Marketing Report

- Marketing engine report list: `marketing-engine/get-report-list`
- Campaign performance data

### Channel-Wise Check-In Report

- Check-in data broken down by distribution channel
- Download report: `download-channel-wise-checkin-report`
- API: `channel-wise-checkin-report`

### Year in Review

- Annual performance summary:
  - Month-wise room nights: `month-wise-room-nights`
  - YoY monthly comparison: `yoy-monthly-room-nights`
  - YoY quarterly comparison: `yoy-quarterly-room-nights`
  - Onboarding date tracking: `onboarding-date`
- Performance report URL: `get-performance-report-url`
- Auto-redirect from base64-encoded URL segment (`cGVyZm9ybWFuY2UveWVhci1pbi1yZXZpZXc=`)

---

## 11. Property Setup

### Property Setup Hub

- Dashboard showing all setup sections as cards
- Property completion score: `property_count_details`
- Property setup percentage: `property_score_percentage/{hotel_id}`
- Navigation to each setup section

### Basic Details

- View and edit: property name, contact details, address
- Country/state cascading dropdowns: `getAllCountries`, `statedetails/getById/{country_id}`
- Email management: add/remove property emails (slider)
- Mobile management: add/remove property mobiles (slider)
- RBAC: requires `BE` access code

### Room Types

- Full CRUD for room types:
  - Create: `hotel_master_new_room_type/add`
  - Edit basic details: `update-basic-detail`
  - Manage amenities: `getAllRoomtypeamenities`, `update-room-type-amenities`
  - Configure occupancy: `update-occupancy`
  - Configure rate plans: `getRoomtypeRateplans`, `update-room-rate-plan`
  - Set min/max pricing: `update-min-max-price`
  - Upload images: `hotel_master_new_room_type/upload_room_type_images/{id}`
  - Reorder images: `update-room-type-image-order`
  - Delete images: `hotel_master_new_room_type/delete_single_roomtype_image/{id}`
  - Delete room type: `hotel_master_new_room_type/delete/{id}`
- Bookable name list: `enum-type/bookable-name`
- RBAC: requires `ROOM` access code

### Floors

- View all floors: `getFloors`
- Save/create floors: `saveFloors`
- RBAC: requires `BE` access code

### Rooms

- Floor-wise room listing: `getRooms2`
- Add room: `addRoom` (room number, floor, room type)
- Edit room: `UpdateRoom`
- Delete room: `DeleteRoom`
- Toggle room status: `UpdateRoomStatus`

### Amenities

- Property-level amenities organized by category
- Available amenities: `getAllPropertyAmenities`
- Hotel's selected amenities: `hotel_amenities/hotelAmenity/{hotel_id}`
- Update: `update_property_amenities`
- Remove: `remove_property_amenities`
- RBAC: requires `HOTEL` access code

### Images

- Property image gallery with upload/delete/reorder
- Fetch: `get_Images/{hotel_id}`
- Upload: `uploadhotelimages`
- Delete: `/file/managePropertyPhotos/remove`
- Reorder: `update-image-order`
- RBAC: requires `HOTEL` access code

### Terms & Policy

- Hotel terms and policies editor
- Fetch: `hotel_policies/{hotel_id}`
- Update: `hotel_policies/update`
- RBAC: requires `HOTEL` access code

### Cancellation Rules

- Cancellation policy configuration
- Fetch: `be_cancellation_policy/fetch_cancellation_policy/{hotel_id}`
- Update: `be_cancellation_policy/update_cancellation_policy`
- Add new rules via slider
- View existing rules

### Financial Details

- Bank account details for payment settlements
- Fetch: `hotel_bank_account_details/{hotel_id}`
- RBAC: requires `HOTEL` access code

### Locale Info

- Currency, timezone, and tax configuration
- Fetch: `locale-details/{hotel_id}`

### Rate Plans

- Master rate plan management:
  - List all: `master_rate_plan/all/{hotel_id}`
  - Create: `master_rate_plan/add`
  - Edit: `master_rate_plan/update/{id}`
  - Delete: `master_rate_plan/{id}`
  - Toggle BE visibility: `master_hotel_rate_plan/update-status-for-be`
- Master plan details: `master-rate-plan-details`
- RBAC: requires `BE` access code

### Derived Rate Plans

- Create rate plans derived from parent rate plans
- Percentage or fixed adjustments per room type
- Room-type-specific derived plan configuration
- Add/view derived plans via slider

### Seasonal Plans

- Configure seasonal pricing for different time periods
- Room-wise rate setup per seasonal plan
- Channel-specific configuration: `get_added_channels_for_seasonal_plan`
- Setup seasonal plan with room type and seasonal plan ID parameters
- RBAC: requires `BE` access code

### Dynamic Pricing

- Occupancy-based automatic rate adjustment rules:
  - List rules: `dynamic-pricing/all/{hotel_id}`
  - Add rule: `dynamic-pricing/add`
  - Edit rule: `dynamic-pricing/update`
  - Delete rule: `dynamic-pricing/delete/{id}`
  - View specific rule: `dynamic-pricing/one/{id}`
- Room type selection: `hotel_master_room_type/room_types/{hotel_id}`
- RBAC: requires `BE` access code

### One-Click OTA Setup

- Automated OTA onboarding wizard:
  1. Check eligibility: `one-click-setup/isoneclicksetupallow/{hotel_id}`
  2. Validate hotel info: `one-click-setup/validatehotelinfo/`
  3. Fetch hotel details from OTA: `one-click-setup/fetchhoteldetails`
  4. Store imported details: `one-click-setup/storehoteldetails`
  5. Fetch room details: `one-click-setup/fetchroomdetails`
  6. Store room details: `one-click-setup/storeroomdetails`
  7. Fetch rate details: `one-click-setup/fetchratedetails`
  8. Store rate details: `one-click-setup/storeratedetails`
  9. Fetch photos: `one-click-setup/fetchphotodetails`
  10. Store photos: `one-click-setup/storephotodetails`
- Progress tracking via Redux `oneClickSetupPropertyData` slice with step-wise status flags

---

## 12. Subscription & Billing

### Plan Selection

- Browse available subscription plans:
  - Tiers: `get-tiers`
  - Packages: `get-packages`
  - Plans: `get-plans` / `get-zoho-plans`
- Plan preview slider with detailed comparison
- Coupon code validation: `validate-subscription-coupon`
- Checkout via Chargebee (`get-checkout-url`) or Zoho (`get-zoho-checkout-url`)
- Subscribe: `subscribe-plan`
- Success page after subscription

### Subscription Overview

- Current plan details, billing history, transaction records
- App subscriptions list
- Auto-debit status and management
- Invoice download (opens invoice URL)
- API: `my-subscription`

### Upgrade Subscription

- Browse upgradable plans: `get-all-upgradable-plans`
- Checkout for upgrade: `get-checkout-url-for-upgrade` / `get-zoho-checkout-url-for-upgrade`

### Renew Subscription

- Renewal flow for expired subscriptions
- Checkout for renewal: `get-checkout-url-for-renewal` / `get-zoho-checkout-url-for-renewal`
- Renewal success page

### Cancel Subscription

- Cancel with confirmation prompt
- Chargebee: `cancel-subscription`
- Zoho: `cancel-zoho-subscription`

### Auto-Debit Authorization

- Set up recurring payment authorization: `get-autodebit-authorization-url`
- UPI-based auto-debit: `get-upi-autodebit-authorization`
- Cancel auto-debit: `cancel-autodebit`

### Manual Payment

- Manual subscription payment via Easebuzz
- API: `make-subscription-payment`

---

## 13. User Management

### Employee/User Management

- List all users for the company: `manage_user/all/{company_id}`
- Add new user: `manage_user/add` (name, email, phone, assigned hotels)
- Edit user details: `manage_user/update`
- Delete user: `manage_user/delete-user/{id}` (with confirmation prompt)
- Hotel list for user assignment: `hotel_admin/get_hotel_list`
- Pagination with configurable page size
- "Manage Users" menu item visible only when `role_id === 1` (admin)

### Role-Based Access Control

- Function-level access permissions per user
- Get user permissions: `get-user-access/{user_id}`
- Get all available functions: `get-access-functionality`
- Update permissions: `user-access-updation-or-creation`
- 5 feature codes: `INVENTORY`, `RATE`, `BE`, `HOTEL`, `ROOM`
- Super admin status (911) bypasses all restrictions
- Access checked per-component via `useEffect` pattern (not route-level)

---

## 14. Support & Help

### Support Tickets

- Raise new ticket: `ticket/raise` (with CKEditor rich text for description)
- List tickets by status (Open/Resolved/Closed): `ticket/list`
- Ticket detail view with comment thread: `ticket/view-details`
- Reply to ticket: `ticket/add-comment` (with CKEditor + image upload)
- Close ticket: `ticket/close`
- Image upload adapter for CKEditor

### FAQ & Help Center

- Searchable FAQ: `faq/search?q=` (external datamart API)
- Initial FAQ display: `faq/initial`
- Category-based browsing: `faq/categories`
- Detailed FAQ slider view: `faq`
- Submit new question: `faq/question`
- Category list for question submission: `faq-manage/categories/list`

### Contact Support

- Contact information display via slider
- Contact info from: `get-contact-us-info`
- WhatsApp support link (opens WhatsApp with pre-filled message)

### Bookingjini Learning

- Daily quick tips: `extranet-quick-tips/daily-tip`
- Blog content: `blog`

---

## 15. Notifications & Announcements

### Notification Center

- Full notification list page at `/notifications`
- Unread count badge in header (refreshed on every route change): `unread-message`
- Notification list: `get-notification-logs`
- Mark as read: `update-viewer-status`
- Count capped at 99+ in badge display

### System Announcements

- HTML-based announcement content from backend
- Modal overlay triggered on property load/switch
- "Don't show again" acknowledgment via API
- Controlled by `announcementprompt.isOpen` Redux state

---

## 16. Other Features

### File Manager

- Global file/image upload management
- Accessible from header "Manage" → "Files" menu
- Route: `/file-manager/:id` (e.g., `/file-manager/Documents`)
- Image upload section and file upload section components
- Refresh trigger via Redux `fileManager.res_status`
- RBAC: requires `BE` access code

### Apps Marketplace

- Browse available Bookingjini apps/integrations
- App list page with app cards
- App detail page with description and features
- Subscribed apps shown in header "Apps" dropdown with SSO launch

### Mart

- Placeholder page for future marketplace
- Displays "Mart Launching Soon 🚀" with promotional image
- External link in sidebar points to `https://mart.bookingjini.com`

### AI Content Generation

- AI-powered description generation:
  - Room descriptions: `https://kernel.bookingjini.com/extranetv4/genai/room?`
  - Property descriptions: `https://kernel.bookingjini.com/extranetv4/genai/property?`

### Aura AI Widget

- `AuraWidgetLoader` component loaded in `DefaultLayout`
- AI assistant widget (visible based on subscription)

### Google Analytics

- Page view tracking via `react-ga`
- `RouteChangeTracker` component fires on every route change
- Tracks all navigation within the SPA

### Legacy Extranet Access

- "Access Older Version" link (currently disabled/commented out)
- SSO into extranetv3.bookingjini.com with base64-encoded credentials

---

## 17. Technical Architecture

### State Management

- Redux (via `@reduxjs/toolkit` package, legacy `createStore` API) with 37 reducers
- `redux-persist` for localStorage persistence (33 of 37 slices persisted)
- `redux-thunk` middleware installed but no thunk actions used
- Key slices: `auth`, `properties`, `userAcess`, `sidebar`, `bookings`, `reservation`, `manage_channels`, `dashBoardData`, `beVersion`
- No React Context usage anywhere in the codebase

### API Layer

- 8 dedicated Axios instances for different backend microservices:
  - `kernelApi` → kernel.bookingjini.com (core operations)
  - `cmApi` → cm.bookingjini.com (channel manager)
  - `cm3Api` → cm3.bookingjini.com (Agoda)
  - `beApi` → be.bookingjini.com (booking engine)
  - `beAlphaAPI` → be-alpha.bookingjini.com (alpha features)
  - `gemsApi` → gems.bookingjini.com (front office/GEMS)
  - `subscriptionApi` → subscription.bookingjini.com (billing)
  - `blApi` → status.bookingjini.tech (health)
- Centralized endpoint definitions in `endPoints.tsx` (~1165 lines, ~350+ endpoints)
- Bearer token auth via request interceptors
- Auto-logout on 401 via response interceptors

### Navigation

- React Router v6 with three route groups:
  - `AuthRoutes`: unauthenticated (login, signup, reset password)
  - `NewUserRoutes`: authenticated without property (select/add property)
  - `AppRoutes`: fully authenticated (all features inside `DefaultLayout`)
- `DefaultLayout`: sidebar + header + content area shell
- Sidebar menu defined in `SidebarMenu.json` (11 items)
- 120+ total routes across all groups

### UI Components

- MUI (Material UI) v5 for form controls, date pickers, tooltips
- Bootstrap 5 for grid layout and utility classes
- `react-sliding-pane` for all drawer/slider UIs
- `react-toastify` for toast notifications
- Nivo charts (@nivo/bar, @nivo/pie, @nivo/line) for analytics
- Swiper for carousels
- CKEditor 5 for rich text editing (tickets)
- Framer Motion for animations
- Lottie for animated illustrations
- Custom SASS stylesheet with module-specific styling

### Build & Deployment

- Create React App with CRACO config overrides
- SASS compilation (watch mode) via `concurrently`
- Firebase hosting deployment
- Nginx configuration for production
- Docker support via Dockerfile
- Semantic release for versioning
- Current version: 4.7.7
