# Dormant Customers Identification

# Problem Statement

The task at hand involves identifying dormant customers within a food delivery service platform, akin to Zomato. Dormant customers are users who have signed up on the platform but have not placed any orders recently. This information is crucial for targeted marketing strategies and efforts to re-engage customers who may have become inactive.


![day 30_1](https://github.com/bhumikadata/-Sure-Below-is-a-README.md-file-for-your-GitHub-repository-Dormant-Customers-Identification/assets/131578649/a76038c4-288f-44fe-af30-cc5ed228aa29)

![day 30_2](https://github.com/bhumikadata/-Sure-Below-is-a-README.md-file-for-your-GitHub-repository-Dormant-Customers-Identification/assets/131578649/c473113e-1a16-466e-8299-d4bd85e54bec)



# SQL Solution

I approached the problem using SQL queries to filter out dormant customers based on specific criteria. The SQL code identifies users who registered more than 6 months ago but have not placed any orders in the last 3 months. Additionally, the query retrieves the last order amount for each dormant customer, displaying it as 0 if the customer hasn't placed any orders.

# SQL Code

```
-- Common Table Expression to Rank Orders
with cte as (
    select * 
    , row_number() over(partition by user_id order by order_date desc) as rn
    from orders
)
select 
    users.user_id,
    coalesce(cte.order_amount,0) as last_order_amount
from 
    users
left join 
    cte 
on 
    users.user_id=cte.user_id and cte.rn=1
where 
    users.registration_date < date('now','-6 months')
and 
    ((julianday('now') - julianday(cte.order_date))/30 > 3 or cte.order_date is null)
```


# Explanation:

#### Common Table Expression (CTE):

The SQL query begins with a CTE named cte, which ranks orders for each user based on the order date, allowing us to identify the most recent order for each user.

#### Joining Tables:

The users table is left-joined with the cte to ensure all users are included in the result set, even if they haven't placed any orders.

#### Filtering Dormant Customers:

The WHERE clause filters out users who registered more than 6 months ago (users.registration_date < date('now','-6 months')) and haven't placed any orders in the last 3 months (((julianday('now') - julianday(cte.order_date))/30 > 3 or cte.order_date is null)).

# Potential Use Cases

##### Targeted Marketing Campaigns:

The platform can utilize the list of dormant customers to launch tailored marketing campaigns, offering discounts or promotions to encourage them to place orders again.

##### Customer Re-engagement Strategies:

By analyzing the behavior of dormant customers, the platform can devise personalized re-engagement strategies, such as sending targeted emails or push notifications with enticing offers.

#### Improving User Retention:

Understanding the reasons behind customer dormancy can provide valuable insights for improving the platform's services or user experience, ultimately leading to higher customer retention rates.
