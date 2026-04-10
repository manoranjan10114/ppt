# Channel Manager Extranet — Architecture Analysis

## Architecture Overview

This is a React 17 + TypeScript SPA (Single Page Application) for the Bookingjini Channel Manager Extranet (v4). It follows a page-centric architecture where features are organized primarily by route/page rather than by domain module. The app is bootstrapped with Create React App (via CRACO for config overrides), uses Redux with redux-persist for global state, and communicates with a distributed microservice backend through multiple Axios instances.

The layout follows a classic admin panel pattern: a `DefaultLayout` shell wrapping a sidebar, header, and content area, with `react-router-dom` v6 handling navigation. The app is desktop-only — mobile visitors are redirected to an app download screen.

---

## Modules Identified

- **`pages/Bookings`** — Full booking lifecycle: front-office view, CRS view, list view, check-in, check-out, stay details, reservations, walk-ins, invoice generation, booking modifications, and cancellations.
- **`pages/Inventory`** — Room inventory management: view/update/block/unblock inventory, sync across channels, out-of-order rooms, sold-out dates. Supports both room-type and channel-type views.
- **`pages/Rates`** — Rate management: bulk/single update, block/unblock rates, sync rates, calendar-based rate editing. Supports room-type and channel-wise views.
- **`pages/ManageChannel`** — Channel distribution hub. Contains three major sub-modules:
  - **`BookingEngine`** — The direct booking engine with 22+ sub-features (basic setup, promotions, coupons, taxes, payment options, paid services, day-outing packages, widgets, plugins, cancellation policies, commissions).
  - **`OtherOTA`** — OTA onboarding and management (room sync, rate sync, Airbnb integration).
  - **`PMSManage`** — PMS integration management.
  - **`agodaOnBoarding`** — Dedicated Agoda onboarding flow (lazy-loaded).
- **`pages/PropertySetup`** — Property configuration: basic details, room types, floors, rooms, amenities, images, terms/policies, cancellation rules, financial details, locale info, rate plans, derived rate plans, seasonal plans, dynamic pricing, one-click OTA setup.
- **`pages/Promotions`** — Promotion engine: basic promotions, early bird, same-day, multi-night, day-of-week, length-of-stay, and Airbnb-specific promotions.
- **`pages/Reviews`** — Review aggregation: Google Business reviews, Booking.com reviews, Airbnb reviews, review summaries.
- **`pages/Performance`** — Analytics and reporting: year-in-review, Booking.com market insights, channel-wise check-in reports, digital marketing reports, direct booking comparisons.
- **`pages/DashboardData`** — Dashboard widgets: confirmed bookings, front-office data, room nights, revenue data, room type comparisons, trend charts.
- **`pages/ManageUsers`** — User management and role-based access control.
- **`pages/ManageSubscription`** — Subscription/plan management with Chargebee/Zoho integration.
- **`pages/AddNewProperty`** — Multi-step property onboarding wizard (type → subtype → details → address → amenities → images → overview → success).
- **`pages/AddRoomType`** — Multi-step room type creation wizard.
- **`pages/AddFloors` / `pages/AddRoom`** — Floor and room creation flows.
- **`pages/SignUp`** — User registration and validation.
- **`pages/Mart`** — Marketplace/app store for add-on services.
- **`pages/Tickets`** — Support ticket management.
- **`pages/FAQ`** — Help/FAQ section.
- **`pages/Apps`** — App listing and details.

---

## Shared Layers

### Components (`src/components/`)

Reusable UI components shared across pages:

- `header/AppHeader`, `sidebar/AppSidebar`, `contents/AppContent` — Layout shell
- `breadcrumbs/` — Navigation breadcrumbs
- `daterangepicker/` — Date range selection (two variants)
- `filemanager/` — File/image upload management
- `charts/` — Nivo-based bar, line, and pie charts
- `carousel/` — Image carousel
- `funnel/` — Funnel chart visualization
- `table/` — Table component
- `toaster/` — Toast notifications
- `nodata/` — Empty state component
- `onboarding/` — Onboarding flow components
- `previewBox/` — Preview panel
- `seasonalPlan/` — Seasonal plan components
- `property/`, `manageproperty/`, `manageuser/` — Domain-specific shared components
- `gethelp/`, `supportcontactus/` — Support widgets
- `AuraWidgetLoader/` — AI assistant widget loader

### Views (`src/views/`)

