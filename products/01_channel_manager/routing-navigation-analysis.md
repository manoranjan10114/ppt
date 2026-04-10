# Channel Manager Extranet — Routing, Navigation & Access Control Analysis

---

## Routes Mapping

### Route Group 1: AuthRoutes (Unauthenticated)

Rendered when `auth_token` is falsy OR when an intranet SSO deep-link is detected.

| Route                                                                                                                    | Page/Component               | Module              |
| ------------------------------------------------------------------------------------------------------------------------ | ---------------------------- | ------------------- |
| `/login`                                                                                                                 | `Login.tsx`                  | Authentication      |
| `/signup`                                                                                                                | `SignUp.tsx`                 | Authentication      |
| `/signup-form`                                                                                                           | `SignUpForm.tsx`             | Authentication      |
| `/signup-validation`                                                                                                     | `SignUpValidation.tsx`       | Authentication      |
| `/reset-password`                                                                                                        | `ResetPassword.tsx`          | Authentication      |
| `/new-password`                                                                                                          | `EnterNewPassword.tsx`       | Authentication      |
| `/:company_id/:comp_hash/:hotel_id/:admin_id/:auth_token/:full_name/:subscription_customer_id/:company_url/:source_name` | `Loginwithoutcredential.tsx` | SSO Deep-Link Login |
| `*` (catch-all)                                                                                                          | `Navigate → /login`          | Redirect            |

### Route Group 2: NewUserRoutes (Authenticated, No Property Selected)

Rendered when `auth_token` exists but `current_property` is null/undefined.

| Route                                  | Page/Component             | Module              |
| -------------------------------------- | -------------------------- | ------------------- |
| `/`                                    | `SelectProperty.tsx`       | Property Selection  |
| `/select-property`                     | `SelectProperty.tsx`       | Property Selection  |
| `/add-new-property`                    | `Navigate → property-type` | Add Property Wizard |
| `/add-new-property/property-type`      | `PropertyType.tsx`         | Add Property Wizard |
| `/add-new-property/property-subtype`   | `PropertySubType.tsx`      | Add Property Wizard |
| `/add-new-property/property-details`   | `PropertyDetails.tsx`      | Add Property Wizard |
| `/add-new-property/property-address`   | `PropertyMapAddress.tsx`   | Add Property Wizard |
| `/add-new-property/property-amenities` | `PropertyAmenities.tsx`    | Add Property Wizard |
| `/add-new-property/property-images`    | `PropertyImages.tsx`       | Add Property Wizard |
| `/add-new-property/property-overview`  | `PropertyOverview.tsx`     | Add Property Wizard |
| `/add-new-property/success`            | `AddPropertySuccess.tsx`   | Add Property Wizard |
| `*` (catch-all)                        | `Error.tsx`                | Error               |

### Route Group 3: AppRoutes (Authenticated + Property Selected)

Rendered when `auth_token` exists AND `current_property` is set. Wrapped in a subscription check — redirects to `/choose-plan` if no plan or expired plan.

#### Inside DefaultLayout (Sidebar + Header shell)

| Route            | Page/Component              | Module        | RBAC Code |
| ---------------- | --------------------------- | ------------- | --------- |
| `/`              | `Dashboard.tsx`             | Dashboard     | —         |
| `/faq`           | `Faq.tsx`                   | FAQ           | —         |
| `/tickets`       | `Tickets.tsx`               | Tickets       | —         |
| `/tickets/:id`   | `TicketDetails.tsx`         | Tickets       | —         |
| `/notifications` | `NotificationPage.tsx`      | Notifications | —         |
| `/guests`        | Placeholder ("Coming soon") | —             | —         |
| `/campaigns`     | Placeholder ("Coming soon") | —             | —         |
| `/analytics`     | Placeholder ("Coming soon") | —             | —         |

#### Inventory Routes

