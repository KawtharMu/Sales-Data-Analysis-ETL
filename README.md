# Sales-Data-Analysis
### Overview
In this project, I used an SQL Dump found on GitHub, which contains data about an India-based company’s customers, markets, products, and transactions.
This project’s objective was to create a dashboard that would allow managers in the company to gain insights into sales performance and make decisions based on that, where the end goal is increasing sales.<br>
Before beginning, defining the metrics and KPIs is crucial to getting the most out of the data in hand, so the questions we need to answer are:
1. 	What’s the company’s revenue growth over time?
2. 	What’s the revenue from each specific client?
3. 	How many sales were made in each year/month?
4. 	How many sales were made by a specific client?
5. 	Who are the top customers?
6. 	Which products are bestsellers?
7. 	What markets bring in the most revenue?
8. 	What markets have the biggest sales quantity?
9. 	What percentage of sales were made online?

The dashboard will help answer the questions above, shedding light on areas of improvement to increase sales.


### Data Analysis Using SQL

1. Show all customer records

    `SELECT * FROM customers;`

1. Show total number of customers

    `SELECT count(*) FROM customers;`

1. Show transactions for Chennai market (market code for chennai is Mark001

    `SELECT * FROM transactions where market_code='Mark001';`

1. Show distrinct product codes that were sold in chennai

    `SELECT distinct product_code FROM transactions where market_code='Mark001';`

1. Show transactions where currency is US dollars

    `SELECT * from transactions where currency="USD"`

1. Show transactions in 2020 join by date table

    `SELECT transactions.*, date.* FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020;`

1. Show total revenue in year 2020,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and transactions.currency="INR\r" or transactions.currency="USD\r";`
	
1. Show total revenue in year 2020, January Month,

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020 and and date.month_name="January" and (transactions.currency="INR\r" or transactions.currency="USD\r");`

1. Show total revenue in year 2020 in Chennai

    `SELECT SUM(transactions.sales_amount) FROM transactions INNER JOIN date ON transactions.order_date=date.date where date.year=2020
and transactions.market_code="Mark001";`


Star Schema
============================
![image](https://user-images.githubusercontent.com/92683172/192153405-22568e52-0b57-40c6-a5d4-8c673a97d7d3.png)


ETL Procedure
============================

<li> 	Extracted the data from the SQL Dump onto MySQL, creating a sales Database containing the following tables: Customers, Date, Markets, Products, Transactions.<br><br>
<li>	Transformed the data to prepare it for loading onto Power BI, the transformation process included cleansing the data and establishing consistency:<br>	 	
1. Formula to remove -1 and 0 values from sales_amount column - in sales_transactions table

`= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))`

2. Formula to create norm_sales_amount column (currency inversion from INR to USD) - in sales_transactions table

`= Table.AddColumn(#"Cleanup currency", "Norm_sales_amount", each if [currency] = "INR" then Number.Round([sales_amount]*0.012309894,2) else [sales_amount])`

3. Formula to remove duplicates - in sales_transactions table

`= Table.SelectRows(#"remove -1/0 from sales amount", each ([currency] = "INR#(cr)" or [currency] = "USD#(cr)"))`

4. Formula to remove NULL zones - in sales_markets table

`= Table.SelectRows(sales_markets, each ([zone] <> ""))`
<br><br>
<li> 	Loaded the clean data onto Power BI to build the dashboard.


Building Dashboard Using Power BI
============================
![image](https://user-images.githubusercontent.com/92683172/192154640-752ed89a-e6ab-4fe0-969b-682dc1658376.png)
	
	
Mobile Version
============================
![image](https://user-images.githubusercontent.com/92683172/192154780-1ace6fb4-377e-41d9-85ac-eae577a7a7b9.png)


Insights
============================
<li>	The company has 12.12M$ revenue and 2M$ total sales.
<li>	The company’s revenue has been declining since February 2020, most likely due to covid.
<li>	Most of the company’s sales (84.44%) are made through Brick & Mortar as opposed to online sales.
<li>	The Delhi Market is the one with the highest revenue and sales quantity, and has been over the years, followed by Mumbai.
<li>	Electricalsara Stores are the customer with the highest revenue.
<li>	100% of  Electricalsara’s sales are done face-to-face and not online.
<li>	January of 2018 was the month in which the company had the highest revenue with 523M$.
<li>	As shown by the Top 5 Products bar chart, most product codes were left blank, showing there is a serious mistake that’s being done when inputting the transactions, this mistake needs to be addressed to be able to get better insights in which product bring in the highest revenue.

