# BookingJini PMS — Feature Details

## About the Application

BookingJini PMS is a hotel property management system designed for hoteliers to manage their day-to-day operations. It covers the entire guest lifecycle from booking to checkout, along with food & beverage management, housekeeping, finance, and reporting. The system also provides guest-facing mobile interfaces for self-service check-in, room service ordering, and reviews.

---

## 1. Dashboard

### 1.1 Booking View
An interactive calendar grid that displays all bookings across a 15-day window, organized by room type and individual rooms. Each booking appears as a colored bar spanning from check-in to check-out date. Color coding distinguishes between not-confirmed, booked, checked-in, and checked-out statuses. Users can drag-select across empty room slots to create new bookings directly from the calendar. All room type rows can be expanded or collapsed individually or together using an "Expand All" toggle. Clicking any booking bar opens a detail panel with full booking information and a booking confirmation option.

### 1.2 Rooms View
A card-based room status board showing every room in the hotel for a selected date. Rooms are grouped by room type, and each room card is color-coded: green for vacant, blue for occupied, orange for booked, and gray for out-of-order. Each room type header displays counts for total, occupied, booked, vacant, and out-of-order rooms. A "New Booking" mode lets staff select multiple vacant rooms via checkboxes and proceed to create a booking. Clicking an occupied room navigates to the stay management page for that room. An info icon on vacant rooms indicates future out-of-order dates.

### 1.3 Hotel Position
A two-month calendar view showing the hotel's occupancy and pricing position at a glance. Each day cell displays the number of available rooms and the minimum room rate. Days are color-coded by occupancy level — green when rooms are available, orange when few rooms remain, and red when fully booked. Users can navigate forward and backward through months.

### 1.4 Quick Check-in
A quick-access panel on the dashboard where staff can enter a Quick Check-in (QC) code to fast-track a guest's check-in process. The code is validated and the user is taken directly to a streamlined check-in page.

### 1.5 Pending Tasks
A notification banner appears on the dashboard when there are overdue check-ins or check-outs. Clicking it opens a dedicated page with two tabs — Pending Checkouts (guests who should have left but haven't) and Pending Checkins (bookings due for arrival that haven't been checked in yet).

### 1.6 Night Audit
An end-of-day reconciliation process that reviews the day's operations across four areas: check-ins, check-outs, bills, and room statuses. Each section must be reviewed and marked complete before the overall audit can be finalized. The night audit blocks dashboard access until completed, ensuring daily reconciliation. The feature can be enabled or disabled by hotel management.

### 1.7 Global Search
A search function accessible from the dashboard for finding bookings and guests across the entire system.

### 1.8 Default View Preference
Each user can set their preferred dashboard tab (Booking View, Rooms View, or Hotel Position) as the default landing view when they log in.

### 1.9 Subscription Check
The system validates the hotel's active subscription status on login. If the subscription has expired, a "Subscription Ended" screen is shown instead of the dashboard.

---

## 2. Front Office

### 2.1 Bookings

#### Booking List
Displays all bookings with filtering by date, source, and status. Booking cards show guest name, booking ID, check-in/out dates, room type, booking source logo, and current status.

#### Reservation
A 3-step guided flow for creating new reservations:
1. Select check-in and check-out dates and check room availability.
2. Choose room types, quantities, and rate plans with pricing details.
3. Enter guest information (name, email, phone, address, ID proof) with support for multiple guests per room.

A success screen confirms the booking with the assigned booking ID and offers options to create another reservation or return to the dashboard.

#### Quick Reservation
A simplified single-page reservation form that bypasses the multi-step wizard for faster booking creation.

#### Walk-in
A 3-step guided flow for walk-in guests:
1. View floor-wise room availability and select rooms across multiple room types.
2. Configure room rates (rack rate or custom), review tax calculations and billing summary.
3. Capture guest details with optional webcam photo and ID proof upload.

