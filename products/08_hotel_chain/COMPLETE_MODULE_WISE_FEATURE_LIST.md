# Hotel Chain Revenue Management — Complete Module-Wise Feature List

**Application:** BookingJini HotelChain V2  
**Platform:** React + TypeScript SPA (Vite)  
**Backend APIs:** kernel.bookingjini.com, datamart.bookingjini.com, subscription.bookingjini.com, be.bookingjini.com, cm.bookingjini.com  
**Date of Analysis:** April 10, 2026

---

## 1. Modules Identified

| #   | Module                        | Route Prefix                   | Key Files                                             |
| --- | ----------------------------- | ------------------------------ | ----------------------------------------------------- |
| 1   | Authentication & Security     | `/` (auth)                     | `Login.tsx`, `useLogin.ts`, `AuthSlice.ts`            |
| 2   | Dashboard — Today's Overview  | `/dashboard/overview`          | `Overview.tsx`, `useOverview.ts`, `useDashboard.ts`   |
| 3   | Dashboard — Summary           | `/dashboard/summary`           | `Summary.tsx`, `useSummary.ts`, `useDashboard.ts`     |
| 4   | Dashboard — Comparisons       | `/dashboard/comparisons`       | `Comparisons.tsx`, `useComparison.ts`                 |
| 5   | Hotels — Hotel List           | `/hotels/hotel-list`           | `HotelList.tsx`, `useHotelList.ts`                    |
| 6   | Hotels — Unblock IP           | `/hotels/unblock-ip`           | `UnblockIP.tsx`, `useUnblockIP.ts`                    |
| 7   | Reports — Coupon Utilization  | `/reports/coupon-utilization`  | `CouponUtilization.tsx`, `useReports.ts`              |
| 8   | Reports — OTA Integration     | `/reports/ota-integration`     | `OTAIntegration.tsx`, `useReports.ts`                 |
| 9   | Reports — Rate Log            | `/reports/rate-log`            | `RateLog.tsx`, `useReports.ts`                        |
| 10  | Reports — Availability Report | `/reports/availability-report` | `AvailabilityReport.tsx`, `useAvailabilityReports.ts` |
| 11  | Reports — Check-In Report     | `/reports/check-in-report`     | `CheckInReport.tsx`, `useCheckInReport.ts`            |
| 12  | Mobile Responsive Layer       | All routes (< 1024px)          | `src/mobile/*`                                        |
| 13  | Layout & Navigation           | Global                         | `Layout.tsx`, `SideBar.tsx`, `Header.tsx`, `Nav.tsx`  |

---

## 2. Module-Wise Feature Extraction

---

### Module: Authentication & Security

#### Features

- **Feature:** Reseller Login
  - **Description:** Email/password authentication for hotel chain reseller users. Captures browser name, IP address (via public-ip), device type, and OS for audit trail. On success, stores auth token, user ID, reseller ID, role ID, and access permissions in both Redux (persisted) and localStorage.
  - **Components:** `Login.tsx`, `useLogin.ts`, `AuthSlice.ts`
  - **APIs:** `POST reseller/login`

- **Feature:** Auth-Gated Routing
  - **Description:** Application renders `AppRoutes` only when `loginStatus` is truthy in Redux state; otherwise renders `AuthRoutes` (login page). Provides complete route protection without a dedicated route guard component.
  - **Components:** `App.tsx`, `AppRoutes.tsx`, `AuthRoutes.tsx`
  - **APIs:** None (client-side state check)

- **Feature:** Auto-Logout on 401
  - **Description:** Axios response interceptor on the `api` instance detects HTTP 401 responses and automatically dispatches `resetAuth()`, clears localStorage, and reloads the page — forcing re-authentication.
  - **Components:** `baseUrl.ts`, `AuthSlice.ts`
  - **APIs:** All API calls via `api` instance

- **Feature:** Last Login Tracking
  - **Description:** Fetches and displays the user's last login timestamp in the header bar. Provides audit visibility for the logged-in user.
  - **Components:** `Header.tsx`, `useLastLogin.ts`
  - **APIs:** `GET get-superadmin-last-login-log/{id}`

- **Feature:** Change Password
  - **Description:** Modal-based password change flow accessible from the header. Supports both "reset password" (old + new + confirm) and "update password" workflows with client-side validation (empty fields, password match).
  - **Components:** `Header.tsx`, `useLastChangePassword.ts`
  - **APIs:** `POST superadmin-change-password`, `POST superadmin/reset-password`

- **Feature:** Session Persistence
  - **Description:** Redux state is persisted to localStorage via `redux-persist`, so the user remains logged in across browser refreshes.
  - **Components:** `store.ts`, `redux-persist` config
  - **APIs:** None

- **Feature:** Not Authorised Page
  - **Description:** Static page displayed when a user attempts to access a resource they lack permissions for.
  - **Components:** `NotAuthorised.tsx`
  - **APIs:** None

#### Observations

- Role-based access control exists in the data model (`role_id`, `can_access_hotels`) but is not enforced in the frontend routing — all authenticated users see all routes.
- Device fingerprinting (browser, OS, IP, device type) is captured at login for security auditing.
- No MFA or CAPTCHA implementation.

---

### Module: Dashboard — Today's Overview

#### Features

