# Bookingjini Host — Detailed Product Features

A comprehensive hotel property management system (PMS) built as a React Native mobile application. The app is branded under "Bookingjini" and serves hotel owners, managers, and staff to manage day-to-day hotel operations from their mobile devices.

---

## 1. Onboarding & Authentication

### Registration
- Phone number-based registration with country dial code selection (fetched from API)
- OTP verification for phone number validation
- Supports international phone numbers

### PIN-Based Security
- Users create a 4-digit master PIN after registration
- PIN is required for login on subsequent sessions
- Master PIN validation is required for sensitive operations (modifying bookings, editing financial data, deleting bookings)
- Forgot PIN flow with OTP-based reset
- Change PIN functionality from user profile

### Multi-Property Support
- A single user account can manage multiple hotel properties
- Property switcher accessible from the dashboard header
- Each property has its own ID, name, and logo
- Switching properties updates all data across the app instantly
- "Add another property" option to onboard new properties

### Property Onboarding Wizard
- Step-by-step setup: choose hotel type → property details → address with geolocation → subscription plan
- City/state/country selection with cascading dropdowns
- GPS-based location detection ("Get My Location")
- Carousel-based subscription plan chooser

### Account Management
- Account deletion request flow with confirmation text
- Logout from any screen via drawer menu or profile modal

---

## 2. Dashboard (Home Screen)

### Header & Navigation
- Drawer menu (hamburger icon) for full navigation
- Property name display with avatar initial
- Property switcher modal with all registered properties
- Global booking search bar (searches by booking ID, guest name, mobile, room number)
- QR code scanner button for quick check-in

### Real-Time Occupancy Cards
- Four cards showing today's counts: Check-in, Stay, Check-out, Vacant
- Each card is tappable and navigates to the respective screen
- Pull-to-refresh to update counts
- Loading indicators while data is being fetched

### Quick Links Panel
- Tabbed interface with three categories: Front Office, Housekeeping, F&B
- Front Office links: Bookings, Walk-in, Reservation, Reports
- Housekeeping links: Requests, Clearance, Room Preparation, Reports
- F&B links: Room Service Order, Restaurant Order, Kitchen Order, Reports
- Role-based access control — shows "No Access" modal if user lacks permission

### Dashboard Tabs
- Overview: revenue and bookings pie charts with BE (Booking Engine) vs OTA split, filterable by Today / MTD / YTD
- Hotel Position: room-level occupancy calendar view showing which rooms are occupied on which dates
- Bookings: calendar-based bookings view

### Night Audit Blocker
- If night audit is pending for a previous date, the entire dashboard is blocked
- User must complete the night audit before accessing any other feature
- Shows the pending audit date and a button to proceed

### Aura AI Chatbot
- Floating animated chatbot button (visible only if hotel has Aura AI subscription)
- Opens a WebView-based AI assistant (auraos.ai) with SSO authentication
- Voice greeting using Amazon Polly TTS ("Hey [name], How can I help you?")

### Announcements
- HTML-based announcement popups from the backend
- "Don't show again" button that acknowledges the announcement via API

### Subscription Management
- Checks subscription status on every property load
- If subscription has expired, shows a "Subscription Ended" screen blocking all features

---

## 3. Front Office

### 3.1 Reservations
- Date range selection via a full-screen calendar component (max 30 nights)
- Past dates are allowed for backdated reservations
- Taxable/non-taxable booking toggle (only shown if property is tax-enabled)
- Room type selection with availability display
- Room allocation: assign specific rooms from available inventory, floor-wise view
- Guest details capture: name, email, phone, address, ID proof (with OCR document parsing), special requests
- Booking voucher generation (PDF) after successful reservation
- Success page with booking ID and navigation options

### 3.2 Walk-in (Direct Check-in)
- 3-step wizard: Choose Date → Billing → Guest Info
- Supports single-room and multi-room walk-in
- Date picker for check-in and check-out
- Available rooms displayed floor-wise with room status (Vacant & Clean / Vacant & Dirty)
- Cart system for selecting multiple rooms across room types
- Early check-in detection: if check-in time is before the configured hotel check-in time, prompts for early check-in charges or exemption with remark
- Billing step: advance payment collection with multiple payment modes
- Guest info capture with ID document scanning (OCR)
- Taxable/non-taxable toggle

