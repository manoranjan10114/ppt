# Channel Manager Extranet — Module & Feature Analysis

---

## Module: Dashboard

### Features

#### Feature: Hotelier Today Overview

- Description: Displays real-time summary of today's check-ins, check-outs, in-house guests, and available rooms. Provides a snapshot of the property's current operational status with date-type filtering.
- Components: `Dashboard.tsx`, `DashboardData/ConfirmedBookings.tsx`, `DashboardData/FrontOfficeData.tsx`
- APIs: `kernelApi` → `locale-details/{hotel_id}`, `subscriptionApi` → `is-plan-subscribed`, `kernelApi` → `getpromotionalbanner/{hotel_id}`

#### Feature: Room Nights & Revenue Analytics

- Description: Visualizes room-type-wise booked room nights and channel-wise revenue data using Nivo charts. Supports MTD and YTD views for trend analysis.
- Components: `DashboardData/RoomNights.tsx`, `DashboardData/RoomNightsRevenueData.tsx`, `DashboardData/RoomTypesComparison.tsx`, `DashboardData/LineChart.tsx`
- APIs: `beApi` → `hotelier-today-overview-new`, `hotelier-summary-new`, `roomtype-wise-booked-room-nights`, `channel-wise-booked-room-nights-revenue`, `hotelier-today-overview-by-date-type-new`, `hotelier-summary-by-date-type-new`

#### Feature: Recent Bookings Feed

- Description: Shows a live feed of the most recent bookings with guest name, phone, check-in/out dates, channel source, and paid amount.
- Components: `DashboardData/RecentBookings.tsx`
- APIs: `beApi` → `booking-details/{hotel_id}`

#### Feature: Promotional Banner Carousel

- Description: Displays promotional banners from the admin in a Swiper carousel. Supports internal navigation links and external URLs.
- Components: `Dashboard.tsx` (Swiper integration)
- APIs: `kernelApi` → `getpromotionalbanner/{hotel_id}`

#### Feature: Announcement Prompt

- Description: Shows system-wide announcements in a modal overlay. Users can dismiss or acknowledge announcements.
- Components: `layout/Annoucement.tsx`, `views/modal/Modal.tsx`
- APIs: `kernelApi` → `get_extranet_announcement`, `no_show_announcement`

#### Feature: Bookingjini Learning Card

- Description: Displays daily quick tips and blog content to educate hoteliers on platform features and hospitality best practices.
- Components: `DashboardData/BookingjiniLearningCard.tsx`
- APIs: `kernelApi` → `blog`, `extranet-quick-tips/daily-tip`

---

## Module: Bookings

### Features

#### Feature: List View

- Description: Tabular listing of all bookings with filtering by source, status, payment type, date range, and search. Supports pagination, sorting, CSV export, and inline actions (modify, cancel, resend mail, charge card, voucher).
- Components: `Bookings/ListView.tsx`, `Bookings/CancelBooking.tsx`, `Bookings/ModifyBooking.tsx`, `Bookings/ResendMail.tsx`, `Bookings/ChargeCard.tsx`, `Bookings/HotelVoucher.tsx`, `Bookings/BookingUpdate.tsx`, `Bookings/NewBookings.tsx`, `Bookings/FetchBookings.tsx`, `Bookings/BookingRetrieval.tsx`
- APIs: `beApi` → `booking-lists`, `listview-bookings-report`, `cmApi` → `source-list`, `kernelApi` → `extranet-bookings/roomdetails`

#### Feature: CRS View (Calendar Reservation System)

- Description: Calendar-based view of bookings showing room allocations across dates. Allows visual management of room assignments and booking status.
- Components: `Bookings/CrsView.tsx`
- APIs: `beApi` → `crs/crs-booking-details`, `kernelApi` → `extranet-bookings/roomdetails`

#### Feature: Front Office View (GEMS)

- Description: Front-office calendar showing today's check-ins, in-house guests, and check-outs. Available only for GEMS-subscribed properties. Falls back to a non-GEMS or no-front-office view.
- Components: `Bookings/FrontofficeViewGems.tsx`, `Bookings/FrontOfficeViewNoGems.tsx`, `Bookings/NoFrontOfficeView.tsx`
- APIs: `gemsApi` → `frontofficeBookingCalendar`, `isGEMSSubscribed`, `get-subscribed-products`

#### Feature: Check-In

- Description: Multi-step check-in process: view booking details, allocate rooms, capture guest details (from mobile lookup), and complete front-office check-in.
- Components: `Bookings/CheckIn.tsx`, `Bookings/CheckinGuestDetails.tsx`
- APIs: `gemsApi` → `frontOfficeCheckin3`, `kernelApi` → `extranet-bookings/allocateroom`, `getRoomAndGuestDetails`, `guestDetails`

#### Feature: Stay Details & Billing

- Description: Detailed view of an active stay including guest info, room allocation, billing (add expenses/collections/payments), and checkout actions. Supports PDF bill and invoice download.
- Components: `Bookings/StayDetails.tsx`, `Bookings/StayDetailsAddBills.tsx`, `Bookings/InvoiceDownload.tsx`
- APIs: `gemsApi` → `getStayDetailsByBookingID`, `getBillingDetailsByBookingidV2`, `getPaymentModes`, `addExpense`, `addCollection`, `addPayment`, `frontOfficeCheckout`, `generatePDFBill`, `generatePDFInvoice`

#### Feature: Cancel Booking

- Description: Cancellation flow with reason selection. Handles cancellation across CRS and OTA channels.
- Components: `Bookings/CancelBooking.tsx`
- APIs: `beApi` → `crs/crs_cancel_booking`

#### Feature: Modify Booking

- Description: Allows modification of guest details and booking parameters for existing reservations.
- Components: `Bookings/ModifyBooking.tsx`
- APIs: `beApi` → `crs/crs-modified-guest-details`, `crs/crs_modify_bookings-new`

#### Feature: Reservation (New Booking)

- Description: Multi-step reservation wizard: select dates → choose room types → enter guest info → select payment method → confirmation. Manages state through Redux reservation slice.
- Components: `Bookings/Reservation/Reservation.tsx`, `Reservation/RoomType.tsx`, `Reservation/SelectRoomType.tsx`, `Reservation/GuestInfo.tsx`, `Reservation/PaymentSelect.tsx`, `Reservation/NewReservation.tsx`, `Reservation/ReservationSummary.tsx`, `Reservation/ReservationSuccess.tsx`
- APIs: `beApi` → `bookingEngine/get-inventory`, `total-rooms-by-room-type`, `manage_inventory/get_room_rates_by_hotel`, `user/register`, `bookingEngine/bookings`

#### Feature: Walk-In Booking

- Description: Quick walk-in booking flow: check room availability floor-wise, capture guest info, process billing, and complete front-office check-in.
- Components: `Bookings/Walkin/Walkin.tsx`, `Walkin/Availability.tsx`, `Walkin/GuestInfo.tsx`, `Walkin/Billing.tsx`
- APIs: `kernelApi` → `getAvailableRoomsFloorwiseForWalkin`, `gemsApi` → `frontOfficeCheckin5`, `gems-booking`, `beApi` → `user/register`, `bookingEngine/bookings`, `manage_inventory/get_room_rates_by_hotel`

#### Feature: Booking Messaging