- **Feature:** Real-Time KPI Dashboard Cards
  - **Description:** Displays today's key performance indicators across 6 card groups: Confirmed Bookings (count, revenue, room nights, ADR), Cancelled Bookings (count, revenue, room nights), Unpaid Bookings (count, revenue, room nights), Rooms (total, unsold, occupied, occupancy %), Front Office (check-ins, check-outs, avg LOS), and Hotels (total, with bookings, without bookings). Each metric is clickable to drill down.
  - **Components:** `Overview.tsx`, `Card.tsx`, `useDashboard.ts`
  - **APIs:** `GET today-overview2/{userId}/{companyId}/{hotelId}/{bookFlag}`

- **Feature:** Multi-Property Filter (Company + Hotel)
  - **Description:** Cascading dropdown filters — select a company to load its hotels, then optionally select a specific hotel. Supports "All Companies" and "All Hotels" aggregation. Drives all data on the page.
  - **Components:** `Overview.tsx`, `useHotelList.ts`
  - **APIs:** `GET reseller/reseller-company/{userId}`, `GET superadmin-dashboard/retrive-details/{companyId}`

- **Feature:** Booking Date vs Check-In Date Toggle
  - **Description:** A "Book Flag" dropdown lets users switch between viewing data by booking date or check-in date. This fundamentally changes the data perspective — booking date shows sales performance, check-in date shows operational load.
  - **Components:** `Overview.tsx` (bookFlag state)
  - **APIs:** Flag passed as URL parameter to all dashboard endpoints

- **Feature:** Confirmed Bookings Drill-Down Modal
  - **Description:** Clicking the confirmed bookings card opens a full-screen modal with a sortable, searchable, paginated data table showing all confirmed booking details (channel, booking ID, hotel, room type, meal plan, guest info, amounts, payment status). Supports CSV export and column visibility toggle.
  - **Components:** `Overview.tsx`, `Modal.tsx`, `ModalTable.tsx`
  - **APIs:** `GET booking-list3/{userId}/{companyId}/{hotelId}/TODAY/null/null/{bookFlag}`

- **Feature:** Cancelled Bookings Drill-Down Modal
  - **Description:** Same drill-down capability as confirmed bookings but for cancellations. Shows full cancellation details with sorting, search, export.
  - **Components:** `Overview.tsx`, `Modal.tsx`, `ModalTable.tsx`
  - **APIs:** `GET cancellation-list3/{userId}/{companyId}/{hotelId}/TODAY/null/null/{bookFlag}`

- **Feature:** Unpaid Bookings Drill-Down Modal
  - **Description:** Drill-down for unpaid/pending payment bookings. Critical for revenue leakage monitoring.
  - **Components:** `Overview.tsx`, `Modal.tsx`, `ModalTable.tsx`
  - **APIs:** `GET unpaid-booking-list/{userId}/{companyId}/{hotelId}/TODAY/null/null/{bookFlag}`

- **Feature:** Room Availability Drill-Down
  - **Description:** Available when viewing by check-in date. Opens a modal showing room availability data parsed from server-rendered HTML table. Shows per-hotel room inventory status.
  - **Components:** `Overview.tsx`, `RoomAvailabilityTable.tsx`
  - **APIs:** `GET get-availability-list/{userId}/{companyId}/{hotelId}/TODAY/null/null`

- **Feature:** Check-In / Check-Out Drill-Down Modals
  - **Description:** Separate modals for today's check-ins and check-outs with full booking detail tables.
  - **Components:** `Overview.tsx`, `Modal.tsx`, `ModalTable.tsx`
  - **APIs:** `GET checkin-list2/...`, `GET checkout-list2/...`

- **Feature:** Hotel Booking Count Drill-Down
  - **Description:** Shows per-hotel booking counts across channels in a multi-header table (parsed from server HTML). Breaks down bookings, room nights, and revenue by OTA channel per hotel.
  - **Components:** `Overview.tsx`, `TotalHotelsDataTable.tsx`
  - **APIs:** `GET get-hotel-booking-count/{userId}/{companyId}/{hotelId}/TODAY/CONFIRMED/{bookFlag}/null/null`

- **Feature:** Print Dashboard
  - **Description:** Browser print functionality via `window.print()` for the overview page.
  - **Components:** `Overview.tsx`
  - **APIs:** None

#### Observations

- ADR (Average Daily Rate) is a key revenue metric calculated server-side.
- Occupancy percentage is provided as a pre-calculated metric.
- The "bookFlag" toggle between booking date and check-in date is a revenue management best practice — it separates sales performance from operational metrics.
- Room availability drill-down is only available in check-in date mode (bookFlag === "2").

---

### Module: Dashboard — Summary

#### Features

- **Feature:** Period-Based KPI Summary Cards
  - **Description:** Extends the Overview module with time-range support. Shows 8 card groups: Confirmed, Cancelled, Unpaid, Room Nights (total/unsold/occupied/occupancy), Front Office (check-ins/check-outs/avg LOS), OTA Bookings (count/revenue/room nights), Direct Bookings (BE — count/revenue/room nights), and Experience Bookings (count/revenue/guests).
  - **Components:** `Summary.tsx`, `Card.tsx`, `useDashboard.ts`
  - **APIs:** `GET group-summary4/{userId}/{companyId}/{hotelId}/{period}/{from}/{to}/{bookFlag}`

- **Feature:** Time Period Selector (MTD / YTD / Custom)
  - **Description:** Dropdown to switch between Month-to-Date, Year-to-Date, or Custom date range. Custom mode shows two date pickers. Future dates are disabled when viewing by booking date.
  - **Components:** `Summary.tsx`, `CustomDatePicker.tsx`
  - **APIs:** Period passed as URL parameter (MTD/YTD/CUSTOM)

