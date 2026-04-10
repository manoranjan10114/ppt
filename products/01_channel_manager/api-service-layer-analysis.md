# Channel Manager Extranet — API & Service Layer Analysis

---

## Backend Microservice Topology

The frontend communicates with 8 distinct backend microservices via dedicated Axios instances, plus 2 external services.

| Axios Instance    | Service              | Base URL (Prod)                       | Auth                                          | Purpose                                                                                                                                       |
| ----------------- | -------------------- | ------------------------------------- | --------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `kernelApi`       | Kernel               | `kernel.bookingjini.com/extranetv4`   | Bearer token + 401 auto-logout                | Core property, user, room, floor, amenity, image, policy, notification, ticket, PMS, plugin, commission, performance, FAQ, OTP, AI generation |
| `cmApi`           | Channel Manager      | `cm.bookingjini.com/extranetv4`       | Bearer token + 401 auto-logout                | Inventory, rates, OTA mapping, channel sync, promotions, reviews, Airbnb rules, Expedia messaging, Booking.com reviews                        |
| `cm3Api`          | Channel Manager v3   | `cm3.bookingjini.com`                 | Bearer token + 401 auto-logout                | Agoda onboarding (room/rate mapping, contract signing)                                                                                        |
| `beApi`           | Booking Engine       | `be.bookingjini.com/extranetv4`       | Bearer token + 401 auto-logout                | BE inventory/rate updates, bookings, coupons, paid services, payment links, cancellation policies, day-outing, widgets, dashboard data        |
| `beAlphaAPI`      | Booking Engine Alpha | `be-alpha.bookingjini.com/extranetv4` | No interceptor                                | Alpha/staging BE features (used via `getResponse` wrapper)                                                                                    |
| `gemsApi`         | GEMS (Front Office)  | `gems.bookingjini.com/api/extranetv4` | Bearer token + 401 auto-logout                | Front-office check-in/out, stay details, billing, walk-in bookings, guest details                                                             |
| `subscriptionApi` | Subscription         | `subscription.bookingjini.com/api`    | Custom header (`X_Auth_Subscription-API-KEY`) | Plan management, billing, checkout URLs, subscription status                                                                                  |
| `blApi`           | Status               | `status.bookingjini.tech`             | None                                          | System health/status checks                                                                                                                   |
| `toolsAPI`        | Tools (Kernel alias) | `kernel.bookingjini.com/extranetv4`   | Bearer token (no 401 handler)                 | Duplicate of kernelApi — used for specific tool operations                                                                                    |
| `getResponse`     | BE Alpha wrapper     | (via `beAlphaAPI`)                    | None                                          | Utility wrapper for GET/POST to beAlphaAPI                                                                                                    |

External services accessed directly (not via Axios instances):

- `https://datamart.bookingjini.com/api` — FAQ system
- `https://gmb-sse.bookingjini.com` — Google Business Reviews SSE stream
- `https://staging.bookingjini.com/api/rateshopper` — Rate shopper trend graph
- `https://kernel.bookingjini.com/extranetv4/genai` — AI content generation
- `https://www.airbnb.com/oauth2/auth` — Airbnb OAuth

---

## API Mapping by Module

### Module: Authentication & Session

| Endpoint                               | Method | Axios Instance | Purpose                                   | Data Flow |
| -------------------------------------- | ------ | -------------- | ----------------------------------------- | --------- |
| `extranet-admin/auth`                  | POST   | kernelApi      | Login with email/password or Google OAuth | Fetch     |
| `verify-token/{company_id}/{admin_id}` | GET    | kernelApi      | Validate auth token (currently disabled)  | Fetch     |
| `get-user-access/{admin_id}`           | GET    | kernelApi      | Fetch RBAC permissions for user           | Fetch     |
| `get-access-functionality`             | GET    | kernelApi      | Get all available access functions        | Fetch     |
| `admin/change_password`                | POST   | kernelApi      | Change user password                      | Push      |
| `admin/check_password_admin`           | POST   | kernelApi      | Validate current password                 | Fetch     |
| `get-company-data`                     | GET    | kernelApi      | Get company/user name data                | Fetch     |
| `select-dropdow-option`                | GET    | kernelApi      | Check super admin status                  | Fetch     |
| `get-admin-email`                      | GET    | kernelApi      | Get admin email                           | Fetch     |
| `update-admin-email`                   | POST   | kernelApi      | Update admin email                        | Push      |
| `sendotpbyemail`                       | POST   | kernelApi      | Send OTP for channel access verification  | Push      |
| `verifyotp`                            | POST   | kernelApi      | Verify OTP code                           | Fetch     |

### Module: Dashboard

| Endpoint                                                        | Method | Axios Instance | Purpose                               | Data Flow |
| --------------------------------------------------------------- | ------ | -------------- | ------------------------------------- | --------- |
| `hotelier-today-overview-new`                                   | GET    | beApi          | Today's check-in/out/in-house summary | Fetch     |
| `hotelier-today-overview-by-date-type-new`                      | POST   | beApi          | Confirmed bookings by date type       | Fetch     |
| `hotelier-summary-new`                                          | GET    | beApi          | MTD/YTD hotelier summary              | Fetch     |
| `hotelier-summary-by-date-type-new`                             | POST   | beApi          | Summary by date type filter           | Fetch     |
| `roomtype-wise-booked-room-nights`                              | POST   | beApi          | Room nights by room type              | Fetch     |
| `channel-wise-booked-room-nights-revenue`                       | POST   | beApi          | Revenue by channel                    | Fetch     |
| `booking-details/{hotel_id}`                                    | GET    | beApi          | Recent bookings feed                  | Fetch     |
| `locale-details/{hotel_id}`                                     | GET    | kernelApi      | Currency/locale info                  | Fetch     |
| `get_extranet_announcement`                                     | GET    | kernelApi      | System announcements                  | Fetch     |
| `no_show_announcement`                                          | POST   | kernelApi      | Dismiss announcement                  | Push      |
| `getpromotionalbanner/{hotel_id}`                               | GET    | kernelApi      | Promotional banner images             | Fetch     |
| `getlogourl`                                                    | GET    | kernelApi      | Extranet panel logo                   | Fetch     |
| `blog`                                                          | GET    | kernelApi      | Learning blog content                 | Fetch     |
| `extranet-quick-tips/daily-tip`                                 | GET    | kernelApi      | Daily quick tips                      | Fetch     |
| `get-user-details/{admin_id}/{hotel_id}`                        | GET    | kernelApi      | ZapScale tracker data                 | Fetch     |
| `https://staging.bookingjini.com/api/rateshopper/gettrendgraph` | GET    | Direct         | Rate shopper trend graph              | Fetch     |

### Module: Inventory