| Route                               | Page/Component           | Module    | RBAC Code   |
| ----------------------------------- | ------------------------ | --------- | ----------- |
| `/inventory`                        | `InventoryRedirect.tsx`  | Inventory | `INVENTORY` |
| `/inventory/room-type-view`         | `Inventory.tsx`          | Inventory | `INVENTORY` |
| `/inventory/inventory-channel-type` | `ViewInvChannelType.tsx` | Inventory | `INVENTORY` |

#### Rates Routes

| Route                   | Page/Component      | Module | RBAC Code |
| ----------------------- | ------------------- | ------ | --------- |
| `/rates`                | `RatesRedirect.tsx` | Rates  | `RATE`    |
| `/rates/room-type-view` | `Rate.tsx`          | Rates  | `RATE`    |
| `/rates/channel-view`   | `RoomTypeRates.tsx` | Rates  | `RATE`    |

#### Bookings Routes

| Route                               | Page/Component             | Module          | RBAC Code |
| ----------------------------------- | -------------------------- | --------------- | --------- |
| `/bookings`                         | `Navigate → list-view`     | Bookings        | —         |
| `/bookings/list-view`               | `ListView.tsx`             | Bookings        | —         |
| `/bookings/crs-view`                | `CrsView.tsx`              | Bookings        | —         |
| `/bookings/gems/frontoffice-view`   | `FrontofficeViewGems.tsx`  | Bookings (GEMS) | —         |
| `/bookings/frontoffice-view`        | `NoFrontOfficeView.tsx`    | Bookings        | —         |
| `/bookings/check-in`                | `CheckIn.tsx`              | Bookings        | —         |
| `/bookings/stay-details`            | `StayDetails.tsx`          | Bookings        | —         |
| `/bookings/charge-card-status/:msg` | `ChargeCardStatusPage.tsx` | Bookings        | —         |
| `/bookings/unpaid-bookings`         | `UnpaidBookings.tsx`       | Bookings        | —         |
| `/bookings/payment-link`            | `QuickPaymentLink.tsx`     | Bookings        | —         |
| `/bookings/reservation`             | `Reservation.tsx`          | Bookings        | —         |

#### Manage Channels Routes

