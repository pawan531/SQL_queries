# SQL_queries

#Amazon want to know count of customer whether they are new customer or old customer on daily basis.

![image](https://user-images.githubusercontent.com/71668492/233616914-37ecec5a-9ab0-44f1-926f-42dbfc51a8aa.png)


select * from customer_orders;

--calculate count of customer whether it is  new customer or old customer

with first_visit as(
select customer_id,min(order_date) as first_visit_date
from customer_orders
group by customer_id)

select co.order_date,sum(case when order_date=first_visit_date then 1 else 0 end) as first_visit_flag,
sum(case when order_date!=first_visit_date then 1 else 0 end) as repeat_visit_flag
from customer_orders co
inner join first_visit fv on co.customer_id=fv.customer_id
group by order_date


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

#problem statement: Do the trasformation for below table:

![image](https://user-images.githubusercontent.com/71668492/233618548-a5520c22-ee0b-45b5-9fda-79d5bba12e50.png)

Select * from icc_worldcup;
select team_name,count(*) as no_of_matches_played,sum(winflag) as no_of_win,count(*)-sum(winflag) as no_of_loss
from
(select team1 as team_name,case when team1=Winner then 1 else 0 end as winflag 
from icc_worldcup
union all
select team2 as team_name,case when team2=Winner then 1 else 0 end as winflag 
from icc_worldcup
) A
group by team_name


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++


#problem statement: select brands whose sales increase every year

![image](https://user-images.githubusercontent.com/71668492/233619632-827aa35f-4ee3-4ffe-b782-2701b2e623fa.png)

select * from brands_que;

with product_cmp as
(
select *,
case when amount<lead (amount,1,amount+1) over (partition by Brand order by year ) then 1
else 0 end as flag
from brands_que
)
select * from brands_que
where Brand not in ( select Brand from product_cmp where flag=0)

+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# problem statement:Udan want to know in particular year it started operation in how many new cities.

![image](https://user-images.githubusercontent.com/71668492/233621440-acb7df86-3038-44a7-a395-1c2aa4799b93.png)

select * from business_city;
with cte2 as (
select datepart(year,business_date)as bus_year,city_id from 
business_city
)
select c1.bus_year,count(case when c2.city_id is null then c1.city_id end)as no_of_newcity_operation from cte2 c1
left join cte2 c2
on c1.bus_year>c2.bus_year and c1.city_id=c2.city_id
group by c1.bus_year


+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++==

problem statement:Uber wants calculate no of profit ride each driver make(Profit rides is nothing if drivers start the ride from same endpoint).

![image](https://user-images.githubusercontent.com/71668492/233622852-40e5c723-9799-4059-a2ec-f5b01d0ae2a9.png)

select * from drivers;
with cte as
(
select *,
case when LAG(end_loc,1,end_loc) over(partition by id order by start_time)=start_loc then 1 else 0 end as Profit_ride from drivers
  )
  select id,count(id) as total_ride,sum(Profit_ride)as profit_ride from cte
  group by id
  
  
  ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

problem Statement:Find out Employee which are In the hospital

![image](https://user-images.githubusercontent.com/71668492/233626176-a12ac94e-a045-4c10-b9db-a8b0dc2bfb1c.png)



select * from hospital;

with emp_latest_time as
(
select emp_id,max(time) as latest_time from hospital
group by emp_id
)
select h.emp_id from hospital h
 join emp_latest_time e
on h.emp_id=e.emp_id
and h.time>=e.latest_time
where case when action='in' then 1 else 0 end=1


++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++==




