# Sales-Data-Analysis
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


Data Analysis Using Power BI
============================

**ETL Procedure** 

1. Formula to remove -1 and 0 values from sales_amount column - in sales_transactions table

`= Table.SelectRows(sales_transactions, each ([sales_amount] <> -1 and [sales_amount] <> 0))`

2. Formula to create norm_sales_amount column (currency inversion from INR to USD) - in sales_transactions table

`= Table.AddColumn(#"Cleanup currency", "Norm_sales_amount", each if [currency] = "INR" then Number.Round([sales_amount]*0.012309894,2) else [sales_amount])`

3. Formula to remove duplicates - in sales_transactions table

`= Table.SelectRows(#"remove -1/0 from sales amount", each ([currency] = "INR#(cr)" or [currency] = "USD#(cr)"))`

4. Formula to remove NULL zones - in sales_markets table

`= Table.SelectRows(sales_markets, each ([zone] <> ""))`