| Route                                                                         | Page/Component                           | Module                | RBAC Code |
| ----------------------------------------------------------------------------- | ---------------------------------------- | --------------------- | --------- |
| `/manage-channels`                                                            | `ManageChannel.tsx`                      | Manage Channel        | `BE`      |
| `/manage-channels/booking-engine`                                             | `BookingEngine.tsx`                      | Booking Engine        | `BE`      |
| `/manage-channels/booking-engine/basic-setup`                                 | `BasicSetup.tsx`                         | BE – Basic Setup      | `BE`      |
| `/manage-channels/booking-engine/nearest-location`                            | `NearestLocation.tsx`                    | BE – Locations        | `BE`      |
| `/manage-channels/booking-engine/nearest-places`                              | `NearestPlaces.tsx`                      | BE – Places           | `BE`      |
| `/manage-channels/booking-engine/package-list`                                | `PackageList.tsx`                        | BE – Packages         | `BE`      |
| `/manage-channels/booking-engine/paid-service`                                | `Paidservice.tsx`                        | BE – Paid Services    | `BE`      |
| `/manage-channels/booking-engine/paid-service/reports`                        | `PaidServiceReports.tsx`                 | BE – Paid Services    | `BE`      |
| `/manage-channels/booking-engine/notification-popup`                          | `NotificationPopup.tsx`                  | BE – Notifications    | `BE`      |
| `/manage-channels/booking-engine/promotional-slider`                          | `PromotionSlider.tsx`                    | BE – Promo Slider     | `BE`      |
| `/manage-channels/booking-engine/promotion`                                   | `BePromotion.tsx`                        | BE – Promotions       | `BE`      |
| `/manage-channels/booking-engine/promotion/basic-promotion`                   | `Coupon.tsx`                             | BE – Coupons (v3)     | `BE`      |
| `/manage-channels/booking-engine/promotion/basic-promotion-inactive`          | `InactivBasicCoupon.tsx`                 | BE – Coupons (v3)     | `BE`      |
| `/manage-channels/booking-engine/promotion/early-bird-promotion`              | `BeEarlyBird.tsx`                        | BE – Early Bird (v3)  | `BE`      |
| `/manage-channels/booking-engine/promotion/early-bird-promotion-inactive`     | `InactiveBeEarlyBirdPromotion.tsx`       | BE – Early Bird (v3)  | `BE`      |
| `/manage-channels/booking-engine/promotion/lastminute-promotion`              | `BeLastMinute.tsx`                       | BE – Last Min (v3)    | `BE`      |
| `/manage-channels/booking-engine/promotion/lastminute-promotion-inactive`     | `InactiveBeLastMinPromotion.tsx`         | BE – Last Min (v3)    | `BE`      |
| `/manage-channels/booking-engine/promotion/same-day-promotion`                | `BeSameDayPromotion.tsx`                 | BE – Same Day (v3)    | `BE`      |
| `/manage-channels/booking-engine/promotion/same-day-promotion-inactive`       | `InactiveBeSameDayPromotion.tsx`         | BE – Same Day (v3)    | `BE`      |
| `/manage-channels/booking-engine/promotion/length-of-stay-promotion`          | `BeLengthOfStayPromotion.tsx`            | BE – LOS (v3)         | `BE`      |
| `/manage-channels/booking-engine/promotion/length-of-stay-promotion-inactive` | `InactiveBeLengthOfStayPromotion.tsx`    | BE – LOS (v3)         | `BE`      |
| `/manage-channels/booking-engine/private-coupon`                              | `BePrivateCoupon.tsx`                    | BE – Coupons (v3)     | `BE`      |
| `/manage-channels/booking-engine/coupon`                                      | `PublicPrivateCoupon.tsx`                | BE – Coupons (non-v3) | `BE`      |
| `/manage-channels/booking-engine/cancellation-rules`                          | `PropertyCancellationRules.tsx`          | BE – Cancellation     | `BE`      |
| `/manage-channels/booking-engine/transactions`                                | `BookingjiniPaymentGatewayCommision.tsx` | BE – Commissions      | `BE`      |
| `/manage-channels/booking-engine/website-widget`                              | `WebsiteWidget.tsx`                      | BE – Widget           | `BE`      |
| `/manage-channels/booking-engine/payment-options`                             | `PaymentOptions.tsx`                     | BE – Payments         | `BE`      |
| `/manage-channels/booking-engine/additional-charges`                          | `AdditionalCharges.tsx`                  | BE – Charges          | `BE`      |
| `/manage-channels/booking-engine/room-rate-plan`                              | `RoomRatePlan.tsx`                       | BE – Rate Plans       | `BE`      |
| `/manage-channels/booking-engine/taxes`                                       | `Taxes.tsx`                              | BE – Taxes            | `BE`      |
| `/manage-channels/booking-engine/instant-booking-widget`                      | `InstantBookingWidget.tsx`               | BE – Widget           | `BE`      |
| `/manage-channels/booking-engine/day-booking`                                 | `DayOutingPackage.tsx`                   | BE – Day Outing       | `BE`      |
| `/manage-channels/booking-engine/day-booking-list`                            | `DayBookingList.tsx`                     | BE – Day Outing       | `BE`      |
| `/manage-channels/booking-engine/day-booking-list/coupon`                     | `DayoutingPromotion.tsx`                 | BE – Day Outing       | `BE`      |
| `/manage-channels/booking-engine/day-booking-calender/:package_id`            | `DayOutingCalender.tsx`                  | BE – Day Outing       | `BE`      |
| `/manage-channels/booking-engine/cancellation-policy`                         | `CancellationPolicy.tsx`                 | BE – Cancellation     | `BE`      |
| `/manage-channels/booking-engine/plugins`                                     | `BookingEnginePlugins.tsx`               | BE – Plugins          | `BE`      |
| `/manage-channels/pms-manage`                                                 | `PMSManage.tsx`                          | PMS Integration       | —         |
| `/manage-channels/agoda-ota-manage`                                           | `AgodaSetup.tsx` (lazy)                  | Agoda Onboarding      | —         |
| `/manage-channels/agoda-ota-manage/agoda-onboarding`                          | `AgodaOnBoarding.tsx` (lazy)             | Agoda Onboarding      | —         |
| `/manage-channels/agoda-ota-manage/create-roomplan`                           | `CreateAgodaRoomplan.tsx` (lazy)         | Agoda Onboarding      | —         |
| `/manage-channels/agoda-ota-manage/create-rateplan`                           | `CreateAgodaRateplan.tsx` (lazy)         | Agoda Onboarding      | —         |
| `/manage-channels/ota-manage`                                                 | `OtaManage.tsx`                          | OTA Management        | —         |
| `/manage-channels/ota-manage/ota-basic-setup`                                 | `OtaBasicSetup.tsx`                      | OTA Management        | —         |
| `/manage-channels/ota-manage/rate-sync`                                       | `RateSync.tsx`                           | OTA Management        | —         |
| `/manage-channels/ota-manage/room-sync`                                       | `RoomSync.tsx`                           | OTA Management        | —         |