### 3.3 Bookings Management
- Date-based booking list with single date or date range filter
- Date navigation arrows (previous/next day)
- Calendar picker for jumping to specific dates
- Each booking card shows: channel logo (OTA source), booking ID, check-in/check-out dates, number of nights, guest name, phone, room type info
- Booking status badges: Upcoming, Checked-in, Partial Checked-in, Checked-out, Cancelled, Expired, No Show
- Business booking indicator icon

### 3.4 Booking Details (BookingInfo)
- Full booking detail view with date display header
- Booking details section: booking ID, status, date, guest info, phone (tappable to call), email, source, company name & GSTIN (for business bookings)
- Room details section: room type, plan type, room count, rate per room — editable with master PIN
- Dues & Collection tabs: itemized list of all charges and payments with dates, remarks, and amounts
- Balance calculation (total receivable vs received)
- Actions available:
  - Modify booking details (requires master PIN)
  - Modify dates (change check-in/check-out)
  - Add new room type to booking
  - Change room count per room type
  - Remove room type from booking
  - Delete booking (requires master PIN + remark)
  - Undo check-in
  - Cancel booking
  - View booking voucher
  - View audit logs timeline
  - Export bill / collection receipt
  - Edit guest details
- Image viewer for uploaded ID documents with download option

### 3.5 Check-in
- Date-based check-in list with pending/completed/all tabs
- Standard check-in flow: select booking → verify guest details → assign rooms floor-wise → confirm
- Group check-in: check in multiple rooms from the same booking simultaneously
- Multi-room check-in support
- Re-check-in for previously checked-out bookings
- Room availability display with floor-wise layout (color-coded: green = clean, gray = dirty)
- Check-in success screen with confirmation
- Quick Check-in via QR code scanning or 6-digit code entry

### 3.6 Pre Check-in
- List of upcoming bookings eligible for pre-check-in
- Send pre-check-in SMS to guests
- Guests can fill their details before arrival
- Check-in from pre-check-in data
- Change mobile number for pre-check-in link

### 3.7 Pre-Allocation
- Assign specific rooms to upcoming bookings before guest arrival
- View check-in list filtered for pre-allocation
- Floor-wise room selection
- Save and view pre-allocated rooms

### 3.8 Stay Management
- Booking-wise stay list showing all currently checked-in guests
- Search/filter by room number
- Filter by: All, Checkout Today, Overstay
- Each stay card shows: booking ID (tappable to booking details), checkout text, room count, guest name, phone, room number
- Tappable to navigate to detailed stay view per room

### 3.9 Stay Details (Per Room)
- Room details: room type, plan type, room image, rate
- Action buttons:
  - Change Room: floor-wise room picker with clean/dirty status, toggle to show all available rooms
  - Extend Stay: extend checkout date
  - Upgrade: meal plan upgrade or room type upgrade
  - Early Checkout / Checkout: if before scheduled checkout, offers "Checkout Now" or "Pre-pone Checkout Date" with date picker
- Guest details: horizontal scrollable guest cards with ID images, add new guest button
- Billing section with Dues/Collection tabs:
  - Itemized expenses and collections with dates and remarks
  - Removable transactions (with confirmation)
  - Collection receipt print/export
  - Share Bill and Add Bill buttons
  - Total receivable/payable balance
- Final price breakup: total, discount, tax, receivable, received, balance
- Audit logs timeline
- Check-in slip generation

### 3.10 Check-out
- Date-based checkout list with Pending/Checked Out/All tabs
- Date navigation and calendar picker
- Room search within checkout list
- Pending/checked-out/all counts displayed on tabs
- Checkout details view with billing summary
- Collect final amount with payment mode selection
- Invoice generation with options:
  - GST invoice toggle
  - IGST toggle
  - Custom invoice number entry
- Invoice preview and PDF generation
- Send feedback link to guest after checkout
- Group checkout support (checkout all rooms in a booking at once)

### 3.11 No-Shows
- List of bookings eligible for no-show marking
- Mark booking as no-show via API

### 3.12 Feedback
- List of bookings eligible for feedback collection
- Send feedback SMS link to guests

### 3.13 Foreign Guests
- List of foreign guests currently staying
- View and update foreign guest details (for Form C compliance / police verification)

### 3.14 Quick Payment Links
- Create new payment links with amount and guest details
- Three tabs: Active Links, Received Links, Expired Links
- Track payment status for each link

