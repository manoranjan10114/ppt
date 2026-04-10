# Data Collection Spec — Feature Monetization Baseline

> Pull this data for ALL customers (active + trial + churned).
> This becomes the frozen baseline for usage-based pricing AND the foundation for every pricing/upsell decision.

---

## HOW TO USE THIS DOCUMENT

1. Share this with your engineering/data team
2. They pull each query against your production database
3. Output as a single CSV/Excel with one row per hotel_id
4. This gives you: what each customer uses, what they pay, and what the monetization opportunity is

---

## MASTER QUERY: ONE ROW PER HOTEL

Every hotel_id should have these columns in the final export:

---

### SECTION A: HOTEL IDENTITY

| Column Name             | Description                         | Source Table (likely)  | Example Value            |
| ----------------------- | ----------------------------------- | ---------------------- | ------------------------ |
| `hotel_id`              | Unique hotel identifier             | hotels / properties    | 4523                     |
| `hotel_name`            | Property name                       | hotels                 | "Hotel Sunrise"          |
| `company_id`            | Parent company (for chains)         | hotels                 | 102                      |
| `company_name`          | Company name                        | companies              | "Sunrise Hotels Pvt Ltd" |
| `city`                  | Hotel city                          | hotels                 | "Jaipur"                 |
| `state`                 | Hotel state                         | hotels                 | "Rajasthan"              |
| `country`               | Hotel country                       | hotels                 | "India"                  |
| `hotel_status`          | Active / Inactive / Trial / Expired | hotels / subscriptions | "active"                 |
| `onboarding_date`       | When the hotel was first created    | hotels                 | "2023-04-15"             |
| `days_since_onboarding` | Calculated: today - onboarding_date | —                      | 730                      |

---

### SECTION B: SUBSCRIPTION & REVENUE

| Column Name                    | Description                                         | Source Table        | Example Value |
| ------------------------------ | --------------------------------------------------- | ------------------- | ------------- |
| `current_plan_name`            | Name of current subscription plan                   | subscriptions       | "Pro Annual"  |
| `current_plan_price_monthly`   | What they pay per month (if annual, divide by 12)   | subscriptions       | 1500          |
| `current_plan_price_annual`    | Annual plan price (if applicable)                   | subscriptions       | 18000         |
| `billing_cycle`                | Monthly / Annual / Quarterly                        | subscriptions       | "annual"      |
| `subscription_status`          | Active / Expired / Cancelled / Trial / On Extension | subscriptions       | "active"      |
| `subscription_start_date`      | When current subscription started                   | subscriptions       | "2024-01-01"  |
| `subscription_end_date`        | When current subscription expires                   | subscriptions       | "2025-01-01"  |
| `days_to_renewal`              | Calculated: end_date - today                        | —                   | 45            |
| `total_revenue_lifetime`       | Total amount paid by this hotel ever                | payments / invoices | 54000         |
| `total_revenue_last_12_months` | Amount paid in last 12 months                       | payments / invoices | 18000         |
| `payment_method`               | Auto-debit / Manual / UPI / Card                    | payments            | "auto-debit"  |
| `last_payment_date`            | Date of most recent payment                         | payments            | "2024-11-01"  |
| `outstanding_amount`           | Any unpaid balance                                  | invoices            | 0             |

---

### SECTION C: CHANNEL MANAGER USAGE

| Column Name                   | Description                                                | Source Table              | Example Value                                     |
| ----------------------------- | ---------------------------------------------------------- | ------------------------- | ------------------------------------------------- |
| `total_ota_connections`       | Number of OTAs currently connected                         | cm_ota_details / channels | 5                                                 |
| `ota_list`                    | Comma-separated list of connected OTAs                     | cm_ota_details            | "Booking.com, MakeMyTrip, Goibibo, Agoda, Airbnb" |
| `total_room_types`            | Number of room types configured                            | hotel_master_room_type    | 4                                                 |
| `total_rate_plans`            | Number of rate plans configured                            | master_rate_plan          | 3                                                 |
| `total_derived_rate_plans`    | Number of derived rate plans                               | derived_rate_plans        | 1                                                 |
| `total_seasonal_plans`        | Number of seasonal pricing plans                           | seasonal_plans            | 2                                                 |
| `dynamic_pricing_active`      | Yes / No — is dynamic pricing enabled                      | dynamic_pricing           | "no"                                              |
| `dynamic_pricing_rules_count` | Number of dynamic pricing rules                            | dynamic_pricing           | 0                                                 |
| `total_active_promotions`     | Number of currently active promotions                      | promotions                | 2                                                 |
| `promotion_types_used`        | Types used: basic, early bird, same-day, multi-night, etc. | promotions                | "basic, early_bird"                               |
| `last_inventory_update_date`  | When inventory was last updated                            | inventory_logs            | "2024-12-10"                                      |
| `last_rate_update_date`       | When rates were last updated                               | rate_logs                 | "2024-12-09"                                      |
| `last_sync_date`              | Last OTA sync timestamp                                    | sync_logs                 | "2024-12-10 14:30"                                |

