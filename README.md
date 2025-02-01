# E-Commerce Growth Analysis: Customer, Product & Sales Insights

![image](https://github.com/user-attachments/assets/4ce28c02-cd64-4ea3-af2e-13c6d5bff5e5)

## Project Overview
This project goes beyond just running SQL queries—it's a **data-driven, business-impactful analysis** of **20,000+ Amazon sales records**. By leveraging advanced SQL techniques, this project uncovers **actionable insights** across key areas such as **sales performance, customer behavior, order trends, and payment analytics**.  

The dataset consists of multiple tables related to **sales, customers, orders, products, and payments**, providing a **comprehensive view of e-commerce operations**. The project focuses on **data analysis, transformation, and optimization**, enabling businesses to **enhance decision-making, optimize inventory, and drive revenue growth**.  

## Business Impact
- **Sales Performance Analysis**: Identifying top-selling products and revenue trends.
- **Customer Insights**: Understanding customer purchasing behavior and segmentation.
- **Order & Shipping Analysis**: Tracking order fulfillment efficiency and return rates.
- **Payment Trends**: Evaluating different payment methods and their impact on sales.

## Key Features
- **Exploratory Data Analysis (EDA)**: Basic queries to understand the dataset.
- **Advanced SQL Queries**: Aggregations, joins, window functions, and subqueries for in-depth analysis.
- **Data Transformation**: Creating new calculated fields and updating tables for enhanced analysis.
- **Performance Optimization**: Use of indexing, efficient joins, and query restructuring.

## SQL Techniques Used
This project employs a variety of advanced SQL techniques, including:

### 1. Window Functions
#### Ranking Best & Least-Selling Products
```sql
SELECT 
    product_id, 
    product_name, 
    SUM(total_sales) AS total_sales, 
    RANK() OVER (ORDER BY SUM(total_sales) DESC) AS sales_rank
FROM order_items
GROUP BY product_id, product_name;
```
#### Comparing Monthly Sales Trends
```sql
SELECT 
    year, month, 
    SUM(total_sales) AS current_month_sales,
    LAG(SUM(total_sales), 1) OVER(ORDER BY year, month) AS last_month_sales
FROM sales_data
GROUP BY year, month;
```

### 2. Common Table Expressions (CTEs)
#### Identifying Least-Selling Product Categories
```sql
WITH category_sales AS (
    SELECT 
        category_id, 
        category_name, 
        SUM(total_sales) AS total_sales
    FROM products p
    JOIN order_items oi ON p.product_id = oi.product_id
    GROUP BY category_id, category_name
)
SELECT * FROM category_sales ORDER BY total_sales ASC LIMIT 5;
```

### 3. Stored Procedures 
#### Automating Revenue Calculation
```sql
CREATE PROCEDURE CalculateMonthlyRevenue(IN report_month INT, IN report_year INT)
LANGUAGE plpgsql
AS $$
BEGIN
    SELECT 
        SUM(total_sales) AS revenue
    FROM sales_data
    WHERE EXTRACT(MONTH FROM order_date) = report_month
    AND EXTRACT(YEAR FROM order_date) = report_year;
END;
$$;
```
#### Generating Customer Order Reports
```sql
CREATE PROCEDURE GetCustomerOrderHistory(IN customer_id INT)
LANGUAGE plpgsql
AS $$
BEGIN
    SELECT 
        o.order_id, o.order_date, SUM(oi.total_sales) AS order_value
    FROM orders o
    JOIN order_items oi ON o.order_id = oi.order_id
    WHERE o.customer_id = customer_id
    GROUP BY o.order_id, o.order_date;
END;
$$;
```
### 4. Advanced Aggregation & Segmentation
#### Customer Segmentation Based on Spending
```sql
SELECT 
    customer_id, 
    SUM(total_sales) AS total_spent,
    DENSE_RANK() OVER (ORDER BY SUM(total_sales) DESC) AS customer_rank
FROM orders o
JOIN order_items oi ON o.order_id = oi.order_id
GROUP BY customer_id;
```
### 5. Query Performance Optimization
#### Using Indexing for Faster Query Execution
```sql
CREATE INDEX idx_total_sales ON order_items(total_sales);
ANALYZE order_items;
```
#### Using EXPLAIN ANALYZE for Query Debugging
```sql
EXPLAIN ANALYZE 
SELECT * 
FROM orders o 
JOIN customers c ON o.customer_id = c.customer_id 
WHERE c.state = 'California';
```

## **Technology Stack**   

- **Database**: PostgreSQL  
- **SQL Queries**: DDL, DML, Aggregations, Joins, Subqueries, Common Table Expressions (CTEs), Window Functions, Indexing, Stored Procedures  
- **Tools**:  
  - **pgAdmin 4** (or any SQL editor)  
  - **PostgreSQL** (via Homebrew, Docker, or direct installation)  

## **How to Run the Project**  
Follow these steps to set up and execute the project:  

1. **Install PostgreSQL and pgAdmin** (if not already installed).  
2. **Set up the database schema** using the provided SQL scripts.  
3. **Load the dataset** by inserting sample sales data into respective tables.  
4. **Execute the SQL queries** to analyze:  
   - **Sales performance and revenue trends**  
   - **Customer segmentation and buying behavior**  
   - **Inventory and order fulfillment efficiency**  
   - **Payment method trends and optimization**  
5. **Optimize queries** using indexing, EXPLAIN ANALYZE, and performance tuning techniques.  