- Description: In-app messaging for OTA bookings. Supports Expedia message threads and general booking messaging with file attachments.
- Components: `Bookings/BookingMessaging/MessagingSlider.tsx`, `BookingMessaging/ExpediaMessagingSlider.tsx`
- APIs: `cmApi` → `expedia-messaging/get-message-thread`, `expedia-messaging/post-message`, `expedia-messaging/upload-attachement`, `expedia-messaging/get-message-by-message-id`

#### Feature: Charge Card & Payment Status

- Description: Allows charging a guest's card for a booking and displays the payment processing status page.
- Components: `Bookings/ChargeCard.tsx`, `Bookings/ChargeCardStatusPage.tsx`
- APIs: `beApi` → payment gateway endpoints (Easebuzz/Razorpay response handlers)

---

## Module: Inventory

### Features

#### Feature: Room-Type Inventory View

- Description: Calendar grid showing inventory (available rooms) per OTA channel for a selected room type and date range. Supports inline editing of room counts and LOS (Length of Stay) values directly in the calendar cells.
- Components: `Inventory/Inventory.tsx`, `Inventory/InventoryRedirect.tsx`
- APIs: `cmApi` → `inventory/get-inventory/{hotel_id}/{from}/{to}/{room_type_id}`, `kernelApi` → `hotel_master_room_type/all/{hotel_id}`, `cmApi` → `all-otas-by-room-type/{hotel_id}`

#### Feature: Channel-Type Inventory View

- Description: Alternative inventory view organized by channel/OTA rather than room type. Shows inventory across all room types for a specific channel.
- Components: `Inventory/viewInvChannelType/ViewInvChannelType.tsx`, `ConfirmChannelTypeInvUpdate.tsx`, `ChannelTypeBlockInv.tsx`, `ChannelTypeUnBlockInv.tsx`, `ChannelTypeSyncInv.tsx`, `ChannelTypePropertySoldout.tsx`, `ChannelTypeSoldoutDates.tsx`, `OutOfOrderChannelView.tsx`
- APIs: `cmApi` → `inventory/get-inventory-channelwise-new`, `inventory/room-wise-calendar-update`, `inventory/calendar-inventory-update`

#### Feature: Bulk Inventory Update

- Description: Update inventory for a date range across selected OTAs in bulk. Separate handling for BE (Booking Engine) and CM (Channel Manager) updates.
- Components: `Inventory/UpdateInventory.tsx`, `Inventory/ConfirmUpdateInventory.tsx`
- APIs: `beApi` → `manage_inventory/update-inv`, `cmApi` → `inventory/bulk-inventory-update-new`, `inventory/individual-inventory-update`

#### Feature: Block/Unblock Inventory

- Description: Block or unblock room availability for specific dates or date ranges. Supports both individual date and bulk date-range operations for BE and CM channels.
- Components: `Inventory/BlockInventory.tsx`, `Inventory/UnblockInventory.tsx`
- APIs: `beApi` → `block-specific-dates`, `cmApi` → `inventory/bulk-inventory-block`, `inventory/individual-inventory-block`, `inventory/bulk-inventory-unblock`, `inventory/individual-inventory-unblock`

#### Feature: Sync Inventory

- Description: Synchronizes inventory data between the Channel Manager and connected OTAs to ensure consistency across all distribution channels.
- Components: `Inventory/SyncInventory.tsx`
- APIs: `cmApi` → `inventory/sync-inventory-new`

#### Feature: Property Soldout / Soldout Dates

- Description: Mark the entire property as sold out or view/manage specific sold-out dates. Supports blocking/unblocking the entire property.
- Components: `Inventory/PropertySoldout.tsx`, `Inventory/SoldoutDates.tsx`
- APIs: `beApi` → `block-property`, `unblock-property`, `cmApi` → `block_specific_property`, `unblock_specific_property`, `get-block-dates`

#### Feature: Out of Order Rooms

- Description: Mark specific rooms as out of order (maintenance, renovation, etc.) to remove them from available inventory without blocking the room type entirely.
- Components: `Inventory/OutOfOrder.tsx`
- APIs: `cmApi` → `inventory/out-of-order`, `increase-decrease-inventory-reasons`, `inventory/inventory-increase-decrease`

#### Feature: Available Till

- Description: Shows the last available inventory and rate dates, helping hoteliers understand how far into the future their inventory is configured.
- Components: `Inventory/AvailableTill.tsx`
- APIs: `cmApi` → `get-last-available-inv-dates`, `get-last-available-rate-dates`

---

## Module: Rates

### Features

#### Feature: Room-Type Rate View

- Description: Calendar grid displaying rates per OTA channel for a selected room type and date range. Supports inline editing of rate values and LOS directly in calendar cells.
- Components: `Rates/Rate.tsx`, `Rates/RatesRedirect.tsx`, `Rates/RatesTill.tsx`, `Rates/ViewMaxOccupancySlider.tsx`
- APIs: `cmApi` → `rates/get-rates`, `kernelApi` → `hotel_master_room_type/all/{hotel_id}`, `master_hotel_rate_plan/room_rate_plan_by_room_type`

#### Feature: Channel-Wise Rate View

- Description: Alternative rate view organized by channel showing rates across all room types for a specific OTA. Supports calendar-based rate editing per channel.
- Components: `Rates/RoomTypeRates.tsx`, `RoomTypeRates/ConfirmNewRates.tsx`, `RoomTypeRates/BlockUpdates.tsx`, `RoomTypeRates/BulkUpdate.tsx`, `RoomTypeRates/SyncUpdate.tsx`, `RoomTypeRates/UnBlockUpdate.tsx`
- APIs: `cmApi` → `rates/getrates-channelwise`, `rates/channel-wise-calendar-update`, `rates/room-wise-calendar-update`

#### Feature: Bulk Rate Update

- Description: Update rates for a date range across selected channels. Handles BE and CM rate updates separately with confirmation step.
- Components: `Rates/BulkUpdate.tsx`, `Rates/ConfirmUpdateRate.tsx`
- APIs: `beApi` → `rates/room_rate_update_new`, `cmApi` → `manage_inventory/room_rate_update`, `rates/individual-rate-update`

#### Feature: Block/Unblock Rates

- Description: Block or unblock rate availability for specific dates or date ranges across BE and CM channels.
- Components: `Rates/BlockUpdate.tsx`, `Rates/UnBlockUpdate.tsx`
- APIs: `beApi` → `block-rate-specific-dates`, `cmApi` → `rates/individual-rate-block-new`, `rates/bulk-rate-block-new`, `rates/individual-rate-unblock-new`, `rates/bulk-rate-unblock-new`, `beApi` → `unblock-rate-range`

#### Feature: Sync Rates

- Description: Synchronizes rate data between the Channel Manager and connected OTAs.
- Components: `Rates/SyncUpdate.tsx`
- APIs: `cmApi` → `rates/sync-rate-new`

#### Feature: Confirm Calendar Rate Update

- Description: Calendar-based rate editing with a confirmation step before applying changes. Shows before/after comparison of rate modifications.
- Components: `Rates/ConfirmUpdateRate.tsx`
- APIs: `cmApi` → `rates/calendar-rate-update`

#### Feature: LOS (Length of Stay) Update

- Description: Update minimum length-of-stay restrictions for rates across date ranges for both BE and CM channels.
- Components: Integrated within Rate views
- APIs: `cmApi` → `rates/bulk-los-update`, `rates/individual-los-update`

---

## Module: ManageChannel

### Features

#### Feature: Channel Overview