#### Reviews Routes

| Route                              | Page/Component              | Module  | RBAC Code |
| ---------------------------------- | --------------------------- | ------- | --------- |
| `/reviews`                         | `ReviewSummery.tsx`         | Reviews | —         |
| `/reviews/google-reviews`          | `GoogleReviewManage.tsx`    | Reviews | —         |
| `/reviews/setup`                   | `Reviews.tsx`               | Reviews | —         |
| `/reviews/reviews-summery-list`    | `ReviewSummertyList.tsx`    | Reviews | —         |
| `/reviews/select-business`         | `SelectBusiness.tsx`        | Reviews | —         |
| `/reviews/google-business-reviews` | `GoogleBusinessReviews.tsx` | Reviews | —         |
| `/reviews/bookingdotcom-reviews`   | `BookingReview.tsx`         | Reviews | —         |
| `/reviews/airbnb-reviews`          | `AirbnbReview.tsx`          | Reviews | —         |

#### Property Setup Routes

| Route                                                                           | Page/Component                  | Module         | RBAC Code |
| ------------------------------------------------------------------------------- | ------------------------------- | -------------- | --------- |
| `/property-setup`                                                               | `PropertySetup.tsx`             | Property Setup | —         |
| `/property-setup/basic-details`                                                 | `PropertyBasicDetails.tsx`      | Property Setup | `BE`      |
| `/property-setup/room-types`                                                    | `PropertyRoomTypes.tsx`         | Property Setup | `ROOM`    |
| `/property-setup/floors`                                                        | `PropertyFloors.tsx`            | Property Setup | `BE`      |
| `/property-setup/rooms`                                                         | `PropertyRooms.tsx`             | Property Setup | —         |
| `/property-setup/amenities`                                                     | `PropertyAmenities.tsx`         | Property Setup | `HOTEL`   |
| `/property-setup/images`                                                        | `PropertyImages.tsx`            | Property Setup | `HOTEL`   |
| `/property-setup/terms-and-policy`                                              | `PropertyTermsPolicy.tsx`       | Property Setup | `HOTEL`   |
| `/property-setup/financial-details`                                             | `PropertyFinancialDetails.tsx`  | Property Setup | `HOTEL`   |
| `/property-setup/cancellation-rules`                                            | `PropertyCancellationRules.tsx` | Property Setup | —         |
| `/property-setup/rate-plan`                                                     | `RatePlan.tsx`                  | Property Setup | `BE`      |
| `/property-setup/locale-info`                                                   | `LocaleInfo.tsx`                | Property Setup | —         |
| `/property-setup/seasonal-plan`                                                 | `PropertySeasonalPLan.tsx`      | Property Setup | `BE`      |
| `/property-setup/seasonal-plan/setup-seasonal-plan/:roomTypeId/:seasonalPlanId` | `SetupSeasonalPlan.tsx`         | Property Setup | `BE`      |
| `/property-setup/dynamic-pricing`                                               | `DynamicPricingSetup.tsx`       | Property Setup | `BE`      |
| `/property-setup/room-rate-plan`                                                | `DerivedRatePlan.tsx`           | Property Setup | `BE`      |
| `/property-setup/one-click-setup-ota`                                           | `OneClickSetupOta.tsx`          | Property Setup | —         |

