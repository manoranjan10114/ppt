# Booking Engine Frontend — Module-Wise Feature List

## Version: 3.2.2 | Tech Stack: Next.js 14, React 18, Zustand, Mantine UI, SWR, TailwindCSS, Firebase Auth

---

## 1. Modules Identified

- Hotel Authentication & Initialization
- Device Detection & Responsive Rendering
- Search / Availability (Calendar & Occupancy)
- Room Listing & Room Cards
- Room Details
- Rate & Pricing Display
- Cart Management
- Booking Flow (Checkout)
- Guest Details & Form Validation
- Payment Integration
- Booking Confirmation (Thank You)
- Booking Voucher / Invoice
- Payment Failure Handling
- Package Booking
- Day Booking (Day Outing)
- User Authentication (Login / OTP)
- My Account / Booking Management
- Multi-Property Support
- Offers & Coupons
- Addon Services (Paid Services)
- About Hotel / Content Pages
- OTA Price Comparison
- Chat Widget & WhatsApp Integration
- Analytics & Tracking Plugins
- Banner / Promotional Carousel
- Offer Notification Modal
- Navigation & Navbar
- Footer
- SEO & Metadata
- Currency Management
- Localization (GST / Tax)
- Error Handling & Fallback UI
- Component Documentation (Internal)

---

## 2. Module-Wise Feature Extraction

---

### Module: Hotel Authentication & Initialization

#### Features

- Feature: Domain-Based Hotel Auth
  - Description: On app load, the system extracts the current domain (hostname) and calls `bookingEngine/auth/{domain}` to fetch hotel-specific configuration including API key, theme color, header colors, hotel ID, company ID, currency, and chat option. This enables white-label multi-tenant deployment where each hotel gets its own branded booking engine based on its domain.
  - Components: `src/screens/Device/Device.tsx`, `src/api/useHotelAuth.ts`
  - APIs: `GET bookingEngine/auth/{domain}`

- Feature: Hotel Info Fetching
  - Description: After auth, fetches comprehensive hotel data (name, logo, policies, check-in/out times, social links, ratings, payment provider config, advance booking days, cutoff time, etc.) using the API key and hotel ID. This data drives the entire UI.
  - Components: `src/api/useHotelInfo.ts`, `src/screens/Device/Device.tsx`
  - APIs: `GET get-hotel-info/{apiKey}/{hotelId}`

- Feature: IP-Based Geolocation
  - Description: Detects user's country, calling code, and IP address via `tools.bookingjini.com/ipfox` to pre-fill country code in phone inputs and set default locale.
  - Components: `src/api/useIpLocation.ts`
  - APIs: `GET https://tools.bookingjini.com/ipfox`

- Feature: Session Persistence with Zustand
  - Description: All global state is persisted to `sessionStorage` via Zustand's `persist` middleware (key: `hoteldata-storage`). On page refresh, session storage is cleared via `beforeunload` event to force fresh data load.
  - Components: `src/store/useStore.ts`, `src/screens/Device/Device.tsx`

- Feature: Visitor Tracking
  - Description: Posts visitor analytics (browser, referrer, page URL, device type) to the backend dashboard on each page load for the hotel.
  - Components: `src/screens/Device/Device.tsx`
  - APIs: `POST https://be.bookingjini.com/dashboard/uniqueVisitors/{hotelId}`

#### Observations

- Firebase config has hardcoded API keys (security concern)
- Console methods are disabled in production via `disableConsoleMethods()`
- Session storage is cleared on every page refresh — no persistent cart across refreshes

---

### Module: Device Detection & Responsive Rendering

#### Features

- Feature: Mobile vs Desktop Detection
  - Description: Uses `mobile-detect` library to detect if the user is on a mobile/tablet or desktop device. Sets `deviceType` in global store. Renders completely different component trees for mobile (`MobileView`) and desktop (`WebView`).
  - Components: `src/screens/Device/Device.tsx`

- Feature: Separate Component Libraries
  - Description: Maintains two full sets of UI components — `src/components/web/` and `src/components/mobile/` — with independent implementations for Header, Footer, Cart, Checkout, Room Cards, Calendar, etc.
  - Components: `src/components/web/`, `src/components/mobile/`

#### Observations

- Full component duplication between web and mobile — significant maintenance overhead
- No shared responsive components; everything is forked

---

### Module: Search / Availability (Calendar & Occupancy)

#### Features

- Feature: Date Range Selection with Calendar
  - Description: Custom calendar component fetches minimum rates per day from `min-rates/{hotelId}/{date}/{currency}/INR` and displays them on each date cell. Users select check-in date, then check-out date. Supports sold-out date detection and invalid range warnings. Maximum 15-night restriction enforced.
  - Components: `src/components/web/Calender/Calendar.tsx`, `src/components/web/Header/Header.tsx`
  - APIs: `GET min-rates/{hotelId}/{date}/{currency}/INR`

- Feature: Occupancy Selection (Rooms, Adults, Children, Infants)
  - Description: Popover-based occupancy selector with increment/decrement controls for rooms, adults, children, and infants. Child and infant age badges are shown from hotel config. Maximum rooms capped at hotel's `total_no_of_rooms`.
  - Components: `src/components/web/Header/Header.tsx`

- Feature: Check Availability
  - Description: On clicking "Check Availability", sends a POST to `get-inventory-rate` with check-in/out dates, hotel ID, API key, currency, adult/child/room counts, device type, and member-only flag. Returns available room types with rate plans. Also fetches addon services in parallel.
  - Components: `src/components/web/Header/Header.tsx`
  - APIs: `POST get-inventory-rate`, `POST active-paid-services`

