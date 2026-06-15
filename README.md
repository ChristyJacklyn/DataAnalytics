# DataAnalytics
Portfolio


Data Preparation
Data Processing and Analytics with PostgreSQL
Project Overview
Developed a PostgreSQL-based data processing workflow to prepare and analyze data for reporting and business insights.
Key Contributions
Implemented an ETL process to import source data into PostgreSQL.
Performed data transformations using SQL, including data type conversion and schema alignment.
Loaded transformed data into target tables for analysis.
Wrote analytical SQL queries to extract insights, calculate metrics, and support reporting requirements.
Used aggregations, filtering, joins, and grouping operations to analyze data and generate summaries.
Improved data consistency by validating and transforming source data before loading.
Technologies
PostgreSQL
SQL
Workflow Source Data → ETL Processing → PostgreSQL Database → Analytical Queries → Reports & Insights
Example 
Extract data 
Copy csv data into tempstore table
\copy tempstore from '/Users/jacklynchristian/Desktop/Jacklyn/Data Analytics/postgress data set/superstore.csv'delimiter',' csv header;

Transform 
i.e. Alter table temp store alter column ship_date type date using ship_date :: date;
Alter table tempstore alter column order_date type date using order_date :: date;

Load data
Into three table to get good insights
i.e
Insert value from the temp table
	insert into superstore_customers(customer_id,customer_name, segment, country, city,state,postal_code,region) select distinct customer_id,customer_name, segment, country, city,state,postal_code,region from tempstore;
                                                                   insert into superstore_orders(order_Id,product_id,customer_id,order_date, ship_date,ship_mode,sales) select order_id,product_id,customer_id,order_date,ship_date,ship_mode,sales from tempstore;

insert into superstore_products(product_id, product_name, category,subcategory) select distict on (product_id) product_id, product_name, category,sub_category from tempstore;


Analytical Query & Insight
Example
SQL Query 1
Objective: State- wise order
select c.state, count(o.order_id) as total_order from superstore_customers c left join superstore_orders o on c.customer_id = o.customer_id group by c.state limit(20);

Insight
Gives the insight most sold orders state- wise, prepares us fro future aspect 
SQL Query 2 
Objective: Monthly Revenue and compare previous month and year revenue
with monthly_sales as (select extract(year from order_date) as year, extract(month from order_date) as month, sum (sales) as monthly_revenue from superstore_orders group by 1,2), growth as (select *, lag(monthly_revenue) over (order by month) as previous_revenue from monthly_sales) select * from growth;

Insight

Monthly and yearly revenue to compare the previous year sales hence helping in production development


SQL Query 3
Objective: Top product sold
select p.subcategory, sum (o.sales) as total_sales_per_procuct from superstore_products p left join superstore_orders o on p.product_id = o.product_id group by subcategory order by total_sales_per_procuct limit 3;
Insight 
Most sold product per catogory



