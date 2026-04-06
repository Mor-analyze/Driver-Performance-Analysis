# Driver-Performance-Analysis

## 1. Problem 
The goal of this project is to evaluate driver performance using operational and financial data. 

This analysis aims to identify high-performing and underperforming drivers, understand key factors affecting profitability and efficiency, and uncover opportunities for improving operational performance.

Key business questions:
- Which drivers generate the highest profit?
- Are there drivers with consistently high costs (fuel, delays, etc.)?
- How does driver behavior impact efficiency and on-time performance?
- Which drivers require performance improvement or operational adjustments?

3. Data Quality Checks

The dataset is based on a logistics database containing information about trips, drivers, fuel consumption, and delivery performance.

Main tables used:
اگر چیز دیگه ای استفاده کردم اینجا اضافه کنم
- drivers: driver details and identifiers
- trips: trip-level data including distance, duration, and assignments
- loads: revenue information per trip
- fuel_purchases: fuel cost and consumption per trip
- delivery_events: timestamps and on-time delivery indicators
- safety_incidents: records of violations and incidents
- driver_monthly_metrics: aggregated monthly performance metrics per driver (e.g., total miles, trips, revenue, and efficiency indicators)

These tables are joined using primary and foreign keys to create a unified dataset at the driver level for analysis.

## 3. Data Quality Checks

During data validation, missing values were identified in the `driver_id` field across multiple tables, including `trips`, `fuel_purchases`, and `safety_incidents`.

Since `driver_id` is a critical key used to attribute performance metrics, the following steps were taken:

- Records with missing `driver_id` in the `trips` table were excluded, as they could not be reliably assigned to any driver and would distort driver-level analysis.
- For related tables (`fuel_purchases`, `safety_incidents`), records were only included if they could be linked to a valid `trip_id` with a known `driver_id`.
- This ensured that all cost and incident data used in the analysis were correctly attributed to drivers.

The impact of removing these records was assessed and found to be minimal relative to the total dataset size. All exclusions were documented to maintain transparency.


## 4. SQL Analysis

👉 You design ONE analysis that answers all of them together
👉 Each question = one visual or insight

