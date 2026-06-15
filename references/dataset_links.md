# Dataset Links, Attribution, And Intake Status

This file tracks dataset provenance, upstream attribution, local intake status, and any adaptations made for the ISOM3360 hotel cancellation project.

## Primary Source Reference

- Source project: `@manthangandhi/hotel_cancellation_prediction`
- Repository URL: `https://github.com/manthangandhi/hotel_cancellation_prediction`
- Project topic: hotel booking cancellation prediction
- Why it is relevant: provides a closely aligned problem statement, variable descriptions, and attribution trail for the hotel booking demand data used in this project context.

## Original Dataset Citation

- Dataset: Hotel Booking Demand
- Original academic citation: Antonio, N., de Almeida, A., & Nunes, L. (2019). *Hotel booking demand datasets.* Data in Brief, 22, 41–49.
- Commonly redistributed on Kaggle under "Hotel booking demand" (nantonio/a-hotels-booking-demand-dataset).
- The copy used here was pulled from the `@manthangandhi/hotel_cancellation_prediction` mirror.

## Local Intake Status

- Local file: `data/raw/hotel_bookings.csv`
- Download method: `curl -L https://raw.githubusercontent.com/manthangandhi/hotel_cancellation_prediction/main/data/hotel_bookings.csv -o data/raw/hotel_bookings.csv`
- Access date: 2026-04-18
- File size: ~16.1 MB
- Row count (including header): 119,391 lines → **119,390 records**
- Column count: **32**
- Expected target: `is_canceled` (binary)
- Rubric size check: 119,390 rows and 32 columns satisfy the ISOM3360 requirement of 5,000–500,000 rows and >20 features.

## Verified Column List

Columns as observed in the local file header (snake_case):

`hotel, is_canceled, lead_time, arrival_date_year, arrival_date_month, arrival_date_week_number, arrival_date_day_of_month, stays_in_weekend_nights, stays_in_week_nights, adults, children, babies, meal, country, market_segment, distribution_channel, is_repeated_guest, previous_cancellations, previous_bookings_not_canceled, reserved_room_type, assigned_room_type, booking_changes, deposit_type, agent, company, days_in_waiting_list, customer_type, adr, required_car_parking_spaces, total_of_special_requests, reservation_status, reservation_status_date`

Naming note: the upstream `@manthangandhi/hotel_cancellation_prediction` README uses PascalCase labels (e.g., `IsCanceled`, `LeadTime`), but the CSV itself uses snake_case. This project uses the snake_case column names as they appear in the file.

## Leakage Review Candidates

These columns are only known after the booking outcome and must be excluded from features unless explicitly justified:

- `reservation_status` — contains the outcome directly.
- `reservation_status_date` — dated the outcome was recorded.
- `assigned_room_type` — assigned at or near check-in; post-booking signal.
- `booking_changes` — may include changes made after cancellation decisions.

Mark any other ambiguous-timing columns for review in [notebooks/02_data_understanding.ipynb](../notebooks/02_data_understanding.ipynb) before modeling begins.

## Local Versus Source Alignment Notes

- File name matches the upstream `hotel_bookings.csv`.
- Column names and order match the upstream CSV.
- No local filtering or adaptation has been performed yet. Any deviations (e.g., dropped rows, renamed columns, derived columns) must be logged here before final submission.

## Attribution Rules For The Report

- Cite `@manthangandhi/hotel_cancellation_prediction` for topic framing and provenance.
- Cite Antonio, de Almeida, & Nunes (2019) for the original dataset.
- Any interpretation, variable description, or figure borrowed from the source repo must be explicitly flagged in the report and in the relevant notebook.