#### Other Module Routes

| Route                                          | Page/Component                   | Module          | RBAC Code |
| ---------------------------------------------- | -------------------------------- | --------------- | --------- |
| `/manage-users`                                | `ManageUsers.tsx`                | User Management | —         |
| `/manage-subscription`                         | `ManageSubscription.tsx`         | Subscription    | —         |
| `/manage-subscription/upgrade-subscription`    | `UpgradeSubscription.tsx`        | Subscription    | —         |
| `/apps`                                        | `AppList.tsx`                    | Apps            | —         |
| `/apps/app-details`                            | `AppDetails.tsx`                 | Apps            | —         |
| `/mart`                                        | `Mart.tsx`                       | Mart            | —         |
| `/file-manager/:id`                            | `FileManager.tsx`                | File Manager    | `BE`      |
| `/performance`                                 | `PerformanceHomeScreen.tsx`      | Performance     | —         |
| `/performance/year-in-review`                  | `PerformanceYearInReview.tsx`    | Performance     | —         |
| `/performance/booking-dot-com-market-inshight` | `BookingDotComMarketInSight.tsx` | Performance     | —         |

#### Promotion Routes

| Route                                        | Page/Component                     | Module            | RBAC Code |
| -------------------------------------------- | ---------------------------------- | ----------------- | --------- |
| `/promotion`                                 | `PromotionTypes.tsx`               | Promotions        | —         |
| `/promotion/basic-promotion`                 | `BasicPromotion.tsx`               | Promotions        | `BE`      |
| `/promotion/inactive-basic-promotion`        | `InactiveBasicPromotion.tsx`       | Promotions        | `BE`      |
| `/promotion/early-bird`                      | `EarlyBird.tsx`                    | Promotions        | `BE`      |
| `/promotion/inactive-early-bird-promotion`   | `InactiveEarlyBirdPromotion.tsx`   | Promotions        | `BE`      |
| `/promotion/same-day`                        | `SameDay.tsx`                      | Promotions        | `BE`      |
| `/promotion/inactive-same-day-promotion`     | `InactiveSameDayPromotion.tsx`     | Promotions        | `BE`      |
| `/promotion/multi-nights`                    | `MultiNights.tsx`                  | Promotions        | `BE`      |
| `/promotion/inactive-multi-nights-promotion` | `InactiveMultiNightsPromotion.tsx` | Promotions        | `BE`      |
| `/promotion/day-of-week`                     | `DayOfWeek.tsx`                    | Promotions        | `BE`      |
| `/promotion/inactive-dayofweek-promotion`    | `InactiveDayOfWeekPromotion.tsx`   | Promotions        | `BE`      |
| `/promotion/promotion_early_bird`            | `AirbnbEarlyBird.tsx`              | Airbnb Promotions | `BE`      |
| `/promotion/promotion_last_min`              | `AirbnbLastmin.tsx`                | Airbnb Promotions | `BE`      |
| `/promotion/promotion_length_of_stay`        | `AirbnbLengthOfStay.tsx`           | Airbnb Promotions | `BE`      |

#### Outside DefaultLayout (No sidebar/header)

