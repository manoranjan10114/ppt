# Channel Manager Extranet — State Management Analysis

---

## State Management Overview

The application uses a single state management approach: **Redux** (via `@reduxjs/toolkit` package, but using the legacy `createStore` API) with **redux-persist** for localStorage persistence and **redux-thunk** for async middleware.

There is **zero usage of React Context** (`createContext`/`useContext` not found anywhere in the codebase). All shared state flows through Redux. Component-local state uses `useState` extensively.

### Technology Stack

| Tool               | Version | Purpose                                                                                                 |
| ------------------ | ------- | ------------------------------------------------------------------------------------------------------- |
| `@reduxjs/toolkit` | ^1.7.1  | Provides `createStore`, `combineReducers`, `nanoid` (RTK's `createSlice`/`configureStore` are NOT used) |
| `react-redux`      | ^7.2.9  | React bindings (`useSelector`, `useDispatch`)                                                           |
| `redux-thunk`      | ^2.4.1  | Async action middleware (though no thunk actions were found — all actions are synchronous)              |
| `redux-persist`    | ^6.0.0  | Persist Redux state to localStorage                                                                     |

### Store Configuration

```typescript
// Legacy pattern — NOT using RTK's configureStore
const store = createStore(rootReducer, applyMiddleware(thunk));
const persistor = persistStore(store);
```

The store is wrapped with `PersistGate` in `App.tsx`, which delays rendering until persisted state is rehydrated from localStorage.

---

## Key Stores / State Slices

### 37 Redux Slices — Categorized

#### Core Application State (Always Needed)

| Slice Key    | Reducer           | State Shape                                                                                                                                                                                                                                                      | Persisted | Purpose                                   |
| ------------ | ----------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ----------------------------------------- |
| `auth`       | LoginReducer      | `{ admin_id, auth_token, company_id, company_url, company_name, email, full_name, subscription_customer_id, chargebee_subscription_id, subscription_days_left, subscription_message, subscrption_is_on_extension, single_sign_on_status, role_id, source_name }` | Yes       | Session/auth data                         |
| `properties` | PropertiesReducer | `{ property_data, current_property, current_property_info, new_property_added_status, subscription_api_key }`                                                                                                                                                    | Yes       | Property list and active property         |
| `userAcess`  | UserAcessReducer  | `{ accessData: [], adminAcess: 0, chargebeeEnvironment, userFullName }`                                                                                                                                                                                          | Yes       | RBAC permissions                          |
| `sidebar`    | SidebarReducer    | `{ active_sidebar_item, active_sidebar_subitem, breadcrumbs, bulk_cm_loader, bulk_be_loader }`                                                                                                                                                                   | Yes       | Navigation state + bulk operation loaders |

#### Feature Domain State

| Slice Key              | Reducer                | State Shape                                                                                                                                                                                                        | Persisted | Purpose                                              |
| ---------------------- | ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- | ---------------------------------------------------- |
| `bookings`             | BookingsReducer        | `{ current_booking, booking_info, billing_details, stay_details, room_details, booking_details, unpaid_booking_details }`                                                                                          | Yes       | Active booking data passed between booking sub-pages |
| `reservation`          | ReservationReducer     | `{ tabOrder, roomTypeInfo[], guestInformation, checkinDate, checkoutDate, availableRooms[], successStatus, invoiceId, reservationRequestFrom, roomdetails[], partialPayment, comment, checkRealTimeAvailability }` | Yes       | Multi-step reservation wizard state                  |
| `manage_channels`      | ManageChannelReducer   | `{ allChannels[], couponId, allRoomType[], ota: {} }`                                                                                                                                                              | Yes       | Channel/OTA data                                     |
| `dashBoardData`        | DashboardReducer       | `{ today_data, mtd_data, ytd_data }`                                                                                                                                                                               | Yes       | Dashboard analytics cache                            |
| `walkInInfoData`       | WalkInInfoData         | Walk-in booking form data                                                                                                                                                                                          | Yes       | Walk-in flow state                                   |
| `airbnbPromotionsData` | AirbnbPromotionReducer | Airbnb room type + rate plan data                                                                                                                                                                                  | Yes       | Airbnb promotion context                             |

#### Multi-Step Wizard State

| Slice Key                   | Reducer                  | State Shape                                                                                                                                                                                                                                                    | Persisted | Purpose                        |
| --------------------------- | ------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ------------------------------ |
| `add_property`              | AddPropertyReducer       | `{ new_property_type, new_property_subtype, new_property_details: {name, mobile, email, landline}, new_property_address: {flat, street, city, state, pin, country, address, place_id}, new_property_location, new_property_amenities, new_property_images[] }` | Yes       | Add Property wizard (8 steps)  |
| `add_room_type`             | AddRoomTypeReducer       | Room type creation wizard data                                                                                                                                                                                                                                 | Yes       | Add Room Type wizard (5 steps) |
| `add_floors`                | AddFloorsReducer         | Floor creation wizard data                                                                                                                                                                                                                                     | Yes       | Add Floors wizard (3 steps)    |
| `oneClickSetupPropertyData` | SaveOneClickSetupReducer | `{ ota_name, ota_hotel_id, img_url, hotel_name, address, validateStatus, propertyConfirm, finalStatus, hotelDetailStatus, hotelRoomTypeStatus, hotelRateStatus, hotelImageStatus }`                                                                            | Yes       | One-click OTA setup wizard     |

#### Booking Engine Configuration

| Slice Key           | Reducer                 | State Shape                   | Persisted | Purpose                             |
| ------------------- | ----------------------- | ----------------------------- | --------- | ----------------------------------- |
| `beUrl`             | BEUrlReducer            | `{ be_url: "" }`              | Yes       | Booking engine URL                  |
| `bePaymentGateway`  | BePaymentGatewayReducer | `{ be_payment_gateway }`      | Yes       | Active payment gateway info         |
| `beVersion`         | BEVersionReducer        | `{ be_version: "" }`          | Yes       | BE version (controls feature flags) |
| `isShowCommissions` | ShowCommissionReducer   | `{ is_show_commissions: "" }` | Yes       | Commission visibility flag          |
| `newFeaturesData`   | NewFeaturesDataReducer  | New features data             | Yes       | Feature announcement data           |

#### UI State (Should Be Local)

| Slice Key                  | Reducer                     | State Shape                                                                 | Persisted | Purpose                       |
| -------------------------- | --------------------------- | --------------------------------------------------------------------------- | --------- | ----------------------------- |
| `prompt`                   | PromptReducer               | `{ isOpen, message, action, onAccept: fn, isSwitchingProperty, buttons[] }` | No        | Global confirmation dialog    |
| `preview`                  | PreviewBoxVisibilityReducer | `{ isVisible }`                                                             | No        | Preview box toggle            |
| `newMenu`                  | NewMenuReducers             | `{ isNewMenu }`                                                             | No        | New menu toggle               |
| `announcements`            | AnnounceReducer             | Announcement data                                                           | Yes       | Announcement content          |
| `announcementprompt`       | AnnouncePromptReducer       | `{ isOpen }`                                                                | Yes       | Announcement modal toggle     |
| `openContactSupportSlider` | ContactSupportReducer       | `{ isContactSliderOpen: false }`                                            | Yes       | Contact support slider toggle |
| `accessManageChannel`      | AccessManageChannelsReducer | `{ manage_channel: false }`                                                 | Yes       | Manage channel access flag    |
| `isRedirectURL`            | RedirectURLReducer          | `{ redirect_status: false }`                                                | Yes       | Performance redirect flag     |
| `callReviewStoreGoogle`    | CallGoogleReviewStore       | `{ callGoogleReviewOnce: false }`                                           | Yes       | Google review sync-once flag  |

#### Single-Value / Utility State

| Slice Key               | Reducer                           | State Shape                                                                                                | Persisted | Purpose                      |
| ----------------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------------- | --------- | ---------------------------- |
| `stateUpdateReducers`   | UpdateInventoryNosReducer         | `""` (string)                                                                                              | No        | Inventory update trigger     |
| `selectedCalendar`      | saveSelectedCalendarReducer       | `{ year, month, date }`                                                                                    | Yes       | Selected calendar date       |
| `fileManager`           | FileManagerReducer                | `{ res_status: null }`                                                                                     | Yes       | Image upload refresh trigger |
| `selectedRoomForSeason` | RoomWiseRateSetupForSeasonReducer | Selected room for seasonal plan                                                                            | Yes       | Seasonal plan context        |
| `gateWay`               | PaymentGatewayReducer             | Payment gateway details                                                                                    | No        | Payment gateway setup state  |
| `addPropertyMail`       | AddPropertyMailReducer            | Property emails                                                                                            | No        | Property email list          |
| `addPropertyMobile`     | AddPropertyMobileReducer          | Property mobiles                                                                                           | No        | Property mobile list         |
| `savePropertyNewMail`   | SavePropertyNewMailReducer        | New email to add                                                                                           | No        | Email form state             |
| `savePropertyNewMobile` | SavePropertyNewMobileReducer      | New mobile to add                                                                                          | No        | Mobile form state            |
| `zapscaledata`          | ZapScaleInitData                  | `{ user_id, user_email, user_name, organization_id, organization_name, role_id, role_name, product_name }` | Yes       | ZapScale analytics data      |

### Persistence Summary

- **33 of 37 slices** are whitelisted for localStorage persistence
- Only 4 slices are NOT persisted: `prompt`, `preview`, `stateUpdateReducers`, `gateWay`
- The entire persisted state tree is stored under the key `"root"` in localStorage

---

## Data Flow Explanation

### Global Data Flow Pattern

```
Component (useDispatch) ──dispatch(action)──> Redux Store
                                                  │
                                                  ▼
                                            Reducer updates state
                                                  │
                                                  ▼
Component (useSelector) <──re-render──── State change detected
                                                  │
                                                  ▼
                                          redux-persist ──> localStorage
```

### How Data Moves Between Components

1. **Parent → Child**: Props (standard React pattern). Used extensively within page modules.

2. **Sibling / Cross-Page**: Redux store. When a user selects a booking in ListView, the booking data is dispatched to `bookings.current_booking`, then StayDetails reads it via `useSelector`.

3. **Multi-Step Wizards**: Redux slices accumulate data across steps. The Add Property wizard stores each step's data (`add_property.new_property_type`, `add_property.new_property_details`, etc.) and reads the full accumulated state on the overview/submit step.

4. **API Response → Global State**: Components fetch data via Axios, then dispatch results to Redux. Example:

   ```typescript
   const res = await kernelApi.get(`get-user-access/${userId}`);
   dispatch(UserAcessDetails(res.data.functions));
   ```

5. **Global UI State**: The confirmation prompt (`prompt` slice) is dispatched from any component and rendered by `ConfirmationPrompt` in `DefaultLayout`. The `onAccept` callback is stored as a function in Redux state (anti-pattern — functions are not serializable).

### Async Handling

- **No thunk actions exist** despite `redux-thunk` being installed. All async operations (API calls) happen directly in components using `async/await` inside `useEffect` or event handlers.
- There is no centralized error handling, retry logic, or loading state management.
- Each component manages its own `isLoading`, `error`, and `data` states via `useState`.

### Caching Strategy

- **No explicit caching**. No TTL, no stale-while-revalidate, no React Query/SWR.
- `redux-persist` acts as an implicit cache — data from previous sessions is available immediately on app load, but there's no invalidation strategy.
- Dashboard data (`today_data`, `mtd_data`, `ytd_data`) is persisted, meaning stale analytics from a previous session are shown briefly before fresh data loads.
- The `callReviewStoreGoogle` flag prevents re-running the Google review SSE sync on every page visit — a manual "cache" for an expensive operation.

### Derived State

- **No selectors or memoized derivations**. No `createSelector` (reselect), no computed values.
- Components read raw state via `useSelector` and compute derived values inline:
  ```typescript
  const currencySymbol = current_property_info?.currency_symbol
    ? String.fromCharCode(parseInt(current_property_info.currency_symbol, 16))
    : "₹";
  ```
- RBAC checks are computed inline in every component rather than derived once.

---

## Observations (Performance / Scalability)

### Critical Issues

1. **Storing functions in Redux**: The `prompt` reducer stores `onAccept: () => {}` — a function reference in the Redux state tree. This breaks serialization, breaks Redux DevTools, and breaks `redux-persist` (functions can't be serialized to localStorage). It works by accident because `prompt` is not in the persist whitelist.

2. **Over-persistence**: 33 of 37 slices are persisted to localStorage. This includes UI toggle states (`isContactSliderOpen`, `announcementprompt.isOpen`), wizard data that should be ephemeral, and analytics data that goes stale immediately. This bloats localStorage and causes stale data bugs.

3. **No state cleanup on logout**: The `Logout()` function in `UtilityFunctions.tsx` clears localStorage and reloads the page, but there's no Redux state reset action. If `persistor.purge()` fails or is slow, stale auth data could persist.

4. **Massive re-render surface**: With 37 combined reducers and no selector memoization, any state change triggers `useSelector` comparisons across all connected components. Components that read `state.properties` will re-render when `current_property_info` changes even if they only need `current_property`.

5. **No state normalization**: Booking data, channel data, and room type data are stored as nested objects/arrays without normalization. Updating a single booking requires replacing the entire bookings array.

### Architectural Concerns

6. **Redux used as a prop-drilling escape hatch**: Many slices exist solely to pass data between two specific components (e.g., `selectedRoomForSeason` passes a room selection from the seasonal plan list to the setup page). React Context or simple URL params would be more appropriate.

7. **12+ single-value flag reducers**: Slices like `beVersion` (`{ be_version: "" }`), `isShowCommissions` (`{ is_show_commissions: "" }`), `isRedirectURL` (`{ redirect_status: false }`), `callReviewStoreGoogle` (`{ callGoogleReviewOnce: false }`) each have their own reducer file, action file, and action type constant — ~60 lines of boilerplate per boolean flag. These should be consolidated into a single `appConfig` or `featureFlags` slice.

8. **No TypeScript in reducers**: All reducers use `action: any` — no typed actions, no discriminated unions, no type safety. RTK's `createSlice` would provide this automatically.

9. **Wizard state persisted indefinitely**: The `add_property`, `add_room_type`, and `add_floors` wizard states are persisted. If a user abandons a wizard mid-flow, the partial data remains in localStorage forever, potentially causing issues when they restart the wizard later.

10. **`redux-thunk` installed but unused**: The middleware is configured but no thunk action creators exist. All async logic lives in components. Either remove the middleware or migrate async logic to thunks/RTK Query for consistency.

### Recommendations

- Migrate to RTK `configureStore` + `createSlice` — eliminates boilerplate, adds type safety, includes thunk by default, and enables Redux DevTools.
- Consolidate the 12+ single-value slices into 2-3 domain slices (`appConfig`, `uiState`, `featureFlags`).
- Reduce persist whitelist to only truly session-critical data: `auth`, `properties`, `sidebar`, `userAcess`.
- Extract a `useFeatureAccess(code)` custom hook to replace the duplicated RBAC check pattern.
- Consider RTK Query or React Query for server state — would eliminate all manual loading/error state management and add caching/invalidation.
- Remove function references from Redux state (the `prompt.onAccept` pattern).