- Feature: Advance Booking Days
  - Description: Respects `hotelInfo.advance_booking_days` — if the selected check-in date is within the advance booking window, it auto-adjusts to the earliest allowed date.
  - Components: `src/components/web/Header/Header.tsx`

- Feature: Cutoff Time Handling
  - Description: Implements cutoff time logic from `hotelInfo.cutoff_time`. If current time is before cutoff, allows same-day check-in (previous day). Runs an interval timer that clears session storage and resets dates when cutoff time is reached.
  - Components: `src/components/web/Header/Header.tsx`

- Feature: Date Persistence via Session Storage
  - Description: Selected dates are stored in `sessionStorage` as `datesArray` so they survive component re-renders but not page refreshes.
  - Components: `src/components/web/Header/Header.tsx`

#### Observations

- Calendar data is fetched 2 months at a time
- Sold-out dates are visually indicated and prevent selection
- Member-only pricing is supported when user is logged in

---

### Module: Room Listing & Room Cards

#### Features

- Feature: Room Type Card Display
  - Description: Each room type is rendered as a card showing room image, room name, bed type, room size, view type, max occupancy, and available rate plans. Images are loaded from CloudFront CDN. Supports image carousel with zoom-in modal.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`, `src/components/web/RoomCardList/RoomCardList.tsx`

- Feature: Rate Plan Selection with Meal Plans
  - Description: Each room type shows multiple rate plans (meal plans) in a carousel (4 per slide). Each plan shows plan name, price (with discount strikethrough if applicable), promotion type badge, and an "Add Room" button. Only one meal plan can be selected per room type.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`

- Feature: Multiple Occupancy Pricing
  - Description: Implements complex pricing logic — if total occupancy is below base occupancy, uses `multiple_occupancy` array pricing. Otherwise uses `price_after_discount`. Extra adult/child charges are calculated separately.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`, `src/utils/addRoomUtils.ts`

- Feature: Add/Remove Rooms
  - Description: Users can add multiple rooms of the same type (up to `min_inv` limit). Each room gets independent occupancy controls. Removing all rooms of a type removes it from cart entirely.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`

- Feature: Per-Room Occupancy Adjustment
  - Description: After adding a room, users can adjust adult/child/infant counts per room. Enforces `max_occupancy` limit per room. Shows extra adult/child price modals when exceeding base occupancy.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`

- Feature: Alternate Date Suggestions
  - Description: When a room is sold out, fetches alternate available dates via `POST alternative-dates-by-roomtype` and displays them. Users can click an alternate date to re-fetch inventory for that date range.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`
  - APIs: `POST alternative-dates-by-roomtype`

- Feature: Minimum Length of Stay Enforcement
  - Description: If the selected date range is shorter than `min_los` for a room type, the "Add Room" button shows "Unavailable" with a notification explaining the minimum stay requirement.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`

- Feature: Free Cancellation / Non-Refundable Badges
  - Description: Displays ribbon badges on room images indicating "Free Cancellation" (green) or "Non-Refundable" (red) status based on `free_cancelation_status`.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`

- Feature: Discount & Promotion Display
  - Description: Shows percentage or flat discount amounts with promotion type badges (e.g., "Early Bird", "Last Minute"). Bar price is shown with strikethrough alongside discounted price.
  - Components: `src/components/web/RoomCard/RoomCard.tsx`

- Feature: Skeleton Loader
  - Description: Shows animated skeleton cards while room inventory is loading.
  - Components: `src/components/web/RoomCardList/RoomCardsLoader.tsx`

#### Observations

- Price display includes "Excluding Tax" label when hotel is taxable
- Price breakdown shows per-adult, per-child, per-room, per-night context
- Future applicable promotions are tracked but display logic is in the card

---

### Module: Room Details

#### Features

- Feature: Room Details Modal/Drawer
  - Description: Clicking a room image opens a detailed view with image carousel (auto-play), bed type, max guests, room view type, room size, amenities list (with "View More"), and room description (with "Read More" truncation).
  - Components: `src/components/web/RoomDetails/RoomDetails.tsx`

#### Observations

- Amenities use a ViewMoreItem component showing 5 initially
- Description uses ReadMore with 300 char initial limit

---

### Module: Cart Management

#### Features

- Feature: Footer Cart Bar
  - Description: A sticky bottom bar appears when items are in the cart. Shows cart summary and a "Checkout" button. If addon services are available, clicking checkout first opens the addon services modal.
  - Components: `src/components/web/FooterCart/FooterCart.tsx`, `src/components/web/FooterCart/FooterCartContent.tsx`

- Feature: Price Breakup in Cart
  - Description: Detailed price breakup showing per-room, per-night pricing with GST, discounts, and service charges. Accessible from the cart footer.
  - Components: `src/components/web/FooterCart/PriceBreakup.tsx`

- Feature: Delete Room from Cart
  - Description: Confirmation modal before removing a room type from cart. Clears paid services if last room is removed.
  - Components: `src/components/web/FooterCart/DeleteModal.tsx`

- Feature: Service Charge Calculation
  - Description: Calculates service charges as a percentage of room total, then applies tax on the service charge itself. Uses `add_on_charges_percentage` and `add_on_tax_percentage` from hotel config.
  - Components: `src/utils/cartUtils.ts`

#### Observations

- Cart is stored in Zustand global state
- Cart is cleared on payment success, date change, or property switch

---

### Module: Booking Flow (Checkout)

#### Features

