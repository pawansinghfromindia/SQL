<!--------Chapter 8. Aggregation and Analytical Functions--------->

# Chapter 8. Aggregation and Analytical Functions

- Aggregate Functions
- Window Basics
- Window Aggregate Function
- Window Ranking Function
- Window Value Function

## 8.1 Aggregate Functions  

**```COUNT()```, ```SUM()```, ```AVG()```, ```MAX()``` , ```MIN()```**

Aggregate Functions are amazing if you're a Data Analyst or Data Scientist where you're usually use Aggregate functions in order to uncover insights about data.

So, Aggregate Functions accepts multiple rows as inputs and result output will be one single value.

| Inputs                     | Aggregate Functions | Result | Explaination                 |
|----------------------------|---------------------|--------|------------------------------|
| ```30 ,10, 50, 20, 40```   |    ```COUNT()```    | 5      | number of rows like 1,2,3,4,5|
| ```30 ,10, 50, 20, 40```   |    ```SUM()```      | 150    | 30+10+50+20+40 = 150         |
| ```30 ,10, 50, 20, 40```   |    ```AVG()```      | 30     | (30+10+50+20+40)/5 = 30      |
| ```30 ,10, 50, 20, 40```   |    ```MAX()```      | 50     | max value = 50               |
| ```30 ,10, 50, 20, 40```   |    ```MIN()```      | 10     | min value = 10               |
 

<details>
  <summary><b>Aggregate Functions with <code>GROUP BY</code> and without <code>GROUP BY</code></b></summary>

- Using aggregate functions without ```GROUP BY``` gives you big number about business.

- But if you using it aggregate functions  ```GROUP BY``` then you will be breaking those big numbers into smaller numbers based on column you are specifying in ```GROUP BY```

Let's say in a database Sales, we have a table name orders of which one column is sales. 

| sales |
|-------|
| 35    |
| 15    |
| 20    |
| 10    |

How many total number of orders ?
- ```COUNT(*)``` gives number of rows inside table.
- If we're using ```COUNT(*)```, SQL starts counting the number of rows in table, don't care about content of table.
- Here, in table row1, row2, row3, row4 So, total rows = 4

What is Total Sales?
- ```SUM()``` sums up all the cells and return total
- Here, 35+15+20+10 = 80 which is total sales

What is Average Sales?
- ```AVG()``` return the average by adding up all value & divide it by number of rows

What is Highest Sales?
- ```MAX()``` return the highest value of the cell, Here actually we're not aggregating the data but searching between multiple values.

What is Lowest Sales?
- ```MIN()``` return the lowest value of the cell, Here actually we're not aggregating the data but searching between multiple values.

```sql
    -- Task : Find the total number of orders
    -- Task : Find the total sales of all orders
    -- Task : Find the average sales of all orders
    -- Task : Find the Highest Sales among orders
    -- Task : Find the Lowest Sales among orders

    SELECT
          COUNT(*) AS total_num_of_orders,
          SUM(sales) AS total_sales,
          AVG(sales) AS average_sales,
          MAX(sales) AS highest_sales,
          MIN(sales) AS lowest_sales
    FROM Sales.orders
```
> Using aggregate functions without ```GROUP BY``` gives you big number about business.

> But if you combine it with  a ```GROUP BY``` then you will be breaking those big numbers in to something like aggregating by customer_id or may be by order_date,
 or by country or others.

> Anything you specify with ```GROUP BY``` It will break those big numbers into smaller numbers based on column you are specifying.

```sql

     SELECT
          customer_id,
          COUNT(*) AS total_num_of_orders,
          SUM(sales) AS total_sales,
          AVG(sales) AS average_sales,
          MAX(sales) AS highest_sales,
          MIN(sales) AS lowest_sales
    FROM Sales.orders
    GROUP BY customer_id
```

```sql
    -- Task : Analyse the scores in the customer table

    SELECT
          COUNT(*) AS total_num_of_customers,
          SUM(scores) AS total_scores,
          AVG(scores) AS average_scores,
          MAX(scores) AS highest_scores,
          MIN(scores) AS lowest_scores
    FROM Sales.customers


    -- Analyzing by each customer individually 
    SELECT
          customer_id,
          COUNT(*) AS total_num_of_customers,
          SUM(scores) AS total_scores,
          AVG(scores) AS average_scores,
          MAX(scores) AS highest_scores,
          MIN(scores) AS lowest_scores
    FROM Sales.customers
    GROUP BY customer_id
```
These are the basics on How to aggregate data using SQL Aggregate Functions. <br/>
Now, we can move to more advanced way on How to aggregate your data ? : Window Functions.