On success, an animated confirmation screen shows the assigned room numbers. Each room number is clickable to generate a check-in slip. Additional options include creating another walk-in, printing the Guest Registration Card (GRC), or returning to the dashboard.

#### Group Walk-in
Bulk walk-in handling for groups, allowing selection of multiple rooms across room types with group-level billing and rate configuration.

#### Booking Details
A comprehensive view of any booking showing guest information, room allocation, check-in/out dates with night count, billing breakdown (room charges, taxes, collections, balance), booking source, and channel logo. Available as both a side panel and a full page. Includes options to generate and download booking vouchers, share documents, and view audit logs.

#### Modify Booking
Extensive booking modification capabilities including:
- Change check-in and check-out dates.
- Adjust room rates per night.
- Add or remove rooms from the booking.
- Change room type assignments.
- Update guest details.
- Edit payment collection records.
- Adjust outstanding balance.
- Add a different room type to an existing booking.
- View rooms already checked in under the booking.

#### Cancel Booking
Two-step cancellation process: select the booking, then confirm cancellation with a reason.

#### Delete Booking
Permanent booking deletion protected by master PIN validation for security.

#### No-Show
Mark bookings as no-show when guests fail to arrive by the expected date.

### 2.2 Check-In

#### Check-In List
Displays bookings ready for check-in with guest and room details.

#### Check-In Process
Guides staff through room allocation, guest detail verification, and billing review before confirming the check-in. A success screen is shown upon completion.

#### Quick Check-in
Code-based fast check-in for guests who have pre-registered, allowing staff to bypass the full check-in flow.

### 2.3 Stay Management

#### Stay List
Shows all currently checked-in guests with room details. Supports a booking-wise list view for properties with group bookings.

#### Manage Room
A full management page for each checked-in room, providing:

**Guest Information:**
- Guest carousel showing each guest's details with navigation between multiple guests.
- View and download ID proof documents.
- DigiLocker verification status indicator for digitally verified guests.
- Add new guests, update existing guest details, or remove guests from the room.
- Editable guest notes.

**Booking & Room Details:**
- Booking date, arrival time, company name, and GSTIN.
- Room type, room number, check-in/out dates with night count.
- Meal plan information.

**Billing Summary:**
- Room charges, other charges, collections, and balance due.
- Dues and Collection tabs for detailed billing breakdown.
- Collection receipt generation and viewing.

**Room Actions:**
- Generate and view check-in slip.
- Navigate to housekeeping/room service requests.
- Change room — move the guest to a different room.
- Extend stay — push the check-out date forward.
- Upgrade meal plan or room type.
- Early checkout with options to checkout immediately or prepone the checkout date.
- Standard checkout initiation (checks for late checkout hours).
- Add bills — add expenses or collections to the room.
- Download bill as PDF.
- View audit log (for hotel owners).

#### Stay Requests
Per-room request management with two sections:

**Housekeeping Requests:**
- View pending requests with a timeline of actions taken.
- Create new housekeeping requests by selecting items needed.
- Assign housekeeping attendants to requests.
- Track request status and add remarks.

**Room Service Requests:**
- View room service orders placed for the room.
- Track order status and details.

### 2.4 Check-Out

#### Check-Out List
Displays guests due for check-out with bill summary and outstanding balance.

#### Check-Out Details
A detailed checkout page with multiple panels:
- List of all bills associated with the booking.
- Itemized bill details showing room charges, taxes, and other charges.
- Bill summary with total amount, amount paid, and balance due.
- Payment collection form supporting cash, card, and UPI modes.
- Bill selection for invoice generation — choose which bills to include.
- Split bill functionality to divide charges across multiple invoices.
- Invoice generation with GST details.
- Invoice viewing and download.
- Raise additional charges if needed.
- Final bill collection before completing checkout.

#### Group Check-Out
Batch checkout for group bookings, processing all rooms in a group simultaneously.

#### Check-Out Success
Confirmation screen displayed after successful checkout.