| Endpoint                                                        | Method | Axios Instance | Purpose                                 | Data Flow |
| --------------------------------------------------------------- | ------ | -------------- | --------------------------------------- | --------- |
| `hotel_master_room_type/all/{hotel_id}`                         | GET    | kernelApi      | Get all room types                      | Fetch     |
| `all-otas-by-room-type/{hotel_id}`                              | GET    | cmApi          | Get channels mapped to room types       | Fetch     |
| `inventory/get-inventory/{hotel_id}/{from}/{to}/{room_type_id}` | GET    | cmApi          | Get inventory calendar data             | Fetch     |
| `inventory/get-inventory-channelwise-new`                       | GET    | cmApi          | Get inventory by channel                | Fetch     |
| `update-property-default-view`                                  | POST   | kernelApi      | Save preferred view type                | Push      |
| `manage_inventory/update-inv`                                   | POST   | beApi          | Single-date BE inventory update         | Update    |
| `inventory/individual-inventory-update`                         | POST   | cmApi          | Single-date CM inventory update         | Update    |
| `inventory/bulk-inventory-update-new`                           | POST   | cmApi          | Bulk date-range inventory update        | Update    |
| `inventory/calendar-inventory-update`                           | POST   | cmApi          | Calendar-based inventory update         | Update    |
| `inventory/room-wise-calendar-update`                           | POST   | cmApi          | Channel-type calendar update            | Update    |
| `block-specific-dates`                                          | POST   | beApi          | Block BE inventory (specific dates)     | Update    |
| `inventory/individual-inventory-block`                          | POST   | cmApi          | Block CM inventory (specific dates)     | Update    |
| `inventory/bulk-inventory-block`                                | POST   | cmApi          | Block CM inventory (date range)         | Update    |
| `unblock-specific-dates`                                        | POST   | beApi          | Unblock BE inventory (specific dates)   | Update    |
| `inventory/individual-inventory-unblock`                        | POST   | cmApi          | Unblock CM inventory (specific dates)   | Update    |
| `inventory/bulk-inventory-unblock`                              | POST   | cmApi          | Unblock CM inventory (date range)       | Update    |
| `block-property`                                                | POST   | beApi          | Block entire property (BE)              | Update    |
| `unblock-property`                                              | POST   | beApi          | Unblock entire property (BE)            | Update    |
| `block_specific_property`                                       | POST   | cmApi          | Block entire property (CM)              | Update    |
| `unblock_specific_property`                                     | POST   | cmApi          | Unblock entire property (CM)            | Update    |
| `inventory/sync-inventory-new`                                  | POST   | cmApi          | Sync inventory across channels          | Push      |
| `get-block-dates`                                               | GET    | cmApi          | Get sold-out dates                      | Fetch     |
| `get-last-available-inv-dates`                                  | GET    | cmApi          | Last available inventory date           | Fetch     |
| `get-last-available-rate-dates`                                 | GET    | cmApi          | Last available rate date                | Fetch     |
| `inventory/out-of-order`                                        | POST   | cmApi          | Mark rooms out of order                 | Update    |
| `increase-decrease-inventory-reasons`                           | GET    | cmApi          | Get inventory change reasons            | Fetch     |
| `inventory/inventory-increase-decrease`                         | POST   | cmApi          | Increase/decrease inventory with reason | Update    |

### Module: Rates

| Endpoint                                             | Method | Axios Instance | Purpose                           | Data Flow |
| ---------------------------------------------------- | ------ | -------------- | --------------------------------- | --------- |
| `rates/get-rates`                                    | GET    | cmApi          | Get rate calendar data            | Fetch     |
| `rates/get-rates-channel-wise`                       | GET    | cmApi          | Get rates by channel              | Fetch     |
| `master_hotel_rate_plan/room_rate_plan_by_room_type` | GET    | kernelApi      | Rate plans for room type          | Fetch     |
| `get_supported_ota_for_rateblock/{hotel_id}`         | GET    | cmApi          | Blockable OTA list                | Fetch     |
| `all-otas-by-room-type-and-rate-plans`               | GET    | cmApi          | Channels by room type + rate plan | Fetch     |
| `rates/individual-rate-update`                       | POST   | beApi          | Single-date BE rate update        | Update    |
| `rates/room_rate_update_new`                         | POST   | beApi          | Bulk date-range BE rate update    | Update    |
| `manage_inventory/room_rate_update`                  | POST   | cmApi          | Bulk date-range CM rate update    | Update    |
| `rates/calendar-rate-update`                         | POST   | cmApi          | Calendar-based rate update        | Update    |
| `rates/channel-wise-calendar-update`                 | POST   | cmApi          | Channel-wise calendar rate update | Update    |
| `rates/room-wise-calendar-update`                    | POST   | cmApi          | Room-wise calendar rate update    | Update    |
| `block-rate-specific-dates`                          | POST   | beApi          | Block BE rates (specific dates)   | Update    |
| `rates/individual-rate-block-new`                    | POST   | cmApi          | Block CM rates (specific dates)   | Update    |
| `rates/bulk-rate-block-new`                          | POST   | cmApi          | Block CM rates (date range)       | Update    |
| `unblock-rate-specific-dates`                        | POST   | beApi          | Unblock BE rates (specific dates) | Update    |
| `rates/individual-rate-unblock-new`                  | POST   | cmApi          | Unblock CM rates (specific dates) | Update    |
| `rates/bulk-rate-unblock-new`                        | POST   | cmApi          | Unblock CM rates (date range)     | Update    |
| `unblock-rate-range`                                 | POST   | beApi          | Unblock BE rates (date range)     | Update    |
| `rates/sync-rate-new`                                | POST   | cmApi          | Sync rates across channels        | Push      |
| `rates/bulk-los-update`                              | POST   | cmApi          | Bulk LOS update                   | Update    |
| `rates/individual-los-update`                        | POST   | cmApi          | Individual LOS update             | Update    |

### Module: Bookings

| Endpoint                              | Method | Axios Instance | Purpose                             | Data Flow |
| ------------------------------------- | ------ | -------------- | ----------------------------------- | --------- |
| `booking-lists`                       | POST   | beApi          | List view bookings with filters     | Fetch     |
| `listview-bookings-report`            | POST   | beApi          | Download booking CSV report         | Fetch     |
| `source-list`                         | GET    | cmApi          | Get all booking sources             | Fetch     |
| `extranet-bookings/roomdetails`       | GET    | kernelApi      | Room types for booking views        | Fetch     |
| `crs/crs-booking-details`             | POST   | beApi          | CRS calendar booking data           | Fetch     |
| `frontofficeBookingCalendar`          | POST   | gemsApi        | Front-office calendar data          | Fetch     |
| `isGEMSSubscribed`                    | GET    | gemsApi        | Check GEMS subscription             | Fetch     |
| `get-subscribed-products`             | GET    | gemsApi        | Get subscribed product list         | Fetch     |
| `getBookingDetails`                   | GET    | gemsApi        | Booking details by ID               | Fetch     |
| `getBookingDetailsByBookingidV2`      | GET    | gemsApi        | Booking details v2                  | Fetch     |
| `getStayDetailsByBookingID`           | GET    | gemsApi        | Stay details for active booking     | Fetch     |
| `getBillingDetailsByBookingidV2`      | GET    | gemsApi        | Billing details                     | Fetch     |
| `getRoomAndGuestDetails`              | GET    | gemsApi        | Room and guest details              | Fetch     |
| `guestDetails`                        | POST   | gemsApi        | Lookup guest by mobile              | Fetch     |
| `frontOfficeCheckin3`                 | POST   | gemsApi        | Complete front-office check-in      | Push      |
| `frontOfficeCheckout`                 | POST   | gemsApi        | Complete front-office checkout      | Push      |
| `extranet-bookings/allocateroom`      | POST   | kernelApi      | Allocate room to booking            | Push      |
| `extranet-bookings/deallocateroom`    | POST   | kernelApi      | Deallocate room (CRS checkout)      | Push      |
| `crs/crs_cancel_booking`              | POST   | beApi          | Cancel booking                      | Push      |
| `crs/crs-modified-guest-details`      | POST   | beApi          | Modify guest details                | Update    |
| `crs/crs_modify_bookings-new`         | POST   | beApi          | Modify booking (new)                | Update    |
| `getPaymentModes`                     | GET    | gemsApi        | Available payment modes             | Fetch     |
| `addExpense`                          | POST   | gemsApi        | Add expense to stay                 | Push      |
| `addCollection`                       | POST   | gemsApi        | Add collection to stay              | Push      |
| `addPayment`                          | POST   | gemsApi        | Add payment to stay                 | Push      |
| `generatePDFInvoice`                  | POST   | gemsApi        | Generate PDF invoice                | Fetch     |
| `generatePDFBill`                     | POST   | gemsApi        | Generate PDF bill                   | Fetch     |
| `check-expedia-booking-update-status` | POST   | beApi          | Check Expedia booking update status | Fetch     |
| `fetch-expedia-reasons`               | GET    | beApi          | Fetch Expedia cancellation reasons  | Fetch     |
| `update-expedia-reservations`         | POST   | beApi          | Update Expedia reservation          | Push      |