</details>
<!--------------------------------------------->

## 8.2 Window Basics

**Window Functions** or sometime we call it as **Analytical Functions**. 
They are very important functions in SQL. 
Everyone must know them especially if you are doing Data Analysis.

> **```Window Function```** is very similar to **```GROUP BY```** but unlike **```GROUP BY```** here, we're not losing rows details.

<details>
  <summary><b>What are  SQL Window functions?</b></summary>

**Window Functions** are allow us to do calculations (e.g. aggregation) on a specific subset of data, without losing the level of details of rows.


| id  | product | sales  |
|-----|---------|--------|
| 1   | Caps    | 10     |
| 2   | Caps    | 30     |
| 4   | Gloves  | 5      |
| 5   | Gloves  | 20     |

Now, We want to see total sales for each product.

If we use ```GROUP BY```, It takes first two order for Caps & put it in one row and same for Gloves
- Here, we have number of rows depends on how many distinct products are there in data.

| product | Total_sales  |
|---------|--------------|
| Caps    | 40           |
| Gloves  | 25           |

> GROUP BY : Returns a single row for each group
> Changes the granularity. It smashes/squeezes the result.
> In input id is controlling the level of details but in output Product is controlling the level of details. 


If we use ```Window Function```, It will execute each rows individually from each other.
- Here, we will not lose the level of details of rows.
  
| id  | product | Total_sales  |
|-----|---------|--------------|
| 1   | Caps    | 40           |
| 2   | Caps    | 40           |
| 4   | Gloves  | 25           |
| 5   | Gloves  | 25           |

> Window Function : Returns a result for each row.
> The granularity stays the same.
> We're still able to do aggregation without losing the level of details. In both input and output, id is controlling the level of details.

</details>

<details>
  <summary> <b> <code>GROUP BY</code> vs <code>WINDOW</code> </b> </summary>

| ```GROUP BY```                                                     | ```WINDOW```                                                         |
|--------------------------------------------------------------------|----------------------------------------------------------------------|
| Simple Aggregations                                                | Aggregations + Keep details                                          |
| Returns a single row for each group                                | Returns a result for each row                                        |
| Changes the granularity                                            | Granularity stays the same                                           |
| Has only Aggregate functions                                       | Has aggregate functions + many more other functions                  |
| ```COUNT()```, ```SUM()```, ```AVG()```, ```MAX()``` , ```MIN()``` | ```COUNT()```, ```SUM()```, ```AVG()```, ```MAX()``` , ```MIN()```   |
|                                                                    | Rank Functions : ```ROW_NUMBER()```, ```RANK()```, ```DENSERANK()``` |
|                                                                    | ```CUME_DIST()``` , ```PERCENT_RANK()```, ```NTILE(n)```             |
|                                                                    | Value Functions : ```LEAD()``` , ```LAG()```, ```FIRST_VALUE()```,```FIRST_VALUE()```   |
| Use it for Simple Data Analysis                                    |  Use it for Advanced Data Analysis                                   |

> In SQL WINDOW Functions, we have a lot of fractions where we can cover a lot of Analytical use cases & advanced complex stuffs.
> But with SQL GROUP BY, we have only the aggregate functions only for simple use cases.
  
</details>
 <!------------------------------------------------------------->

<details>
  <summary><b>Why do we need Window Functions?</b></summary>

Why do we need WINDOW Functions? <br/>
Why ```GROUP BY``` is not enough?

```sql
   -- Task 1 : Find the total sales across all orders.

   SELECT
        SUM(sales) AS total_sales
   FROM Sales.orders
   --Here, we are doing highest level of aggregation.


    -- Task 2 : Find the total sales for each product.

   SELECT
        product_id,
        SUM(sales) AS product_total_sales
   FROM Sales.orders
   GROUP BY product_id
   --Here, we don't have one value means no highest level of aggregation
   --Here, we're drilling to the next level of details

   -- Task 3 : Find the total sales for each product, additionally provide the details such as order_id & order_date.

  SELECT
       product_id,
       order_id,
       order_date,
       SUM(sales) AS product_total_sales
  FROM Sales.orders
  GROUP BY product_id, order_id, order_date
  -- BUT this is not giving you exact thing what is asked for.
  --```GROUP BY``` Limitation : Can't do aggregations and provide details at the same time
  -- Here, comes Window function
```