- Description: Displays all connected OTA channels and the Booking Engine with their status. Provides entry points to configure each channel. Includes OTP-based access verification for security.
- Components: `ManageChannel/ManageChannel.tsx`, `ManageChannel/AddChannelSlider.tsx`, `ManageChannel/OTPModel.tsx`
- APIs: `cmApi` → `get_added_channels`, `getallchannel`, `kernelApi` → `sendotpbyemail`, `verifyotp`

### Sub-Module: Booking Engine

#### Feature: Basic Setup (Logo, Banner, Theme, URL, Other Settings)

- Description: Configure the direct booking engine's visual identity — upload logo/banners with drag-and-drop reordering, set theme colors, manage booking engine URL, and configure other settings like bank details and payment settings.
- Components: `BookingEngine/BasicSetup/BasicSetup.tsx`, `BasicSetup/Logo.tsx`, `BasicSetup/Banner.tsx`, `BasicSetup/DraggableImages.tsx`, `BasicSetup/ImageViewAreaBanner.tsx`, `BasicSetup/ThemeSettings.tsx`, `BasicSetup/ManageURLsSettings.tsx`, `BasicSetup/OtherSettings.tsx`, `BasicSetup/VosaSetting.tsx`, `OtherSettings/BankDetails.tsx`, `OtherSettings/BePaymentSetting.tsx`, `OtherSettings/OtherSettingDetails.tsx`, `OtherSettings/RatingsCard.tsx`
- APIs: `kernelApi` → `company_profile/get`, `company_profile/get-logo`, `/upload`, `company_profile/booking_page`, `deleteImage`, `beApi` → `manage-url`, `manage-url/update`, `other-settings`, `other-settings/update`, `theme-setup`, `theme-setup/update`, `fetch-other-settings`, `update-other-settings`, `update-banner-order`

#### Feature: Payment Options

- Description: Configure payment gateways (Razorpay, Easebuzz, PayU, etc.) for the booking engine. Add, setup, and manage multiple payment modes.
- Components: `BookingEngine/PaymentOptions/PaymentOptions.tsx`, `PaymentOptions/AddPayment.tsx`, `PaymentOptions/SetupPayment.tsx`, `PaymentOptions/PayUSetupPayment.tsx`, `PaymentOptions/CheckHavingPayU.tsx`
- APIs: `beApi` → `get-payment-gateways`, `update-paymentgateway-setup`, `select-paymentgateway-setup`

#### Feature: Promotions (BE-specific)

- Description: Manage booking engine promotions including basic coupons, early bird, last minute, same-day, and length-of-stay promotions. Each type has add/modify/view/deactivate flows with active and inactive lists.
- Components: `BookingEngine/Promotion/Promotion.tsx`, `Promotion/Coupon/Coupon.tsx`, `Promotion/EarlyBird/EarlyBird.tsx`, `Promotion/Belastmin/BeLastminute.tsx`, `Promotion/SameDay/BeSameDayPromotion.tsx`, `Promotion/LengthOfStay/BeLengthOfStayPromotion.tsx` (plus Add/Modify/Inactive variants for each)
- APIs: `cmApi` → `get-all-promotion`, `insert-hotel-promotion-new`, `deactivate-promotion-new`, `activate-promotion-new`, `get-hotel-promotion`, `update-hotel-promotion-new`, `get-all-inactive-promotion`, `get_room_type_rate_plans`, `advance-days`

#### Feature: Coupons (Private & Public)

- Description: Create and manage discount coupons for the booking engine. Supports private coupons (code-based) and public/private coupon management with multi-hotel and single-hotel scoping.
- Components: `BookingEngine/Coupon/BePrivateCoupon.tsx`, `Coupon/AddPrivateCoupon.tsx`, `Coupon/ModifyBePrivateCoupon.tsx`, `Coupon/BeInactiveCoupon.tsx`, `PublicPrivateCoupon/PublicPrivateCoupon.tsx`, `PublicPrivateCoupon/OneHotelCoupon.tsx`, `PublicPrivateCoupon/MultiHotelCoupon.tsx`, `PublicPrivateCoupon/EditCoupon.tsx`
- APIs: `beApi` → `coupons/get`, `coupons/add`, `coupons/update`, `coupons/fetch`, `coupons`, `coupons/list/type`

#### Feature: Paid Services

- Description: Manage add-on paid services offered through the booking engine (e.g., airport pickup, spa). Includes service listing, add/update/delete, and revenue reports with Excel export.
- Components: `BookingEngine/PaidServices/Paidservice.tsx`, `PaidServices/PaidServiceReports.tsx`
- APIs: `beApi` → `paid_services/list`, `paid_services/add`, `paid_services/update`, `paid_services/delete`, `paid_services/report`, `paid-services-report/excel`

#### Feature: Package List

- Description: Create and manage room packages with custom pricing, inclusions, and availability. Supports add, edit, and delete operations.
- Components: `BookingEngine/PackageList/PackageList.tsx`, `PackageList/AddPackage.tsx`, `PackageList/EditPackage.tsx`
- APIs: `kernelApi` → `hotel_other_information`, `hotel_other_information/update`

#### Feature: Day Outing Packages

- Description: Full day-outing/day-booking management: create packages with menus, manage blackout dates, special pricing, availability calendar, booking list, and day-outing-specific promotions.
- Components: `BookingEngine/DayOutingPackage/DayOutingPackage.tsx`, `DayOutingPackage/AddDayOuting.tsx`, `DayOutingPackage/EditDayOuting.tsx`, `DayOutingPackage/DayOutingCalender.tsx`, `DayOutingPackage/DayBookingList.tsx`, `DayOutingPackage/DayBookingDrawer.tsx`, `DayOutingPackage/BlackoutDatesTab.tsx`, `DayOutingPackage/SpecialPriceTab.tsx`, `DayOutingPackage/DayOutingDetialsTab.tsx`, `DayOutingPackage/dayBookingSetting.tsx`, `Promotion/DayoutingPromotion.tsx`, `Promotion/DayBookingCoupon.tsx`
- APIs: `beApi` → `day-booking/active-packages`, `day-booking/package-add`, `day-booking/package-update`, `day-booking/package-details`, `day-booking/package-status`, `day-booking/update-special-price`, `day-booking/update-blackout-dates`, `day-booking/availability-calendar`, `day-booking/booking-list`, `day-booking/booking-details`, `day-booking/menu-setup`, `day-booking/image-delete`

#### Feature: Additional Charges

- Description: Configure additional charges (e.g., extra bed, early check-in) that can be applied to bookings through the booking engine.
- Components: `BookingEngine/AdditionalCharges/AdditionalCharges.tsx`, `AdditionalCharges/AddNewAddonCharges.tsx`, `AdditionalCharges/EditAddonCharges.tsx`
- APIs: `kernelApi` → `select-all-charges`, `add-charges`, `update-charges`, `select-charges`, `delete-charges`, `active-charges`

#### Feature: Taxes

- Description: Configure tax settings for the booking engine based on locale and property type.
- Components: `BookingEngine/Taxes/Taxes.tsx`
- APIs: `kernelApi` → `locale-details/{hotel_id}`

#### Feature: Room Rate Plan

- Description: Manage room-rate plan mappings for the booking engine, controlling which rate plans are visible and active per room type.
- Components: `BookingEngine/RoomRatePlan/RoomRatePlan.tsx`
- APIs: `kernelApi` → `master_hotel_rate_plan/all`, `master_hotel_rate_plan/update-status-for-be`