| Route                             | Page/Component                 | Module             |
| --------------------------------- | ------------------------------ | ------------------ |
| `/choose-plan`                    | `ChoosePlan.tsx`               | Subscription       |
| `/choose-plan/success`            | `ChoosePlanSuccess.tsx`        | Subscription       |
| `/choose-plan/renew-subscription` | `RenewSubscription.tsx`        | Subscription       |
| `/choose-plan/renew-success`      | `RenewSubscriptionSuccess.tsx` | Subscription       |
| `/airbnb-code`                    | `AirBnbCodeGet.tsx`            | Airbnb OAuth       |
| `/login`                          | `Navigate → /select-property`  | Redirect           |
| `/signup`                         | `Navigate → /select-property`  | Redirect           |
| `/select-property`                | `SelectProperty.tsx`           | Property Selection |
| `/add-new-property/*`             | Property wizard (8 steps)      | Add Property       |
| `/add-room-type/*`                | Room type wizard (5 steps)     | Add Room Type      |
| `/add-floors/*`                   | Floor wizard (3 steps)         | Add Floors         |
| `/add-rooms/*`                    | Room creation (2 steps)        | Add Rooms          |
| `*` (catch-all)                   | `Error.tsx`                    | Error              |

#### Conditional Routes (Feature-Flagged)

| Condition                     | Routes Affected                                                                                                   |
| ----------------------------- | ----------------------------------------------------------------------------------------------------------------- |
| `be_version === 3`            | All `/manage-channels/booking-engine/promotion/*` sub-routes and `/manage-channels/booking-engine/private-coupon` |
| `be_version !== 3`            | `/manage-channels/booking-engine/coupon` (PublicPrivateCoupon instead of BePrivateCoupon)                         |
| `single_sign_on_status !== 0` | SSO deep-link route `/:company_id/:comp_hash/...` is available                                                    |

---

## Navigation Flow

### Primary Navigation Decision Tree (App.tsx)

```
User visits any URL
│
├─ Is this an intranet SSO deep-link? (base64 segments detected)
│   └─ YES → Render AuthRoutes (handles SSO login via Loginwithoutcredential)
│
├─ Does auth_token exist in Redux (persisted)?
│   ├─ NO → Render AuthRoutes
│   │        └─ All unknown paths → Redirect to /login
│   │
│   └─ YES → Is current_property set?
│       ├─ NO → Render NewUserRoutes
│       │        ├─ / or /select-property → Property selection page
│       │        ├─ /add-new-property/* → Property creation wizard
│       │        └─ * → Error page
│       │
│       └─ YES → Render AppRoutes
│                ├─ Subscription check runs first (loading spinner)
│                │   ├─ NO-PLAN → Redirect to /choose-plan
│                │   ├─ PLAN-EXPIRED → Redirect to /choose-plan/renew-subscription
│                │   └─ ACTIVE → Render routes normally
│                │
│                └─ DefaultLayout (sidebar + header + content)
│                    └─ All feature routes
```

### Sidebar Navigation Structure

The sidebar is defined in `SidebarMenu.json` and renders 11 top-level items:

```
Dashboard          → /
Inventory          → /inventory
Rates              → /rates
Bookings           → /bookings
Promotion          → /promotion
Reviews            → /reviews
Apps               → /apps
Performance        → /performance
Mart               → https://mart.bookingjini.com (external link)
─── separator ───
Booking Engine     → /manage-channels/booking-engine
Setup              → /property-setup
```

Notable: The sidebar does NOT include links to:

- `/manage-channels` (the OTA channel hub) — accessed from within Booking Engine or directly
- `/manage-users` — accessed from the header
- `/manage-subscription` — accessed from the header
- `/tickets`, `/faq` — accessed from the header help section
- `/notifications` — accessed from the header bell icon

### In-App Navigation Patterns

1. **Wizard flows** (multi-step, linear): Add Property (8 steps), Add Room Type (5 steps), Add Floors (3 steps), Reservation (4 steps). These use Redux to persist step state and `useNavigate()` for step transitions.