### 2.5 Pre-Allocation
Assign specific physical rooms to upcoming bookings before the guest arrives, ensuring preferred rooms are reserved and prepared.

### 2.6 Pre Check-In Management
View and manage guests who have completed the mobile pre-check-in process, reviewing the details they submitted remotely.

### 2.7 Foreign Guests
Track foreign national guests for regulatory compliance. Includes Form C generation — the foreigner registration form required by Indian law — with passport details, visa information, and submission status tracking.

### 2.8 Feedback Management
View bookings eligible for feedback and send feedback request SMS messages to guests.

### 2.9 Quick Payment Links
Create and manage payment links sent to guests:
- Active links — currently valid payment links.
- Received — links where payment has been completed.
- Expired — links that have passed their validity period.
Includes options to create new links and resend emails.

---

## 3. Food & Beverages

### 3.1 Restaurant Orders

#### Restaurant Selection
A card-based screen showing all configured restaurants with images, star ratings, names, and cuisine types. Selecting a restaurant opens its order management page.

#### Order Management
Table-based order management for each restaurant with tabs for active, completed, and cancelled orders.

#### Creating and Updating Orders
- Select a table for the order.
- Browse the menu with item variants (sizes, options).
- Add items to the order with quantity controls.
- Add special remarks or instructions per item.
- Support for grouped table orders (combining multiple tables).

#### Kitchen Order Tokens (KOT)
- Generate KOTs to send orders to the kitchen.
- Add new items to existing KOTs.
- Remove items from a KOT before sending.
- Print KOT slips for kitchen staff.

#### Restaurant Billing
- Generate itemized restaurant bills.
- View bill breakdown with taxes.
- Collect payment via multiple modes (cash, card, UPI, POS device).
- Credit sale option to charge the bill to a guest's room.
- Close/complete orders after payment.

### 3.2 Room Service Orders

#### Order Management
View and manage room service orders across three tabs: pending, completed, and cancelled.

#### Creating and Updating Orders
- Select an occupied room for the order.
- Browse the room service menu with item variants.
- Cart management with quantity adjustments.
- Order confirmation with success notification.

#### Room Service Billing
- Generate room service bills.
- View bill details with itemized breakdown.
- Collect payment with POS device integration.
- Generate formal invoices.
- Print order slips for kitchen preparation.
- Push orders to POS devices.

### 3.3 Kitchen Orders

#### Kitchen Selection
Card-based view of all kitchens with pending order counts displayed on each card.

#### Kitchen Order Management
- View incoming kitchen order tokens (KOTs) for each kitchen.
- View completed KOTs.
- See order details with full item list.
- Accept incoming orders to acknowledge receipt.
- Mark orders as prepared when cooking is complete.

### 3.4 Meal Tracking
Track meal plan fulfillment for in-house guests:
- List of all guests with their meal plan status.
- View meal details per guest showing which meals have been served.
- Mark individual meals as served or not served.

---

## 4. Housekeeping

### 4.1 Room Preparation
Displays rooms that need preparation after guest checkout. Each room card shows the request date, time, room number, and room type. Staff can proceed to a checklist-based preparation verification form to mark rooms as ready.

### 4.2 Housekeeping Requests
Manages all housekeeping service requests:
- View all requests with status filtering (pending, in-progress, completed).
- Assign housekeeping attendants to specific requests.
- Mark requests as completed.
- View detailed issue information for each request.
- Searchable attendant selection dropdown.
- Track which attendants are currently active/working.

### 4.3 Room Clearance
Room clearance inspection management:
- View pending clearance inspections.
- Complete checklist-based inspection forms with configurable questions.
- View history of completed clearance inspections.

### 4.4 Hotel Laundry
Manage the hotel's laundry operations:
- Issue laundry items for cleaning.
- Receive returned laundry items.
- Track items currently in the laundry process.
- View completed laundry records.

---

## 5. Finance