#### Feature: Notification Popup

- Description: Configure notification popups displayed on the booking engine to guests (e.g., special offers, announcements).
- Components: `BookingEngine/NotificatrionPopUp/NotificationPopup.tsx`
- APIs: `beApi` → `be-notifications`, `be-notifications-popup`

#### Feature: Promotional Slider

- Description: Upload and manage promotional slider images displayed on the booking engine homepage.
- Components: `BookingEngine/PromotionalSlider/PromotionSlider.tsx`, `PromotionalSlider/ImageViewAreaPromotion.tsx`
- APIs: `kernelApi` → `fetch-be-notification-slider-images`, `upload-be-notification-slider-images`, `delete-be-notification-slider-image`

#### Feature: Nearest Locations & Places

- Description: Configure nearby transportation hubs (airport, bus, railway) and points of interest displayed on the booking engine.
- Components: `BookingEngine/Location/NearestLocation.tsx`, `Location/AirportWay.tsx`, `Location/BusWay.tsx`, `Location/RailwayWay.tsx`, `BookingEngine/Places/NearestPlaces.tsx`
- APIs: `kernelApi` → `hotel_other_information`, `hotel_other_information/update`

#### Feature: Quick Payment Link

- Description: Generate and manage quick payment links and generic payment links for bookings. Supports resending emails and checking payment status.
- Components: `BookingEngine/QuickPaymentLink/QuickPaymentLink.tsx`, `QuickPaymentLink/AddNewLink.tsx`, `QuickPaymentLink/GenericPaymentLink.tsx`, `QuickPaymentLink/ViewPayment.tsx`, `QuickPaymentLink/ViewGenericPayment.tsx`
- APIs: `beApi` → `reservation-payment-link/all`, `reservation-payment-link/resend-email`, `reservation-payment-link/check`, `generic-payment-link/add`, `generic-payment-link/resend-email`, `generic-payment-link/check`, `quick-payment-link`

#### Feature: Unpaid Bookings

- Description: View and manage bookings with pending payments. Allows sending payment reminders.
- Components: `BookingEngine/UnpaidBookings/UnpaidBookings.tsx`, `UnpaidBookings/UnpaidBookingSlider.tsx`
- APIs: `beApi` → `beBookingsEndPoint`, `quickPaymentLinkEndPoint`

#### Feature: Cancellation Policy

- Description: Create and manage date-range-based cancellation policies for the booking engine with default policy marking.
- Components: `BookingEngine/CancellationPolicy/CancellationPolicy.tsx`, `CancellationPolicy/AddCancellationPolicy.tsx`
- APIs: `beApi` → `cancellation_policy/date-range/add`, `cancellation_policy/date-range/update`, `cancellation_policy/date-range/list`, `cancellation_policy/date-range/delete`, `cancellation_policy/date-range/status`

#### Feature: Commissions / Transactions

- Description: View Bookingjini payment gateway commission details and payout history with detailed booking-level breakdowns.
- Components: `BookingEngine/Commisions/BookingjiniPaymentGatewayCommisions.tsx`, `Commisions/BookingsDetailedView.tsx`, `Commisions/CommisionColumns.tsx`, `Commisions/CommsionPayoutColumns.tsx`, `Commisions/PayoutBookingsColumns.tsx`
- APIs: `kernelApi` → `commission-list`

#### Feature: Instant Booking Widget

- Description: Configure an embeddable instant booking widget for external websites.
- Components: `BookingEngine/InstantBookingWidget/InstantBookingWidget.tsx`
- APIs: `beApi` → `instant-booking/setup`

#### Feature: Website Widget

- Description: Configure and manage a website widget that can be embedded on the hotel's website for direct bookings.
- Components: `BookingEngine/WesiteWidget/WebsiteWidget.tsx`
- APIs: `beApi` → `website-widget/fetch`, `website-widget/save`, `website-widget/status`

#### Feature: Plugins

- Description: Manage third-party plugins integrated with the booking engine (e.g., analytics, chat widgets).
- Components: `BookingEngine/Plugins/BookingEnginePlugins.tsx`, `Plugins/AddPlugInData.tsx`, `Plugins/LabelDisplay.tsx`
- APIs: `kernelApi` → `fetch-plugin`, `store-plugin`

### Sub-Module: OTA Management (OtherOTA)

#### Feature: OTA Basic Setup

- Description: Configure OTA connection details including hotel ID mapping, API credentials, and toggle OTA active/inactive status.
- Components: `OtherOTA/OtaManage.tsx`, `OtherOTA/OtaBasicSetup.tsx`, `OtherOTA/OtaPaymentLink.tsx`
- APIs: `cmApi` → `cm_ota_hotel_details`, `add-ota-hotel-details`, `update-ota-hotel-details`, `cm_ota_details/add`, `cm_ota_details/update`, `cm_ota_details/toggle`, `cm_ota_details/sync`, `get_unmapped_room_types_n_last_sync`, `cm_ota_details/update-ota-payment-link`

#### Feature: Room Sync (OTA ↔ CM)

- Description: Map hotel room types to OTA room types for inventory synchronization. Supports add, edit, and delete room mappings.
- Components: `OtherOTA/RoomSync/RoomSync.tsx`, `RoomSync/AddRoomSyncSlider.tsx`, `RoomSync/EditRoomSyncSlider.tsx`
- APIs: `cmApi` → `cm_ota_roomtype_sync_new/add`, `cm_ota_rateplan_sync_new/rooms`, `cm_ota_roomtype_sync/ota_room_type`, `/cm_ota_roomtype_sync`, `/cm_ota_roomtype_sync/update`, `/cm_ota_roomtype_sync/add`

#### Feature: Rate Sync (OTA ↔ CM)

- Description: Map hotel rate plans to OTA rate plans for rate synchronization. Supports add, edit, and delete rate plan mappings.
- Components: `OtherOTA/RateSync/RateSync.tsx`, `RateSync/AddRateSyncSlider.tsx`, `RateSync/EditRateSyncSlider.tsx`
- APIs: `cmApi` → `cm_ota_rateplan_sync_new/ota_room_type`, `cm_ota_rateplan_sync_new/add`, `cm_ota_rateplan_sync`, `cm_ota_roomtype_sync/ota_rate_plan`, `master_hotel_rate_plan/rate_plan_by_room_type`

#### Feature: Airbnb OAuth Integration

- Description: Handles Airbnb OAuth code callback for connecting Airbnb listings to the channel manager.
- Components: `OtherOTA/AirBnbCodeGet.tsx`
- APIs: `kernelApi` → `air-bnb/save-code`, Airbnb OAuth URL (`airbnb.com/oauth2/auth`)

### Sub-Module: PMS Management

#### Feature: PMS Integration

- Description: Configure PMS (Property Management System) integration including hotel code setup, booking push URL, room mapping, and rate plan mapping between PMS and CM.
- Components: `PMSManage/PMSManage.tsx`, `PMSBasicSetup/PMSBasicSetup.tsx`, `RoomMapping/RoomMapping.tsx`, `RoomMapping/AddPMSRoomType.tsx`, `RateMapping/RateMapping.tsx`, `RateMapping/AddPMSRatePlan.tsx`
- APIs: `kernelApi` → `cm_pms_details/get_pms_hotel_code`, `cm_pms_details/get_pms_booking_push_url`, `cm_pms_details/update_pms_booking_push_url`, `cm_pms_details/get_pms_rooms`, `cm_pms_details/add_pms_room`, `cm_pms_details/save_pms_room_sync`, `cm_pms_details/get_pms_room_sync_all`, `cm_pms_details/remove_pms_room_sync`, `cm_pms_details/get_pms_rateplans`, `cm_pms_details/get_pms_rateplan_sync_all`, `cm_pms_details/save_pms_rateplan_sync`, `cm_pms_details/add_pms_rateplan`, `cm_pms_details/remove_pms_rateplan_sync`, `fetch_pms_rooms`