### Module: Bookings — Reservation

| Endpoint                                   | Method | Axios Instance | Purpose                           | Data Flow |
| ------------------------------------------ | ------ | -------------- | --------------------------------- | --------- |
| `bookingEngine/get-inventory`              | POST   | beApi          | Get available inventory for dates | Fetch     |
| `total-rooms-by-room-type`                 | GET    | beApi          | Total rooms per room type         | Fetch     |
| `manage_inventory/get_room_rates_by_hotel` | GET    | kernelApi      | Room rates for hotel              | Fetch     |
| `user/register`                            | POST   | beApi          | Register guest user               | Push      |
| `bookingEngine/bookings`                   | POST   | beApi          | Create booking                    | Push      |
| `generateBookingVoucherPDF`                | POST   | beApi          | Generate booking voucher          | Fetch     |
| `updateBookingAdvance`                     | POST   | beApi          | Update advance payment            | Update    |

### Module: Bookings — Walk-In

| Endpoint                              | Method | Axios Instance | Purpose                      | Data Flow |
| ------------------------------------- | ------ | -------------- | ---------------------------- | --------- |
| `getAvailableRoomsFloorwiseForWalkin` | GET    | kernelApi      | Floor-wise room availability | Fetch     |
| `frontOfficeCheckin5`                 | POST   | gemsApi        | Walk-in check-in             | Push      |
| `gems-booking`                        | POST   | gemsApi        | Create GEMS booking          | Push      |
| `generatePDFGRC`                      | POST   | gemsApi        | Generate GRC PDF             | Fetch     |
| `getAllCountry`                       | GET    | kernelApi      | Country list for guest       | Fetch     |
| `hotel_bank_account_details/get`      | GET    | kernelApi      | Bank details for billing     | Fetch     |

### Module: Bookings — Messaging

| Endpoint                                      | Method | Axios Instance | Purpose                          | Data Flow |
| --------------------------------------------- | ------ | -------------- | -------------------------------- | --------- |
| `expedia-messaging/get-message-thread`        | POST   | cmApi          | Get Expedia message thread       | Fetch     |
| `expedia-messaging/post-message`              | POST   | cmApi          | Send Expedia message             | Push      |
| `expedia-messaging/upload-attachement`        | POST   | cmApi          | Upload file attachment           | Push      |
| `expedia-messaging/get-message-by-message-id` | GET    | cmApi          | Get specific message             | Fetch     |
| `bookingdotcom-messaging/getotacode`          | GET    | cmApi          | Get OTA hotel code for messaging | Fetch     |

### Module: Manage Channel — OTA Management

| Endpoint                                        | Method | Axios Instance | Purpose                         | Data Flow |
| ----------------------------------------------- | ------ | -------------- | ------------------------------- | --------- |
| `get_added_channels`                            | GET    | cmApi          | List connected OTAs             | Fetch     |
| `getallchannel`                                 | GET    | cmApi          | List all available OTAs         | Fetch     |
| `add-ota-hotel-details`                         | POST   | cmApi          | Add OTA hotel mapping           | Push      |
| `cm_ota_hotel_details`                          | GET    | cmApi          | Get specific OTA details        | Fetch     |
| `update-ota-hotel-details`                      | POST   | cmApi          | Update OTA hotel details        | Update    |
| `cm_ota_details/add`                            | POST   | cmApi          | Add OTA connection              | Push      |
| `cm_ota_details/update`                         | POST   | cmApi          | Update OTA connection           | Update    |
| `cm_ota_details/toggle`                         | POST   | cmApi          | Toggle OTA active/inactive      | Update    |
| `cm_ota_details/sync`                           | POST   | cmApi          | Sync OTA details                | Push      |
| `get_unmapped_room_types_n_last_sync`           | GET    | cmApi          | Sync history and unmapped rooms | Fetch     |
| `cm_ota_roomtype_sync_new/add`                  | POST   | cmApi          | Add room type sync mapping      | Push      |
| `cm_ota_rateplan_sync_new/rooms`                | GET    | cmApi          | Get room mapping data           | Fetch     |
| `cm_ota_rateplan_sync_new/ota_room_type`        | GET    | cmApi          | Get OTA rate plans              | Fetch     |
| `cm_ota_rateplan_sync_new/add`                  | POST   | cmApi          | Add rate plan sync mapping      | Push      |
| `cm_ota_roomtype_sync/ota_room_type`            | GET    | cmApi          | Get OTA room types              | Fetch     |
| `cm_ota_roomtype_sync/ota_rate_plan`            | GET    | cmApi          | Get OTA rate plans for room     | Fetch     |
| `master_hotel_rate_plan/rate_plan_by_room_type` | GET    | kernelApi      | Hotel rate plans by room type   | Fetch     |
| `cm_ota_details/update-ota-payment-link`        | POST   | cmApi          | Send OTA payment link           | Push      |
| `get-all-airbnb-listing`                        | GET    | cmApi          | Get Airbnb listings             | Fetch     |
| `air-bnb/save-code`                             | POST   | kernelApi      | Save Airbnb OAuth code          | Push      |

### Module: Manage Channel — Booking Engine