- **Feature:** OTA vs Direct Booking Segmentation
  - **Description:** Summary separates OTA bookings from direct (Booking Engine) bookings with dedicated cards showing count, revenue, and room nights for each channel type. This is critical for measuring direct booking share — a key revenue management KPI.
  - **Components:** `Summary.tsx`, `Card.tsx`
  - **APIs:** Data comes from `group-summary4` response fields: `ota_confirmed_bookings`, `direct_confirmed_bookings`, etc.

- **Feature:** OTA Channel-Wise Booking Breakdown Modal
  - **Description:** Opens a modal with a multi-header table showing per-hotel booking data broken down by OTA channel (Goibibo, MakeMyTrip, Cleartrip, etc.) with bookings, room nights, and revenue per channel.
  - **Components:** `Summary.tsx`, `OTABookingTable.tsx`
  - **APIs:** `GET get-ota-Booking-count-by-channel/{userId}/{companyId}/{hotelId}/{period}/{from}/{to}/{bookFlag}`

- **Feature:** Direct Booking (BE) Channel Breakdown Modal
  - **Description:** Similar to OTA breakdown but for Booking Engine channels. Shows per-hotel direct booking performance.
  - **Components:** `Summary.tsx`, `BEBookingTable.tsx`
  - **APIs:** `GET get-be-Booking-count-by-channel/{userId}/{companyId}/{hotelId}/{period}/{from}/{to}/{bookFlag}`

- **Feature:** Experience Bookings Tracking
  - **Description:** Dedicated card for "experience bookings" (non-room bookings like activities, spa, etc.) showing booking count, revenue, and guest count.
  - **Components:** `Summary.tsx`, `Card.tsx`
  - **APIs:** `GET exp-booking-list/{userId}/{companyId}/{hotelId}/{period}/{from}/{to}/{bookFlag}`

- **Feature:** BE Visitor Count
  - **Description:** Tracks booking engine website visitor count as part of the summary data — useful for conversion rate analysis.
  - **Components:** Data field `be_visitor_count` in `AllSummaryType`
  - **APIs:** Included in `group-summary4` response

#### Observations

- The OTA vs Direct split is the most revenue-critical feature — it measures channel mix and direct booking share.
- Experience bookings suggest the platform supports non-room revenue streams.
- BE visitor count enables conversion funnel analysis (visitors → bookings).

---

### Module: Dashboard — Comparisons

#### Features

- **Feature:** Multi-Dimensional Comparison Charts
  - **Description:** Bar chart visualizations comparing booking performance across 5 dimensions: State-wise, City-wise, OTA-wise, Hotel-wise, and Year-to-Year. Uses Chart.js (react-chartjs-2) with tabbed navigation.
  - **Components:** `Comparisons.tsx`, `useComparison.ts`
  - **APIs:** `GET get-comparison-data/{userId}/{companyId}/{option}/{dimension}/{period}/{from}/{to}`

- **Feature:** Comparison Category Filter
  - **Description:** Dropdown to switch between Confirmed Bookings, Cancelled Bookings, and Revenue as the comparison metric.
  - **Components:** `Comparisons.tsx`
  - **APIs:** Category passed as `option` parameter (CONFIRMED/CANCELLED/REVENUE)

- **Feature:** Year-to-Year Performance Comparison
  - **Description:** Dedicated tab showing year-over-year booking/revenue trends. Critical for identifying growth patterns and seasonal trends.
  - **Components:** `Comparisons.tsx` (Year to Year tab)
  - **APIs:** `GET get-comparison-data/.../YEAR/...`

- **Feature:** Geographic Performance Analysis
  - **Description:** State-wise and City-wise comparison charts reveal geographic booking concentration and help identify underperforming markets.
  - **Components:** `Comparisons.tsx` (State Wise, City Wise tabs)
  - **APIs:** `GET get-comparison-data/.../STATE/...`, `.../CITY/...`

- **Feature:** OTA Channel Performance Comparison
  - **Description:** Compares booking performance across OTA channels to identify top-performing and underperforming distribution channels.
  - **Components:** `Comparisons.tsx` (OTA Wise tab)
  - **APIs:** `GET get-comparison-data/.../OTA/...`

- **Feature:** Hotel-Wise Performance Ranking
  - **Description:** Compares individual hotel performance within the chain, enabling identification of top performers and properties needing attention.
  - **Components:** `Comparisons.tsx` (Hotel Wise tab)
  - **APIs:** `GET get-comparison-data/.../HOTEL/...`

#### Observations

- Comparison data uses the same time period controls (MTD/YTD/Custom) as Summary.
- Chart.js bar charts with legend and title — functional but basic visualization.
- No drill-down from chart bars to underlying data.

---

### Module: Hotels — Hotel List

#### Features

- **Feature:** Hotel Portfolio View
  - **Description:** Paginated table listing all hotels under the reseller's management. Shows Hotel ID (linked to extranet), Hotel Name (red if inactive), Company, City, State, Onboarding Date (with days since onboarding), and Action column.
  - **Components:** `HotelList.tsx`, `HotelTableColumn.tsx`, `DataTableWithoutPagination.tsx`
  - **APIs:** `GET reseller/reseller-hotels/{userId}`

- **Feature:** Hotel Search
  - **Description:** Client-side search by hotel name or hotel ID. Filters the loaded hotel list in real-time.
  - **Components:** `HotelList.tsx`
  - **APIs:** None (client-side filtering)