### Sub-Module: Agoda Onboarding

#### Feature: Agoda Channel Setup

- Description: Dedicated onboarding flow for Agoda: basic setup with hotel details, room mapping with amenities, rate plan mapping, and contract signing. Lazy-loaded for performance.
- Components: `agodaOnBoarding/agodaSetup/AgodaSetup.tsx`, `agodaBasicSetup/AgodaOnBoarding.tsx`, `agodaBasicSetup/AgodaBasicSetup.tsx`, `agodaBasicSetup/AgodaCancelPolicySlider.tsx`, `agodaBasicSetup/AgodaServicesSlider.tsx`, `agodaRoomMapping/AgodaRoomMapping.tsx`, `agodaRoomMapping/CreateAgodaRoomplan.tsx`, `agodaRoomMapping/AgodaAmenitiesSlider.tsx`, `agodaRateMapping/AgodaRateMapping.tsx`, `agodaRateMapping/CreateAgodaRateplan.tsx`
- APIs: `cm3Api` → `gethoteldetails`, `onboard`, `getroomtype`, `getroomdetails`, `createroom`, `getrateplantype`, `getrateplan`, `createrateplan`, `getmapedroom`, `createhotelproduct`, `createdroom`, `cancelpolicyforform`, `iscontractsigned`

---

## Module: PropertySetup

### Features

#### Feature: Property Basic Details

- Description: View and edit core property information: name, address, contact details, country/state selection, and property address with map integration.
- Components: `PropertySetup/PropertyBasicDetails.tsx`, `PropertySetup/PropertyBasicSetupAddMailSlider.tsx`, `PropertySetup/PropertyBasicSetupAddMobileSlider.tsx`
- APIs: `kernelApi` → `hotel_admin/get_all_hotels_by_id/{hotel_id}`, `update_new_property/{hotel_id}`, `update_property_address/{hotel_id}`, `hotelkite-countrydetails/getall`, `getAllCountries`, `statedetails/getById/{country_id}`

#### Feature: Room Types Management

- Description: View, add, edit, and delete room types. Configure room type details, amenities, occupancy, rate plans, images (with reordering), and min/max pricing.
- Components: `PropertySetup/PropertyRoomTypes.tsx`, `PropertySetup/AddNewRoomTypeSlider.tsx`
- APIs: `kernelApi` → `hotel_master_room_type/all/{hotel_id}`, `getRatePlansByRoomtypeID`, `getAllRoomtypeamenities`, `getRoomtypeRateplans`, `update-basic-detail`, `update-room-type-amenities`, `hotel_master_new_room_type/upload_room_type_images/{hotel_id}`, `update-occupancy`, `hotel_master_new_room_type/delete/{id}`, `hotel_master_new_room_type/delete_single_roomtype_image/{id}`, `update-room-rate-plan`, `update-min-max-price`, `hotel_master_new_room_type/add`, `update-room-type-image-order`, `enum-type/bookable-name`

#### Feature: Floors Management

- Description: View and manage property floors. Add new floors and configure serviceable floors.
- Components: `PropertySetup/PropertyFloors.tsx`
- APIs: `kernelApi` → `getFloors`, `saveFloors`

#### Feature: Rooms Management

- Description: View rooms organized by floor, add/edit/delete individual rooms, and update room status (active/inactive/maintenance).
- Components: `PropertySetup/PropertyRooms.tsx`
- APIs: `kernelApi` → `getRooms2`, `addRoom`, `DeleteRoom`, `UpdateRoom`, `UpdateRoomStatus`

#### Feature: Property Amenities

- Description: Manage property-level amenities organized by category. Add/remove amenities from the property profile.
- Components: `PropertySetup/PropertyAmenities.tsx`, `PropertySetup/AmenitiesCategoryComp.tsx`
- APIs: `kernelApi` → `getAllPropertyAmenities`, `hotel_amenities/hotelAmenity/{hotel_id}`, `remove_property_amenities`, `update_property_amenities`

#### Feature: Property Images

- Description: Upload, delete, and reorder property images with drag-and-drop support.
- Components: `PropertySetup/PropertyImages.tsx`
- APIs: `kernelApi` → `get_Images/{hotel_id}`, `uploadhotelimages`, `/file/managePropertyPhotos/remove`, `update-image-order`

#### Feature: Terms & Policy

- Description: View and edit hotel terms and policies (check-in/out times, cancellation policy text, child policy, pet policy, etc.).
- Components: `PropertySetup/PropertyTermsPolicy.tsx`
- APIs: `kernelApi` → `hotel_policies/{hotel_id}`, `hotel_policies/update`

#### Feature: Cancellation Rules

- Description: Configure cancellation rules with different policies for different booking windows.
- Components: `PropertySetup/PropertyCancellationRules.tsx`, `PropertySetup/PropertyAddCancellationRules.tsx`, `PropertySetup/PropertyVeiwCancellation.tsx`
- APIs: `kernelApi` → `be_cancellation_policy/fetch_cancellation_policy/{hotel_id}`, `be_cancellation_policy/update_cancellation_policy`

#### Feature: Financial Details

- Description: View and manage property bank account details for payment settlements.
- Components: `PropertySetup/PropertyFinancialDetails.tsx`
- APIs: `kernelApi` → `hotel_bank_account_details/{hotel_id}`

#### Feature: Locale Info

- Description: View property locale settings including currency, timezone, and tax configuration.
- Components: `PropertySetup/LocaleInfo.tsx`
- APIs: `kernelApi` → `locale-details/{hotel_id}`

#### Feature: Rate Plans

- Description: Create, edit, and delete master rate plans. Manage rate plan visibility for the booking engine.
- Components: `PropertySetup/RatePlans/RatePlan.tsx`, `RatePlans/AddRatePlan.tsx`, `RatePlans/EditRatePlan.tsx`
- APIs: `kernelApi` → `master-rate-plan-details`, `master_rate_plan/all/{hotel_id}`, `master_rate_plan/add`, `master_rate_plan/update/{id}`, `master_rate_plan/{id}`, `master_hotel_rate_plan/update-status-for-be`

#### Feature: Derived Rate Plans

- Description: Create derived rate plans that automatically calculate rates based on a parent rate plan with percentage or fixed adjustments per room type.
- Components: `PropertySetup/DerivedRatePlan/DerivedRatePlan.tsx`, `DerivedRatePlan/DerivedPlanRoomType.tsx`, `DerivedRatePlan/AddDerivedPlanRoomType.tsx`, `DerivedRatePlan/DerivedPlanSlider.tsx`
- APIs: `kernelApi` → rate plan APIs (derived from master rate plan endpoints)

#### Feature: Seasonal Plans