| Endpoint                                      | Method   | Axios Instance | Purpose                                    | Data Flow    |
| --------------------------------------------- | -------- | -------------- | ------------------------------------------ | ------------ |
| `check-all-setup`                             | GET      | beApi          | Check BE setup completion                  | Fetch        |
| `company_profile/get`                         | GET      | kernelApi      | Get company profile                        | Fetch        |
| `company_profile/get-logo`                    | GET      | kernelApi      | Get logo and banners                       | Fetch        |
| `/upload`                                     | POST     | kernelApi      | Upload logo/banner images                  | Push         |
| `company_profile/booking_page`                | POST     | kernelApi      | Update booking page images                 | Push         |
| `deleteImage`                                 | POST     | kernelApi      | Delete uploaded image                      | Push         |
| `manage-url`                                  | GET      | beApi          | Get booking engine URL                     | Fetch        |
| `manage-url/update`                           | POST     | beApi          | Update booking engine URL                  | Update       |
| `other-settings`                              | GET      | beApi          | Get BE other settings                      | Fetch        |
| `other-settings/update`                       | POST     | beApi          | Update BE other settings                   | Update       |
| `theme-setup`                                 | GET      | beApi          | Get theme configuration                    | Fetch        |
| `theme-setup/update`                          | POST     | beApi          | Update theme configuration                 | Update       |
| `fetch-other-settings`                        | GET      | beApi          | Fetch additional settings                  | Fetch        |
| `update-other-settings`                       | POST     | beApi          | Update additional settings                 | Update       |
| `update-banner-order`                         | POST     | beApi          | Reorder banner images                      | Update       |
| `coupons/get`                                 | GET      | beApi          | Get all coupons                            | Fetch        |
| `coupons/add`                                 | POST     | beApi          | Create coupon                              | Push         |
| `coupons/update`                              | POST     | beApi          | Update coupon                              | Update       |
| `coupons/fetch`                               | GET      | beApi          | Get specific coupon                        | Fetch        |
| `coupons`                                     | DELETE   | beApi          | Delete coupon                              | Push         |
| `coupons/list/type`                           | GET      | beApi          | Get coupon types                           | Fetch        |
| `paid_services/list`                          | GET      | beApi          | List paid services                         | Fetch        |
| `paid_services/add`                           | POST     | beApi          | Add paid service                           | Push         |
| `paid_services/update`                        | POST     | beApi          | Update paid service                        | Update       |
| `paid_services/delete`                        | POST     | beApi          | Delete paid service                        | Push         |
| `paid_services/report`                        | POST     | beApi          | Paid services report                       | Fetch        |
| `paid-services-report/excel`                  | POST     | beApi          | Download report as Excel                   | Fetch        |
| `be-notifications`                            | GET      | beApi          | Get BE notifications                       | Fetch        |
| `be-notifications-popup`                      | POST     | beApi          | Create notification popup                  | Push         |
| `fetch-be-notification-slider-images`         | GET      | kernelApi      | Get promotional slider images              | Fetch        |
| `upload-be-notification-slider-images`        | POST     | kernelApi      | Upload slider images                       | Push         |
| `delete-be-notification-slider-image`         | POST     | kernelApi      | Delete slider image                        | Push         |
| `get-payment-gateways`                        | GET      | beApi          | List payment gateways                      | Fetch        |
| `update-paymentgateway-setup`                 | POST     | beApi          | Configure payment gateway                  | Update       |
| `select-paymentgateway-setup`                 | GET      | beApi          | Get gateway configuration                  | Fetch        |
| `select-all-charges`                          | GET      | kernelApi      | Get additional charges                     | Fetch        |
| `add-charges`                                 | POST     | kernelApi      | Add charge                                 | Push         |
| `update-charges`                              | POST     | kernelApi      | Update charge                              | Update       |
| `delete-charges`                              | POST     | kernelApi      | Delete charge                              | Push         |
| `active-charges`                              | POST     | kernelApi      | Toggle charge active status                | Update       |
| `master_hotel_rate_plan/all`                  | GET      | kernelApi      | Get all rate plans                         | Fetch        |
| `master_hotel_rate_plan/update-status-for-be` | POST     | kernelApi      | Toggle rate plan BE visibility             | Update       |
| `reservation-payment-link/all`                | GET      | beApi          | List payment links                         | Fetch        |
| `reservation-payment-link/resend-email`       | POST     | beApi          | Resend payment link email                  | Push         |
| `reservation-payment-link/check`              | GET      | beApi          | Check payment link status                  | Fetch        |
| `generic-payment-link/add`                    | POST     | beApi          | Create generic payment link                | Push         |
| `generic-payment-link/resend-email`           | POST     | beApi          | Resend generic link email                  | Push         |
| `generic-payment-link/check`                  | GET      | beApi          | Check generic link status                  | Fetch        |
| `cancellation_policy/date-range/list`         | GET      | beApi          | List cancellation policies                 | Fetch        |
| `cancellation_policy/date-range/add`          | POST     | beApi          | Add cancellation policy                    | Push         |
| `cancellation_policy/date-range/update`       | POST     | beApi          | Update cancellation policy                 | Update       |
| `cancellation_policy/date-range/delete`       | POST     | beApi          | Delete cancellation policy                 | Push         |
| `cancellation_policy/date-range/status`       | POST     | beApi          | Set default policy                         | Update       |
| `commission-list`                             | GET      | kernelApi      | Get commission/payout data                 | Fetch        |
| `instant-booking/setup`                       | GET/POST | beApi          | Instant booking widget config              | Fetch/Update |
| `website-widget/fetch`                        | GET      | beApi          | Get website widget config                  | Fetch        |
| `website-widget/save`                         | POST     | beApi          | Save website widget config                 | Push         |
| `website-widget/status`                       | POST     | beApi          | Toggle widget status                       | Update       |
| `fetch-plugin`                                | GET      | kernelApi      | List plugins                               | Fetch        |
| `store-plugin`                                | POST     | kernelApi      | Save plugin configuration                  | Push         |
| `hotel_other_information`                     | GET      | kernelApi      | Get hotel other info (packages, locations) | Fetch        |
| `hotel_other_information/update`              | POST     | kernelApi      | Update hotel other info                    | Update       |

### Module: Manage Channel — Day Outing

| Endpoint                            | Method | Axios Instance | Purpose                         | Data Flow |
| ----------------------------------- | ------ | -------------- | ------------------------------- | --------- |
| `day-booking/active-packages`       | GET    | beApi          | List active day-outing packages | Fetch     |
| `day-booking/inactive-packages`     | GET    | beApi          | List inactive packages          | Fetch     |
| `day-booking/package-add`           | POST   | beApi          | Create package                  | Push      |
| `day-booking/package-update`        | POST   | beApi          | Update package                  | Update    |
| `day-booking/package-details`       | GET    | beApi          | Get package details             | Fetch     |
| `day-booking/package-status`        | POST   | beApi          | Toggle package status           | Update    |
| `day-booking/update-special-price`  | POST   | beApi          | Set special pricing             | Update    |
| `day-booking/delete-special-price`  | POST   | beApi          | Remove special pricing          | Push      |
| `day-booking/update-blackout-dates` | POST   | beApi          | Set blackout dates              | Update    |
| `day-booking/delete-blackout-dates` | POST   | beApi          | Remove blackout dates           | Push      |
| `day-booking/availability-calendar` | GET    | beApi          | Availability calendar           | Fetch     |
| `day-booking/booking-list`          | GET    | beApi          | Day booking list                | Fetch     |
| `day-booking/booking-details`       | GET    | beApi          | Day booking details             | Fetch     |
| `day-booking/menu-setup`            | POST   | beApi          | Configure menu                  | Update    |
| `day-booking/image-delete`          | POST   | beApi          | Delete package image            | Push      |

### Module: Manage Channel — PMS

| Endpoint                                     | Method | Axios Instance | Purpose                         | Data Flow |
| -------------------------------------------- | ------ | -------------- | ------------------------------- | --------- |
| `cm_pms_details/get_pms_hotel_code`          | GET    | kernelApi      | Get PMS hotel code              | Fetch     |
| `cm_pms_details/get_pms_booking_push_url`    | GET    | kernelApi      | Get PMS booking push URL        | Fetch     |
| `cm_pms_details/update_pms_booking_push_url` | POST   | kernelApi      | Update PMS push URL             | Update    |
| `cm_pms_details/get_pms_rooms`               | GET    | kernelApi      | Get PMS room types              | Fetch     |
| `cm_pms_details/add_pms_room`                | POST   | kernelApi      | Add PMS room type               | Push      |
| `cm_pms_details/save_pms_room_sync`          | POST   | kernelApi      | Save room sync mapping          | Push      |
| `cm_pms_details/get_pms_room_sync_all`       | GET    | kernelApi      | Get all room sync mappings      | Fetch     |
| `cm_pms_details/remove_pms_room_sync`        | POST   | kernelApi      | Remove room sync mapping        | Push      |
| `cm_pms_details/get_pms_rateplans`           | GET    | kernelApi      | Get PMS rate plans              | Fetch     |
| `cm_pms_details/get_pms_rateplan_sync_all`   | GET    | kernelApi      | Get all rate plan sync mappings | Fetch     |
| `cm_pms_details/save_pms_rateplan_sync`      | POST   | kernelApi      | Save rate plan sync mapping     | Push      |
| `cm_pms_details/add_pms_rateplan`            | POST   | kernelApi      | Add PMS rate plan               | Push      |
| `cm_pms_details/remove_pms_rateplan_sync`    | POST   | kernelApi      | Remove rate plan sync mapping   | Push      |
| `fetch_pms_rooms`                            | GET    | kernelApi      | Fetch PMS rooms                 | Fetch     |