---

### SECTION D: BOOKING ENGINE USAGE

| Column Name                   | Description                                                | Source Table              | Example Value           |
| ----------------------------- | ---------------------------------------------------------- | ------------------------- | ----------------------- |
| `booking_engine_active`       | Yes / No — is BE enabled                                   | be_config / subscriptions | "yes"                   |
| `be_url`                      | Booking engine URL (if configured)                         | be_config                 | "book.hotelsunrise.com" |
| `be_total_bookings_last_12m`  | Total bookings via BE in last 12 months                    | bookings (source=BE)      | 145                     |
| `be_total_revenue_last_12m`   | Revenue from BE bookings in last 12 months                 | bookings                  | 580000                  |
| `be_bookings_last_30d`        | BE bookings in last 30 days                                | bookings                  | 12                      |
| `be_active_coupons`           | Number of active coupons/promotions on BE                  | coupons                   | 2                       |
| `be_total_coupons_ever`       | Total coupons ever created                                 | coupons                   | 5                       |
| `be_paid_services_count`      | Number of paid services/add-ons listed                     | paid_services             | 3                       |
| `be_active_packages`          | Number of active packages                                  | packages                  | 1                       |
| `be_active_day_outings`       | Number of active day outing packages                       | day_booking_packages      | 0                       |
| `be_payment_gateways_count`   | Number of payment gateways configured                      | payment_gateways          | 1                       |
| `be_payment_gateway_names`    | Which gateways: Razorpay, Easebuzz, PayU                   | payment_gateways          | "razorpay"              |
| `be_plugins_count`            | Number of analytics plugins active                         | be_plugins                | 2                       |
| `be_plugin_names`             | Which plugins: GA, GTM, Pixel, Clarity, WhatsApp, Interakt | be_plugins                | "GA, GTM"               |
| `be_widget_active`            | Website widget / instant booking widget active             | be_config                 | "yes"                   |
| `be_visitor_count_last_30d`   | BE website visitors in last 30 days                        | visitor_analytics         | 850                     |
| `be_conversion_rate_last_30d` | Bookings / visitors last 30 days                           | calculated                | 1.4%                    |

---

### SECTION E: PMS USAGE

| Column Name                      | Description                            | Source Table                | Example Value |
| -------------------------------- | -------------------------------------- | --------------------------- | ------------- |
| `pms_active`                     | Yes / No — is PMS enabled              | subscriptions / gems_config | "yes"         |
| `pms_platform`                   | Web / Mobile / Both                    | login_logs                  | "both"        |
| `total_physical_rooms`           | Number of rooms configured in PMS      | rooms                       | 32            |
| `total_floors`                   | Number of floors configured            | floors                      | 4             |
| `pms_checkins_last_30d`          | Check-ins processed in last 30 days    | checkin_logs                | 85            |
| `pms_checkouts_last_30d`         | Check-outs processed in last 30 days   | checkout_logs               | 78            |
| `pms_walkins_last_30d`           | Walk-in bookings in last 30 days       | bookings (source=walkin)    | 12            |
| `pms_last_login_date`            | Last PMS login                         | login_logs                  | "2024-12-10"  |
| `fnb_module_active`              | Yes / No — F&B module enabled          | subscriptions / config      | "no"          |
| `fnb_restaurants_count`          | Number of restaurants configured       | restaurants                 | 0             |
| `fnb_kitchens_count`             | Number of kitchens configured          | kitchens                    | 0             |
| `fnb_menu_items_count`           | Total menu items                       | menu_items                  | 0             |
| `fnb_orders_last_30d`            | F&B orders in last 30 days             | orders                      | 0             |
| `fnb_revenue_last_30d`           | F&B revenue in last 30 days            | orders                      | 0             |
| `housekeeping_active`            | Yes / No                               | config                      | "yes"         |
| `hk_requests_last_30d`           | Housekeeping requests in last 30 days  | hk_requests                 | 45            |
| `night_audit_enabled`            | Yes / No                               | config                      | "yes"         |
| `night_audit_completed_last_30d` | Night audits completed in last 30 days | night_audit_logs            | 28            |
| `guest_self_checkin_active`      | Yes / No                               | config                      | "no"          |
| `guest_room_service_qr_active`   | Yes / No                               | config                      | "no"          |
| `guest_review_page_active`       | Yes / No                               | config                      | "no"          |