- Description: Configure seasonal pricing plans with different rates for different seasons/periods. Supports room-wise rate setup per seasonal plan.
- Components: `PropertySetup/PropertySeasonalPLan.tsx`, `PropertySetup/SetupSeasonalPlan.tsx`
- APIs: `cmApi` → `get_added_channels_for_seasonal_plan`

#### Feature: Dynamic Pricing

- Description: Set up occupancy-based dynamic pricing rules that automatically adjust rates based on room availability thresholds.
- Components: `PropertySetup/DynamicPricing/DynamicPricingSetup.tsx`, `DynamicPricing/AddDynamicPricing.tsx`, `DynamicPricing/EditDynamicPricing.tsx`
- APIs: `kernelApi` → `dynamic-pricing/all/{hotel_id}`, `dynamic-pricing/add`, `dynamic-pricing/update`, `dynamic-pricing/delete/{id}`, `dynamic-pricing/one/{id}`, `hotel_master_room_type/room_types/{hotel_id}`

#### Feature: One-Click OTA Setup

- Description: Automated OTA onboarding that fetches hotel details, room types, rates, and images from an existing OTA listing and imports them into the system.
- Components: `PropertySetup/OneClickSetupOTA/OneClickSetupOta.tsx`, `OneClickSetupOTA/ValidateProperty.tsx`, `OneClickSetupOTA/PropertyConfirmation.tsx`, `OneClickSetupOTA/FinalOneClickSetup.tsx`
- APIs: `kernelApi` → `one-click-setup/isoneclicksetupallow/{hotel_id}`, `one-click-setup/validatehotelinfo/`, `one-click-setup/fetchhoteldetails`, `one-click-setup/storehoteldetails`, `one-click-setup/fetchroomdetails`, `one-click-setup/storeroomdetails`, `one-click-setup/fetchratedetails`, `one-click-setup/storeratedetails`, `one-click-setup/fetchphotodetails`, `one-click-setup/storephotodetails`

---

## Module: Promotions (OTA-level)

### Features

#### Feature: Basic Promotion

- Description: Create percentage or fixed-amount discount promotions applicable across selected OTA channels and room types. Supports blackout dates, date range targeting, and active/inactive management.
- Components: `Promotions/BasicPromotion/BasicPromotion.tsx`, `BasicPromotion/AddBasicPromotion.tsx`, `BasicPromotion/ModifyBasicPromotion.tsx`, `BasicPromotion/ViewBasicPromotion.tsx`, `BasicPromotion/InactiveBasicPromotion.tsx`, `BasicPromotion/SeeMoreBlackOutSlider.tsx`
- APIs: `cmApi` → `get-all-promotion`, `insert-hotel-promotion-new`, `deactivate-promotion-new`, `activate-promotion-new`, `get-hotel-promotion`, `update-hotel-promotion-new`, `view-promotion`, `get-all-inactive-promotion`, `get-promotion-ota`, `get_room_type_rate_plans`

#### Feature: Early Bird Promotion

- Description: Advance booking discounts — guests who book X days in advance get a discount. Configurable advance days, discount type, and applicable channels.
- Components: `Promotions/EarlyBird/EarlyBird.tsx`, `EarlyBird/AddEarlyBirdPromotion.tsx`, `EarlyBird/ModifyEarlyBirdPromotions.tsx`, `EarlyBird/ViewEarlyBirdPromotions.tsx`, `EarlyBird/InactiveEarlyBirdPromotion.tsx`, `EarlyBird/SeeMoreBlackOutSlider.tsx`
- APIs: `cmApi` → `get-all-promotion`, `advance-days`, `insert-hotel-promotion-new`, `get-hotel-promotion`, `update-hotel-promotion-new`

#### Feature: Same-Day Promotion

- Description: Last-minute discounts for same-day bookings to fill unsold inventory.
- Components: `Promotions/SameDay/SameDay.tsx`, `SameDay/AddSameDayPromotion.tsx`, `SameDay/ModifySameDayPromotion.tsx`, `SameDay/ViewSameDayPromotion.tsx`, `SameDay/InactiveSameDayPromotion.tsx`, `SameDay/SeeMoreBlackOutSlider.tsx`
- APIs: `cmApi` → same promotion API endpoints as Basic Promotion

#### Feature: Multi-Night Promotion

- Description: Discounts for guests staying multiple nights (e.g., stay 3 nights, get 10% off).
- Components: `Promotions/MultiNights/MultiNights.tsx`, `MultiNights/AddMultiNightsPromotion.tsx`, `MultiNights/ModifyMultiNightsPromotion.tsx`, `MultiNights/ViewMultiNightPromotion.tsx`, `MultiNights/InactiveMultiNightsPromotion.tsx`, `MultiNights/SeeMoreBlackOutSlider.tsx`
- APIs: `cmApi` → same promotion API endpoints as Basic Promotion

#### Feature: Day-of-Week Promotion

- Description: Day-specific discounts (e.g., weekday specials, weekend deals).
- Components: `Promotions/Day-of-week/DayOfWeek.tsx`, `Day-of-week/AddDayOfWeekPromotion.tsx`, `Day-of-week/ModifyDayOfWeekPromotion.tsx`, `Day-of-week/ViewDayOfWeekPromotion.tsx`, `Day-of-week/InactiveDayOfWeekPromotion.tsx`, `Day-of-week/SeeMoreBlackOutSlider.tsx`
- APIs: `cmApi` → same promotion API endpoints as Basic Promotion

#### Feature: Airbnb-Specific Promotions

- Description: Airbnb channel-specific promotions: early bird, last minute, and length-of-stay rules. Uses Airbnb's rule-based promotion system with apply/create/deactivate/view-log flows.
- Components: `Promotions/AirbnbPromotion/AirbnbEarlyBird/AirbnbEarlyBird.tsx`, `AirbnbEarlyBird/AirbnbEarlyBirdApplySlider.tsx`, `AirbnbEarlyBird/AirbnbEarlyBirdNewRule.tsx`, `AirbnbEarlyBird/AirbnbEarlyBirdViewLogSlider.tsx`, `Airbnblastmin/AirbnbLastmin.tsx` (plus Apply/NewRule/ViewLog variants), `AirbnbLos/AirbnbLengthOfStay.tsx` (plus Apply/NewRule/ViewLog variants)
- APIs: `cmApi` → `airbnb/rule/apply`, `airbnb/rule/get/{hotel_id}`, `airbnb/rule/create`, `check-airbnb-status`, `airbnb/rule/get/inactive/{hotel_id}`, `airbnb/rule/deactive`, `get_room_type_rate_plans_airbnb`, `airbnb/viewlog`, `airbnb/rule/remove`

#### Feature: Promotion Types Hub

- Description: Landing page showing all available promotion types with navigation to each specific promotion management screen.
- Components: `Promotions/PromotionTypes.tsx`
- APIs: None (navigation only)

---

## Module: Reviews

### Features

#### Feature: Review Summary Dashboard

- Description: Aggregated review summary across all connected OTA sources with overall rating, review count, and source-wise breakdown.
- Components: `Reviews/ReviewsSummery/ReviewSummery.tsx`, `ReviewsSummery/ReviewSummertyList.tsx`, `ReviewsSummery/ReviewSummerySingleView.tsx`
- APIs: `cmApi` → `review/get_hotel_summary`, `review/allreview`, `review/ota-review-list`

#### Feature: Google Business Reviews

