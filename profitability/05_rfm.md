# Recency, Frequency, Monetary (RFM) Analytics
```sql
create table users (
	id bigint generated always as identity,
	name text not null unique,
	primary key (id)
);

insert into users (name)
select 'user-' || left(md5(n::text), 8) as name
from generate_series(1, 1000) n;


table users;

create table orders (
	id bigint generated always as identity,
	amount bigint not null check (amount >= 0),
	created_at timestamptz not null default current_timestamp,
	user_id bigint not null,
	primary key (id),
	foreign key (user_id) references users(id)
);

create or replace function random_range(_min int, _max int) returns int as $$
	select floor(random() * (greatest(_min, _max) - least(_min, _max))) + least(_min, _max);
$$ language sql;

select setseed(0.5), random_range(1, 100);

-- This must be run first.
select setseed(0);
insert into orders (
	amount,
	created_at,
	user_id
)
select
	random_range(50, 1000000) / 10 * 10 as amount,
	now() - make_interval(days => random_range(0, 365)) as created_at,
	random_range(1, (select count(*) from users)) as user_id
from generate_series(1, 100000);


-- Top 10 users with orders.
select user_id, count(*)
from orders
group by user_id
order by count(*) desc
limit 10;

with order_summary as (
	select
		count(*) as total_orders,
		max(created_at) as last_purchased_at,
		avg(amount) as avg_amount,
		user_id
	from orders
	group by user_id
),
rfm as (
	select
		ntile(4) over (order by total_orders) as rfm_frequency,
		ntile(4) over (order by last_purchased_at) as rfm_recency,
		ntile(4) over (order by avg_amount) as rfm_monetary,
		total_orders,
		last_purchased_at,
		avg_amount,
		user_id
	from order_summary
)
select *
from rfm
where rfm_frequency = 4
and rfm_recency = 4
and rfm_monetary = 4
order by rfm_frequency desc, rfm_recency desc, rfm_monetary desc;
```

With 1 being lowest score, and 4 the highest, we can segment the customers with the RFM score.

An RFM score of 444 means a recency of 4, frequency of 4 and monetary of 4.


| Segment           | RFM Score  | Description                                                                                                                                                                           |
|-------------------|------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Top Customers     | 444        | Most valuable customers:  made the highest amount of purchases, with the least days since last order and the highest monetary value.                                                  |
| Loyal Customers   | X4X        | Customers that made a great number of purchases. This segment does not indicate performance regarding days since the last order or monetary value.                                    |
| High Potentials   | XX4        | Customers that made had a great AOV along their lifecycle. This segment does not indicate performance regarding days since the last order or number of orders in their lifecycle.     |
| Small Buyers      | X42 or X41 | Customers that place few orders will have a small monetary value. This segment does not indicate performance regarding days since the last order.                                     |
| Dormant Customers | 11X        | Customers that placed few orders a long time ago. This segment does not indicate performance regarding the monetary value of those orders.                                            |
| Worst Customers   | 111        | Customers that placed few orders, a long time ago, with small monetary value.                                                                                                         |
| Other             | ---        | Customers who do not fall under the specifications above will be categorized as Other. All customers are assigned an RFM score; however, not all scores have a predefined RFM status.
You can create custom segments based on the RFM scores using our segment builder! |

# References

- http://www.silota.com/docs/recipes/sql-recency-frequency-monetary-rfm-customer-analysis.html
- https://retentionx.zendesk.com/hc/en-us/articles/360017410739-RFM-Analysis