- Feature: Checkout Page Layout
  - Description: Two-column layout — left side shows booking details (room summary, addon services, coupon, price breakdown) and right side shows guest details form with payment options. Breadcrumb navigation (Home > Checkout). "Modify" button to go back.
  - Components: `src/screens/web/Checkout/Checkout.tsx`, `src/components/web/BookingDetails/BookingDetails.tsx`, `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Booking Loader
  - Description: Full-screen loader with "Hold On! We are getting you booking status..." message while payment is being processed. Prevents user from closing/refreshing.
  - Components: `src/screens/web/Checkout/Checkout.tsx`

- Feature: Empty Cart Redirect
  - Description: If user navigates to checkout with an empty cart, automatically redirects to home page.
  - Components: `src/screens/web/Checkout/Checkout.tsx`

---

### Module: Guest Details & Form Validation

#### Features

- Feature: Personal / Business Toggle
  - Description: Radio toggle between "Personal" and "Business" checkout modes. Business mode adds Company Name and GSTIN fields as required.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Guest Details Form
  - Description: Collects name, email, phone, address, zip code, country, state, city. Country/State/City are cascading dropdowns fetched from APIs. Supports arrival time picker and guest notes.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`
  - APIs: `GET get-all-country`, `GET get-all-states/{countryId}`, `GET get-all-city/{stateId}`

- Feature: Minimal Checkout Mode
  - Description: When `hotelInfo.min_field_in_checkout_page === 1`, only name, email, and phone are required — address fields become optional.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Form Validation
  - Description: Validates email format (regex), GSTIN format (15-char Indian GST regex), and phone number format. Shows inline error messages and notification toasts.
  - Components: `src/utils/guestDetails.ts`

- Feature: Auto-Fill from Login
  - Description: When user is logged in, guest details are pre-populated from their profile (name, email, phone, address, country, state, city, company, GSTIN).
  - Components: `src/screens/Device/Device.tsx`, `src/components/web/BookingDetails/GuestDetails.tsx`
  - APIs: `GET guest-info`

- Feature: Default Country from Hotel Locale
  - Description: Auto-selects the hotel's country as default in the country dropdown using `localedetails.country_id`.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Terms & Conditions Acceptance
  - Description: Checkbox for accepting terms and conditions. Links to a modal showing hotel policies (child policy, cancellation policy, T&C, hotel policy). Required before payment.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Mandatory Guest Notes
  - Description: When `hotelInfo.is_note_mandatory` is true, guest notes become a required field.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

---

### Module: Payment Integration

#### Features

- Feature: Razorpay Integration
  - Description: Dynamically loads Razorpay checkout.js, creates order via `GET razorpay-orderid/{invoiceId}`, opens Razorpay payment modal with hotel's accent color theme. On success, verifies via `POST onpage-razorpay-response` and redirects to thank-you page. On failure, redirects to `/payment-failure`.
  - Components: `src/utils/paymentGateway.ts`, `src/components/web/BookingDetails/GuestDetails.tsx`
  - APIs: `GET razorpay-orderid/{invoiceId}`, `POST onpage-razorpay-response`

- Feature: Easebuzz Integration
  - Description: Dynamically loads Easebuzz checkout SDK, fetches access key via `GET /easebuzz/access-key/{invoiceId}`, initiates payment. On response, verifies via `POST onpage-easebuzz-response`.
  - Components: `src/utils/paymentGateway.ts`, `src/components/web/BookingDetails/GuestDetails.tsx`
  - APIs: `GET /easebuzz/access-key/{invoiceId}`, `POST onpage-easebuzz-response`

- Feature: Generic Payment Gateway Redirect
  - Description: For hotels not using Razorpay or Easebuzz, redirects to `be-alpha.bookingjini.com/payment/{encodedInvoiceId}/{secureHash}` for external payment processing.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Multiple Payment Modes
  - Description: Supports 4 payment modes configurable per hotel: (1) Prepaid — full amount, (2) Partial Payment — percentage of total, (3) Pay at Hotel — zero online payment, (4) Bank Transfer — configurable amount (percentage, fixed, or first-night). Radio selection with amount display for each mode.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Pay at Hotel
  - Description: Calls `pay-at-hotel-success/{invoiceId}/pay_at_hotel` endpoint which confirms booking without online payment and redirects to confirmation.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

- Feature: Bank Transfer
  - Description: Similar to pay-at-hotel but with `bank-transfer-success/{invoiceId}/pay_at_hotel` endpoint. Booking shows "WAITING FOR PAYMENT" status on thank-you page.
  - Components: `src/components/web/BookingDetails/GuestDetails.tsx`

#### Observations

- Payment provider is determined by `hotelInfo.payment_provider` (razorpay/easebuzz)
- Partial payment percentage comes from `hotelInfo.partial_pay_amt`
- Bank transfer amount calculation supports 3 types: percentage, fixed, first-night

---

### Module: Booking Confirmation (Thank You)

#### Features

- Feature: Thank You Page
  - Description: Split-screen layout — left shows hotel logo, "THANKYOU" message, payment status (confirmed or waiting for bank transfer), and amount paid. Right shows booking details with check-in/out dates, night count, room/package details with images. Supports PDF voucher download.
  - Components: `src/app/thank-you/[invoice_id]/[trxn_id]/page.tsx`
  - APIs: `GET booking-invoice-details/{invoiceId}`, `GET download-invoice-details/{invoiceId}`

- Feature: Invoice PDF Download
  - Description: Fetches base64-encoded PDF from API and provides download link. Also supports print-via-popup-window approach.
  - Components: `src/app/thank-you/[invoice_id]/[trxn_id]/page.tsx`

- Feature: Facebook Pixel Purchase Event
  - Description: Fires Facebook Pixel "Purchase" event with paid amount and currency on the thank-you page.
  - Components: `src/components/common/facebookpixel/pixelevent.tsx`

---

### Module: Booking Voucher / Invoice