- **Feature:** Direct Extranet Access
  - **Description:** Each hotel row has a login icon that opens the hotel's extranet URL in a new tab, enabling quick property-level management.
  - **Components:** `HotelTableColumn.tsx` (ActionCell, HotelIdCell)
  - **APIs:** None (uses `extranet_url` from hotel data)

- **Feature:** Hotel Status Indicator
  - **Description:** Hotel names are displayed in red when the hotel status is not active (status !== 1), providing visual identification of inactive properties.
  - **Components:** `HotelTableColumn.tsx` (HotelNameCell)
  - **APIs:** None (uses `status` field)

- **Feature:** Total Hotel Count
  - **Description:** Displays total number of hotels in the portfolio.
  - **Components:** `HotelList.tsx`
  - **APIs:** Derived from `hotelList.data.length`

#### Observations

- Onboarding date with "days since" calculation helps track new property ramp-up.
- No hotel creation/editing from this interface — it's a read-only portfolio view with extranet links.

---

### Module: Hotels — Unblock IP

#### Features

- **Feature:** Blocked IP Management
  - **Description:** Lists all IP addresses blocked due to failed login attempts for the hotel chain. Shows username and IP address. Supports bulk selection via checkboxes and batch unblocking.
  - **Components:** `UnblockIP.tsx`, `useUnblockIP.ts`
  - **APIs:** `GET blocked-hotel-chain-access-ip/get/{userId}`, `POST blocked-hotel-chain-access-ip/delete`

- **Feature:** Bulk IP Unblock
  - **Description:** Select multiple blocked IPs via checkboxes (including "select all") and unblock them in a single action. Table refreshes after successful unblock.
  - **Components:** `UnblockIP.tsx`
  - **APIs:** `POST blocked-hotel-chain-access-ip/delete` with `{ wrong_id: [...] }`

- **Feature:** IP Search/Filter
  - **Description:** Filter blocked IPs by username using a text input.
  - **Components:** `UnblockIP.tsx`
  - **APIs:** None (client-side filtering via TanStack Table)

#### Observations

- Auto-fetches blocked IP list on component mount.
- Security feature to manage brute-force protection across the hotel chain.

---

### Module: Reports — Coupon Utilization

#### Features

- **Feature:** Coupon Performance Report
  - **Description:** Shows coupon usage across hotels with hotel ID, hotel name, coupon code, coupon name, utilization count, and revenue generated. Filterable by company, hotel, and date range.
  - **Components:** `CouponUtilization.tsx`, `CouponColumns.tsx`, `useReports.ts`
  - **APIs:** `POST extranetv4/get-applied-coupon-details` (via `be` instance — be.bookingjini.com)

- **Feature:** Multi-Level Filter (Company → Hotel + Date Range)
  - **Description:** Cascading company/hotel selection with from/to date pickers. Future dates disabled.
  - **Components:** `CouponUtilization.tsx`
  - **APIs:** Company/hotel list APIs

#### Observations

- Coupon tracking is a revenue management feature — measures promotional effectiveness.
- Revenue per coupon helps calculate ROI of discount strategies.

---

### Module: Reports — OTA Integration

#### Features

- **Feature:** OTA Connectivity Report
  - **Description:** Shows which OTA channels each hotel is connected to, with a count of connected channels. Displays total hotels vs connected hotels summary. Filterable by company.
  - **Components:** `OTAIntegration.tsx`, `OTAIntegrationColumns.tsx`, `useReports.ts`
  - **APIs:** `GET extranetv4/fetch-connected-ota-hotel-list/{companyId}` (via `cm` instance — cm.bookingjini.com)

- **Feature:** Channel Distribution Summary
  - **Description:** Header shows total hotels and connected hotels count, giving a quick view of distribution coverage.
  - **Components:** `OTAIntegration.tsx`
  - **APIs:** `total_hotel` and `connected_hotels` from response

#### Observations

- This is a channel manager integration report — shows distribution reach.
- Helps identify hotels not yet connected to OTAs (revenue opportunity).

---

### Module: Reports — Rate Log

#### Features

- **Feature:** Rate Change Audit Trail
  - **Description:** Comprehensive log of all rate changes across the system. Shows hotel ID, room type, rate plan, date range, bar price, multiple occupancy pricing, day-wise pricing, channel, extra adult/child pricing, block status, LOS restrictions, client IP, and user ID. Filterable by hotel, room type, rate plan, channel, and stay date.
  - **Components:** `RateLog.tsx`, `RateLogColumns.tsx`, `useReports.ts`
  - **APIs:** `GET get-rate-logs-filters/{hotelId}` (filters), `POST extranetv4/get-rate-logs` (data, via `cm` instance)

- **Feature:** Dynamic Filter Loading
  - **Description:** Selecting a hotel dynamically loads its room types, rate plans, and connected OTA channels as filter options.
  - **Components:** `RateLog.tsx`, `useReports.ts`
  - **APIs:** `GET get-rate-logs-filters/{hotelId}`

- **Feature:** Rate Log CSV Export
  - **Description:** Export filtered rate log data to CSV with all columns including serialized multiple_days and multiple_occupancy data.
  - **Components:** `RateLog.tsx` (export-to-csv library)
  - **APIs:** None (client-side export)

#### Observations