### Module: Manage Channel — Agoda Onboarding

| Endpoint              | Method | Axios Instance | Purpose                        | Data Flow |
| --------------------- | ------ | -------------- | ------------------------------ | --------- |
| `gethoteldetails`     | GET    | cm3Api         | Get Agoda hotel details        | Fetch     |
| `onboard`             | POST   | cm3Api         | Onboard hotel with Agoda       | Push      |
| `getroomtype`         | GET    | cm3Api         | Get Agoda room types           | Fetch     |
| `getroomdetails`      | GET    | cm3Api         | Get room details               | Fetch     |
| `createroom`          | POST   | cm3Api         | Create Agoda room mapping      | Push      |
| `getrateplantype`     | GET    | cm3Api         | Get Agoda rate plan types      | Fetch     |
| `getrateplan`         | GET    | cm3Api         | Get selected rate plans        | Fetch     |
| `createrateplan`      | POST   | cm3Api         | Create Agoda rate plan mapping | Push      |
| `getmapedroom`        | GET    | cm3Api         | Get mapped rooms               | Fetch     |
| `createhotelproduct`  | POST   | cm3Api         | Create hotel product on Agoda  | Push      |
| `createdroom`         | GET    | cm3Api         | Get created room data          | Fetch     |
| `cancelpolicyforform` | GET    | cm3Api         | Get cancellation policy form   | Fetch     |
| `iscontractsigned`    | GET    | cm3Api         | Check Agoda contract status    | Fetch     |

### Module: Promotions

| Endpoint                              | Method | Axios Instance | Purpose                             | Data Flow |
| ------------------------------------- | ------ | -------------- | ----------------------------------- | --------- |
| `get-all-promotion`                   | GET    | cmApi          | List active promotions              | Fetch     |
| `get-all-promotion/last-minute`       | GET    | cmApi          | List Airbnb last-minute promotions  | Fetch     |
| `get-all-promotion/length-of-stay`    | GET    | cmApi          | List Airbnb LOS promotions          | Fetch     |
| `get-promotion-ota`                   | GET    | cmApi          | Get channels for promotions         | Fetch     |
| `get_room_type_rate_plans`            | GET    | cmApi          | Room type rate plans for promotions | Fetch     |
| `get_room_type_rate_plans_airbnb`     | GET    | cmApi          | Airbnb-specific rate plans          | Fetch     |
| `insert-hotel-promotion-new`          | POST   | cmApi          | Create promotion                    | Push      |
| `update-hotel-promotion-new`          | POST   | cmApi          | Update promotion                    | Update    |
| `update-hotel-promotion-airbnb`       | POST   | cmApi          | Update Airbnb promotion             | Update    |
| `deactivate-promotion-new`            | POST   | cmApi          | Deactivate promotion                | Update    |
| `activate-promotion-new`              | POST   | cmApi          | Activate promotion                  | Update    |
| `get-hotel-promotion`                 | GET    | cmApi          | Get promotion details               | Fetch     |
| `get-all-inactive-promotion`          | GET    | cmApi          | List inactive promotions            | Fetch     |
| `view-promotion`                      | GET    | cmApi          | View promotion details              | Fetch     |
| `advance-days`                        | GET    | cmApi          | Get advance booking day options     | Fetch     |
| `check-airbnb-status`                 | GET    | cmApi          | Check Airbnb connection status      | Fetch     |
| `airbnb/rule/apply`                   | POST   | cmApi          | Apply Airbnb promotion rule         | Push      |
| `airbnb/rule/create`                  | POST   | cmApi          | Create Airbnb rule                  | Push      |
| `airbnb/rule/get/{hotel_id}`          | GET    | cmApi          | Get active Airbnb rules             | Fetch     |
| `airbnb/rule/get/inactive/{hotel_id}` | GET    | cmApi          | Get inactive Airbnb rules           | Fetch     |
| `airbnb/rule/deactive`                | POST   | cmApi          | Deactivate Airbnb rule              | Update    |
| `airbnb/rule/remove`                  | POST   | cmApi          | Remove applied Airbnb rule          | Push      |
| `airbnb/viewlog`                      | GET    | cmApi          | View Airbnb rule application log    | Fetch     |

### Module: Reviews

| Endpoint                                                 | Method | Axios Instance | Purpose                              | Data Flow |
| -------------------------------------------------------- | ------ | -------------- | ------------------------------------ | --------- |
| `review/get_hotel_summary`                               | GET    | cmApi          | Aggregated review summary            | Fetch     |
| `review/allreview`                                       | GET    | cmApi          | All reviews across sources           | Fetch     |
| `review/ota-review-list`                                 | GET    | cmApi          | Reviews by OTA source                | Fetch     |
| `review/single-review`                                   | GET    | cmApi          | Single review detail                 | Fetch     |
| `review/replyreview`                                     | POST   | cmApi          | Reply to a review                    | Push      |
| `review/sync-all-review`                                 | POST   | cmApi          | Sync all reviews                     | Push      |
| `review/review-sources`                                  | GET    | cmApi          | Get review source list               | Fetch     |
| `review/gmb-status`                                      | GET    | cmApi          | Google My Business status            | Fetch     |
| `google-review/login`                                    | POST   | cmApi          | Google OAuth login for reviews       | Push      |
| `google-review/googlelogin`                              | POST   | cmApi          | Google login callback                | Push      |
| `google-review/logout`                                   | POST   | cmApi          | Google review logout                 | Push      |
| `google-review/fetchAccount`                             | GET    | cmApi          | Fetch Google accounts                | Fetch     |
| `google-review/fetchLocation`                            | GET    | cmApi          | Fetch Google business locations      | Fetch     |
| `google-review/setLocation`                              | POST   | cmApi          | Set Google business location         | Push      |
| `google-review/deleteReply`                              | POST   | cmApi          | Delete Google review reply           | Push      |
| `bookingdotcom-review/otareviewlist`                     | GET    | cmApi          | Booking.com review list              | Fetch     |
| `bookingdotcom-review/allreviews`                        | GET    | cmApi          | All Booking.com reviews              | Fetch     |
| `bookingdotcom-review/sync-review`                       | POST   | cmApi          | Sync Booking.com reviews             | Push      |
| `airbnb-review/sync-review`                              | POST   | cmApi          | Sync Airbnb reviews                  | Push      |
| `airbnb-review/allreviews`                               | GET    | cmApi          | All Airbnb reviews                   | Fetch     |
| `airbnb-review/single-review`                            | GET    | cmApi          | Single Airbnb review                 | Fetch     |
| `airbnb-review/reply-review`                             | POST   | cmApi          | Reply to Airbnb review               | Push      |
| `https://gmb-sse.bookingjini.com/review-store?hotel_id=` | SSE    | EventSource    | Real-time Google review store stream | Stream    |

### Module: Property Setup