#### Features

- Feature: Booking Voucher Page
  - Description: Standalone page at `/booking-voucher/{invoiceId}` that fetches and renders the booking voucher PDF in an iframe. Shows loading spinner and error states.
  - Components: `src/app/booking-voucher/[invoice_id]/page.tsx`
  - APIs: `GET download-invoice-details/{invoiceId}`

---

### Module: Payment Failure Handling

#### Features

- Feature: Payment Failure Page
  - Description: Dedicated `/payment-failure` page with error illustration, "PAYMENT ERROR" message, user-friendly explanation, and "Try again" button that navigates back to the previous page.
  - Components: `src/app/payment-failure/page.tsx`

---

### Module: Package Booking

#### Features

- Feature: Package Listing Page
  - Description: Dedicated `/packages` route showing available hotel packages. Each package card displays package name, image, price, number of nights, inclusions, and availability calendar. Uses separate inventory and cart from room bookings.
  - Components: `src/screens/web/PackageBooking/Home.tsx`, `src/components/web/PackageBooking/PackageCard/`
  - APIs: `GET package_booking/get_package_details-new`, `GET /package_booking/check-package-availability`

- Feature: Package Cart (Footer)
  - Description: Separate footer cart for packages with package-specific pricing, GST calculation, and checkout flow.
  - Components: `src/components/web/PackageBooking/FooterPackageCart/`

- Feature: Package Checkout
  - Description: Dedicated checkout at `/packages/checkout` with package details panel (left) and guest details form (right). Sends `package-bookings/{apiKey}` POST with package-specific payload including package ID, occupancy per package, and check-in date.
  - Components: `src/screens/web/PackageBooking/CheckOut.tsx`, `src/components/web/PackageBooking/PackageDetails/`
  - APIs: `POST package-bookings`

- Feature: Package GST Calculation
  - Description: Separate GST calculation for packages using slab-based or flat tax on package price.
  - Components: `src/utils/gstCalculation.ts`

- Feature: Multi-Property Package Support
  - Description: Fetches hotels by company that have packages via `package/hotels_by_company/{hash}/{companyId}`.
  - Components: `src/api/useHotelsByCompanyWithPackages.ts`
  - APIs: `GET package/hotels_by_company/{hash}/{companyId}`

#### Observations

- Package booking has its own thank-you page at `/packages/thank-you/[invoice_id]/[trxn_id]`
- Package cart is stored separately from room cart in Zustand (`packageCart`)

---

### Module: Day Booking (Day Outing)

#### Features

- Feature: Day Booking Listing Page
  - Description: Dedicated `/day-booking` route for day-use/day-outing packages. Shows available day packages with pricing, availability by date, and occupancy selection.
  - Components: `src/screens/web/DayoutBooking/Home.tsx`, `src/components/web/DayoutBooking/DayoutCard/`
  - APIs: `POST day-outing-package-list-new`, `POST day-outing-package-availability`

- Feature: Day Booking Cart
  - Description: Separate footer cart for day bookings with day-specific pricing and tax calculation.
  - Components: `src/components/web/DayoutBooking/FooterDayoutCart/`

- Feature: Day Booking Checkout
  - Description: Checkout at `/day-booking/checkout` with day outing details and guest form. Sends `day-bookings/{apiKey}` POST. Day bookings default to prepaid payment mode only.
  - Components: `src/screens/web/DayoutBooking/CheckOut.tsx`
  - APIs: `POST day-bookings`

- Feature: Day Booking Menu Customization
  - Description: The navigation label for day booking is customizable via `hotelInfo.day_booking_menu` (e.g., "Day Outing", "Day Use").
  - Components: `src/components/web/Navbar/Navbar.tsx`

- Feature: Multi-Property Day Booking Support
  - Description: Fetches hotels by company that have day bookings via `day-booking/hotels_by_company/{companyId}`.
  - Components: `src/api/useHotelsByCompanyWithdaybook.ts`
  - APIs: `GET day-booking/hotels_by_company/{companyId}`

#### Observations

- Day booking has its own thank-you page at `/day-booking/thank-you/[invoice_id]/[trxn_id]`
- Day booking cart stored separately as `dayoutingCart` in Zustand
- Conditional rendering: only shown if `hotelInfo.is_day_booking_available === 1`

---

### Module: User Authentication (Login / OTP)

#### Features