### 5.1 Expenses
Daily expense tracking with date-based navigation:
- View expense transactions for any selected date.
- Transaction table showing transaction number, account name, and amount.
- Total expense amount for the day.
- Add new expenses with account selection, vendor, amount, payment mode, and description.
- Edit existing expense records.
- Delete expenses with confirmation dialog.
- Conditional controls — add/edit/delete buttons are enabled or disabled based on permissions and business rules.

### 5.2 Incomes
Daily income tracking (mirrors the expense interface):
- View income transactions by date.
- Add, edit, and delete income records.
- Income account selection with amount and payment mode.

### 5.3 Credit Collections
Track outstanding credit transactions for company settlements and credit sales:
- Filter by status (All, Open, Partially Paid, Fully Paid) and type (Company Settlement, Credit Sale).
- Total outstanding amount displayed prominently.
- Transaction table showing date, booking ID, type, customer name, total amount, paid amount, outstanding amount, and status.
- Color-coded status badges (Open, Partially Paid, Fully Paid).
- Type badges with icons distinguishing company settlements from credit sales.
- Collect payment against open or partially paid transactions.
- View details of fully paid transactions.

---

## 6. Reports

### 6.1 Front Office Reports
- **Search Guest** — Search and view guest details across all bookings in the system.
- **GRC (Guest Registration Card)** — Generate and download Guest Registration Card PDFs for any booking.
- **Feedback Report** — View aggregated guest feedback data.
- **Departure Report** — Daily list of departing guests with downloadable report.
- **Arrival Report** — Daily list of arriving guests with downloadable report.
- **Police Verification Report** — Generate police verification reports for regulatory compliance.
- **Sales Report** — Revenue and sales data analysis.
- **Booking Report** — Booking statistics, trends, and source-wise breakdown.
- **Manager Report** — Summary report designed for hotel managers with key operational metrics.
- **Daily Sales Report** — Day-wise sales breakdown with detailed figures.
- **Daily Statement** — Comprehensive daily financial statement covering all transactions.
- **Occupancy Report** — Daily room occupancy statistics and trends.
- **Availability Report** — Room availability overview across room types and dates.

### 6.2 Finance Reports
- **Cashier Report** — Cashier-wise transaction summary with Excel download option.
- **Balance Report** — Account balance overview with PDF download.
- **Expense Report** — Expense breakdown by account and vendor with PDF download.
- **Vendor Report** — Vendor-wise expense summary with PDF download.

### 6.3 Food & Beverage Reports
- **Restaurant Sales** — Restaurant revenue report by date range.
- **Room Service Sales** — Room service revenue report by date range.
- **Kitchen Orders Report** — Kitchen order volume and statistics.
- **Restaurant Orders Report** — Restaurant order volume and statistics.
- **Room Service Orders Report** — Room service order volume and statistics.
- **Search Orders** — Search across all F&B orders by order ID, date, or other criteria.
- **Table Utilization** — Restaurant table usage analytics showing how efficiently tables are being used.
- **Kitchen Items Report** — Kitchen item-wise sales data showing which items are most/least popular.
- **Restaurant Items Report** — Restaurant item-wise sales data.
- **Room Service Items Report** — Room service item-wise sales data.
- **Top 10 Order Items** — Ranking of the most frequently ordered items across all F&B channels.

### 6.4 Housekeeping Reports
Housekeeping activity and performance data covering request volumes, completion times, and attendant performance.

---

## 7. Manage

### 7.1 Manage Rooms
- View all rooms organized by floor.
- Add new rooms with room number, room type, and floor assignment.
- Edit room details (room number, type, floor).
- Change room status (available, occupied, maintenance, out of order).
- Manage out-of-order rooms with date ranges and reasons.
- Floor management — create and organize hotel floors.
- Delete rooms that are no longer in use.