| Endpoint                                                       | Method | Axios Instance | Purpose                      | Data Flow |
| -------------------------------------------------------------- | ------ | -------------- | ---------------------------- | --------- |
| `property_count_details`                                       | GET    | kernelApi      | Property completion score    | Fetch     |
| `property_score_percentage/{hotel_id}`                         | GET    | kernelApi      | Property setup percentage    | Fetch     |
| `hotel_admin/get_all_hotels_by_id/{hotel_id}`                  | GET    | kernelApi      | Get hotel details            | Fetch     |
| `update_new_property/{hotel_id}`                               | POST   | kernelApi      | Update basic details         | Update    |
| `update_property_address/{hotel_id}`                           | POST   | kernelApi      | Update address               | Update    |
| `getAllCountries`                                              | GET    | kernelApi      | Country list                 | Fetch     |
| `statedetails/getById/{country_id}`                            | GET    | kernelApi      | States by country            | Fetch     |
| `hotel_master_room_type/all/{hotel_id}`                        | GET    | kernelApi      | All room types               | Fetch     |
| `hotel_master_new_room_type/add`                               | POST   | kernelApi      | Create room type             | Push      |
| `hotel_master_new_room_type/delete/{id}`                       | DELETE | kernelApi      | Delete room type             | Push      |
| `update-basic-detail`                                          | POST   | kernelApi      | Update room type details     | Update    |
| `update-occupancy`                                             | POST   | kernelApi      | Update room type occupancy   | Update    |
| `update-room-rate-plan`                                        | POST   | kernelApi      | Update room rate plan        | Update    |
| `update-min-max-price`                                         | POST   | kernelApi      | Update min/max price         | Update    |
| `hotel_master_new_room_type/upload_room_type_images/{id}`      | POST   | kernelApi      | Upload room type images      | Push      |
| `hotel_master_new_room_type/delete_single_roomtype_image/{id}` | DELETE | kernelApi      | Delete room type image       | Push      |
| `update-room-type-image-order`                                 | POST   | kernelApi      | Reorder room type images     | Update    |
| `getFloors`                                                    | GET    | kernelApi      | Get all floors               | Fetch     |
| `saveFloors`                                                   | POST   | kernelApi      | Save floors                  | Push      |
| `getRooms2`                                                    | GET    | kernelApi      | Get rooms by floor           | Fetch     |
| `addRoom`                                                      | POST   | kernelApi      | Add room                     | Push      |
| `DeleteRoom`                                                   | POST   | kernelApi      | Delete room                  | Push      |
| `UpdateRoom`                                                   | POST   | kernelApi      | Update room                  | Update    |
| `UpdateRoomStatus`                                             | POST   | kernelApi      | Toggle room status           | Update    |
| `getAllPropertyAmenities`                                      | GET    | kernelApi      | All available amenities      | Fetch     |
| `hotel_amenities/hotelAmenity/{hotel_id}`                      | GET    | kernelApi      | Hotel's selected amenities   | Fetch     |
| `update_property_amenities`                                    | POST   | kernelApi      | Update amenities             | Update    |
| `remove_property_amenities`                                    | POST   | kernelApi      | Remove amenity               | Push      |
| `get_Images/{hotel_id}`                                        | GET    | kernelApi      | Property images              | Fetch     |
| `uploadhotelimages`                                            | POST   | kernelApi      | Upload property images       | Push      |
| `/file/managePropertyPhotos/remove`                            | POST   | kernelApi      | Delete property image        | Push      |
| `update-image-order`                                           | POST   | kernelApi      | Reorder images               | Update    |
| `hotel_policies/{hotel_id}`                                    | GET    | kernelApi      | Terms and policies           | Fetch     |
| `hotel_policies/update`                                        | POST   | kernelApi      | Update policies              | Update    |
| `be_cancellation_policy/fetch_cancellation_policy/{id}`        | GET    | kernelApi      | Cancellation rules           | Fetch     |
| `be_cancellation_policy/update_cancellation_policy`            | POST   | kernelApi      | Update cancellation rules    | Update    |
| `hotel_bank_account_details/{hotel_id}`                        | GET    | kernelApi      | Bank account details         | Fetch     |
| `locale-details/{hotel_id}`                                    | GET    | kernelApi      | Locale/currency info         | Fetch     |
| `master-rate-plan-details`                                     | GET    | kernelApi      | Master rate plan details     | Fetch     |
| `master_rate_plan/all/{hotel_id}`                              | GET    | kernelApi      | All rate plans               | Fetch     |
| `master_rate_plan/add`                                         | POST   | kernelApi      | Create rate plan             | Push      |
| `master_rate_plan/update/{id}`                                 | POST   | kernelApi      | Update rate plan             | Update    |
| `master_rate_plan/{id}`                                        | DELETE | kernelApi      | Delete rate plan             | Push      |
| `dynamic-pricing/all/{hotel_id}`                               | GET    | kernelApi      | Dynamic pricing rules        | Fetch     |
| `dynamic-pricing/add`                                          | POST   | kernelApi      | Add dynamic pricing rule     | Push      |
| `dynamic-pricing/update`                                       | POST   | kernelApi      | Update dynamic pricing rule  | Update    |
| `dynamic-pricing/delete/{id}`                                  | DELETE | kernelApi      | Delete dynamic pricing rule  | Push      |
| `dynamic-pricing/one/{id}`                                     | GET    | kernelApi      | Specific pricing rule        | Fetch     |
| `one-click-setup/isoneclicksetupallow/{id}`                    | GET    | kernelApi      | Check one-click eligibility  | Fetch     |
| `one-click-setup/validatehotelinfo/`                           | POST   | kernelApi      | Validate hotel for one-click | Fetch     |
| `one-click-setup/fetchhoteldetails`                            | POST   | kernelApi      | Fetch OTA hotel details      | Fetch     |
| `one-click-setup/storehoteldetails`                            | POST   | kernelApi      | Store imported hotel details | Push      |
| `one-click-setup/fetchroomdetails`                             | POST   | kernelApi      | Fetch OTA room details       | Fetch     |
| `one-click-setup/storeroomdetails`                             | POST   | kernelApi      | Store imported room details  | Push      |
| `one-click-setup/fetchratedetails`                             | POST   | kernelApi      | Fetch OTA rate details       | Fetch     |
| `one-click-setup/storeratedetails`                             | POST   | kernelApi      | Store imported rate details  | Push      |
| `one-click-setup/fetchphotodetails`                            | POST   | kernelApi      | Fetch OTA photos             | Fetch     |
| `one-click-setup/storephotodetails`                            | POST   | kernelApi      | Store imported photos        | Push      |
| `get_added_channels_for_seasonal_plan`                         | GET    | cmApi          | Channels for seasonal plan   | Fetch     |

### Module: Performance & Analytics

| Endpoint                                   | Method | Axios Instance | Purpose                         | Data Flow |
| ------------------------------------------ | ------ | -------------- | ------------------------------- | --------- |
| `direct-booking-performance-new`           | POST   | kernelApi      | Direct booking performance data | Fetch     |
| `booking-status-performance`               | POST   | kernelApi      | Booking status breakdown        | Fetch     |
| `download-direct-booking-performance`      | POST   | kernelApi      | Download performance report     | Fetch     |
| `channel-wise-checkin-report`              | POST   | kernelApi      | Channel-wise check-in data      | Fetch     |
| `download-channel-wise-checkin-report`     | POST   | kernelApi      | Download check-in report        | Fetch     |
| `month-wise-room-nights`                   | POST   | kernelApi      | Monthly room nights             | Fetch     |
| `yoy-monthly-room-nights`                  | POST   | kernelApi      | YoY monthly comparison          | Fetch     |
| `yoy-quarterly-room-nights`                | POST   | kernelApi      | YoY quarterly comparison        | Fetch     |
| `onboarding-date`                          | GET    | kernelApi      | Property onboarding date        | Fetch     |
| `get-performance-report-url`               | GET    | kernelApi      | Performance report URL          | Fetch     |
| `market-insight/areademanddata`            | POST   | kernelApi      | Booking.com area demand         | Fetch     |
| `market-insight/bookerinsightdata`         | POST   | kernelApi      | Booking.com booker insights     | Fetch     |
| `market-insight/bookwindowdata`            | POST   | kernelApi      | Booking window analysis         | Fetch     |
| `market-insight/pacereportdata`            | POST   | kernelApi      | Pace report                     | Fetch     |
| `market-insight/salesstatisticsreportdata` | POST   | kernelApi      | Sales statistics                | Fetch     |
| `market-insight/checkstatus`               | GET    | kernelApi      | Market insight availability     | Fetch     |
| `marketing-engine/get-report-list`         | GET    | kernelApi      | Marketing engine reports        | Fetch     |