- Description: Connect Google Business profile, fetch and display Google reviews, reply to reviews, and delete replies. Requires Google OAuth login and business location selection.
- Components: `Reviews/GoogleReviewManage.tsx`, `Reviews/SelectBusiness.tsx`, `Reviews/GoogleReview/GoogleBusinessReviews.tsx`, `GoogleReview/ReviewReplySlider.tsx`, `Reviews/Reviews.tsx`
- APIs: `cmApi` → `google-review/login`, `google-review/googlelogin`, `google-review/logout`, `google-review/fetchAccount`, `google-review/fetchLocation`, `google-review/setLocation`, `google-review/deleteReply`, `review/gmb-status`, `review/sync-all-review`, `review/single-review`, `review/replyreview`

#### Feature: Booking.com Reviews

- Description: Fetch and display Booking.com reviews with sync capability and individual review detail view.
- Components: `Reviews/BookingReview/BookingReview.tsx`, `BookingReview/BookingReviewSingleView.tsx`
- APIs: `cmApi` → `bookingdotcom-review/otareviewlist`, `bookingdotcom-review/allreviews`, `bookingdotcom-review/sync-review`

#### Feature: Airbnb Reviews

- Description: Fetch and display Airbnb reviews with sync capability, individual review view, and reply functionality.
- Components: `Reviews/AirbnbReview/AirbnbReview.tsx`, `AirbnbReview/AirbnbSingleReviewView.tsx`
- APIs: `cmApi` → `airbnb-review/sync-review`, `airbnb-review/allreviews`, `airbnb-review/single-review`, `airbnb-review/reply-review`

---

## Module: Performance

### Features

#### Feature: Direct Booking Performance

- Description: Analyze direct booking performance with date range filtering and comparison views. Shows booking trends, revenue, and channel performance metrics.
- Components: `Performance/PerformanceHomeScreen.tsx`, `Performance/DirectBookingsPerfermance.tsx`, `Performance/DirectBookingsComparison.tsx`, `Performance/PerformanceFilters.tsx`
- APIs: `kernelApi` → `direct-booking-performance-new`, `booking-status-performance`, `download-direct-booking-performance`

#### Feature: Booking.com Market Insight & Area Demand

- Description: Market intelligence reports from Booking.com showing area demand data, booker insights, booking window analysis, pace reports, and sales statistics.
- Components: `Performance/BookingDotComMarketInSight.tsx`, `Performance/BookingDotComAreaDemandReport.tsx`
- APIs: `kernelApi` → `market-insight/areademanddata`, `market-insight/bookerinsightdata`, `market-insight/bookwindowdata`, `market-insight/pacereportdata`, `market-insight/salesstatisticsreportdata`, `market-insight/checkstatus`

#### Feature: Digital Marketing Report

- Description: View digital marketing engine reports and campaign performance data.
- Components: `Performance/DigitalMarketingReport.tsx`
- APIs: `kernelApi` → `marketing-engine/get-report-list`

#### Feature: Channel-Wise Check-In Report

- Description: Report showing check-in data broken down by distribution channel with download capability.
- Components: `Performance/ChannelWiseCheckInReport.tsx`
- APIs: `kernelApi` → `channel-wise-checkin-report`, `download-channel-wise-checkin-report`

#### Feature: Year in Review / Performance Dashboard

- Description: Annual performance summary with month-wise and YoY room night comparisons, quarterly breakdowns, and onboarding date tracking.
- Components: `Performance/PerformanceYearInReview.tsx`, `Performance/PerformanceDashboard.tsx`, `Performance/Performance.tsx`, `Performance/PieChart.tsx`
- APIs: `kernelApi` → `get-performance-report-url`, `month-wise-room-nights`, `yoy-monthly-room-nights`, `yoy-quarterly-room-nights`, `onboarding-date`

---

## Module: ManageUsers

### Features

#### Feature: User CRUD

- Description: Add, edit, and delete sub-users for the property. Assign users to specific hotels within the company. Supports pagination.
- Components: `ManageUsers/ManageUsers.tsx`, `ManageUsers/AddUsers.tsx`, `ManageUsers/EditUsers.tsx`
- APIs: `kernelApi` → `manage_user/all/{company_id}`, `manage_user/add`, `manage_user/update`, `manage_user/delete-user/{id}`, `hotel_admin/get_hotel_list`

#### Feature: Role-Based Access Control

- Description: Configure function-level access permissions for each user. Granular control over which features a user can access.
- Components: `ManageUsers/EditAccess.tsx`
- APIs: `kernelApi` → `get-user-access/{user_id}`, `get-access-functionality`, `user-access-updation-or-creation`

---

## Module: ManageSubscription

### Features

#### Feature: Subscription Overview

- Description: View current subscription plan details, billing history, transaction records, and app subscriptions. Supports invoice download.
- Components: `ManageSubscription/ManageSubscription.tsx`
- APIs: `subscriptionApi` → `my-subscription`

#### Feature: Upgrade Subscription

- Description: Browse and select higher-tier subscription plans with checkout flow via Chargebee or Zoho.
- Components: `ManageSubscription/UpgradeSubscription.tsx`, `ManageSubscription/ChooseSubscription.tsx`
- APIs: `subscriptionApi` → `get-all-upgradable-plans`, `get-checkout-url-for-upgrade`, `get-zoho-checkout-url-for-upgrade`

#### Feature: Cancel Subscription

- Description: Cancel the current subscription with confirmation prompt.
- Components: `ManageSubscription/ManageSubscription.tsx` (inline)
- APIs: `subscriptionApi` → `cancel-subscription`, `cancel-zoho-subscription`

#### Feature: Auto-Debit Authorization

- Description: Set up and manage auto-debit payment authorization for recurring subscription payments, including UPI-based authorization.
- Components: `ManageSubscription/AuthorizeTheAutoDebit.tsx`
- APIs: `subscriptionApi` → `get-autodebit-authorization-url`, `get-upi-autodebit-authorization`, `cancel-autodebit`

#### Feature: Make Payment

- Description: Manual subscription payment flow with Easebuzz payment gateway integration.
- Components: `ManageSubscription/MakePayment.tsx`
- APIs: `subscriptionApi` → `make-subscription-payment`

---

## Module: ChoosePlan

### Features

#### Feature: Plan Selection

- Description: Browse available subscription plans with tier comparison, package details, and checkout. Supports coupon code validation. Redirects here when no active plan exists.
- Components: `ChoosePlan/ChoosePlan.tsx`, `ChoosePlan/ChoosePlanOld.tsx`, `ChoosePlan/PlanPreviewSlider.tsx`, `ChoosePlan/ChoosePlanSuccess.tsx`
- APIs: `subscriptionApi` → `get-tiers`, `get-packages`, `get-plans`, `get-zoho-plans`, `subscribe-plan`, `get-checkout-url`, `get-zoho-checkout-url`, `validate-subscription-coupon`

#### Feature: Renew Subscription

- Description: Renewal flow for expired subscriptions with plan selection and payment processing.
- Components: `ChoosePlan/RenewSubscription.tsx`, `ChoosePlan/RenewSubscriptionSuccess.tsx`
- APIs: `subscriptionApi` → `get-checkout-url-for-renewal`, `get-zoho-checkout-url-for-renewal`

---

## Module: AddNewProperty

### Features

#### Feature: Property Onboarding Wizard