---

### SECTION F: CRM USAGE

| Column Name                            | Description                                       | Source Table               | Example Value |
| -------------------------------------- | ------------------------------------------------- | -------------------------- | ------------- |
| `crm_active`                           | Yes / No — is CRM enabled                         | subscriptions / crm_config | "yes"         |
| `crm_total_contacts`                   | Total contacts in CRM                             | contacts                   | 1250          |
| `crm_contacts_added_last_30d`          | New contacts added in last 30 days                | contacts                   | 45            |
| `crm_total_agents`                     | Number of CRM agents/users                        | crm_users                  | 3             |
| `crm_total_enquiries`                  | Total enquiries ever                              | enquiries                  | 320           |
| `crm_open_enquiries`                   | Currently open enquiries                          | enquiries                  | 12            |
| `crm_converted_enquiries_last_12m`     | Enquiries converted to bookings in last 12 months | enquiries                  | 85            |
| `crm_total_interactions`               | Total interactions logged                         | interactions               | 580           |
| `crm_total_tickets`                    | Total tickets ever                                | tickets                    | 45            |
| `crm_open_tickets`                     | Currently open tickets                            | tickets                    | 3             |
| `crm_campaigns_active`                 | Yes / No — campaign module active                 | crm_config                 | "no"          |
| `crm_email_campaigns_sent_last_12m`    | Email campaigns sent in last 12 months            | campaigns                  | 0             |
| `crm_sms_campaigns_sent_last_12m`      | SMS campaigns sent                                | campaigns                  | 0             |
| `crm_whatsapp_campaigns_sent_last_12m` | WhatsApp campaigns sent                           | campaigns                  | 0             |
| `crm_total_emails_sent_last_30d`       | Individual emails sent in last 30 days            | email_logs                 | 0             |
| `crm_total_sms_sent_last_30d`          | SMS sent in last 30 days                          | sms_logs                   | 0             |
| `crm_total_whatsapp_sent_last_30d`     | WhatsApp messages sent in last 30 days            | whatsapp_logs              | 0             |
| `crm_membership_active`                | Yes / No — loyalty module active                  | crm_config                 | "no"          |
| `crm_active_members`                   | Number of active loyalty members                  | memberships                | 0             |
| `crm_membership_tiers`                 | Number of membership tiers                        | membership_programs        | 0             |
| `crm_qr_codes_count`                   | Number of QR codes created                        | qr_codes                   | 2             |
| `crm_ivr_active`                       | Yes / No — IVR integration active                 | crm_config                 | "no"          |
| `crm_ivr_users`                        | Number of IVR users                               | ivr_users                  | 0             |
| `crm_groups_count`                     | Number of contact groups                          | groups                     | 3             |
| `crm_organisations_count`              | Number of organisations                           | organisations              | 5             |

---

### SECTION G: HOTEL CHAIN USAGE

| Column Name                     | Description                                            | Source Table             | Example Value |
| ------------------------------- | ------------------------------------------------------ | ------------------------ | ------------- |
| `hotel_chain_active`            | Yes / No — Hotel Chain dashboard access                | subscriptions / reseller | "no"          |
| `chain_properties_count`        | Number of properties in chain dashboard                | reseller_hotels          | 0             |
| `chain_advanced_reports_active` | Yes / No — rate log, availability grid, coupon reports | config                   | "no"          |

---

### SECTION H: CROSS-PRODUCT USAGE

| Column Name                   | Description                                  | Source Table             | Example Value |
| ----------------------------- | -------------------------------------------- | ------------------------ | ------------- |
| `total_properties_managed`    | Number of properties under this account      | hotel_admin / properties | 1             |
| `total_users_cm`              | Users with Channel Manager access            | manage_user              | 2             |
| `total_users_pms`             | Users with PMS access                        | manage_user              | 3             |
| `total_users_crm`             | Users with CRM access                        | crm_users                | 2             |
| `total_users_all`             | Total unique users across all products       | manage_user              | 4             |
| `api_access_active`           | Yes / No                                     | config                   | "no"          |
| `api_calls_last_30d`          | API calls in last 30 days                    | api_logs                 | 0             |
| `ai_features_active`          | Yes / No — Aura AI / content generation      | config                   | "no"          |
| `ai_generations_last_30d`     | AI content generations in last 30 days       | ai_logs                  | 0             |
| `last_login_date_any_product` | Most recent login across any product         | login_logs               | "2024-12-10"  |
| `days_since_last_login`       | Calculated: today - last_login               | —                        | 2             |
| `review_management_active`    | Yes / No — Google/Booking.com/Airbnb reviews | review_config            | "yes"         |
| `review_sources_count`        | Number of review sources connected           | review_sources           | 2             |