> Result Granularity : The number of rows in the output is defined by the dimension.

> ```GROUP BY``` Rule : All columns in ```SELECT``` must be included in ```GROUP BY```

> ```GROUP BY``` Limitation : Can't do aggregations and provide details at the same time. Here, ```Window function``` will come to rescue you!

> ```Window Function``` returns a result for each row.

```sql
  -- Task 3 : Find the total sales for each product, additionally provide the details such as order_id & order_date.

         SELECT
              SUM(sales) OVER() total_sales
         FROM Sales.orders;

         SELECT
              product_id,
              SUM(sales) OVER(PARTITION BY product_id) total_salesByProducts
         FROM Sales.orders

         SELECT
              order_id,
              order_date,
              product_id,
              SUM(sales) OVER(PARTITION BY product_id) total_salesByProducts
         FROM Sales.orders
```
  
</details>
<!---------------------------------------------->

<details>
  <summary><b>Syntax of Window Functions</b></summary>

Let's understand the basic parts(components) of each Window Syntax.  <br/>
Mainly we have 2 parts : 
1. Window function : ```COUNT()```, ```SUM()```, ```AVG()```, ```MAX()``` , ```MIN()```
2. Over Clause
    - Partition Clause
    - Order clause
    - Frame Clause

```python
    AVG(sales) OVER (PARTITION BY category ORDER BY order_date ROWS UNBOUNDED PRECEDING)
```
- ```AVG()``` is a window function

<details>
  <summary>Window functions</summary>
- Window functions perform calculations within a window. 
- We have a long group of Window functions. We categorized them in 3 groups :
1. **Window Aggregate Functions** : used for Aggregations
2. **Window Rank Functions** : used for ranking data
3. **Window Value(Analytical) Functions** : used for access specific value

| Window <br/> Aggregate Functions  | Window <br/> Rank Functions  | Window <br/> Value (Analytical) Functions |
|-----------------------------------|------------------------------|-------------------------------------------|
| ```COUNT()```                     | ```ROW_NUMBER()```           |  ```LEAD(expr, offset, default)```        |
| ```SUM()```                       | ```RANK()```                 |  ```LAG(expr, offset, default)```         |
| ```AVG()```                       | ```DENSERANK()```            |  ```FIRST_VALUE(expr)```                  |
| ```MAX()```                       | ```CUME_DIST()```            |  ```FIRST_VALUE(expr)```                  |
| ```MIN()```                       | ```PERCENT_RANK()```         |                                           |
|                                   | ```PERCENT_RANK()```         |                                           |
|                                   | ```NTILE(n)```               |                                           |
</details>

- ```AVG(sales) OVER(ORDER BY order_date)``` : Here, ```AVG()``` function takes sales **column** as an argument. Arrument is what we pass to a function.
- ```RANK() OVER(ORDER BY order_date)``` : Here, ```RANK()``` function is **empty**, doesn't take any argument, bcuz that's defined like that.
  If we pass something to it, it gives us error.
- ```NTEIL(2) OVER(ORDER BY order_date)``` : Here, ```NTEIL()``` function takes a **number** as an argument.
- ```LEAD(sales, 2, 10) OVER(ORDER BY order_date)``` : Here, ```LEAD()``` function takes **multiple arhuments**
- ```SUM(CASE WHEN sales>100 ThEN 1 ELSE 0 END) OVER(ORDER BY order_date)```, : Here, ```SUM()``` function takes **Conditional Logic** as an argument

</details>
<!---------------------------------------------->

## 8.3 Window Aggregate Function
<details>
  <summary><b>Window Aggregate Function</b></summary>
  
</details>
<!---------------------------------------------->

## 8.4 Window Ranking Function
<details>
  <summary><b>Window Ranking Function</b></summary>
  
</details>
<!----------------------------------------------->

## 8.5 Window Value Function
<details>
  <summary><b>Window Value Function</b></summary>
  
</details>
<!------------------------------------------------>

<!--------Chapter 8. Aggregation and Analytical Functions--------->

[< Row-Level Functions](https://github.com/pawansinghfromindia/SQL/blob/main/07_Row-Level_Functions/row_level_functions.md) | 
**Aggregations and Analytical Functions** | [Advanced SQL Techniques >]()