Low-level UI primitives/atoms:

- `buttons/`, `inputtextfield/`, `colorpicker/` — Form elements
- `modal/`, `sliders/` — Overlay components
- `loader/` — Loading spinner
- `cardwrapper/`, `datacard/` — Card layouts
- `circularprogress/` — Progress indicator
- `mobileapp/` — Mobile redirect screen
- `iconHoverImg/` — Icon hover effects

### Utilities (`src/UtilityFunctions.tsx` + `src/utils/`)

- Validation helpers (email, URL, mobile, PAN, GST, IFSC, etc.)
- Date/time formatting and conversion
- Auth token storage/retrieval (localStorage)
- Logout handler
- PDF download, Base64 conversion
- Sidebar state management helper
- Payment gateway integration helpers (Easebuzz)
- `utils/CustomCropper.tsx` — Image cropping
- `utils/ImagePopup.tsx` — Image lightbox

### API Layer (`src/API/`)

12 Axios instances targeting distinct backend microservices:

| Instance          | Service               | Base URL (Prod)                |
| ----------------- | --------------------- | ------------------------------ |
| `kernelApi`       | Core/Kernel           | `kernel.bookingjini.com`       |
| `cmApi`           | Channel Manager       | `cm.bookingjini.com`           |
| `cm3Api`          | Channel Manager v3    | `cm3.bookingjini.com`          |
| `beApi`           | Booking Engine        | `be.bookingjini.com`           |
| `beAlphaAPI`      | Booking Engine Alpha  | `be-alpha.bookingjini.com`     |
| `gemsApi`         | GEMS (Front Office)   | `gems.bookingjini.com`         |
| `subscriptionApi` | Subscription/Billing  | `subscription.bookingjini.com` |
| `blApi`           | Status/Health         | `status.bookingjini.tech`      |
| `toolsAPI`        | Tools (Kernel alias)  | `kernel.bookingjini.com`       |
| `getResponse`     | External integrations | (likely GetResponse)           |
| `airBnb`          | Airbnb OAuth          | `airbnb.com/oauth2`            |

All authenticated instances share the same interceptor pattern: inject Bearer token from localStorage on request, auto-logout on 401 response.

`endPoints.tsx` is a massive (~1165 lines) centralized endpoint registry organized by domain (PROPERTY_SETUP, INVENTORY, RATE, BOOKINGS, MANAGECHANNEL, DASHBOARD, etc.).

---

## State Management

Redux (via `@reduxjs/toolkit`) with `redux-thunk` middleware and `redux-persist` for localStorage persistence.