2. **Slider/Drawer pattern**: Most CRUD operations (add/edit) open as `react-sliding-pane` drawers from the right side rather than navigating to a new route. This means many features are accessible only through component state, not via URL.

3. **Redirect routes**: `/bookings` → `/bookings/list-view`, `/inventory` → `InventoryRedirect` (decides room-type vs channel-type based on saved preference), `/rates` → `RatesRedirect`.

4. **Tab-based sub-navigation**: Within pages like Bookings (List View / CRS View / Front Office View), Performance (dropdown selector), and Reviews (tab switching), navigation happens via in-page tabs/dropdowns rather than route changes.

5. **Deep-link from external systems**: The SSO route `/:company_id/:comp_hash/:hotel_id/:admin_id/:auth_token/:full_name/:subscription_customer_id/:company_url/:source_name` allows intranet and hotel chain portals to deep-link users directly into the extranet with pre-authenticated credentials.

6. **Performance redirect**: If `redirect_status` is true in Redux (set from a base64-encoded URL segment), the app auto-navigates to `/performance/year-in-review` on load.

---

## Access Control

### Layer 1: Authentication (Route-Level)

Authentication is handled entirely through the route-group switching logic in `App.tsx`:

```typescript
{hotelIdOneTime
  ? <AuthRoutes />           // SSO flow
  : auth_token
    ? !current_property
      ? <NewUserRoutes />    // Authenticated, no property
      : <AppRoutes />        // Fully authenticated
    : <AuthRoutes />         // Not authenticated
}
```

- `auth_token` is stored in Redux (`state.auth.auth_token`) and persisted via `redux-persist` to localStorage.
- There are NO route guard components, HOCs, or middleware. The entire auth gate is a single ternary expression in `App.tsx`.
- Token verification (`verify-token/{company_id}/{admin_id}`) exists in `AppRoutes.tsx` but is currently commented out.
- All Axios instances have response interceptors that auto-logout (clear Redux + redirect) on HTTP 401.

### Layer 2: Subscription Gating (Route-Level)

Inside `AppRoutes.tsx`, a subscription check runs on every route change:

```
API: subscriptionApi → property-subscription-status
├─ subscription_status === "NO-PLAN"     → navigate("/choose-plan")
├─ subscription_status === "PLAN-EXPIRED" → navigate("/choose-plan/renew-subscription")
└─ Otherwise                              → Allow access
```

- This check runs in a `useEffect` triggered by `current_property.hotel_id` and `location.pathname`.
- The `/choose-plan/*` routes are outside `DefaultLayout`, so they render without sidebar/header.
- The renew-subscription path is explicitly excluded from the user access API call.

### Layer 3: User Access / RBAC (Component-Level)

RBAC is implemented at the component level, NOT at the route level. Every protected page independently checks access.

**How it works:**

1. On every route change, `AppRoutes.tsx` calls `GET get-user-access/{admin_id}` via `kernelApi`.
2. The API returns either:
   - `status: 911` → Super admin (full access to everything)
   - `status: 1` + `functions: [...]` → Array of feature access objects
3. This data is stored in Redux: `state.userAcess.adminAcess` (911 or 0) and `state.userAcess.accessData` (array).

**Access data structure** (inferred from usage):

```typescript
accessData: Array<{
  code: string; // Feature code: "INVENTORY", "RATE", "BE", "HOTEL", "ROOM"
  access_value: number; // 1 = granted, 0 = denied
}>;
```

**Feature codes identified:**

| RBAC Code   | Protected Features                                                                                                                                                                         |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `INVENTORY` | Inventory room-type view, Inventory channel-type view                                                                                                                                      |
| `RATE`      | Rates room-type view, Rates channel-type view                                                                                                                                              |
| `BE`        | All Booking Engine pages, ManageChannel hub, Promotions (all types), Property Setup (basic details, floors, rate plans, seasonal plans, dynamic pricing, derived rate plans), File Manager |
| `HOTEL`     | Property amenities, Property images, Property terms & policy, Property financial details                                                                                                   |
| `ROOM`      | Property room types management                                                                                                                                                             |

