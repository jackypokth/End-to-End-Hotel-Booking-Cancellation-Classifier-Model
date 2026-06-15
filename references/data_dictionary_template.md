# Data Dictionary — Hotel Booking Demand

Working data dictionary for the local file `data/raw/hotel_bookings.csv` (119,390 rows × 32 columns). Column names below match the CSV header exactly (snake_case). Descriptions marked *(source-derived)* come from the upstream `@manthangandhi/hotel_cancellation_prediction` README and the original Antonio, de Almeida, & Nunes (2019) paper; verify them against the local data in [02_data_understanding](../notebooks/02_data_understanding.ipynb).

## Dataset Overview

- Dataset name: Hotel Booking Demand
- Source or citation: Antonio, de Almeida, & Nunes (2019); mirror via `@manthangandhi/hotel_cancellation_prediction`
- Access date: 2026-04-18
- Local file name: `data/raw/hotel_bookings.csv`
- Unit of observation: one hotel booking record
- Target variable: `is_canceled` (binary: 1 = cancelled, 0 = not cancelled)
- Local adaptation notes: none yet; log any filtering, renaming, or derived columns here.

## Column Reference

Legend for `leakage_risk`: **None** (safe), **Review** (timing unclear, verify in NB02), **High** (known post-outcome signal — must be excluded from features).

| # | column | dtype (expected) | leakage_risk | short description *(source-derived unless noted)* |
|---|---|---|---|---|
| 1 | `hotel` | categorical | None | Hotel type: "Resort Hotel" or "City Hotel". |
| 2 | `is_canceled` | binary int | — (target) | 1 if the booking was cancelled, else 0. |
| 3 | `lead_time` | int | None | Days between booking date and arrival date. |
| 4 | `arrival_date_year` | int | None | Year of arrival. |
| 5 | `arrival_date_month` | categorical | None | Month of arrival (string name). |
| 6 | `arrival_date_week_number` | int | None | ISO week number of arrival. |
| 7 | `arrival_date_day_of_month` | int | None | Day of arrival. |
| 8 | `stays_in_weekend_nights` | int | None | Weekend nights (Sat/Sun) booked. |
| 9 | `stays_in_week_nights` | int | None | Weekday nights booked. |
| 10 | `adults` | int | None | Number of adults. |
| 11 | `children` | float/int | None | Number of children (may contain NaN). |
| 12 | `babies` | int | None | Number of babies. |
| 13 | `meal` | categorical | None | Meal plan code (BB, HB, FB, SC, Undefined). |
| 14 | `country` | categorical | None | Country of origin (ISO code); high cardinality. |
| 15 | `market_segment` | categorical | None | Market segment designation. |
| 16 | `distribution_channel` | categorical | None | Booking distribution channel. |
| 17 | `is_repeated_guest` | binary int | None | 1 if the guest is a repeat customer. |
| 18 | `previous_cancellations` | int | None | Prior cancellations by the guest before this booking. |
| 19 | `previous_bookings_not_canceled` | int | None | Prior non-cancelled bookings by the guest. |
| 20 | `reserved_room_type` | categorical | None | Room type reserved (code). |
| 21 | `assigned_room_type` | categorical | Review | Room type assigned; may change at or after check-in. |
| 22 | `booking_changes` | int | Review | Count of changes to the booking; timing ambiguous. |
| 23 | `deposit_type` | categorical | None | Deposit type: No Deposit / Non Refund / Refundable. |
| 24 | `agent` | categorical | None | Travel agent ID (missing allowed). |
| 25 | `company` | categorical | None | Booking company ID (often missing). |
| 26 | `days_in_waiting_list` | int | None | Days the booking waited for confirmation. |
| 27 | `customer_type` | categorical | None | Transient / Contract / Group / Transient-Party. |
| 28 | `adr` | float | None | Average Daily Rate. May contain outliers/negatives. |
| 29 | `required_car_parking_spaces` | int | None | Parking spaces required. |
| 30 | `total_of_special_requests` | int | None | Count of special requests. |
| 31 | `reservation_status` | categorical | **High** | Final status: Canceled / Check-Out / No-Show — leaks the target. |
| 32 | `reservation_status_date` | date | **High** | Date of the final status; leaks timing of outcome. |

## Column Detail Template

Use this block when adding notes, observed ranges, or preprocessing decisions for any column that warrants deeper documentation.

### Column: `column_name`

- Description:
- Observed dtype:
- Observed range / cardinality:
- Missing values present: Yes / No (count)
- Preprocessing decision:
- Leakage risk: None / Review / High
- Keep for modeling: Yes / No / Undecided
- Notes:

## Priority Fields To Document First

- `is_canceled` (target distribution; class imbalance check)
- `lead_time`, `deposit_type`, `market_segment`, `distribution_channel`, `customer_type` (strong candidate predictors per source)
- `adr`, `total_of_special_requests`, `previous_cancellations`, `previous_bookings_not_canceled`, `required_car_parking_spaces` (behavioural signals)
- `reservation_status`, `reservation_status_date` (leakage — must be dropped before modeling)
- `country`, `agent`, `company` (high-cardinality / missingness)