### 7.2 Manage Users
- View all hotel staff and user accounts.
- Add new users with personal details and role assignment.
- Edit user details and activate/deactivate accounts.
- Role management with granular permissions covering:
  - Front Office access (bookings, check-in, stay, check-out, etc.)
  - Food & Beverage access (restaurant orders, kitchen orders, room service)
  - Housekeeping access
  - Maintenance access
  - Admin/management access
- Multi-property access control — assign which hotel properties each user can access.

---

## 8. Settings

### 8.1 Front Office Setup
- **Invoice Setup** — Configure invoice template including header, footer, logo, GST details, and terms. Preview the invoice layout before saving.
- **Check-In/Out Time Setup** — Set default check-in and check-out times for the property.
- **Guest Message Setup** — Configure automated messages sent to guests at various stages (booking confirmation, pre-arrival, etc.).
- **Booking Source Setup** — Manage booking source channels (OTA names, direct booking, walk-in, corporate, etc.). Add, edit, or remove sources.
- **Guest Field Configuration** — Configure which guest information fields are required, optional, or hidden during the check-in process.
- **Self Check-in Setup** — Configure the self check-in feature including QR code generation for guests to scan and check in on their own devices.

### 8.2 Housekeeping Setup
- **Clearance Setup** — Configure room clearance checklist questions that housekeeping staff must answer during room inspections. Add, edit, or remove questions.
- **Items Setup** — Manage the list of housekeeping supply items available for room requests.
- **Laundry Items Setup** — Configure laundry item categories and pricing for the hotel laundry service.

### 8.3 Food & Beverage Setup

#### Kitchen Setup
Create and manage kitchens:
- Kitchen details (name, type, description).
- Item groups — organize menu items into categories.
- Menu items with images, descriptions, pricing, and variants.
- Kitchen photos for display.
- Bulk item upload/download via Excel templates.
- AI-powered image generation for menu items.

#### Restaurant Setup
Configure restaurants:
- Restaurant details (name, type, rating, description).
- Table management — add, edit, remove tables with seating capacity.
- Operating hours/timings — set opening and closing times for different meal periods.
- Menu items linked from kitchen items with restaurant-specific pricing and availability.
- Tax configuration (GST/service tax rates).
- Restaurant logo upload.
- QR code generation for guest-facing digital menu.
- Restaurant photos.
- Bulk item upload/download via Excel.

#### Room Service Setup
Configure the room service offering:
- Assign which kitchen handles room service orders.
- Menu items linked from kitchen items with room service-specific pricing.
- Operating hours/timings for room service availability.
- Tax configuration.
- QR code for in-room ordering.
- Activation/deactivation of the room service feature.

#### Thermal Printer Setup
Configure thermal printers for printing KOT slips and bills.

### 8.4 Finance Setup
- **Expense Accounts** — Create and manage expense account categories for organizing expenses.
- **Income Accounts** — Create and manage income account categories.
- **Vendor Setup** — Create and manage vendor profiles for expense tracking.
- **Customer Setup** — Create and manage customer profiles for credit transactions and company settlements.
- **QR Code Setup** — Configure payment QR codes displayed to guests for direct payments.
- **Payment Gateway Setup** — Configure online payment gateway integrations for collecting payments.
- **POS Setup** — Configure Point of Sale device integrations for card and digital payments.

### 8.5 Security Setup
- **Master PIN Setup** — Set, change, or reset the master PIN used for sensitive operations like deleting bookings. Includes OTP verification for PIN reset.
- **User PIN Setup** — Individual user PIN configuration for quick authentication during operations.

---

## 9. Guest-Facing Modules

### 9.1 Mobile Pre Check-In
A mobile-optimized flow for guests to complete check-in formalities before arriving at the hotel:
- OTP-based identity verification.
- Guest data form for submitting personal details, ID documents, and preferences.
- Available guests screen showing who can pre-check-in.
- DigiLocker integration for digital document verification (Aadhaar, PAN, etc.).
- QR code scanning to initiate the pre-check-in process.
- Responsive design — shows a "please use mobile" message on desktop browsers.