- Description: Multi-step wizard for adding a new property: select property type → subtype → enter details → set map address → choose amenities → upload images → review overview → success. State managed through Redux `add_property` slice.
- Components: `AddNewProperty/PropertyType.tsx`, `AddNewProperty/PropertySubType.tsx`, `AddNewProperty/PropertyDetails.tsx`, `AddNewProperty/PropertyMapAddress.tsx`, `AddNewProperty/PropertyAmenities.tsx`, `AddNewProperty/PropertyImages.tsx`, `AddNewProperty/PropertyOverview.tsx`, `AddNewProperty/AddPropertySuccess.tsx`
- APIs: `kernelApi` → `getAllPropertyTypes`, `getAllPropertySubTypes/{type_id}`, `add_new_property_new`, `uploadhotelimages`, `getAllPropertyAmenities`, `getAllSelectedPropertyAmenities`, `hotel_admin/get_all_hotels_by_company`, `can_add_property`, `add_hotel_to_company`

---

## Module: AddRoomType

### Features

#### Feature: Room Type Creation Wizard

- Description: Multi-step wizard for creating a new room type: enter details → set occupancy → configure pricing with rate plans → upload images → success.
- Components: `AddRoomType/RoomTypeDetails.tsx`, `AddRoomType/RoomTypeOccupancy.tsx`, `AddRoomType/RoomTypePrice.tsx`, `AddRoomType/RoomPriceDetails.tsx`, `AddRoomType/RoomRatePlan.tsx`, `AddRoomType/RoomTypeImages.tsx`, `AddRoomType/RoomTypeSuccess.tsx`
- APIs: `kernelApi` → `hotel_master_new_room_type/add`, `hotel_master_new_room_type/upload_room_type_images/{hotel_id}`, `enum-type/bookable-name`

---

## Module: AddFloors

### Features

#### Feature: Floor Creation Wizard

- Description: Multi-step floor creation: specify number of floors → select serviceable floors → success confirmation.
- Components: `AddFloors/CreateFloors.tsx`, `AddFloors/ServiceableFloors.tsx`, `AddFloors/FloorCreationSuccess.tsx`
- APIs: `kernelApi` → `saveFloors`

---

## Module: AddRoom

### Features

#### Feature: Room Creation

- Description: Add rooms to existing floors with room number, room type assignment, and status configuration.
- Components: `AddRoom/AddRoomDetails.tsx`, `AddRoom/AddRoomsSuccess.tsx`
- APIs: `kernelApi` → `addRoom`

---

## Module: Tickets

### Features

#### Feature: Support Ticket Management

- Description: Raise, view, and manage support tickets. Supports ticket listing by status (Open/Resolved/Closed), ticket detail view with comment thread, reply with rich text (CKEditor), and ticket closure.
- Components: `Tickets/Tickets.tsx`, `Tickets/RaiseNewTicket.tsx`, `Tickets/TicketDetails.tsx`, `Tickets/ImageUploadAdapter.tsx`
- APIs: `kernelApi` → `ticket/raise`, `ticket/list`, `ticket/view-details`, `ticket/add-comment`, `ticket/close`

---

## Module: FAQ

### Features

#### Feature: FAQ & Help Center

- Description: Searchable FAQ system with category-based browsing, initial FAQ display, detailed FAQ slider view, and ability to submit new questions.
- Components: `FAQ/Faq.tsx`, `FAQ/TopFaqSlider.tsx`, `FAQ/AddQuestionSlider.tsx`
- APIs: External (`datamart.bookingjini.com/api`) → `faq/search?q=`, `faq/initial`, `faq`, `faq/categories`, `faq-manage/categories/list`, `faq/question`, `kernelApi` → `get-contact-us-info`

---

## Module: Apps

### Features

#### Feature: App Marketplace

- Description: Browse available apps/integrations and view detailed information about each app.
- Components: `Apps/AppList.tsx`, `Apps/AppDetails.tsx`
- APIs: `kernelApi` → app listing endpoints

---

## Module: Mart

### Features

#### Feature: Marketplace (Coming Soon)

- Description: Placeholder page for a future marketplace feature. Currently displays a "Launching Soon" message.
- Components: `Mart/Mart.tsx`
- APIs: None

---

## Module: Authentication & SignUp

### Features

#### Feature: Login

- Description: Email/password login with Google OAuth support. Handles SSO deep-link login from intranet/hotel chain portals via base64-encoded URL parameters.
- Components: `Login.tsx`, `Loginwithoutcredential.tsx`
- APIs: `kernelApi` → `extranet-admin/auth`, `verify-token/{company_id}/{admin_id}`

#### Feature: Sign Up

- Description: New user registration with validation step and form submission.
- Components: `SignUp/SignUp.tsx`, `SignUp/SignUpForm.tsx`, `SignUp/SignUpValidation.tsx`
- APIs: `kernelApi` → registration endpoints

#### Feature: Password Reset

- Description: Password reset flow with email verification and new password entry.
- Components: `ResetPassword.tsx`, `EnterNewPassword.tsx`
- APIs: `kernelApi` → `admin/change_password`, `admin/check_password_admin`

#### Feature: Property Selection

- Description: After login, select which property to manage from the user's assigned properties.
- Components: `SelectProperty.tsx`
- APIs: `kernelApi` → `hotel_admin/get_all_hotels_by_company`

---

## Module: Notifications

### Features

#### Feature: Notification Center

- Description: View all system notifications with read/unread status tracking.
- Components: `NotificationPage.tsx`
- APIs: `kernelApi` → `get-notification-logs`, `unread-message`, `update-viewer-status`

---

## Observations

### Cross-Cutting Patterns

- Every page component calls `UpdateSidebar()` at the top to sync the sidebar active state — this is a side-effect-in-render anti-pattern that should be moved to a `useEffect` or handled via routing.
- RBAC is checked per-page by filtering `accessData` from Redux for a feature code (e.g., `"INVENTORY"`). This is duplicated in every page rather than abstracted into a route guard or HOC.
- All slider/drawer UIs use `react-sliding-pane` with local boolean state for open/close — no shared drawer management.
- API calls are made directly in components using `async/await` with try/catch. No service layer, no custom hooks, no React Query or SWR. This leads to significant boilerplate duplication.
- The dual BE/CM update pattern (separate API calls for Booking Engine vs Channel Manager) is repeated across Inventory, Rates, and Promotions modules — a strong candidate for abstraction.

### Hidden Logic

- Subscription gating runs on every route change in `AppRoutes.tsx` via `checkHotelSubscription()`, silently redirecting to `/choose-plan` if the plan is expired.
- User access is fetched on every route change via `getUserAcess()` in `AppRoutes.tsx`.
- The `be_version` flag (v3 vs older) conditionally renders different promotion routes and coupon types in the Booking Engine.
- `single_sign_on_status` controls whether the SSO deep-link route is available.
- The `Inventory.tsx` component alone is 2727 lines — it contains the entire room-type inventory view logic including all inline editing, block/unblock, sync, and calendar update logic in a single component.
- Commented-out code is prevalent across many files (old implementations, alternative approaches), adding noise and making maintenance harder.

### Assumptions

- The `toolsAPI` Axios instance appears to be a duplicate of `kernelApi` (same base URL) — likely created for a specific use case that was later merged.
- The `beAlphaAPI` instance points to a staging/alpha version of the Booking Engine, suggesting feature-flagged or A/B tested functionality.
- The `getResponse` API file likely integrates with the GetResponse email marketing platform, though its usage wasn't directly observed in the explored pages.
- The Mart module is a planned feature that hasn't been implemented yet.