### Module: Subscription & Billing

| Endpoint                            | Method | Axios Instance  | Purpose                       | Data Flow |
| ----------------------------------- | ------ | --------------- | ----------------------------- | --------- |
| `property-subscription-status`      | POST   | subscriptionApi | Check subscription status     | Fetch     |
| `my-subscription`                   | POST   | subscriptionApi | Current subscription details  | Fetch     |
| `get-tiers`                         | GET    | subscriptionApi | Available tiers               | Fetch     |
| `get-packages`                      | GET    | subscriptionApi | Available packages            | Fetch     |
| `get-plans`                         | GET    | subscriptionApi | Available plans               | Fetch     |
| `get-zoho-plans`                    | GET    | subscriptionApi | Zoho plans                    | Fetch     |
| `subscribe-plan`                    | POST   | subscriptionApi | Subscribe to plan             | Push      |
| `get-checkout-url`                  | POST   | subscriptionApi | Chargebee checkout URL        | Fetch     |
| `get-zoho-checkout-url`             | POST   | subscriptionApi | Zoho checkout URL             | Fetch     |
| `get-checkout-url-for-upgrade`      | POST   | subscriptionApi | Chargebee upgrade URL         | Fetch     |
| `get-zoho-checkout-url-for-upgrade` | POST   | subscriptionApi | Zoho upgrade URL              | Fetch     |
| `get-checkout-url-for-renewal`      | POST   | subscriptionApi | Chargebee renewal URL         | Fetch     |
| `get-zoho-checkout-url-for-renewal` | POST   | subscriptionApi | Zoho renewal URL              | Fetch     |
| `cancel-subscription`               | POST   | subscriptionApi | Cancel Chargebee subscription | Push      |
| `cancel-zoho-subscription`          | POST   | subscriptionApi | Cancel Zoho subscription      | Push      |
| `get-all-upgradable-plans`          | GET    | subscriptionApi | Upgradable plans              | Fetch     |
| `my-subscribed-plan`                | GET    | subscriptionApi | Subscribed plan details       | Fetch     |
| `validate-subscription-coupon`      | POST   | subscriptionApi | Validate coupon code          | Fetch     |
| `all-property-subscription-status`  | POST   | subscriptionApi | All properties' status        | Fetch     |
| `get-autodebit-authorization-url`   | POST   | subscriptionApi | Auto-debit auth URL           | Fetch     |
| `get-upi-autodebit-authorization`   | POST   | subscriptionApi | UPI auto-debit auth           | Fetch     |
| `cancel-autodebit`                  | POST   | subscriptionApi | Cancel auto-debit             | Push      |
| `make-subscription-payment`         | POST   | subscriptionApi | Manual payment                | Push      |
| `get-hotel-user-profile`            | GET    | subscriptionApi | User profile                  | Fetch     |

### Module: User Management

| Endpoint                           | Method | Axios Instance | Purpose                        | Data Flow |
| ---------------------------------- | ------ | -------------- | ------------------------------ | --------- |
| `manage_user/all/{company_id}`     | GET    | kernelApi      | List all users                 | Fetch     |
| `manage_user/add`                  | POST   | kernelApi      | Add user                       | Push      |
| `manage_user/update`               | POST   | kernelApi      | Update user                    | Update    |
| `manage_user/delete-user/{id}`     | GET    | kernelApi      | Delete user                    | Push      |
| `hotel_admin/get_hotel_list`       | GET    | kernelApi      | Hotel list for user assignment | Fetch     |
| `get-user-access/{user_id}`        | GET    | kernelApi      | Get user access permissions    | Fetch     |
| `get-access-functionality`         | GET    | kernelApi      | Get all access functions       | Fetch     |
| `user-access-updation-or-creation` | POST   | kernelApi      | Update user access             | Push      |

### Module: Notifications

| Endpoint                | Method | Axios Instance | Purpose                   | Data Flow |
| ----------------------- | ------ | -------------- | ------------------------- | --------- |
| `get-notification-logs` | GET    | kernelApi      | Get notification list     | Fetch     |
| `unread-message`        | GET    | kernelApi      | Unread notification count | Fetch     |
| `update-viewer-status`  | POST   | kernelApi      | Mark notification as read | Update    |

### Module: Tickets & FAQ

| Endpoint              | Method | Axios Instance      | Purpose                | Data Flow |
| --------------------- | ------ | ------------------- | ---------------------- | --------- |
| `ticket/raise`        | POST   | kernelApi           | Submit new ticket      | Push      |
| `ticket/list`         | POST   | kernelApi           | List tickets by status | Fetch     |
| `ticket/view-details` | POST   | kernelApi           | Ticket detail view     | Fetch     |
| `ticket/add-comment`  | POST   | kernelApi           | Reply to ticket        | Push      |
| `ticket/close`        | POST   | kernelApi           | Close ticket           | Update    |
| `faq/search?q=`       | GET    | External (datamart) | Search FAQs            | Fetch     |
| `faq/initial`         | GET    | External (datamart) | Initial FAQ list       | Fetch     |
| `faq`                 | GET    | External (datamart) | FAQ details            | Fetch     |
| `faq/categories`      | GET    | External (datamart) | FAQ categories         | Fetch     |
| `faq/question`        | POST   | External (datamart) | Submit question        | Push      |
| `get-contact-us-info` | GET    | kernelApi           | Contact support info   | Fetch     |

### Module: AI Content Generation

| Endpoint                                                    | Method | Axios Instance | Purpose                          | Data Flow |
| ----------------------------------------------------------- | ------ | -------------- | -------------------------------- | --------- |
| `https://kernel.bookingjini.com/extranetv4/genai/room?`     | GET    | Direct         | AI-generate room description     | Fetch     |
| `https://kernel.bookingjini.com/extranetv4/genai/property?` | GET    | Direct         | AI-generate property description | Fetch     |

---

## Integrations

### OTA / Channel Integrations

| Integration | Type        | How Connected                   | APIs Used                                                                                                                                  |
| ----------- | ----------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Booking.com | OTA Channel | Via CM room/rate sync mapping   | `cm_ota_*` endpoints, `bookingdotcom-review/*`, `bookingdotcom-messaging/*`, `market-insight/*`                                            |
| Airbnb      | OTA Channel | OAuth 2.0 flow + CM mapping     | `air-bnb/save-code`, `get-all-airbnb-listing`, `airbnb/rule/*`, `airbnb-review/*`, Airbnb OAuth URL                                        |
| Agoda       | OTA Channel | Dedicated onboarding via cm3Api | All `AGODAONBOARDING` endpoints via cm3Api                                                                                                 |
| Expedia     | OTA Channel | Via CM room/rate sync mapping   | `cm_ota_*` endpoints, `expedia-messaging/*`, `check-expedia-booking-update-status`, `fetch-expedia-reasons`, `update-expedia-reservations` |
| Goibibo     | OTA Channel | Via CM room/rate sync mapping   | `cm_ota_*` endpoints (generic OTA flow)                                                                                                    |
| MakeMyTrip  | OTA Channel | Via CM room/rate sync mapping   | `cm_ota_*` endpoints (generic OTA flow)                                                                                                    |
| Other OTAs  | OTA Channel | Via CM generic OTA flow         | `cm_ota_*` endpoints, `getallchannel` lists all supported OTAs                                                                             |

