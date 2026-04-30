# Driver-Performance-Analysis

## 1. Problem 
The goal of this project is to evaluate driver performance using operational and financial data. 

This analysis aims to identify high-performing and underperforming drivers, understand key factors affecting profitability and efficiency, and uncover opportunities for improving operational performance.

Key business questions:
- Which drivers generate the highest profit?
- Are there drivers with consistently high costs (fuel, delays, etc.)?
- How does driver behavior impact efficiency and on-time performance?
- Which drivers require performance improvement or operational adjustments?

## 2. DataSet

The dataset is based on a logistics database containing information about trips, drivers, fuel consumption, and delivery performance.

Main tables used:
- drivers: driver details and identifiers
- trips: trip-level data including distance, duration, and assignments
- loads: revenue information per trip
- fuel_purchases: fuel cost and consumption per trip
- delivery_events: timestamps and on-time delivery indicators
- safety_incidents: records of violations and incidents
- driver_monthly_metrics: aggregated monthly performance metrics per driver (e.g., total miles, trips, revenue, and efficiency indicators)

These tables are joined using primary and foreign keys to create a unified dataset at the driver level for analysis.

## 3. Data Quality Checks

- Identifying missing values in critical fields
- Checking for duplicate records using primary keys
- Validating data types and logical values (e.g., non-negative cost and distance values)
- During data validation, missing values were identified in the `driver_id` field across multiple tables, including `trips`, `fuel_purchases`, and `safety_incidents`. 

The full analysis, including data preparation and exploration, is available in the Jupyter Notebook within this repository.

## 4. SQL Analysis
The goal of this step was to evaluate driver performance and identify high-performing and underperforming drivers. By aggregating operational and financial data at the driver level, this analysis enables:

Identification of low-profit or loss-making drivers
Detection of cost inefficiencies (e.g., high fuel cost per mile)
Evaluation of operational performance through on-time delivery rates
Support for performance improvement and operational decision-making
Methodology

Driver-level metrics were calculated by combining multiple data sources, including revenue, fuel purchases, trip distances, and safety incidents.

Profit was calculated using the following formula:
Profit = Total Revenue − Fuel Cost − Total Damage Cost

Additional performance metrics such as profit margin, fuel cost per mile, and average on-time delivery rate were derived to provide deeper insights into efficiency and reliability.

This approach ensures a holistic evaluation of each driver by considering both financial outcomes and operational behavior.

SQL code:
```
select 
	r.driver_id, 
	r.total_revenue , 
	f.feul_cost ,
	m.total_miles,
	(f.feul_cost / nullif(m.total_miles,0)) as fuel_cost_per_mile,
	d.total_damage_cost , 
	d.NumberOFincidents,
	(r.total_revenue-f.feul_cost - d.total_damage_cost) as profit,
	((r.total_revenue-f.feul_cost - d.total_damage_cost)/r.total_revenue)*100 as profit_margin,
	r.avg_on_time_delivery_rate
from revenue_per_driver r 
join fuel_cost_per_driver f on r.driver_id = f.driver_id
join damage_cost_per_driver as d on r.driver_id = d.driver_id
join miles_per_driver as m on r.driver_id = m.driver_id
order by profit_margin desc
```
Full SQL logic is available in the /sql folder.

Output

<img width="781" height="199" alt="Screenshot 2026-04-14 164844" src="https://github.com/user-attachments/assets/68dc0ab8-037a-4c5b-ac1d-c1c41c378c92" />

## 5. Visualization

<img width="671" height="383" alt="image" src="https://github.com/user-attachments/assets/3ff76a7f-f130-4f5d-8fab-5a5a79fbd43b" />



## 6. Insights
- Only ~7% of drivers fall below a 60% profit margin, indicating that overall driver performance is strong with minimal low-performing outliers.
- There is no clear relationship between on-time delivery rate and profit margin, indicating that operational punctuality is not currently a key driver of profitability.
- The top drivers with the highest at-fault incident damage costs are also part of the lower-performing segment, highlighting a link between safety issues and reduced profitability.
- No loss-making drivers were identified, meaning the opportunity lies in optimizing performance rather than eliminating unprofitable drivers.
- The scatter plot shows a negative relationship between total miles and fuel cost per mile, indicating that drivers covering longer distances tend to operate more efficiently in terms of fuel consumption.