### 9.2 Self Check-In
A mobile self-service check-in flow for guests:
- Hotel identification via encoded URL/QR code.
- Booking lookup by guest details.
- OTP verification for identity confirmation.
- Multi-step self check-in process with guest detail confirmation.
- Booking list view when multiple bookings exist for the guest.
- DigiLocker verification integration.
- Mobile-only interface with desktop redirect message.

### 9.3 Room Service Menu (Guest-Facing)
A digital menu for guests to order room service from their own devices:
- Browse the menu organized by food categories.
- View item details with images, descriptions, and pricing.
- Select item variants (size, preparation style, etc.).
- Add items to cart with quantity controls.
- Floating cart summary and floating menu navigation.
- Enter/verify guest and room details.
- Place orders with confirmation screen and animated success state.
- Track order status in real-time.
- View order history.

### 9.4 Restaurant Menu (Guest-Facing)
A digital restaurant menu for dine-in guests:
- Browse the restaurant menu by category.
- View item cards with variant selection.
- Cart management with quantity adjustments.
- Enter or change user details for the order.
- Place orders with confirmation.
- Track order preparation status.
- View past orders.
- "No Menu" fallback screen when the restaurant has no items configured.

### 9.5 Guest Review
A guest-facing review and rating system:
- Star rating interface for different aspects of the stay.
- Written feedback submission.
- Review submission confirmation screen.
- Error handling for invalid or expired review links.

### 9.6 Feedback Links
A hotel-branded page with links to the hotel's social media and review platforms:
- Displays hotel logo and name.
- Links to Google Reviews, Google Maps, TripAdvisor, Facebook, Instagram, YouTube, WhatsApp, and custom websites.
- Each link shows the platform icon and a customizable display name.
- Customizable background color with gradient effect.
- Share button to share the feedback page link.

---

## 10. Kitchen Display System (KDS)

A dedicated display interface for kitchen staff to manage incoming orders:
- TV/large screen optimized view for kitchen environments.
- Web browser view for standard screens.
- KDS number screen showing order token numbers.
- Separate layouts optimized for TV-sized displays and smaller screens.

---

## 11. Booking Voucher

Generate and display booking vouchers for confirmed reservations. Vouchers contain booking details, guest information, room details, and hotel policies. Displayed in an embedded viewer for easy printing or sharing.

---

## 12. Rate Analyzer

A tool for analyzing and comparing room rates, helping hoteliers make informed pricing decisions. Displayed in an embedded viewer interface.

---

## 13. Hotel Information Page

A public-facing hotel information display with:
- Hotel details and description.
- Itinerary planner — a guest-facing tool for planning activities and local attractions during their stay.

---

## 14. Billing Settlement

A comprehensive billing system for managing guest bills during checkout:
- Create bills with selectable billable items.
- Update and modify existing bills.
- Settle bills by collecting payments.
- Delete bills if created in error.
- Track collections against each bill.
- Generate formal invoices from settled bills.

---

## 15. Company Settlement

Manage credit arrangements with corporate clients and travel companies:
- Add company settlement records with transaction details.
- View settlement list with outstanding amounts.
- Update settlement records.
- Delete settlements.
- View company-wise outstanding summary for accounts receivable tracking.

---

## 16. Profile & Authentication

- **Login** — Secure authentication with token-based session management.
- **Single Sign-On** — Seamless login from the parent BookingJini platform without re-entering credentials.
- **Session Management** — Automatic detection of expired sessions with a timeout popup prompting re-login.
- **My Profile** — View and manage personal profile information.
- **PIN Setup** — Configure a personal PIN for quick authentication during sensitive operations.
- **Role-Based Access Control** — Each page and feature is protected by permission checks. Users only see and access features their role allows.
- **Multi-Property Support** — Users with access to multiple hotel properties can switch between them seamlessly.