---

## 4. Housekeeping

### 4.1 Requests
- Two tabs: Pending Requests and Completed Requests
- Create new housekeeping request for any occupied room
- Assign request to a working housekeeping attendant (fetched from active attendants list)
- Request lifecycle: Created → Assigned → In Progress → Completed
- Request details view with full timeline
- Process and complete requests with status updates

### 4.2 Clearance
- Two tabs: Pending and Completed
- Clearance requests are raised when a room needs post-checkout inspection
- Process clearance: answer configurable clearance questions (e.g., "Are all amenities intact?", "Is the minibar stocked?")
- Track clearance with detailed timeline
- Complete clearance to mark room as inspected
- Clearance questions are configurable from Settings

### 4.3 Room Preparation
- List of rooms that need preparation (cleaning after checkout)
- Mark individual rooms or all rooms as prepared
- Room status updates reflected across the system (affects room availability for check-in)

### 4.4 Hotel Laundry
- Issue laundry items with details modal
- Receive laundry items back
- Track laundry status per issue

---

## 5. Food & Beverages (F&B)

### 5.1 Room Service Orders
- Select from occupied rooms to place an order
- Menu item selection with quantity and customization
- KOT (Kitchen Order Token) generation and printing
- Order lifecycle: Pending → In Progress → Completed → Billed → Cancelled
- Billing with tax calculation
- Invoice generation (PDF)
- POS integration for payment collection
- Add room service bill to room folio
- Completed and cancelled order views
- QR code generation for room service menu

### 5.2 Restaurant Orders
- Restaurant selection from list of configured restaurants
- Table-based ordering system:
  - View all tables with status (Available / Booked)
  - Select table(s) to start an order, supports table grouping
- Multi-KOT support: add multiple Kitchen Order Tokens to a single order
  - Each KOT can have different items
  - Remove items from KOT
  - Print individual KOT PDFs
- Order flow: Select Table → Add Items from Menu → Generate KOT → Send to Kitchen → Complete Order → Generate Bill
- Menu items displayed with price and availability
- Billing: generate restaurant bill with tax, collect payment (cash/card/UPI/POS)
- Invoice generation and PDF export
- POS integration for card payments
- Completed orders and cancelled orders views
- Restaurant-specific QR code for self-ordering

### 5.3 Kitchen Orders (KOT Management)
- Kitchen selection from list of configured kitchens
- View incoming Kitchen Order Tokens
- Accept KOT → Prepare → Complete workflow
- Completed orders history
- Kitchen-specific order management

### 5.4 Meal Tracking
- Track meal status for guests (breakfast, lunch, dinner)
- Mark meals as served/not served
- Tab-based interface for different meal types

---

## 6. Finance

### 6.1 Expense Tracking
- Add expense transactions with:
  - Expense account selection
  - Amount, date, remark
  - Vendor selection (searchable)
  - Payment mode
- View, edit, and delete expense transactions
- Transaction details view

### 6.2 Vendor Management
- Add, edit, delete vendors
- Vendor details: name, contact, address
- Search vendors
- Vendor-wise transaction reports

---

## 7. Manage Inventory

- View room inventory across all channels (OTA + Booking Engine)
- Channel-wise inventory display
- Bulk inventory update: set available rooms for date ranges
- Block inventory: prevent bookings for specific dates
- Unblock inventory: re-enable blocked dates
- Sync inventory across all connected channels
- Separate controls for Booking Engine inventory

---

## 8. Manage Rates

- View room rates channel-wise
- Bulk rate update for date ranges across OTAs
- Block rates: hide rates for specific dates
- Unblock rates: restore blocked rates
- Sync rates across all connected channels
- Separate Booking Engine rate management

---

## 9. Reports

Reports are organized into modules (Front Office, Housekeeping, F&B, Finance) with sub-groups. Report menus are dynamically fetched from the API, allowing server-side configuration of available reports.

### 9.1 Front Office Reports
- Sales Report: revenue breakdown by date range
- Arrival Report: expected arrivals for a date, downloadable
- Departure Report: expected departures for a date, downloadable
- Cashier Report: payment collections summary, downloadable
- Balance Report: outstanding balances across bookings, downloadable
- GRC Report: Guest Registration Card list
- Booking Report: booking details with drill-down to individual booking
- Occupancy Report: daily occupancy percentages
- Availability Calendar Report: room availability visualization
- Manager Report: high-level management summary
- Daily Sales Report: day-by-day sales breakdown
- Police Verification Report: guest data for police submission, downloadable
- Feedback Report: guest feedback scores and comments
- Guest Search: search guests across all bookings

