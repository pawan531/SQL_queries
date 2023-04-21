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