---

### SECTION I: BOOKING & REVENUE DATA

| Column Name                         | Description                                   | Source Table            | Example Value |
| ----------------------------------- | --------------------------------------------- | ----------------------- | ------------- |
| `total_bookings_last_12m`           | All bookings (all channels) in last 12 months | bookings                | 1200          |
| `total_revenue_bookings_last_12m`   | Total booking revenue in last 12 months       | bookings                | 3600000       |
| `ota_bookings_last_12m`             | Bookings from OTAs in last 12 months          | bookings (source != BE) | 1055          |
| `ota_revenue_last_12m`              | Revenue from OTA bookings                     | bookings                | 3160000       |
| `direct_bookings_last_12m`          | Bookings from Booking Engine                  | bookings (source = BE)  | 145           |
| `direct_revenue_last_12m`           | Revenue from direct bookings                  | bookings                | 440000        |
| `direct_booking_percentage`         | Calculated: direct / total × 100              | —                       | 12.1%         |
| `estimated_ota_commission_last_12m` | Calculated: ota_revenue × 0.20 (avg 20%)      | —                       | 632000        |
| `cancelled_bookings_last_12m`       | Cancelled bookings in last 12 months          | bookings                | 95            |
| `cancellation_rate`                 | Calculated: cancelled / total × 100           | —                       | 7.9%          |
| `avg_booking_value`                 | Calculated: total_revenue / total_bookings    | —                       | 3000          |
| `avg_length_of_stay`                | Average nights per booking                    | bookings                | 1.8           |

---

## OUTPUT FORMAT

### Option 1: Single CSV (Recommended)

One row per hotel_id, all columns above. ~90 columns. Export as `customer_baseline_snapshot.csv`.

### Option 2: Multiple CSVs (If easier)

- `hotels_identity.csv` — Section A
- `hotels_subscription.csv` — Section B
- `hotels_cm_usage.csv` — Section C
- `hotels_be_usage.csv` — Section D
- `hotels_pms_usage.csv` — Section E
- `hotels_crm_usage.csv` — Section F
- `hotels_chain_usage.csv` — Section G
- `hotels_cross_product.csv` — Section H
- `hotels_booking_data.csv` — Section I

All joinable on `hotel_id`.

---

## WHAT WE DO WITH THIS DATA

| Analysis                                 | Data Needed                                                     | Outcome                                       |
| ---------------------------------------- | --------------------------------------------------------------- | --------------------------------------------- |
| **Frozen baseline per customer**         | All sections                                                    | Each customer's free quota for monetization   |
| **ARPU distribution**                    | Section B                                                       | Understand where ₹1,100 average comes from    |
| **Underpaying customers**                | Section B + C + D + E + F                                       | Who uses Growth features but pays Basic price |
| **BE activation opportunity**            | Section D (be_active + be_bookings = 0)                         | Hotels to activate for retention + upsell     |
| **Upsell candidates (Basic → Growth)**   | Section C + D + I (high OTA %, no BE)                           | Hotels losing money to OTAs                   |
| **Upsell candidates (Growth → Premium)** | Section E + F + G (multi-property, high usage)                  | Hotels that need chain dashboard              |
| **Churn risk**                           | Section H (days_since_last_login) + Section B (days_to_renewal) | Health scoring input                          |
| **Feature adoption rates**               | All sections (active flags)                                     | Which features are used vs available          |
| **Add-on opportunity**                   | Section E (F&B, HK, night audit inactive)                       | Modules to sell                               |
| **Revenue per feature**                  | Section I + feature flags                                       | Which features correlate with higher revenue  |
| **OTA commission savings potential**     | Section I (estimated_ota_commission)                            | ROI pitch for BE upsell                       |

---

## PRIORITY: PULL THIS FIRST

If you can't pull everything at once, start with these 15 columns:

1. `hotel_id`
2. `hotel_name`
3. `hotel_status`
4. `current_plan_name`
5. `current_plan_price_monthly`
6. `total_ota_connections`
7. `total_users_all`
8. `total_physical_rooms`
9. `booking_engine_active`
10. `be_total_bookings_last_12m`
11. `crm_active`
12. `crm_total_contacts`
13. `fnb_module_active`
14. `total_bookings_last_12m`
15. `direct_booking_percentage`

This alone tells you: who they are, what they pay, what they use, and where the money is.