### 9.2 F&B Reports
- Restaurant Sales Report
- Room Service Sales Report
- Restaurant Orders Report
- Kitchen Orders Report
- Room Service Orders Report
- Kitchen Items Report
- Restaurant Items Report (with Excel download)
- Room Service Items Report
- Table Utilization Report
- Top 10 Ordered Items
- Search Order (find specific F&B orders)
- GST Report (tax compliance)

### 9.3 Finance Reports
- Expense Report: expense breakdown with downloadable Excel/PDF
- Vendor Report: vendor-wise expense summary, downloadable

### Report Features
- Most reports support date range filtering
- PDF and Excel export/download capabilities
- Some reports open as WebView-based pages for rich visualization
- Others are native screens with custom UI

---

## 10. Hotel & Property Management

### Hotel Profile
- View and manage hotel details: name, logo, address, contact
- Navigate to room management, settings, user management

### Room Type Management
- Create new room types with: name, base occupancy, max occupancy, base rate
- Edit room type: basic info, rate plans, occupancy settings, images (upload/remove), amenities (select from categories)
- View rate plans associated with each room type

### Room Management
- Two tabs: Room Type and Rooms
- Add individual rooms: room number, floor assignment, room type
- Edit room details
- Update room status (available, out of order, etc.)
- Delete rooms
- Floor-wise room organization

### Floor Management
- Create and manage hotel floors
- Assign rooms to floors

### Room View
- Visual room grid showing all rooms with color-coded status
- Room details popup with current occupancy info
- Panorama room view: upload 360° images for rooms
- Navigate to booking details from occupied rooms

### OTA List
- View connected OTA (Online Travel Agency) channels

### Manage Reviews
- View and manage guest reviews

---

## 11. Settings & Configuration

### 11.1 Front Office Setup
- Invoice Setup: configure invoice template, header, footer, terms
- Invoice Preview: preview how invoices will look
- Check-in/Check-out Time Setup: set standard check-in and check-out times, enable early check-in detection
- Guest Message Setup: configure welcome messages and thank-you messages sent to guests
- Booking Source Setup: add, edit, remove custom booking sources
- Guest Configuration Setup: configure which guest fields are required/optional during check-in
- Tax Setup: configure tax rates and rules
- Self Check-in Setup: configure QR-based self check-in options

### 11.2 Housekeeping Setup
- Items Setup: add/remove housekeeping items (towels, toiletries, etc.) from a master list
- Clearance Questions: add, edit, delete custom clearance checklist questions
- Hotel Laundry Setup: configure laundry tracking

### 11.3 Food & Beverages Setup
- Kitchen Setup:
  - Add/edit kitchens with type selection
  - Manage item groups (categories)
  - Add/edit kitchen items with images
  - Upload items via Excel template
  - Kitchen photos management
  - Kitchen details and configuration
- Restaurant Setup:
  - Add restaurants with type, name, details
  - Table management: add, edit, remove tables
  - Timing slots: configure operating hours
  - Tax configuration
  - Menu items: add individually or upload via Excel
  - Restaurant photos and logo upload
  - QR code generation for restaurant menu
- Room Service Setup:
  - Assign kitchen to room service
  - Configure timings
  - Tax setup
  - Menu items management (add/upload via Excel)
  - Item price and availability updates
  - QR code for room service menu

### 11.4 Finance Setup
- Expense Accounts: create, edit, delete expense categories
- Vendor Management: add, edit, delete vendors with contact details
- QR Code Setup: configure UPI QR code for payments
- Payment Gateway Setup: configure payment gateway parameters
- POS Device Setup: add, edit, enable/disable POS terminals

### 11.5 Maintenance Setup
- Departments: add, edit, remove maintenance departments
- Service Partners: add, edit, remove external service partners

### 11.6 Night Audit
- Enable/disable night audit (irreversible once enabled)
- When enabled, daily night audit must be completed before proceeding to next day

---

## 12. Night Audit