- This is a critical revenue management audit feature — tracks who changed what rate, when, from which IP.
- Multiple occupancy pricing and day-wise pricing indicate sophisticated rate management.
- Block status and LOS in the log show restriction management is tracked.
- Channel-specific rate logging indicates rates can differ by distribution channel.

---

### Module: Reports — Availability Report

#### Features

- **Feature:** Multi-Property Availability Grid
  - **Description:** Date-wise room availability matrix showing each hotel's available rooms per day. Color-coded: green border = available, red background = blocked (stop sell), gray background = zero inventory. Shows hotel ID, name, total rooms, and per-date availability.
  - **Components:** `AvailabilityReport.tsx`, `AvailabilityReportTable.tsx`, `useAvailabilityReports.ts`
  - **APIs:** `POST reseller/get-availability-report`

- **Feature:** Block Status Visualization
  - **Description:** Rooms with `block_status === 1` are highlighted in red, indicating stop-sell restrictions. Zero-availability rooms shown in gray. This visual coding enables quick identification of inventory restrictions.
  - **Components:** `AvailabilityReport.tsx` (cell renderer)
  - **APIs:** `block_status` field in availability data

- **Feature:** Sticky Column Headers
  - **Description:** First 3 columns (S.No, Hotel ID, Hotel Name) are sticky/frozen while scrolling horizontally through date columns. Enables viewing hotel identity while scanning dates.
  - **Components:** `AvailabilityReportTable.tsx` (getStickyStyle function)
  - **APIs:** None

- **Feature:** PDF Download
  - **Description:** Server-generated PDF report of availability data. Decodes base64 PDF content and opens in a new browser tab.
  - **Components:** `AvailabilityReport.tsx`, `useAvailabilityReports.ts`
  - **APIs:** `POST reseller/download-availability-report`

#### Observations

- Stop-sell (block_status) visualization is a key revenue management restriction indicator.
- The availability grid is the closest feature to an inventory management view.
- Dynamic date columns are generated from the API response data.

---

### Module: Reports — Check-In Report

#### Features

- **Feature:** Monthly Check-In Report
  - **Description:** Server-rendered HTML report of monthly check-in data. User selects month and year, and the report is fetched and rendered via `dangerouslySetInnerHTML`. Auto-fetches on date change.
  - **Components:** `CheckInReport.tsx`, `MonthAndYearPicker.tsx`, `useCheckInReport.ts`
  - **APIs:** `GET monthly-booking-report/{userId}/{month}/{year}`

- **Feature:** Month/Year Picker
  - **Description:** Custom month and year picker component for selecting the report period.
  - **Components:** `MonthAndYearPicker.tsx`
  - **APIs:** None

#### Observations

- Report is server-rendered HTML — no client-side data manipulation.
- Uses `dangerouslySetInnerHTML` which has XSS implications if server output is not sanitized.

---

### Module: Mobile Responsive Layer

#### Features

- **Feature:** Responsive Breakpoint Detection
  - **Description:** Automatic detection of viewport width using `matchMedia`. Below 1024px triggers mobile UI; above triggers desktop UI. All routes render mobile or desktop variants based on this hook.
  - **Components:** `breakpoint.ts`, `AppRoutes.tsx`
  - **APIs:** None

- **Feature:** Complete Mobile Page Parity
  - **Description:** Every desktop page has a mobile counterpart: MobileOverviewPage, MobileSummaryPage, MobileComparisonsPage, MobileHotelListPage, MobileUnblockIPPage, MobileCouponUtilizationPage, MobileOTAIntegrationPage, MobileRateLogPage, MobileAvailabilityReportPage, MobileCheckInReportPage.
  - **Components:** `src/mobile/pages/*`
  - **APIs:** Same APIs as desktop

- **Feature:** Mobile-Specific UI Components
  - **Description:** Dedicated mobile component library: MobileCard, MobileCompactCard, MobileCardList, MobileMetricTile, MobileFilterBar, MobileSearchBar, MobileEmptyState, MobileBottomSheet, MobilePaginationBar, MobileFullPageModal, BookingDetailCard.
  - **Components:** `src/mobile/components/common/*`, `src/mobile/components/dashboard/*`
  - **APIs:** None

- **Feature:** Mobile Layout System
  - **Description:** Mobile-specific layout with MobileHeader, MobileDrawer (side navigation), MobileBottomNav (bottom tab bar), and MobileLayout wrapper.
  - **Components:** `src/mobile/layouts/*`
  - **APIs:** None

- **Feature:** Mobile CSV Export
  - **Description:** Dedicated export utility for mobile that converts data arrays to CSV and triggers download.
  - **Components:** `exportUtils.ts`
  - **APIs:** None

- **Feature:** Mobile Theme System
  - **Description:** Centralized theme constants and badge styles for consistent mobile UI.
  - **Components:** `theme.ts`
  - **APIs:** None

#### Observations

- Non-invasive architecture — all mobile code isolated in `src/mobile/`.
- Reuses all existing Redux state, API hooks, and business logic.
- Full feature parity with desktop — not a stripped-down mobile view.

---

### Module: Layout & Navigation

#### Features

- **Feature:** Sidebar Navigation with Accordion Groups
  - **Description:** Collapsible sidebar with 3 main sections: Dashboard (Overview, Summary, Comparisons), Hotels (Hotel List, Unblock IP), Reports (Coupon Utilization, OTA Integration, Rate Log, Availability Report, Check-In Report). Active route highlighting with accordion expand/collapse.
  - **Components:** `SideBar.tsx`, `Nav.tsx`
  - **APIs:** None