**Check pattern** (duplicated in 50+ components):

```typescript
useEffect(() => {
  const status = accessData.filter((iteam) => {
    return iteam?.code === "BE"; // or "INVENTORY", "RATE", "HOTEL", "ROOM"
  })[0]?.access_value;
  adminAcess === 911
    ? setAccess(true)
    : status === 1
      ? setAccess(true)
      : setAccess(false);
}, [accessData, adminAcess]);
```

When `access` is `false`, the component typically renders a "You don't have access" message or hides action buttons, but the route itself is still accessible — the user can navigate to the URL, they just see a restricted view.

**Pages with NO RBAC check:**

- Dashboard, Bookings (all views), Manage Users, Manage Subscription, Reviews, Performance, Apps, Mart, FAQ, Tickets, Notifications, Property Setup hub, some Property Setup sub-pages (rooms, cancellation rules, locale info, one-click setup)

### Layer 4: OTP Verification (Feature-Level)

The Manage Channels page (`ManageChannel.tsx`) has an additional OTP-based access gate:

- Before showing channel management options, the user must verify via email OTP.
- APIs: `kernelApi` → `sendotpbyemail`, `verifyotp`
- This is a one-time verification per session, stored in component state.

### Layer 5: Feature Flags (Conditional Routes)

Some routes are conditionally rendered based on Redux state:

| Flag                          | Source                             | Effect                                                                                           |
| ----------------------------- | ---------------------------------- | ------------------------------------------------------------------------------------------------ |
| `be_version === 3`            | `state.beVersion.be_version`       | Enables BE v3 promotion routes (early bird, last minute, same-day, LOS) and private coupon route |
| `single_sign_on_status !== 0` | `state.auth.single_sign_on_status` | Enables the SSO deep-link route                                                                  |
| `is_show_commissions`         | `state.isShowCommissions`          | Was used to conditionally show transactions route (now always shown)                             |

### Layer 6: Mobile Device Block

```typescript
<BrowserView>
  {/* Full application */}
</BrowserView>
<MobileView>
  <AppDownloadScreen />  {/* "Download our app" screen */}
</MobileView>
```

Mobile and tablet users are completely blocked from the web application and shown an app download screen instead. This is enforced via `react-device-detect`.

---

## Observations

### Architecture Gaps

1. **No route-level guards**: RBAC is checked inside each component after the route renders. A user without access still loads the page, makes API calls, and then sees a restricted message. A proper `ProtectedRoute` wrapper or route middleware would prevent unnecessary rendering and API calls.

2. **Duplicated RBAC pattern**: The exact same access-check `useEffect` is copy-pasted across 50+ components. This should be a custom hook like `useFeatureAccess("BE")` that returns `{ hasAccess, isAdmin }`.

3. **No centralized route config**: Routes are defined inline in JSX across 3 files (737+ lines in AppRoutes alone). A route configuration object would enable programmatic route generation, breadcrumb generation, and centralized access control.

4. **Subscription check on every navigation**: `checkHotelSubscription()` fires on every `location.pathname` change, making an API call on every single route transition. This should be debounced or cached with a TTL.

5. **User access fetched on every navigation**: `getUserAcess()` also fires on every route change. Combined with the subscription check, every navigation triggers 2 API calls before the page even loads.

6. **No lazy loading strategy**: Only the Agoda onboarding routes use `React.lazy()`. All other 100+ routes are eagerly imported, resulting in a large initial bundle.

7. **Commented-out token verification**: The `verifyAuthToken()` call is commented out in `AppRoutes.tsx`, meaning there's no server-side token validation — the app trusts the persisted Redux token indefinitely until a 401 response from any API call triggers logout.

8. **SSO credentials in URL**: The SSO deep-link passes `auth_token`, `admin_id`, and other sensitive data as URL path segments. These end up in browser history, server logs, and analytics. A POST-based or short-lived token approach would be more secure.