- Triggered when night audit is enabled and a pending audit exists for a past date
- Four audit sections, each must be completed:
  - Checkins: review and confirm all check-ins for the audit date
  - Checkouts: review and confirm all checkouts
  - Bills: review and confirm all bills/transactions
  - Room Status: verify room statuses are correct
- Each section has its own completion status (PENDING / COMPLETED)
- "Complete Night Audit" button only enabled when all four sections are done
- After completion, navigates back to dashboard with refresh

---

## 13. User Management

### Employee Management
- View list of all employees/users
- Add new employee: name, phone, role, department
- Edit employee details
- Toggle employee active/inactive status
- Change employee PIN

### Role-Based Access Control
- Screen-level permission system: each menu item checks if the user's role has access
- "No Access" modal shown when user tries to access unauthorized features
- Roles fetched by user group
- Hotel-level user access: configure which properties a user can access

### User Profile
- View and edit personal profile
- View profile details

### Jini Coins (Loyalty System)
- View coin balance
- Recent transactions list
- Transaction history by date
- Auto-load coins configuration (enable/disable, cancel)
- Buy coins
- Use offers/rewards
- Coins earned tracking

### Other
- Rate Us: prompt to rate the app
- Refer Bookingjini: share referral
- Learn & Practice sections for training

---

## 14. Maintenance

### Issue Tracking
- Add new maintenance issues with:
  - Department selection
  - Issue description and details
- View ongoing issues
- View completed issues
- Issue details with full information
- Track issue progress with logs
- Mark issues as complete
- Remove/delete issues

### Setup
- Department management (add, edit, remove)
- Service partner management (add, edit, remove)

---

## 15. Cross-Cutting Features

### Push Notifications
- Firebase-based push notification support
- Notification permission request on app launch
- Notification handling and routing

### QR Code Scanner
- Built-in camera-based QR scanner using react-native-camera-kit
- Custom scanning overlay with animated scan line
- Used for quick check-in: scans guest QR code and navigates to check-in flow

### Document Scanning (OCR)
- Identity document parsing via API
- Captures guest ID images and extracts text data
- Used during check-in and reservation guest detail capture

### PDF & Excel Generation
- PDF generation for: invoices, bills, GRC, booking vouchers, KOT, collection receipts
- Excel download for: reports, item lists, inventory data
- Share functionality for generated documents

### POS Integration
- Push transactions to connected POS devices
- Check POS transaction status
- Supported in both front office billing and F&B billing

### Payment Gateway Integration
- Online payment collection via configured payment gateways
- UPI QR code-based payments
- Multiple payment modes: cash, card, UPI, bank transfer, POS

### Signature Capture
- Digital signature capture component (available but usage varies)

### Multi-Currency Support
- Currency symbol fetched from property configuration
- Displayed across all financial screens using Unicode code point conversion

### Offline-Friendly Design
- No-internet detection with Lottie animation
- AsyncStorage for caching user data, tokens, and preferences

### App Updates
- Update app screen for forcing version updates

---

## 16. Technical Architecture

### State Management
- Redux for global state (user, hotel, common states, checkin, housekeeping, F&B, night audit)
- Multiple reducers: UserReducer, HotelReducer, CommonStatesReducer, CheckinReducer, ClearanceQuestionReducer, NightAuditReducer, RestaurantOrderReducer, RoomServiceOrderReducer, SetupReducer

### API Layer
- Multiple API clients for different backends:
  - `api` (gems API): main PMS backend
  - `kernelApi`: hotel summary and analytics
  - `beApi`: booking engine operations
  - `cmApiNew`: channel manager operations
  - `nodeGemsApi`: newer Node.js-based services (night audit, etc.)
  - `subscriptionApi`: subscription management
  - `publicGemsApi`: public-facing APIs
- Centralized endpoint definitions in `endPoints.js`

### Navigation
- React Navigation with drawer + stack navigators
- Separate navigation stacks: FrontOffice, Housekeeping, FandB, Hotel, Finance, NewUser, Onboarding
- Deep linking support for QR code scanning

### UI Components
- Custom component library: Cards, Buttons, Modals, Headers, Typography, Layouts, Dropdowns, Pills, Checkboxes, Radio Buttons
- Lottie animations for loading states and celebrations
- Inter font family throughout the app
- Consistent color theming per module (Front Office = orange, Housekeeping = green, F&B = purple, Finance = blue, Maintenance = gray)