- **Feature:** Header Bar with User Context
  - **Description:** Shows logged-in user's full name, last login timestamp, change password button, and logout button.
  - **Components:** `Header.tsx`
  - **APIs:** Last login API

- **Feature:** Version Display
  - **Description:** Application version number displayed at the bottom of the sidebar, pulled from `package.json`.
  - **Components:** `Nav.tsx`
  - **APIs:** None

---

## 3. Revenue Management Features

### Pricing & Rate Features

- **Feature:** Bar Price Tracking
  - **Description:** Rate log captures `bar_price` (Best Available Rate) changes per room type, rate plan, and channel. BAR is the foundational pricing metric in hotel revenue management.

- **Feature:** Multiple Occupancy Pricing
  - **Description:** Rate log tracks `multiple_occupancy` arrays — indicating the system supports different pricing for single, double, triple occupancy. This is a standard revenue optimization lever.

- **Feature:** Day-of-Week Pricing
  - **Description:** Rate log captures `multiple_days` as a key-value map (e.g., `{"Mon": 5000, "Fri": 7000}`), indicating day-of-week rate differentiation — a core yield management technique.

- **Feature:** Extra Person Pricing
  - **Description:** `extra_adult_price` and `extra_child_price` tracked in rate logs, showing supplemental pricing for additional guests.

- **Feature:** Channel-Specific Rates
  - **Description:** Rate logs include a `channel` field, indicating rates can be set differently per OTA/distribution channel — enabling rate parity monitoring or intentional channel-specific pricing.

### Inventory & Restriction Features

- **Feature:** Stop Sell / Block Status
  - **Description:** `block_status` field in both rate logs and availability reports. When `block_status === 1`, rooms are blocked from sale. Visualized as red cells in the availability grid. This is a core inventory restriction mechanism.

- **Feature:** Length of Stay (LOS) Restrictions
  - **Description:** `los` field tracked in rate logs, indicating minimum length of stay restrictions can be applied per room type/rate plan/channel.

- **Feature:** Room Availability Monitoring
  - **Description:** Date-wise availability grid across all properties with color-coded status (available/blocked/zero). Enables chain-wide inventory visibility.

### Revenue KPI Features

- **Feature:** ADR (Average Daily Rate)
  - **Description:** Calculated and displayed in Overview and Summary dashboards. ADR = Total Revenue / Total Room Nights Sold. Key revenue performance metric.

- **Feature:** Occupancy Rate
  - **Description:** Displayed as percentage in Overview (rooms) and Summary (room nights). Occupancy = Occupied Rooms / Total Rooms. Core operational metric.

- **Feature:** RevPAR Inputs
  - **Description:** While RevPAR (Revenue Per Available Room) is not explicitly displayed, all its inputs are available: ADR, Occupancy, Total Revenue, Available Room Nights. RevPAR = ADR × Occupancy.

- **Feature:** Channel Mix Analysis
  - **Description:** OTA vs Direct booking split in Summary, OTA channel-wise breakdown, and comparison charts by OTA. Measures distribution channel effectiveness and direct booking share.

- **Feature:** Revenue by Channel
  - **Description:** Revenue broken down by OTA channel (Goibibo, MakeMyTrip, Cleartrip) and direct (Booking Engine) in Summary drill-downs.

- **Feature:** Cancellation Revenue Impact
  - **Description:** Cancelled bookings tracked with revenue and room night impact — enables cancellation rate and revenue leakage analysis.

- **Feature:** Unpaid Booking Revenue Tracking
  - **Description:** Separate tracking of unpaid bookings with revenue and room nights — critical for cash flow management and payment follow-up.

---

## 4. Cross-Module Features

- **Feature:** Global Company/Hotel Filter
  - **Description:** Nearly every page uses the same cascading Company → Hotel filter pattern. Company list loaded via `reseller/reseller-company/{userId}`, hotels under company via `superadmin-dashboard/retrive-details/{companyId}`. Consistent UX across Dashboard, Reports modules.
  - **Components:** Used in Overview, Summary, Comparisons, CouponUtilization, OTAIntegration, AvailabilityReport

- **Feature:** Unified Data Table Component
  - **Description:** `DataTableWithoutPagination` component used across Hotel List, Coupon Utilization, OTA Integration, and Rate Log. Built on TanStack Table with sorting, filtering, and column definitions.
  - **Components:** `DataTableWithoutPagination.tsx`

- **Feature:** Modal Booking Detail Table
  - **Description:** `ModalTable` component reused across Overview and Summary for all booking drill-downs. Features global search, column visibility toggle, CSV export, sorting, and pagination.
  - **Components:** `ModalTable.tsx`

- **Feature:** CSV/Excel Export
  - **Description:** Multiple export mechanisms: `export-to-csv` library in ModalTable and RateLog, `react-html-table-to-excel` for HTML tables, custom `exportUtils.ts` for mobile. Enables data extraction across all major views.

- **Feature:** Toast Notifications
  - **Description:** `react-hot-toast` used globally for success/error feedback on all API operations.

- **Feature:** Loading States
  - **Description:** Consistent loading spinner (Lucide `Loader` with `animate-spin`) across all pages during API calls.

- **Feature:** Empty State Handling
  - **Description:** All data views show "No Data Available" or similar messages when API returns empty results.

- **Feature:** Date Picker Components
  - **Description:** `CustomDatePicker` component with configurable date disabling (e.g., no future dates for booking date mode). Used across Summary, Comparisons, CouponUtilization, RateLog, AvailabilityReport.