- Feature: OTP-Based Login
  - Description: Users enter mobile number with country code selector (auto-detected from hotel's country). System calls `user-signin` to verify user exists, then `send-otp` to send OTP. 4-digit OTP input with auto-focus, auto-submit on complete, 59-second resend timer, and "Change Number" option.
  - Components: `src/components/web/Login/Login.tsx`, `src/components/web/OTP/OTPInput.tsx`
  - APIs: `POST user-signin`, `POST send-otp`, `POST verify-otp`

- Feature: Google Sign-In
  - Description: Firebase-based Google OAuth sign-in. On success, calls `user-signin` with the Google email to get auth token. Sets cookie-based session.
  - Components: `src/utils/loginUtils.tsx`, `src/config/firebase.ts`
  - APIs: `POST user-signin`

- Feature: Cookie-Based Session Management
  - Description: Auth token stored as browser cookie with 24-hour expiration. `checkLoginStatus()` checks for cookie presence. Logout clears the cookie.
  - Components: `src/utils/sessionUtils.ts`

- Feature: Phone Number Validation by Country
  - Description: Validates phone number digit count based on the selected country's `phone_number_digit` configuration.
  - Components: `src/components/web/Login/Login.tsx`

- Feature: Auto-Login on Return
  - Description: On page load, checks for existing auth cookie. If found, fetches guest info via `GET guest-info` with Bearer token and auto-populates guest details.
  - Components: `src/screens/Device/Device.tsx`
  - APIs: `GET guest-info`

#### Observations

- Login modal is triggered from the user icon in NavHeader
- Login is optional — guests can book without logging in
- Logged-in users get member-only pricing (`member_only: 1` in inventory request)

---

### Module: My Account / Booking Management

#### Features

- Feature: Booking Tabs (Upcoming, Completed, Cancelled)
  - Description: Three-tab interface showing upcoming, completed, and cancelled bookings. Each tab fetches booking list via `POST guest-booking-list`. Supports month picker filter for completed/cancelled tabs.
  - Components: `src/screens/web/MyAccount/MyAccount.tsx`, `src/components/web/Bookings/`
  - APIs: `POST guest-booking-list`

- Feature: Booking Type Filter
  - Description: Dropdown filter to show All, Room Bookings, Package Bookings, or Day Bookings.
  - Components: `src/screens/web/MyAccount/MyAccount.tsx`

- Feature: Booking Cancellation
  - Description: Users can cancel bookings from their account.
  - APIs: `POST cancell-booking`

- Feature: No Bookings State
  - Description: Shows "No Bookings" illustration when no bookings exist for the selected filter.
  - Components: `src/screens/mobile/Bookings/NoBookings.tsx`

#### Observations

- My Account is accessible at both `/myAccount` and `/my-account` routes
- Mobile has separate booking detail screens

---

### Module: Multi-Property Support

#### Features

- Feature: Property Switch Dropdown
  - Description: When a hotel company has multiple properties, a searchable dropdown appears in the header allowing users to switch between properties. Switching clears cart, paid services, and re-fetches inventory.
  - Components: `src/components/web/Select/PropertySwitchSelect.tsx`, `src/components/web/Header/Header.tsx`
  - APIs: `GET hotel_admin/hotels_by_company/{hash}/{companyId}`

- Feature: Dynamic Property Route
  - Description: Supports `/[propertyName]/property` route for direct property access via URL.
  - Components: `src/app/[propertyName]/property/page.tsx`

- Feature: Property List for Packages & Day Bookings
  - Description: Separate API calls to fetch properties that have packages or day bookings enabled.
  - Components: `src/api/useHotelsByCompanyWithPackages.ts`, `src/api/useHotelsByCompanyWithdaybook.ts`

---

### Module: Offers & Coupons

#### Features

- Feature: Coupon Code Validation
  - Description: Input field on checkout page to enter private coupon codes. Validates via `POST check-private-coupon` with room type IDs, rate plan IDs, and dates. On success, recalculates room prices with discounted rates. Shows applied coupon badge with remove option.
  - Components: `src/components/web/BookingDetails/BookingDetails.tsx`, `src/utils/guestDetails.ts`
  - APIs: `POST check-private-coupon`

- Feature: Offer Notification Modal
  - Description: On page load, fetches HTML offer content from `be-notifications/{hotelId}` and displays it in a centered modal with custom styling. Can be dismissed and re-opened via the bell icon widget on the left side.
  - Components: `src/components/web/OfferModel/OfferModal.tsx`, `src/components/common/OfferWidget/OfferWidget.tsx`
  - APIs: `GET https://be.bookingjini.com/be-notifications/{hotelId}`

#### Observations

- Coupon applies per-room-type with date-specific rate overrides
- Offer modal uses `dangerouslySetInnerHTML` for CMS content (potential XSS risk if not sanitized server-side)

---

### Module: Addon Services (Paid Services)

#### Features

- Feature: Addon Services Modal
  - Description: Grid-based modal showing available paid services with images, descriptions (with tooltip), pricing with tax breakdown, and quantity controls. Services have optional count limits (`service_count`). Users can add/remove quantities before confirming.
  - Components: `src/components/web/AddonServices/AddonServices.tsx`, `src/components/web/FooterCart/AddonServices.tsx`
  - APIs: `POST active-paid-services`

- Feature: Addon in Checkout Summary
  - Description: Added addons appear in the checkout booking details with image, quantity badge, per-unit price, tax calculation, and total. Each addon has a "Remove" option with confirmation modal.
  - Components: `src/components/web/BookingDetails/BookingDetails.tsx`

- Feature: Addon Service Tax Calculation
  - Description: Each addon has its own tax percentage. Tax is calculated as `(service_price * service_tax) / 100` and added to the total.
  - Components: `src/components/web/AddonServices/AddonServices.tsx`

- Feature: Addon Access from Navbar
  - Description: When addon services are available, an "Add-ons" link appears in the navigation bar for quick access.
  - Components: `src/components/web/Navbar/Navbar.tsx`

---

### Module: About Hotel / Content Pages

#### Features

- Feature: About Hotel Tabs
  - Description: Tabbed interface with: About Hotel (description/content), Policies (child, cancellation, T&C, hotel policy), Near By Places (tourist places, tour info), Map (Google Maps embed using lat/long), and How To Reach (air/rail/bus directions).
  - Components: `src/components/web/AboutHotel/AboutHotel.tsx`, `src/components/web/AboutHotel/HotelContent.tsx`, `src/components/web/AboutHotel/Policies.tsx`, `src/components/web/AboutHotel/NearByPlaces.tsx`, `src/components/web/AboutHotel/Locations.tsx`

- Feature: Google Maps Integration
  - Description: Embeds Google Maps iframe using hotel's latitude and longitude coordinates.
  - Components: `src/components/web/AboutHotel/AboutHotel.tsx`

- Feature: Hotel Policies Display
  - Description: Renders hotel policies including child policy, cancellation policy, terms & conditions, and general hotel policy. Used in both About section and checkout T&C modal.
  - Components: `src/components/web/AboutHotel/Policies.tsx`, `src/components/mobile/About/Policies.tsx`

---

### Module: OTA Price Comparison

#### Features

- Feature: OTA Rate Comparison Widget
  - Description: Fixed left-side "Deal of the day" toggle that opens a slide-in panel comparing the hotel's direct booking price against OTA prices (with logos). Fetches data from `POST connected-ota-rates` with hotel ID, dates, and member status. Shows shimmer loading state.
  - Components: `src/components/common/OtaPriceComparition/OtaPrice.tsx`, `src/components/common/AllPageComponent/AllPageComponent.tsx`
  - APIs: `POST connected-ota-rates`

#### Observations

- Encourages direct booking by showing price advantage
- Updates on date change

---

### Module: Chat Widget & WhatsApp Integration

#### Features

- Feature: Tawk.to Chat Integration
  - Description: When `chatOption` is enabled for the hotel, loads Tawk.to live chat widget via script injection. Positioned bottom-right with custom offsets for desktop and mobile.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`

- Feature: WhatsApp Floating Button
  - Description: When WHATSAPP plugin is configured, shows a fixed green WhatsApp icon (bottom-right) that opens WhatsApp chat with the hotel's phone number.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`

- Feature: Interakt WhatsApp Widget
  - Description: Supports Interakt (Kiwi SDK) WhatsApp business integration as a plugin.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`

---

### Module: Analytics & Tracking Plugins

#### Features

- Feature: Plugin System
  - Description: Dynamic plugin loading system that fetches configured plugins from `get-be-plugin-details/{hotelId}`. Supports multiple providers: GTM, GA (Google Analytics), PIXEL (Facebook), CLARITY (Microsoft), INTERAKT, and WHATSAPP.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`
  - APIs: `GET https://be.bookingjini.com/get-be-plugin-details/{hotelId}`

- Feature: Facebook Pixel Events
  - Description: Fires PageView on load, InitiateCheckout on checkout page, and Purchase on thank-you page with amount and currency.
  - Components: `src/components/common/facebookpixel/pixelevent.tsx`

- Feature: Google Tag Manager
  - Description: Injects GTM script and noscript iframe with hotel-specific container ID.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`

- Feature: Google Analytics
  - Description: Injects GA4 config script with hotel-specific measurement ID.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`

- Feature: Microsoft Clarity
  - Description: Injects Clarity tracking script with hotel-specific project ID.
  - Components: `src/components/common/AllPageComponent/AllPageComponent.tsx`

---

### Module: Banner / Promotional Carousel

#### Features

- Feature: Hotel Banner Carousel
  - Description: Auto-playing image carousel at the top of the page (when `hotelInfo.is_banner_active === 1`). Fetches banner images from `get-hotel-banner-list/{hotelId}`. Auto-scrolls past banner after 3 seconds on page load.
  - Components: `src/components/common/Banner/Banner.tsx`
  - APIs: `GET get-hotel-banner-list/{hotelId}`

---

### Module: Navigation & Navbar

#### Features

- Feature: Dynamic Navigation Items
  - Description: Navbar dynamically shows: Home (hotel's home URL), Rooms (customizable label via `room_booking_menu`), Packages (if `package_available`), Day Booking (if `is_day_booking_available`, customizable label), and Add-ons (if services available).
  - Components: `src/components/web/Navbar/Navbar.tsx`

- Feature: OTA Rating Display
  - Description: Shows OTA ratings (e.g., TripAdvisor, Google) with star icons and logos in the navbar area.
  - Components: `src/components/web/Navbar/Navbar.tsx`

- Feature: Sticky Header on Scroll
  - Description: NavHeader becomes sticky with shadow effect when user scrolls down.
  - Components: `src/screens/web/Home/Home.tsx`

- Feature: Currency Display
  - Description: Shows current currency code and symbol in the header.
  - Components: `src/components/web/Header/NavHeader.tsx`

---

### Module: Footer

#### Features

- Feature: Footer with Social Links
  - Description: Dark footer showing copyright, social media links (Facebook, Instagram, Twitter, LinkedIn) from hotel config, payment method icons (Visa, Mastercard, RuPay, Amex, UPI, Netbanking), and "Powered by Bookingjini" with version number.
  - Components: `src/components/web/Footer/Footer.tsx`
  - APIs: `GET get-be-version-data`

- Feature: Dynamic Bottom Margin
  - Description: Footer adds bottom margin when cart is visible to prevent overlap with the sticky footer cart bar.
  - Components: `src/components/web/Footer/Footer.tsx`

---

### Module: SEO & Metadata

#### Features

- Feature: Server-Side Metadata Generation
  - Description: Uses Next.js `generateMetadata` to fetch hotel info server-side and set page title, description, OpenGraph tags, favicon, and robots directives. Supports link previews on social media.
  - Components: `src/app/page.tsx`, `src/components/common/MetaData/index.tsx`

- Feature: Dynamic Document Title
  - Description: Each page (checkout, my account, etc.) updates `document.title` to the hotel name.
  - Components: Various screen components

---

### Module: Currency Management

#### Features

- Feature: Currency Fetching
  - Description: Fetches available currencies from `GET currency` endpoint. Currency symbol is decoded from hex code point (e.g., `0x20B9` → `₹`).
  - Components: `src/api/useCurrency.ts`, `src/screens/Device/Device.tsx`
  - APIs: `GET currency`

---

### Module: Localization (GST / Tax)

#### Features

- Feature: Locale-Based Tax Calculation
  - Description: Fetches locale/tax details from `GET locale-details/{hotelId}`. Supports two tax types: "slab" (range-based) and "flat". Supports two slab modes: "Bundled" (tax on total) and "Individual" (tax excluding extra adult/child). Tax name is dynamic (e.g., "GST", "VAT").
  - Components: `src/utils/addRoomUtils.ts`, `src/utils/gstCalculation.ts`
  - APIs: `GET locale-details/{hotelId}`

- Feature: Child Setup Configuration
  - Description: Fetches child age/pricing setup per hotel from `GET get-child-setup/{hotelId}`.
  - Components: `src/api/useChildSetup.ts`
  - APIs: `GET get-child-setup/{hotelId}`

---

### Module: Error Handling & Fallback UI

#### Features

- Feature: 404 Not Found Page
  - Description: Custom styled 404 page with "4ZERO4" typography and "Go Back" button linking to home.
  - Components: `src/app/not-found.tsx`

- Feature: Loading Spinner
  - Description: CSS-based loading spinner used across the app during data fetching.
  - Components: `src/app/loading.tsx`, `src/screens/Device/loader.module.css`

- Feature: Notification Toasts
  - Description: Mantine Notifications system configured globally (top-right, z-index 2077, max 5, auto-close 4s). Used throughout for success/error/warning messages.
  - Components: `src/app/layout.tsx`

- Feature: Fallback Image
  - Description: When image IDs are missing, falls back to `/error-image.svg`.
  - Components: `src/utils/imageUtils.ts`

---

## 3. Cross-Module Features

- Feature: Global State Management (Zustand)
  - Description: Single Zustand store with ~80+ state variables and setters covering hotel config, dates, occupancy, cart, guest details, UI toggles, and booking state. Persisted to sessionStorage.

- Feature: SWR Data Fetching with Caching
  - Description: All read APIs use SWR with `revalidateIfStale: false, revalidateOnFocus: false, revalidateOnReconnect: false` — effectively caching data for the session lifetime.

- Feature: Theming / White-Label
  - Description: Accent color, header color, and header text color are fetched per-hotel and applied globally via CSS variables, inline styles, and Mantine theme overrides.

- Feature: CDN Image Delivery
  - Description: All hotel/room images served from CloudFront CDN (`d3ki85qs1zca4t.cloudfront.net/bookingEngine/`).

- Feature: Shared Notification System
  - Description: Mantine notifications used consistently across all modules for user feedback.

- Feature: Console Suppression in Production
  - Description: All console methods (log, error, debug, info, warn) are disabled in production builds.

---

## 4. Key User Flows

- Flow 1 (Room Booking): Land on Hotel Page → Select Check-in/Check-out Dates → Set Occupancy → Check Availability → Browse Room Types → Select Meal Plan → Add Room(s) → Adjust Per-Room Occupancy → (Optional) Add Addon Services → Proceed to Checkout → Fill Guest Details → Select Payment Mode → Pay → Thank You Page → Download Voucher

- Flow 2 (Package Booking): Land on Hotel Page → Navigate to Packages → Browse Package Cards → Select Package Date → Add Package with Occupancy → Proceed to Checkout → Fill Guest Details → Pay → Thank You Page

- Flow 3 (Day Booking): Land on Hotel Page → Navigate to Day Booking → Browse Day Outing Packages → Select Date → Add Package → Checkout → Fill Guest Details → Pay (Prepaid Only) → Thank You Page

- Flow 4 (Returning Guest): Land on Hotel Page → Click Login Icon → Enter Mobile → Receive OTP → Verify → Auto-fill Guest Details → Book with Member-Only Pricing

- Flow 5 (Booking Management): Login → Navigate to My Account → View Upcoming/Completed/Cancelled Bookings → Filter by Type/Month → Cancel Booking

- Flow 6 (Coupon Redemption): Add Rooms to Cart → Go to Checkout → Enter Coupon Code → Apply → See Discounted Prices → Complete Booking

---

## 5. Integrations

- Razorpay — Online payment gateway (primary)
- Easebuzz — Online payment gateway (alternative)
- Generic Payment Gateway — Redirect-based fallback payment
- Firebase Authentication — Google Sign-In via Firebase Auth
- Tawk.to — Live chat widget
- Interakt (Kiwi SDK) — WhatsApp business messaging
- WhatsApp Direct — Floating WhatsApp chat button
- Google Tag Manager (GTM) — Tag management
- Google Analytics (GA4) — Web analytics
- Facebook Pixel — Conversion tracking (PageView, InitiateCheckout, Purchase)
- Microsoft Clarity — Session recording and heatmaps
- Google Maps — Embedded map for hotel location
- CloudFront CDN — Image delivery
- Bookingjini Backend API — Core booking engine API (`be-alpha.bookingjini.com`)
- Bookingjini Kernel API — Locale/tax details (`kernel.bookingjini.com`)
- IPFox — IP-based geolocation (`tools.bookingjini.com/ipfox`)

---

## 6. Final Consolidated Feature List

### Search & Discovery

- Domain-based hotel authentication and white-label setup
- Calendar with per-day minimum rate display
- Occupancy selection (rooms, adults, children, infants)
- Check availability with inventory fetching
- Advance booking days enforcement
- Cutoff time handling for same-day bookings
- Alternate date suggestions for sold-out rooms
- Multi-property switching
- OTA price comparison widget

### Room & Rate Management (UI)

- Room type cards with image carousel
- Multiple rate plans per room type
- Multiple occupancy pricing
- Discount/promotion display with strikethrough pricing
- Free cancellation / non-refundable badges
- Room details modal with amenities and description
- Minimum length of stay enforcement
- Per-room occupancy adjustment with extra charges
- Skeleton loading states

### Booking Flow

- Multi-room cart with add/remove
- Footer cart bar with price summary
- Price breakup (room total, discount, tax, service charge, addons)
- Checkout page with booking summary and guest form
- Personal/Business checkout modes
- Minimal checkout mode (fewer required fields)
- Coupon code validation and application
- Terms & conditions acceptance
- Booking loader during payment processing
- Empty cart redirect protection

### Payments

- Razorpay integration
- Easebuzz integration
- Generic payment gateway redirect
- Prepaid (full amount) mode
- Partial payment (percentage) mode
- Pay at hotel (zero online) mode
- Bank transfer mode (percentage/fixed/first-night)
- Payment failure page with retry

### User Management

- OTP-based mobile login
- Google Sign-In (Firebase)
- Cookie-based session (24h expiry)
- Auto-login on return visit
- Guest info auto-fill from profile
- Member-only pricing for logged-in users

### Booking Management (My Account)

- Upcoming bookings tab
- Completed bookings tab
- Cancelled bookings tab
- Booking type filter (Room/Package/Day)
- Month picker filter
- Booking cancellation
- Booking voucher PDF download

### Packages & Day Bookings

- Package listing with availability calendar
- Package cart and checkout
- Package GST calculation
- Day outing listing with date selection
- Day booking cart and checkout
- Customizable menu labels

### Offers & Discounts

- Private coupon code validation
- Offer notification modal (CMS HTML content)
- Offer bell widget for re-opening
- Promotion type badges on rate plans

### Addon Services

- Addon services modal with images and descriptions
- Quantity controls with limits
- Per-addon tax calculation
- Addon management in checkout (add/modify/remove)
- Navbar quick access to addons

### Content & CMS

- About Hotel content tab
- Hotel policies (child, cancellation, T&C, general)
- Near by places / tourist info
- Google Maps embed
- How to reach (air/rail/bus)
- Banner image carousel
- Hotel features display (check-in/out times, couple policy, local ID policy)

### System / UX Features

- Responsive design (separate mobile/desktop component trees)
- Zustand global state with session persistence
- SWR caching for API calls
- White-label theming (accent color, header colors)
- CDN image delivery with fallback
- Dynamic SEO metadata (OpenGraph, favicon)
- Notification toast system
- Custom 404 page
- Loading spinners
- Console suppression in production
- Visitor tracking analytics
- Plugin system (GTM, GA, Pixel, Clarity, Interakt, WhatsApp)
- Tawk.to live chat
- WhatsApp floating button
- Facebook Pixel events (PageView, InitiateCheckout, Purchase)
- IP-based geolocation for country detection

---

## 7. Product Insights

### Missing Features (vs. Modern Booking Engines)

- No multi-language / i18n support — only English
- No multi-currency conversion — currency is fixed per hotel
- No guest reviews/ratings display — HotelReview component exists but is commented out and unused
- No room comparison feature
- No wishlist / save for later
- No email-based login — only mobile OTP and Google
- No social login beyond Google (Apple, Facebook missing)
- No loyalty/rewards program integration
- No dynamic pricing visualization (price trends, "prices are rising" nudges)
- No accessibility features (ARIA labels, keyboard navigation, screen reader support)
- No PWA support (offline, push notifications)
- No booking modification (only cancellation)
- No group booking / event booking support
- No child age input during occupancy selection (only count)
- No room upgrade suggestions
- No cross-sell between room bookings and packages
- No search/filter for room types (by amenity, price range, etc.)
- No date flexibility search ("cheapest dates this month")

### UX Improvements

- Duplicate mobile/desktop codebases should be unified with responsive components — current approach doubles maintenance
- Calendar UX: checkout date selection could auto-open occupancy popover (partially implemented)
- Guest details form could benefit from auto-save/draft functionality
- Coupon error messages should be inline, not just notifications (partially done)
- Payment mode selection UI could show clearer amount breakdowns
- Room card rate plan carousel (4 per slide) is unintuitive — a simple list would be clearer
- The "Add Room" → occupancy adjustment flow has a learning curve; consider a single-step room configuration
- About Hotel tabs could use lazy loading for map iframe
- Offer modal on every page load can be annoying — consider showing once per session
- No breadcrumb on home page; inconsistent breadcrumb patterns across pages

### Performance Improvements

- Session storage is cleared on every page refresh (`beforeunload`) — this forces full re-fetch of all data on every navigation, hurting performance
- AllPageComponent makes 3+ API calls on every page (plugins, offers, OTA rates) — should be cached or fetched once
- OTA price comparison re-fetches on every date change — should debounce
- Room card component is very large (~1700 lines) — should be split into smaller sub-components
- No image lazy loading strategy beyond Next.js defaults
- No code splitting for mobile vs desktop — both bundles may be loaded
- Calendar data fetching could be pre-fetched or cached more aggressively
- `dangerouslySetInnerHTML` used for offer content without client-side sanitization (DOMPurify is installed but not used here)
- Multiple `useEffect` hooks with overlapping concerns in Header component — could be consolidated

### AI & Conversion Optimization Opportunities

- AI-powered dynamic pricing recommendations based on demand patterns
- Personalized room recommendations based on past bookings and preferences
- Smart date suggestions ("Save 20% by shifting your dates by 2 days")
- Chatbot integration for booking assistance (beyond basic Tawk.to)
- Abandoned cart recovery via email/SMS
- A/B testing framework for checkout flow optimization
- Predictive occupancy suggestions based on group size
- Upsell engine for addon services based on booking context
- Price drop alerts for returning visitors
- Review sentiment analysis for room recommendations
- Conversion funnel analytics (search → view → add to cart → checkout → payment → confirmation drop-off rates)