The store uses the legacy `createStore` + `combineReducers` pattern (not RTK's `configureStore` or `createSlice`). There are **37 reducers** combined into a single root reducer, with **33 slices whitelisted** for persistence.

Key state slices:

- `auth` — Login/session state
- `properties` — Property list and current property selection
- `bookings` — Booking data and details
- `manage_channels` — Channel/OTA data
- `stateUpdateReducers` — Inventory update state
- `sidebar` — Navigation state
- `dashBoardData` — Dashboard analytics
- `userAcess` — RBAC permissions
- `reservation` — Reservation flow state
- `walkInInfoData` — Walk-in booking data
- `beUrl`, `beVersion`, `bePaymentGateway` — Booking engine config
- `fileManager` — File upload state
- `zapscaledata` — Analytics tracker data

Each reducer has a corresponding action file. Actions use plain action creators (not RTK's `createSlice`), dispatched via `redux-thunk`.

---

## Routing Overview

Three route groups managed in `App.tsx` based on auth state:

1. **`AuthRoutes`** — Unauthenticated: `/login`, `/signup`, `/reset-password`, `/new-password`, SSO deep-link route.
2. **`NewUserRoutes`** — Authenticated but no property selected: `/select-property`, `/add-new-property/*` wizard.
3. **`AppRoutes`** — Fully authenticated with property: All feature routes wrapped in `DefaultLayout`.

`AppRoutes` is the main router (~737 lines) with deeply nested routes:

- Top-level: `/` (Dashboard), `/bookings/*`, `/inventory/*`, `/rates/*`, `/manage-channels/*`, `/property-setup/*`, `/promotion/*`, `/reviews/*`, `/performance/*`, `/manage-users`, `/manage-subscription/*`, `/mart`, `/apps/*`, `/tickets/*`, `/faq`, `/notifications`
- Setup wizards outside layout: `/add-new-property/*`, `/add-room-type/*`, `/add-floors/*`, `/add-rooms/*`
- Subscription gate: `/choose-plan/*`, `/choose-plan/renew-subscription`
- Subscription check runs on every route change, redirecting to plan selection if expired

Conditional routing exists based on `be_version` (v3 features) and `single_sign_on_status`.

---

## Business Domains Identified

1. **Property Management** — Property CRUD, room types, floors, rooms, amenities, images, policies, locale, financial details
2. **Inventory Management** — Room availability across channels, block/unblock, sync, out-of-order
3. **Rate Management** — Pricing across channels, bulk/individual updates, block/unblock, sync, dynamic pricing
4. **Booking Management** — Full lifecycle from reservation to checkout, CRS, front-office, invoicing, payment links
5. **Channel Distribution** — OTA onboarding/mapping (Booking.com, Airbnb, Agoda, Expedia, etc.), room/rate sync, PMS integration
6. **Direct Booking Engine** — Self-hosted booking engine with promotions, coupons, packages, paid services, payment gateways, widgets, day-outing packages
7. **Promotions & Revenue** — Multi-type promotion engine (early bird, last minute, LOS, day-of-week, multi-night)
8. **Reviews & Reputation** — Aggregated reviews from Google, Booking.com, Airbnb
9. **Analytics & Performance** — Dashboard KPIs, revenue reports, market insights, year-in-review
10. **User & Access Management** — Multi-user RBAC with function-level permissions
11. **Subscription & Billing** — Plan management via Chargebee/Zoho, subscription gating

---

## Observations

### Code Organization Quality

- The codebase is **page-centric** rather than domain-driven. Business logic, API calls, and UI are co-located in page components rather than separated into service/hook layers. This makes individual pages self-contained but leads to duplication.
- `endPoints.tsx` at 1165 lines is a monolithic endpoint registry. It works as a single source of truth but is unwieldy.
- The API layer is well-structured with consistent interceptor patterns across all Axios instances, though `toolsAPI` appears to be a duplicate of `kernelApi`.
- `UtilityFunctions.tsx` is a catch-all utility file mixing validation, formatting, auth, payment, and UI helpers — a candidate for decomposition.
- Redux uses the older `createStore` pattern despite having `@reduxjs/toolkit` installed. Migrating to `configureStore` + `createSlice` would reduce boilerplate significantly.
- 37 reducers with 1:1 action files is verbose. Many slices store simple flags or single values that could be consolidated.
- `AppRoutes.tsx` at 737 lines with 200+ imports is the single largest routing file — no code-splitting beyond the Agoda onboarding lazy imports.

### Scalability Concerns

- **No code-splitting strategy.** Nearly all routes are eagerly imported, which impacts initial bundle size significantly for a 100+ page app.
- **No custom hooks layer.** Data fetching and business logic live directly in components, making reuse and testing difficult.
- **No TypeScript interfaces/types directory.** Type safety appears inconsistent (heavy use of `any`).
- The `ManageChannel/BookingEngine` module alone has 22+ sub-features — it's effectively an app within the app and would benefit from its own routing module.
- `redux-persist` whitelists 33 of 37 slices, meaning nearly the entire state tree is persisted. This can cause stale data issues and bloated localStorage.
- **No error boundary implementation** visible beyond a basic `Error` page.
- **No testing infrastructure** beyond the default CRA setup (no test files found in the explored structure).

---

## Suggested Next Areas to Analyze

1. **`pages/Bookings/`** — The most complex domain with 20+ files spanning the full booking lifecycle. Understanding its data flow and component interactions is critical since it touches inventory, rates, payments, and front-office operations.

2. **`pages/ManageChannel/BookingEngine/`** — The largest sub-module with 22 feature folders. It's essentially a separate product embedded within the extranet and likely contains the most technical debt and duplication.

3. **`redux/reducers/` + `redux/actions/`** — A deep audit of the state management layer would reveal opportunities to consolidate slices, migrate to RTK `createSlice`, and identify state that should be local rather than global.

4. **`components/sidebar/AppSidebar` + `components/header/AppHeader`** — These control navigation and RBAC-based menu visibility, which is central to the user experience and permission model.

5. **`pages/Inventory/` and `pages/Rates/`** — These are the core channel manager operations. Understanding how they handle multi-channel sync, calendar views, and bulk operations would reveal the most performance-sensitive code paths.