- **Feature:** Indian Currency Formatting
  - **Description:** `formatToIndianCurrency()` utility using `Intl.NumberFormat` with INR currency — indicates Indian market focus.

---

## 5. Key User Flows

- **Flow 1:** Login → View Today's Overview → Filter by Company/Hotel → Drill into Confirmed Bookings → Export CSV
- **Flow 2:** Login → Summary → Select MTD/YTD/Custom Period → Compare OTA vs Direct Bookings → View OTA Channel Breakdown
- **Flow 3:** Login → Comparisons → Select Revenue Category → View State/City/OTA/Hotel/Year-to-Year Charts → Identify Underperformers
- **Flow 4:** Login → Hotels → Search Hotel → Click Extranet Link → Manage Property Directly
- **Flow 5:** Login → Reports → Rate Log → Select Hotel → Load Room Types/Rate Plans/Channels → Search Rate Changes → Export CSV
- **Flow 6:** Login → Reports → Availability Report → Select Company + Date → View Availability Grid → Identify Blocked/Zero Rooms → Download PDF
- **Flow 7:** Login → Reports → OTA Integration → Select Company → Identify Hotels Not Connected to OTAs
- **Flow 8:** Login → Reports → Coupon Utilization → Filter by Company/Hotel/Date → Analyze Coupon ROI
- **Flow 9:** Login → Hotels → Unblock IP → Select Blocked IPs → Bulk Unblock
- **Flow 10:** Login → Header → Change Password → Submit

---

## 6. Integrations

| Integration                      | Type                    | Usage                                                                                                   | API Base                     |
| -------------------------------- | ----------------------- | ------------------------------------------------------------------------------------------------------- | ---------------------------- |
| BookingJini Kernel               | Core Backend            | Authentication, dashboard data, hotel management, booking data                                          | kernel.bookingjini.com       |
| BookingJini Datamart             | Analytics Backend       | Data warehousing (axios instance configured but limited direct usage in current routes)                 | datamart.bookingjini.com     |
| BookingJini Subscription         | Subscription Management | Subscription API key-based auth for subscription services (endpoints defined but not in current routes) | subscription.bookingjini.com |
| BookingJini Booking Engine (BE)  | Direct Booking          | Coupon utilization reports, booking engine data                                                         | be.bookingjini.com           |
| BookingJini Channel Manager (CM) | OTA Distribution        | OTA integration reports, rate logs, rate management                                                     | cm.bookingjini.com           |
| Goibibo                          | OTA Channel             | Booking data tracked per channel in OTA breakdown tables                                                | Via CM                       |
| MakeMyTrip                       | OTA Channel             | Booking data tracked per channel in OTA breakdown tables                                                | Via CM                       |
| Cleartrip                        | OTA Channel             | Booking data tracked per channel in OTA breakdown tables                                                | Via CM                       |
| Redux Persist / localStorage     | Client Storage          | Session persistence, auth token storage                                                                 | Local                        |
| Chart.js                         | Visualization           | Bar charts for comparison module                                                                        | Client-side                  |
| TanStack Table                   | Data Grid               | All data tables across the application                                                                  | Client-side                  |
| export-to-csv                    | Export                  | CSV generation and download                                                                             | Client-side                  |
| react-html-table-to-excel        | Export                  | HTML table to Excel export                                                                              | Client-side                  |
| public-ip                        | Security                | Captures user's public IP at login for audit                                                            | External API                 |
| react-device-detect              | Security                | Captures browser, OS, device type at login                                                              | Client-side                  |

---

## 7. Final Consolidated Feature List

### Dashboard & Analytics

- Real-time KPI cards (Confirmed, Cancelled, Unpaid, Rooms, Front Office, Hotels)
- Period-based summary (MTD/YTD/Custom)
- OTA vs Direct booking segmentation
- Multi-dimensional comparison charts (State/City/OTA/Hotel/Year-to-Year)
- Booking date vs Check-in date toggle
- ADR, Occupancy, Room Nights, Revenue metrics
- Experience bookings tracking
- BE visitor count (conversion funnel input)
- Drill-down modals with full booking details
- Channel-wise booking/revenue breakdown (OTA and BE)

### Pricing & Rate Management

- Bar price tracking and audit trail
- Multiple occupancy pricing
- Day-of-week rate differentiation
- Extra adult/child pricing
- Channel-specific rate tracking
- Rate change audit log with user/IP tracking
- Rate plan and room type filtering

### Inventory Management

- Multi-property availability grid (date-wise)
- Stop sell / block status visualization
- Room count monitoring (total/occupied/unsold)
- Available room nights tracking

### Restrictions & Controls

- Length of Stay (LOS) restrictions (tracked in rate logs)
- Stop sell / block status management (visible in availability and rate logs)

### Multi-Property Management

- Cascading Company → Hotel filter across all modules
- Portfolio-wide KPI aggregation
- Per-hotel drill-down capability
- Hotel status monitoring (active/inactive)
- Onboarding date tracking with age calculation
- Direct extranet access per hotel

### Channel Management (UI Layer)

- OTA integration connectivity report
- OTA channel-wise booking breakdown
- Direct booking (BE) channel breakdown
- Channel-specific rate log tracking
- Connected vs total hotels summary

### Reports & Insights

- Coupon utilization report with revenue impact
- Rate change audit log with CSV export
- Room availability report with PDF download
- Monthly check-in report
- OTA integration status report

