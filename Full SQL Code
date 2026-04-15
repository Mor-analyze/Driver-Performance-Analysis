---- total revenue for each driver
with revenue_per_driver as (
select driver_id, 
sum(total_revenue) as total_revenue,
avg(on_time_delivery_rate) as AVG_on_time_delivery_rate
from driver_monthly_metrics
where driver_id is not null
group by driver_id
),

--- total fuel cost for each driver
fuel_cost_per_driver as (
select driver_id, sum(total_cost) as feul_cost from FUEL_PURCHASES
where driver_id is not null
group by driver_id 
 ),

 --- total miles for each driver
 miles_per_driver as (
 select driver_id, sum(actual_distance_miles) as total_miles from trips
 where driver_id is not null
 group by driver_id
 ),

--- total damage cost and number of incidents for each driver
damage_cost_per_driver as (
select 
	si.driver_id , 
	count(incident_id) as NumberOFincidents,
	sum(vehicle_damage_cost) as vehicle_damage_cost ,
	sum(cargo_damage_cost) as cargo_damage_cost,
	sum(vehicle_damage_cost) + sum(cargo_damage_cost) as total_damage_cost
from safety_incidents as sI join drivers as d on  d.driver_id = si.driver_id
group by si.driver_id)

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