### Payment Gateway Integrations

| Integration        | Type                 | APIs Used                                                                                                                           |
| ------------------ | -------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| Easebuzz           | Payment Gateway      | `onpage-easebuzz-response`, `payout-paymentgateway-response`, `handleEasebuzPayment()` in UtilityFunctions                          |
| Razorpay           | Payment Gateway      | `onpage-razorpay-response`, `get-payment-gateways`, `update-paymentgateway-setup`                                                   |
| PayU               | Payment Gateway      | `PaymentOptions/PayUSetupPayment.tsx`, `CheckHavingPayU.tsx`                                                                        |
| Chargebee          | Subscription Billing | `get-checkout-url`, `get-checkout-url-for-upgrade`, `get-checkout-url-for-renewal`, `cancel-subscription`                           |
| Zoho Subscriptions | Subscription Billing | `get-zoho-checkout-url`, `get-zoho-plans`, `cancel-zoho-subscription`, `get-zoho-payment-method-update-url`, `get-zoho-saved-cards` |

### Third-Party Service Integrations

| Integration        | Type              | How Used                                                                                                       |
| ------------------ | ----------------- | -------------------------------------------------------------------------------------------------------------- |
| Google OAuth       | Authentication    | `@react-oauth/google` — Google login for user auth                                                             |
| Google My Business | Reviews           | OAuth login → fetch accounts → fetch locations → sync reviews → reply. SSE stream at `gmb-sse.bookingjini.com` |
| Google Maps        | Property Setup    | `react-google-autocomplete` for address, `google-map-react` for map display                                    |
| Google reCAPTCHA   | Security          | `react-google-recaptcha` on login/signup                                                                       |
| Google Analytics   | Tracking          | `react-ga` with `RouteChangeTracker` on every navigation                                                       |
| Tawk.to            | Live Chat         | `@tawk.to/tawk-messenger-react` (currently commented out)                                                      |
| ZapScale           | Product Analytics | Customer success tracking (currently commented out)                                                            |
| CKEditor           | Rich Text         | `ckeditor5-react` for ticket replies                                                                           |
| Lottie             | Animations        | `lottie-react-web` for success/empty state animations                                                          |

### PMS Integrations

| Integration | Type                       | APIs Used                                                                                     |
| ----------- | -------------------------- | --------------------------------------------------------------------------------------------- |
| Generic PMS | Property Management System | All `cm_pms_details/*` endpoints — room mapping, rate mapping, booking push URL configuration |

---

## Data Flow Summary

### Primary Data Flow Pattern

```
Frontend (React) ──HTTP──> Backend Microservices ──> Database / OTA APIs
                                                         │
                                                         ▼
                                                   OTA Channels
                                              (Booking.com, Airbnb,
                                               Agoda, Expedia, etc.)
```

The frontend is a pure consumer — it reads data and pushes updates to the backend. The backend handles all OTA communication, sync operations, and webhook reception. The frontend never communicates directly with OTA APIs (except Airbnb OAuth redirect).

### Data Flow by Operation Type

| Operation              | Flow Direction                | Pattern                                                                     |
| ---------------------- | ----------------------------- | --------------------------------------------------------------------------- |
| Read/Fetch             | Backend → Frontend            | GET requests, on-demand per page load                                       |
| Create                 | Frontend → Backend            | POST requests, form submissions                                             |
| Update                 | Frontend → Backend            | POST requests (no PUT/PATCH used)                                           |
| Delete                 | Frontend → Backend            | POST or GET requests (inconsistent)                                         |
| Sync (Inventory/Rates) | Frontend → Backend → OTAs     | POST triggers backend to push to OTAs                                       |
| Review Sync            | Frontend → Backend → OTA APIs | POST triggers backend to pull from OTAs                                     |
| File Upload            | Frontend → Backend            | POST with FormData                                                          |
| PDF Generation         | Frontend → Backend            | POST, returns PDF data/URL                                                  |
| Payment Processing     | Frontend → Backend → Gateway  | Redirect-based (Chargebee/Zoho hosted pages) or in-page (Easebuzz/Razorpay) |

### Real-Time / Async Patterns

| Pattern                  | Where Used                            | Implementation                                                                                                       |
| ------------------------ | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| Server-Sent Events (SSE) | Google Business Reviews               | `EventSource` at `gmb-sse.bookingjini.com/review-store` — streams review count during initial sync, closes on "DONE" |
| Polling (60s interval)   | Booking Messaging (Expedia & general) | `setInterval(() => fetchMessages(), 60000)` — polls for new messages every 60 seconds                                |
| Countdown Timer          | Agoda Rate Plan creation              | `setInterval` for OTP countdown timer                                                                                |
| Debounced Search         | Property address autocomplete         | `setTimeout(() => getMapSuggestions(), 300)` — 300ms debounce                                                        |

### No WebSockets, No Push Notifications

The application has no WebSocket connections, no service workers, and no push notification infrastructure. All data freshness relies on:

1. Page-load fetches (most common)
2. Manual refresh triggers (user clicks refresh/sync)
3. The 60-second polling for messaging only

### API Call Volume per Navigation

Every route change triggers:

1. `GET get-user-access/{admin_id}` — RBAC check (kernelApi)
2. `POST property-subscription-status` — Subscription check (subscriptionApi)
3. Page-specific data fetches (1-5 additional calls depending on the page)

This means a minimum of 3 API calls per navigation, with complex pages like Inventory or Rates making 5-8 calls on load.

---

## Observations

1. **No API abstraction layer**: Components call Axios instances directly with inline `async/await`. No service classes, no React Query, no SWR. This leads to duplicated error handling, loading state management, and retry logic across 100+ components.

2. **Inconsistent HTTP methods**: Deletes use GET (`manage_user/delete-user/{id}`), POST (`DeleteRoom`), or actual DELETE (`master_rate_plan/{id}`). Updates universally use POST instead of PUT/PATCH.

3. **Dual-path updates**: Inventory and Rate updates always require two separate API calls — one to `beApi` (Booking Engine) and one to `cmApi` (Channel Manager). This is handled with parallel promises in components but has no shared abstraction.

4. **Hardcoded external URLs**: The rate shopper URL (`staging.bookingjini.com`), AI generation URLs, FAQ base URL, and SSE URL are hardcoded strings rather than environment-configured.

5. **Subscription API key hardcoded**: `dispatch(saveSubscriptionKey("Vz9P1vfwj6P6hiTCc9ddC5bNieqA5ScT"))` is hardcoded in `AppRoutes.tsx`.

6. **Legacy + new endpoints coexist**: `INVENTORY` and `INVENTORY_NEW` sections contain overlapping endpoints (old vs new API versions). Same for `RATE` and `RATES_NEW`. The old endpoints are still referenced in some components.

7. **No request caching**: Frequently-accessed data like room types, rate plans, and channel lists are re-fetched on every page load with no caching strategy.

8. **No request cancellation**: No `AbortController` usage — navigating away from a page doesn't cancel in-flight requests, which can cause state updates on unmounted components.

9. **`toolsAPI` is redundant**: Points to the same base URL as `kernelApi` but lacks the 401 auto-logout interceptor, creating an inconsistency.

10. **Total unique endpoints**: ~350+ unique API endpoints across all modules, making this a substantial API surface area for a single frontend application.