### User & Access Control

- Reseller login with device fingerprinting
- Auth token-based API security
- Auto-logout on 401
- Change password functionality
- Blocked IP management and bulk unblock
- Session persistence via Redux Persist
- Not Authorised page

### System / UX Features

- Responsive mobile UI (full feature parity below 1024px)
- Mobile-specific component library
- Mobile layout (header, drawer, bottom nav)
- CSV/Excel export across all data views
- Print functionality
- Toast notifications
- Loading states and empty state handling
- Sidebar navigation with accordion groups
- Version display
- Indian currency formatting (INR)

---

## 8. Product Insights

### Missing Enterprise-Grade RMS Features

1. **No Dynamic Pricing Engine** — No automated rate adjustment based on demand, occupancy, or competitor rates. All pricing appears to be manual via the extranet.
2. **No Demand Forecasting** — No predictive analytics for future demand. The system is entirely retrospective.
3. **No Competitor Rate Intelligence** — No rate shopping or competitor benchmarking. Revenue managers have no visibility into market positioning.
4. **No Auto Rule Engine** — No ability to set rules like "if occupancy > 80%, increase rate by 15%" or "if booking pace is below forecast, trigger alert."
5. **No RevPAR Display** — Despite having all inputs (ADR + Occupancy), RevPAR is not calculated or displayed.
6. **No Pickup/Pace Reports** — No booking pace analysis comparing current pickup to historical patterns.
7. **No Segment-Based Pricing** — No guest segmentation (corporate, leisure, group, wholesale) for differentiated pricing.
8. **No Restriction Management UI** — LOS, CTA (Closed to Arrival), CTD (Closed to Departure) restrictions are tracked in logs but there's no UI to set/manage them.
9. **No Rate Parity Monitoring** — Despite channel-specific rate tracking, there's no parity check or alert system.
10. **No Budget/Forecast Module** — No ability to set revenue targets and track actual vs budget.

### Revenue Optimization Opportunities

1. **Channel Mix Optimization Dashboard** — Add a dedicated view showing direct booking share trend over time with cost-per-acquisition by channel.
2. **Cancellation Pattern Analysis** — Add cancellation rate trends, lead time analysis, and cancellation reason tracking.
3. **Conversion Funnel** — BE visitor count exists but no conversion rate calculation or funnel visualization.
4. **Revenue Calendar View** — A calendar-based view showing daily revenue, occupancy, and ADR would be more intuitive than tabular data.
5. **Automated Alerts** — Trigger notifications when occupancy drops below threshold, cancellation rate spikes, or a hotel goes dark (no bookings).
6. **Coupon ROI Calculator** — Extend coupon report with discount amount vs incremental revenue analysis.

### UX Improvements

1. **Dashboard is Card-Heavy** — 6-8 cards with 3-4 metrics each creates cognitive overload. Consider a summary row + expandable detail pattern.
2. **No Saved Filters/Views** — Users must re-select company/hotel/date on every page visit. Saved filter presets would save significant time.
3. **Modal Tables are Full-Screen** — 90vw × 90vh modals are jarring. Consider slide-out panels or inline expansion.
4. **No Keyboard Navigation** — No keyboard shortcuts for common actions (search, export, navigate).
5. **Date Range Presets** — Add "Last 7 days", "Last 30 days", "Last Quarter" quick-select buttons alongside MTD/YTD/Custom.
6. **Comparison Charts Lack Interactivity** — No click-to-drill-down on chart bars, no tooltip with detailed data.
7. **Server-Rendered HTML Tables** — OTA Booking, BE Booking, Room Availability, and Check-In Report use `dangerouslySetInnerHTML` to render server HTML. This limits client-side sorting/filtering and poses XSS risk.

### Performance Improvements

1. **No Data Caching** — Every page navigation triggers fresh API calls. React Query or SWR would add caching, deduplication, and background refresh.
2. **No Pagination on Large Datasets** — `DataTableWithoutPagination` loads all data at once. For chains with hundreds of hotels, this will cause performance issues.
3. **No Lazy Loading** — All routes are eagerly imported. Code splitting with `React.lazy()` would improve initial load time.
4. **localStorage Token Storage** — Auth tokens in localStorage are vulnerable to XSS. HttpOnly cookies would be more secure.
5. **Multiple API Instances** — 5 separate axios instances (api, datamart, subscription, be, cm) without shared error handling or retry logic.
6. **No API Request Deduplication** — Rapid filter changes can trigger multiple concurrent requests for the same data.

### Automation / AI Opportunities

1. **AI-Based Dynamic Pricing** — Implement ML models that analyze historical booking patterns, seasonality, events, and competitor rates to recommend optimal pricing.
2. **Demand Forecasting** — Time-series forecasting (Prophet, LSTM) on booking data to predict future demand by property, room type, and channel.
3. **Smart Alerts** — Anomaly detection on booking pace, cancellation rate, and revenue to auto-alert revenue managers.
4. **Auto Rate Recommendations** — Based on current occupancy and booking pace, suggest rate adjustments with expected revenue impact.
5. **Chatbot for Quick Insights** — "What's my occupancy for next weekend?" or "Which hotel has the highest cancellation rate this month?"
6. **Automated Report Scheduling** — Email daily/weekly summary reports to stakeholders without manual export.
7. **Guest Segmentation Engine** — Cluster guests by booking behavior, geography, and spend to enable targeted pricing and promotions.
