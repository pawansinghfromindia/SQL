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
  <summary>Window functions</summary>
 
- Window functions perform calculations within a window. 
- We have a long group of Window functions. We categorized them in 3 groups :

1. **Window Aggregate Functions** : used for Aggregations
2. **Window Rank Functions** : used for ranking data
3. **Window Value(Analytical) Functions** : used for access specific value

| Window <br/> Aggregate Functions  | Window <br/> Rank Functions   | Window <br/> Value (Analytical) Functions             |
|-----------------------------------|-------------------------------|-------------------------------------------------------|
| ```COUNT()```  -> All data types  | ```ROW_NUMBER()```   -> Empty |  ```LEAD(expr, offset, default)```  -> All data types |
| ```SUM()```    -> Numeric         | ```RANK()```         -> Empty |  ```LAG(expr, offset, default)```   -> All data types |
| ```AVG()```    -> Numeric         | ```DENSERANK()```    -> Empty |  ```FIRST_VALUE(expr)```            -> All data types |    
| ```MAX()```    -> Numeric         | ```CUME_DIST()```    -> Empty |  ```FIRST_VALUE(expr)```            -> All data types |
| ```MIN()```    -> Numeric         | ```PERCENT_RANK()``` -> Empty |                                     -> All data types |   
|                                   | ```PERCENT_RANK()``` -> Empty |                                     -> All data types |  
|                                   | ```NTILE(n)```       -> Number|                                     -> All data types |   

</details>

<!-------------------------------------------------------------->

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


| Window <br/> Aggregate Functions  | Window <br/> Rank Functions   | Window <br/> Value (Analytical) Functions             |
|-----------------------------------|-------------------------------|-------------------------------------------------------|
| ```COUNT()```  -> All data types  | ```ROW_NUMBER()```   -> Empty |  ```LEAD(expr, offset, default)```  -> All data types |
| ```SUM()```    -> Numeric         | ```RANK()```         -> Empty |  ```LAG(expr, offset, default)```   -> All data types |
| ```AVG()```    -> Numeric         | ```DENSERANK()```    -> Empty |  ```FIRST_VALUE(expr)```            -> All data types |    
| ```MAX()```    -> Numeric         | ```CUME_DIST()```    -> Empty |  ```FIRST_VALUE(expr)```            -> All data types |
| ```MIN()```    -> Numeric         | ```PERCENT_RANK()``` -> Empty |                                     -> All data types |   
|                                   | ```PERCENT_RANK()``` -> Empty |                                     -> All data types |  
|                                   | ```NTILE(n)```       -> Number|                                     -> All data types |   

</details>

- ```AVG(sales) OVER(ORDER BY order_date)``` : Here, ```AVG()``` function takes sales **column** as an argument. Arrument is what we pass to a function.
- ```RANK() OVER(ORDER BY order_date)``` : Here, ```RANK()``` function is **empty**, doesn't take any argument, bcuz that's defined like that.
  If we pass something to it, it gives us error.
- ```NTEIL(2) OVER(ORDER BY order_date)``` : Here, ```NTEIL()``` function takes a **number** as an argument.
- ```LEAD(sales, 2, 10) OVER(ORDER BY order_date)``` : Here, ```LEAD()``` function takes **multiple arhuments**
- ```SUM(CASE WHEN sales>100 ThEN 1 ELSE 0 END) OVER(ORDER BY order_date)```, : Here, ```SUM()``` function takes **Conditional Logic** as an argument

> Each function has its own specification, we have to be careful while which data type we're using with which function.

**```over()``` Clause** : Tells SQL that the function used is a window function.
 - ```over()``` Clause is used in order to define the window or subset of data.
 - Inside it, we can define multiple stuffs like ```PARTITION BY```, ```ORDER BY```, even frame ```ROWS UNBOUNDED PRECEDING``` But all these are optional.


### Window Function ```PARTITION BY```
- It divides the entire datasets into windows(partitions)/groups
- ```PARTITION BY``` divides the rows into groups, based on the column/s.
- It is very similar to ```GROUP BY```

```python
    SUM(sales) OVER()          ##Sum will be calculated on entire dataset one window
```

```python
    SUM(sales) OVER(PARTITION BY product)
    ##PARTITION BY divides dataset into windows, so Sum will be calculated on individually on each window 
```

| Month | Product | Sales |
|-------|---------|-------|
| Jan   | Bottle  | 20    |
| Jan   | Caps    | 10    |
| Jan   | Bottle  | 30    |
| Feb   | Gloves  | 5     |
| Feb   | Caps    | 70    |
| Feb   | Gloves  | 40    |

Result of ```SUM(sales) OVER()``` : 175 (Entire dataset as a single window & Sum will be calculated)

Result of ```SUM(sales) OVER(PARTITION BY product)``` : (Entire dataset is divided into multiple windows, sum will be calculated on each window)

| Product | SUM(sales) |
|---------|------------|
| Bottle  | 50         |
| Caps    | 80         |
| Bottle  | 45         |

Result of ```SUM(sales) OVER(PARTITION BY month)``` : (Entire dataset is divided into multiple windows, sum will be calculated on each window)

| Month | SUM(sales) |
|-------|------------|
| Jan   | 60         |
| Feb   | 115        |

**Without ```PARTITION BY```** 
```python 
   SUM(sales) OVER()
```
- Total sales across all rows (Entire Result set)

**```PARTITION BY``` single column** 
```python 
   SUM(sales) OVER(PARTITION BY product)
```
- Total sales for each product (multiple windows)

**```PARTITION BY``` combined column** 
```python 
   SUM(sales) OVER(PARTITION BY product, order_status)
```
- Total sales for each combination of product and order_status (multiple windows)

> Note : ```PARTITION BY``` is optional for all the windows functions.

```python
   # Task : Find the total sales across all orders,
   #        Additionally provide details such as order_id and order_date

  SELECT
         order_id,
         order_date,
         SUM(sales) OVER() AS total_sales
  FROM Sales.orders
```

```python
   # Task : Find the total sales for each product,
   #        Additionally provide details such as order_id and order_date

  SELECT
         product,
         order_id,
         order_date,
         SUM(sales) OVER(PARTITION BY product) AS total_sales
  FROM Sales.orders
```
```python
   # Task : Find the total sales across all orders,
   #        Find the total sales for each product
   #        Additionally provide details such as order_id and order_date

  SELECT
         order_id,
         order_date,
         product,
         sales,
         SUM(sales) OVER() AS total_sales,
         SUM(sales) OVER(PARTITION BY product) AS TotalSalesByProduct
  FROM Sales.orders
```

> Flexibility of Window allows aggregation of data at different granularities within the same query.

```python
   # Task : Find the total sales for each combination of product and order_status,
   #        Additionally provide details such as order_id and order_date

  SELECT
         product,
         order_status,
         order_id,
         order_date,
         SUM(sales) OVER(PARTITION BY product, order_status) AS TotalSalesByEachProductAndOrderStatus
  FROM Sales.orders
```

### Window Function ```ORDER BY```
- ```ORDER BY``` sort the data within a window. (Ascending | Descending)
- ```ORDER BY``` is optional for Window Aggregate function but for Window Rank function & Window Value function ```ORDER BY``` is mandatory in query. bcuz It make no sense if we are ranking data with sorting it first.

```python
     RANK() OVER(PARTITION BY month ORDER BY sales DESC)
```
| Month | Product | Sales |
|-------|---------|-------|
| Jan   | Bottle  | 20    |
| Jan   | Caps    | 10    |
| Jan   | Bottle  | 30    |
| Feb   | Gloves  | 5     |
| Feb   | Caps    | 70    |
| Feb   | Gloves  | 40    |

Here, What this query will do is : first ```PARTITION BY``` is going to divide dataset into 2 partitions(months : 1 for Jan & another for Feb ) then ```ORDER BY``` sales in ```DESC``` order, So SQL will go each window seperately & start sorting data in desc order in the first window once it's done now It will sort the next window in desc order. Now Rank will rank the values in window one & similarly for window two as well. 

| Month | Product | Sales | Rank |
|-------|---------|-------|------|
| Jan   | Bottle  | 30    |  1   |
| Jan   | Bottle  | 20    |  2   |
| Jan   | Caps    | 10    |  3   |
|_______|_________|_______|______|
| Feb   | Caps    | 70    |  1   |
| Feb   | Gloves  | 40    |  2   |
| Feb   | Gloves  | 5     |  3   |

```python
   # Task : Rank each order based on their sales from highest to lowest.
   #        Additionally provide the details such as order_id, order_date.

    SELECT
         order_id,
         order_date, 
         RANK() OVER(ORDER BY sales DESC) AS order_rank
    FROM Sales.orders
```

### Window Frame ```ROWS UNBOUNDED PRECEDING```
- We call it **Window frame** or **frame clause**
- Window Frame defines a subset of rows within each window that is relevant for the calculation.

> Window Frame : Define a subset of rows in a window.

Let's understand How it works? <br/>
If we do aggregation & we don't use window function, that means we are considering entire data (rows inside table). <br/>
What we can do is divide the data by using ```PARTITION BY``` into multiple windows, suppose we have window1 and window2. Now if we do aggregation all the window1 will be aggregates & also window2 will be also aggregated. <br/>
So, What we can say to SQL is, we dont't want all rows inside the window. We want subset of row inside the window. <br/>
So, what we can do here is as we have 2 windows, now we can specify scope or subset of data from each window to be involve in aggregation. <br/>
So, Here we will get Window inside a window this can be done using ```Window Frame``` clause. <br/>

> ```PARTITION BY``` is used in order to divide entire dataset into multiple windows

> ```FRAME``` clause is used if you don't want to consider all the rows in each window for calculation(aggregation). You just want to focus on specified subset of data in each window then we can use Frame clause.

```python
     AVG(sales) OVER (PARTITION BY category ORDER BY order_date ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
                                             
```
- ```ROWS``` is Frame Types, It can be of 2 types:
  - 1. ```ROWS``` 
  - 2. ```RANGE```
      
-  ```CURRENT ROW``` : Frame Boundary(Lower Value)
   - ```CURRENT ROW```
   - ```N PRECEDING```
   - ```UNBOUNDED PRECEDING```
     
- ```UNBOUNDED FOLLOWING``` : Frame Boundary(Higher Value)
  - ```CURRENT ROW```
  - ```N FOLLOWING```
  - ```UNBOUNDED FOLLOWING```

As we can see, we are defining the boundary or range from low value to high value.

> Rule 1 : Frame clause can only be used together with ```ORDER BY``` clause.
> Rule 2 : Lower value must be before the higher value.

```python
    SUM(sales) OVER(ORDER BY month ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING)
```

| Month | Sales |  Result |
|-------|-------|---------|
| Jan   | 20    |  60     |
| Feb   | 10    |  45     |
| Mar   | 30    |  105    |
| Apr   | 5     |  75     |
| Jun   | 70    |  70     |

> ```n-FOLLOWING``` : nth row after the current row.

Here, SQL will start with current_row which is row1 and 2 following that means after row1 two more rows i.e. row3
So, in the scope row1 to row3,It will sum() the sales for the dataset.
Next is current_row will move to row2 & after that 2 following which is after the current_row, so row2 & after that 2 more rows i.e. row4
So, in the scope row2 to row4, it will sum() the sales for this dataset.
And this will continue until current becomes last row

```python
    SUM(sales) OVER(ORDER BY month ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
```
| Month | Sales |  Result |
|-------|-------|---------|
| Jan   | 20    |  135    |
| Feb   | 10    |  115    |
| Mar   | 30    |  105    |
| Apr   | 5     |  75     |
| Jun   | 70    |  70     |

> ```UNBOUNDED FOLLOWING``` : The last possible row within a window.


```python
    SUM(sales) OVER(ORDER BY month ROWS BETWEEN 1 PRECEDING  AND CURRENT ROW)
```
> ```n PRECEDING ``` : The nth row before the current row.

| Month | Sales |  Result |
|-------|-------|---------|
| Jan   | 20    |  20     |
| Feb   | 10    |  30     |
| Mar   | 30    |  40     |
| Apr   | 5     |  35     |
| Jun   | 70    |  75     |

```python
    SUM(sales) OVER(ORDER BY month ROWS BETWEEN UNBOUNDED PRECEDING  AND CURRENT ROW)
```

> ```UNBOUNDED PRECEDING``` : The first possible row within a window.

| Month | Sales |  Result |
|-------|-------|---------|
| Jan   | 20    |  20     |
| Feb   | 10    |  30     |
| Mar   | 30    |  60     |
| Apr   | 5     |  65     |
| Jun   | 70    |  135    |


```python
    SUM(sales) OVER(ORDER BY month ROWS BETWEEN 1 PRECEDING  AND 1 FOLLOWING)
```
| Month | Sales |  Result |
|-------|-------|---------|
| Jan   | 20    |  30     |
| Feb   | 10    |  60     |
| Mar   | 30    |  45     |
| Apr   | 5     |  105    |
| Jun   | 70    |  75     |

```python
    SUM(sales) OVER(ORDER BY month ROWS BETWEEN UNBOUNDED PRECEDING  AND UNBOUNDED FOLLOWING)
```
| Month | Sales |  Result |
|-------|-------|---------|
| Jan   | 20    |  135    |
| Feb   | 10    |  135    |
| Mar   | 30    |  135    |
| Apr   | 5     |  135    |
| Jun   | 70    |  135    |

```python
     SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING) totalSales
     FROM Sales.orders
```
| order_id | order_date | order_status | sales | totalSales |
|----------|------------|--------------|-------|------------|
| 1        | 2026-01-01 | Delivered    | 10    | 55         |
| 3        | 2026-01-10 | Delivered    | 20    | 95         |
| 5        | 2026-02-01 | Delivered    | 25    | 105        |
| 6        | 2026-02-05 | Delivered    | 50    | 80         |
| 7        | 2026-02-15 | Delivered    | 30    | 30         |
|__________|____________|______________|_______|____________|
| 2        | 2026-01-05 | Shipped      | 15    | 165        |
| 4        | 2026-01-20 | Shipped      | 60    | 170        |
| 8        | 2026-02-18 | Shipped      | 90    | 170        |
| 9        | 2026-03-10 | Shipped      | 20    | 80         |
| 10       | 2026-03-15 | Shipped      | 60    | 60         |

```python
     SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) totalSales
     FROM Sales.orders
```

### Compact Frame (Short form)
- We can use shortcuts but we can use them only with the PRECEDING.
- For only ```PRECEDING```, the ```CURRENT ROW``` can be skipped.

|              | FRAME can be short, which can skip PRECEDING                   | valid/invalid |
|--------------|----------------------------------------------------------------|---------------|
| Normal Frame | ```ROWS BETWEEN CURRENT ROW AND 2 FOLLOWING```                 | ✅           |
| Normal Frame | ```ROWS BETWEEN 2 PRECEDING AND CURRENT ROW```                 | ✅           |
| Normal Frame | ```ROWS BETWEEN 2 PRECEDING AND 2 FOLLOWING```                 | ✅           |
| Normal Frame | ```ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW```         | ✅           |
| Normal Frame | ```ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING```         | ✅           |
| Normal Frame | ```ROWS BETWEEN 3 PRECEDING AND UNBOUNDED FOLLOWING```         | ✅           |
| Normal Frame | ```ROWS BETWEEN UNBOUNDED PRECEDING AND 2 FOLLOWING```         | ✅           |
| Normal Frame | ```ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING``` | ✅           |
|______________|________________________________________________________________|______________|
| Short  Frame | ```ROWS  2 PRECEDING```                                        | ✅           |
| Short  Frame | ```ROWS  UNBOUNDED PRECEDING```                                | ✅           |
| Short  Frame | ```ROWS  2 FOLLOWING```                                        | ❌           |
| Short  Frame | ```ROWS  UNBOUNDED FOLLOWING```                                | ❌           |

```python
     SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS 2 PRECEDING) totalSales
     FROM Sales.orders
```

### Default Frame 
- SQL uses Default Frame, if ```ORDER BY``` is used without Frame.
- The default frame is ```ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW```

```python
    SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) totalSales
```
Default Frame :
```python
    SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) totalSales
```
Shortcut of Default Frame :
```python
   SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS UNBOUNDED PRECEDING) totalSales3
```


```python
     SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) totalSales1
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) totalSales2
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date ROWS UNBOUNDED PRECEDING) totalSales3 --shortcut
           --both totalSales1 == totalSales2 == totalSales3, bcuz of default frame, third one is shortcut of default frame.
     FROM Sales.orders
```

> ```ORDER BY``` always uses a frame. Even if you don't define the frame, it uses default frame.

</details>
<!---------------------------------------------->
<details>
  <summary><b>Window Functions 4xRules : Limitations of Window Functions</b></summary>

 Let's understand the Rules of Window Functions, in other words Limitations.

 Here, We will learn what's we are not allowed to do while using Window functions.

 **Rule 1.** : Window functions can be used only in ```SELECT``` and ```ORDER BY``` clauses.
 ```python
     SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) totalSales
     FROM Sales.orders
     ORDER BY SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) DESC
```
> Meaning : **Window Functions can't be used to filter data.**

Not Allowed : Throw Error ❌
```python
     SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) totalSales
     FROM Sales.orders
     WHERE SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) > 100
     GROUP BY SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date) 
```
> EXCEPTION : Window function can be used together with ```GROUP BY``` in the same query, only if the same columns are used.

 **Rule 2.** : Nesting Window functions is not allowed.

Not Allowed : Throw Error ❌
```python
       SELECT
           order_id,
           order_date,
           order_status,
           sales.
           SUM(SUM(sales) OVER(PARTITION BY order_status ORDER BY order_date)) OVER(PARTITION BY order_status ORDER BY order_date) totalSales
     FROM Sales.orders
```

**Rule 3.** : SQL execute Window functions after ```WHERE``` clause.

```python
     # Task : Find the total sales for each order_status But only for the two product 101 and 102

     SELECT
          product_id,
          order_id,
          order_status,
          sales,
          SUM(sales) OVER(PARTITION BY order_status) as totalSales
     FROM Sales.orders
     WHERE product_id IN(101, 102)
```
**Rule 4.** : Window function can be used together with ```GROUP BY``` in the same query, only if the same columns are used.

```python
    # Rank customer based on their total sales.
    # Sounds easy but it's not!
    # Here, 1st we have to rank customers then 2nd is aggregate it based on the totalSales by each customer.

    # Here, At step1 : we don't have to show ant details  so, we can go by GROUP BY
    SELECT
          customer_id,
          SUM(sales) as totalSales
    FROM Sales.orders
    GROUP BY customer_id

    ## But We have to rank in step2, to here we're forced to use window function.
    ## Now on top of GROUP BY, we're going to use Window function in order to rank.

    SELECT
          customer_id,
          SUM(sales) as totalSales,
          RANK() OVER(ORDER BY  SUM(sales) DESC) AS rankCustomerByTotalSales
    FROM Sales.orders
    GROUP BY customer_id
```

| customer_id | totalSales | rankCustomerByTotalSales |
|-------------|------------|--------------------------|
|  3          | 125        |  1                       |
|  1          | 110        |  2                       |
|  4          | 90         |  3                       |
|  2          | 55         |  4                       |

> SQL allows us to use Window Function together with the ```GROUP BY``` but only with the one rule, Anything which you're using inside the window function must be part of the ```GROUP BY```, in above example we're using ```SUM(sales)``` inside the Window function bcuz ```SUM(sales)``` is the part of ```GROUP BY```
> If we remove SUM and just only use sales then it is not allowed as sales is not part of group by. It will throw error.

Not Allowed : Throw Error ❌
```python
   SELECT
          customer_id,
          SUM(sales) as totalSales,
          RANK() OVER(ORDER BY  sales DESC) AS rankCustomerByTotalSales
    FROM Sales.orders
    GROUP BY customer_id
```

Allowed : Rank based on customer_id ✅
```python
   SELECT
          customer_id,
          SUM(sales) as totalSales,
          RANK() OVER(ORDER BY  customer_id DESC) AS rankCustomerByTotalSales
    FROM Sales.orders
    GROUP BY customer_id
```
> 1st step : Just build the query by ```GROUP BY``` <br/>
> 2nd step : Define & build window function <br/>
> This will result into nice analytical use cases in solving your problems. <br/>
  
</details>

<details>
  <summary><b>Summary of Window Function</b></summary>

**SQL Window Function** : Performs calculations on subset of data without losing details.
- We can do aggregation & not losing details unlike ```GROUP BY```

**Window function VS ```GROUP BY```**
- Window functions are very powerful & dynamic compare to ```GROUP BY```
- Advanced Data Analysis using Window Functions & Simple Data Analysis using  ```GROUP BY```
- We can use ```GROUP BY``` + Window function in same query only if same column is used.
  - First step is do ```GROUP BY```
  - Later use Window function in the same query.

**Window Function Components**
- Window Functions
  - Aggregation Functions in Window
- Window Definition using ```OVER()``` Clause
  - If we have to Divide the data : ```PARTITION BY``` 
  - If we have to sort the data : ```ORDER BY```
  - If we have to define subset of data : Frame ```ROWS BETWEEN UNBOUNDED PRECEDENT AND 2 FOLLOWING```

**Rules/Limitations of Window Functions**
- We can use window function only in ```SELECT``` and ```ORDER BY``` clause. We can't use window function in ```WHERE``` clause in order to filter data
- Nesting of Window Function is not allowed. For that we have to use multiple sub-queries.
- SQL excecutes window functions always after filtering data using ```WHERE```
- We can use ```GROUP BY``` + Window function in same query only if same column is used.
  
</details>

With that, We have learned the basics about the Window functions. 
Next we will learn about Window functions like 
1. Window Aggregate Functions
2. Window Ranking Functions
3. Window Value functions
<!----------------------------------------------->

## 8.3 Window Aggregate Function

Here, We will learn How to summarize data for specific group of rows.

<details>
  <summary><b>Window Aggregate Function ?</b></summary>

Let's say we have data in table inside the database

| Month | Sales |
|-------|-------|
| Jan   | 20    |
| Feb   | 10    |
| Mar   | 30    |
| Apr   | 5     |
| Jun   | 70    |
| Jul   | 40    |

If we apply any aggregate function in SQL, What SQL does is go through all rows of entire dataset or window and start aggregating the data and in the result output SQL gives one single aggregated value.

| totalSales|
|-----------|
|  175      |
</details>

<details>
 <summary> <b> Syntax of Window Aggregate Functions </b> </summary>
 
**Syntax of Aggregate Functions :**
```python
    AVG(sales) OVER(PARTITION BY product_id ORDER BY sales)
```
- Except ```COUNT()``` which takes both numeric and string value as input, rest every other aggregate function ```AVG()```, ```SUM()```, ```MAX()```,```MIN()``` takes only numeric value bcuz we can't sum or average or max or min of string value.

- ```PARTITION BY``` is optional
- ```ORDER BY``` is also optional

```python
    AVG(sales) OVER()
```
> That means, the whole definition of Window could be empty for the aggregate function.

| Window <br/> Aggregate Functions | Expression     |   Partition Clause | Order Clause | Frame Clause |
|----------------------------------|----------------|--------------------|--------------|--------------|
| ```COUNT()```                    | All data types |  Optional          | Optional     | Optional     |
| ```SUM()```                      | Numeric values |  Optional          | Optional     | Optional     |
| ```AVG()```                      | Numeric values |  Optional          | Optional     | Optional     |
| ```MAX()```                      | Numeric values |  Optional          | Optional     | Optional     |
| ```MIN()```                      | Numeric values |  Optional          | Optional     | Optional     |

| Window <br/> Aggregate Functions | Definition                               |   Syntax                                     |
|----------------------------------|------------------------------------------|----------------------------------------------|
| ```COUNT()```                    | Returns the number of Rows in a window   | ```COUNT(*) OVER(PARTITION BY product )```   |
| ```SUM()```                      | Returns the sum of values in a window    | ```SUM(sales) OVER(PARTITION BY product )``` |  
| ```AVG()```                      | Returns the average of values in a window| ```AVG(sales) OVER(PARTITION BY product )``` |   
| ```MAX()```                      | Returns the maximum of value in a window | ```MAX(sales) OVER(PARTITION BY product )``` |  
| ```MIN()```                      | Returns the minimum of value in a window | ```MIN(sales) OVER(PARTITION BY product )``` |

</details>

<details>
 <summary> <b> <code> COUNT() </code></b> </summary>
- Returns the number of rows within a window.

```python
    # Task : Find the total number of orders for each product

    SELECT
         product,
         sales,
         COUNT(*) OVER(PARTITION BY product) AS count
    FROM Sales.orders  
```

| product | sales | count |
|---------|-------|-------|
| Caps    |  20   |   3   |
| Caps    |  10   |   3   |
| Caps    |  5    |   3   |
|_________|_______|_______|
| Gloves  |  30   |   3   |
| Gloves  |  70   |   3   |
| Gloves  | NULL  |   3   |

⚠️ WARNING : Careful with NULL values while you're using Aggregate functions.

> ```COUNT(*)``` counts all the rows in a table, regardless of whether any value is NULL.

> Sometime people used ```COUNT(1)``` in place of ```COUNT(*)```. Don't worry both are exactly same in result & performaces.

> ```COUNT(*)```==```COUNT(1)```

**```COUNT(column)``` vs ```COUNT(*)```==```COUNT(1)```**
- ```COUNT(column)``` : counts the number of non-NULL values in column. It ignore NULLs.
- ```COUNT(*)``` : counts the number of all values in column whether it is NULL or non-NULL.

```python
    # Task : Find the total number of orders for each product, if there is null don't count it.
    # Here, Don't count blindly, We have to count only those rows which have not null values. 

    SELECT
         product,
         sales,
         count(sales) OVER(PARTITION BY product) AS count
    FROM Sales.orders  
```

| product | sales | count |
|---------|-------|-------|
| Caps    |  20   |   3   |
| Caps    |  10   |   3   |
| Caps    |  5    |   3   |
|_________|_______|_______|
| Gloves  |  30   |   2   |
| Gloves  |  70   |   2   |
| Gloves  | NULL  |   2   |

**```COUNT( )```** is exception, It allows any data type numbers, characters, dates and so on.
- counts the number of values in a column regardless of their data type.


Note : Counts the total number of rows, including duplicates, not the unique values.

```python
    # Task : Find the total number of product for each product, if there is null don't count it including duplicates, not the unique values 

    SELECT
         product,
         sales,
         count(product) OVER(PARTITION BY product) AS count
    FROM Sales.orders  
```
| product | sales | count |
|---------|-------|-------|
| Caps    |  20   |   3   |
| Caps    |  10   |   3   |
| Caps    |  5    |   3   |
|_________|_______|_______|
| Gloves  |  30   |   3   |
| Gloves  |  70   |   3   |
| Gloves  | NULL  |   3   |

```python
   # Find the total number of orders.

   SELECT
       COUNT(*) AS TotalOrders
   FROM Sales.orders

  # Find the total number of orders.
  # Additionally provide details such as order_id & order_date.

  SELECT
       order_id,
       order_date,
       COUNT(*) OVER() AS TotalOrders
   FROM Sales.orders

  # Find the total number of orders for each customers

  SELECT
       customer_id,
       order_id,
       order_date,
       COUNT(*) OVER() AS TotalOrders,
       COUNT(*) OVER(PARTITION BY customer_id) AS OrdersByCustomers
   FROM Sales.orders
```
| order_id | order_date | customer_id  | TotalOrders | OrdersByCustomers |
|----------|------------|--------------|-------------|-------------------|
| 3        | 2026-01-10 | 101          | 10          | 3                 |
| 4        | 2026-01-20 | 101          | 10          | 3                 |
| 7        | 2026-02-15 | 101          | 10          | 3                 |
|__________|____________|______________|_____________|___________________|
| 1        | 2026-01-01 | 201          | 10          | 3                 |
| 5        | 2026-02-01 | 201          | 10          | 3                 |
| 9        | 2026-03-10 | 201          | 10          | 3                 |
|__________|____________|______________|_____________|___________________|
| 10       | 2026-03-15 | 301          | 10          | 3                 |
| 6        | 2026-02-05 | 301          | 10          | 3                 |
| 2        | 2026-01-05 | 301          | 10          | 3                 |
|__________|____________|______________|_____________|___________________|
| 8        | 2026-02-18 | 401          | 10          | 1                 |

```python
   # Find total number of customers, doesn't matter whether customer is NULL inside database count all of them.
   # Additionally provide all customers's details.

   SELECT
        customer_id,
        customer_name,
        COUNT(*) OVER() AS TotalCustomers
  FROM Sales.customers

   # Find total number of scores for the customers.

   SELECT
        COUNT(score) OVER() AS TotalNumOfScore
   FROM Sales.customers
```


**Use case of Count()** : 
- 1. Quick summary or snapshot of the entire dataset.
 Example : How many employees? How many orders? etc etc.

- 2. Total Per Groups : Group-wise analysis, to understand patterns within different categories



- 3. Data Quality Check : Detecting number of NULLs by comparing to total number of rows.

```python
   SELECT
        *,
        COUNT(*) OVER() AS TotalCustomersStar
        COUNT(1) OVER() AS TotalCustomersOne
        COUNT(score) OVER() AS TotalNumOfScore
  FROM Sales.customers
```


- 4. Data Quality Issue : Duplicates lead to inaccuracies in analysis. ```COUNT()``` can be used to identify deuplicates to improve data quality.

```python
   # Task : Check whether the table orders contains any duplicate rows
   # To solve this 1st thing is to check & understand what is the primary key of table orders

SELECT *
FROM( 
  SELECT
        order_id,
        COUNT(*) OVER(PARTITION BY order_id) checkPK,  --Divides the data by order_id which is a Primary key
   FROM Sales.orders
) WHERE checkPK > 1
```
**Use case of Count()** 
1. Overall Analysis
2. Category Analysis
3. Quality Checks : Identify NULLs
4. Quality Checks : Identify Duplicates

</details>

<details>
 <summary> <b> <code> SUM() </code></b> </summary>
- Returns the sum of values within a window.
```python
    # Task : Find the total sales for each product

    SELECT
         product,
         sales,
         SUM(sales) OVER(PARTITION BY product) AS TotalSales
    FROM Sales.orders  
```

| product | sales | TotalSales |
|---------|-------|------------|
| Caps    |  20   |   35       |
| Caps    |  10   |   35       |
| Caps    |  5    |   35       |
|_________|_______|____________|
| Gloves  |  30   |   100      |
| Gloves  |  70   |   100      |
| Gloves  | NULL  |   100      |

> ```SUM()``` ignores the NULLs in calculation.

> ```SUM()``` accepts only numberic data type.

```python
   # Task : Find the total sales across all orders
   #        and the total sales for each product
   #        Additonally provide details such as order_id and order_date.

   SELECT
       order_id,
       order_date,
       sales,
       product,
       SUM(sales) OVER() AS TotalSales
       SUM(sales) OVER(PARTITION BY product) AS TotalSales
   FROM Sales.order
```

### Comparison Use Case
Compare the current value and aggregated value of window functions.

| Month | Sales |
|-------|-------|
| Jan   | 20    |
| Feb   | 10    |
| Mar   | 30    | 
| Apr   | 5     |
| Jun   | 70    |
| Jul   | 40    |

Suppose current is currently at March and sales is 30, So we can compare this value i.e. current sales with an aggregated value for example TotalSales using ```SUM()```. So, what will happen with the current value with the totalSales is we can doing comparison analysis called **part-to-whole Analysis** where we are going to understand how important was the sales at March month compared to TotalSales. or we can compare it with best month(highest sales) or worst month(lowest sales) using ```MAX()``` or ```MIN()``` or ```AVG()``` in order to understand above the typical sale or below the average.
This is very important analysis in order to study & understand the performance of current data.

```python
   # Task : Find the percentage contribution of each product's sales to the total sales.

      SELECT
          product,
          order_id,
          sales,
          SUM(sales) OVER() TotalSales,
          ROUND(CAST(sales AS Float)/SUM(sales) OVER() * 100, 2) AS percentageOfTotal
     FROM Sales.order
 
```
> PART-TO-WHOLE shows the contribution of each data point to the overall dataset.

</details>

<details>
 <summary> <b> <code> AVG() </code></b> </summary>
 
- Return the average of values within a window.
```python
    # Task : Find the average sales for each product

    SELECT
         product,
         sales,
         AVG(sales) OVER(PARTITION BY product) AS avgSales
    FROM Sales.orders  
```

| product | sales | avgSales   |
|---------|-------|------------|
| Caps    |  20   |   11.66    |
| Caps    |  10   |   11.66    |
| Caps    |  5    |   11.66    |
|_________|_______|____________|
| Gloves  |  30   |   50       |
| Gloves  |  70   |   50       |
| Gloves  | NULL  |   50       |

- Here, NULL doesn't means Zero in every business. So handle NULLs we can either use ```ISNULL()``` or ```COALESCE()``` function.

```python
    SELECT
         product,
         sales,
         AVG(COALESCE(sales, 0)) OVER(PARTITION BY product) AS avgSales
    FROM Sales.orders  
```
| product | sales | avgSales   |
|---------|-------|------------|
| Caps    |  20   |   11.66    |
| Caps    |  10   |   11.66    |
| Caps    |  5    |   11.66    |
|_________|_______|____________|
| Gloves  |  30   |   33.33    |
| Gloves  |  70   |   33.33    |
| Gloves  |  0    |   33.33    |

```python
   # Task : Find the average sales across all order and the average sales for each product
   #         Additionally, provide details such as order_id, order_date
    SELECT
        product,
        order_id,
        order_date,
        sales,
        AVG(COALSCE(sales, 0)) OVER() AS avgSales,
        AVG(COALSCE(sales, 0)) OVER(PARTITION BY product) AS prodAVGSales
   FROM Sales.orders
```

```python
    # Task : Find the average scores of customers.
    #        Additionally, provide details such as customer_id and customer_name

     SELECT
          customer_id,
          customer_name,
          score,
          AVG(COALESCE(score), 0) OVER() AS customerAVG
    FROM Sales.customers
```
- COMPARE TO AVERAGE : Helps to evaluate whether a value is above or below the average.

```python
    # Task : Find all orders where sales are higher than the average sales across all orders.

   SELECT *
   FROM   (
            SELECT
                order_id,
                sales,
                AVG(sales) OVER() AS avgSales
            FROM Sales.customers 
           ) WHERE sales > avgSales  
```
</details>

<details>
 <summary> <b> <code> MAX() </code> and <code> MIN() </code> </b> </summary>

- They are very simple but yet very powerful in analytics.
- ```MIN()``` return the minimum(lowest) value within a window.
- ```MAX()``` return the maximum(highest) value within a window.

```python
   # Task : Find the highest sales for each product
   # Task : Find the lowest sales for each product

       SELECT
           product,
           sales,
           MAX(sales) OVER() AS highestSales,
           MIN(sales) OVER() AS lowestSales
      FROM Sales.orders

```

| product | sales | MIN | MAX |
|---------|-------|-----|-----|
| Caps    |  20   |  5  | 20  |
| Caps    |  10   |  5  | 20  |
| Caps    |  5    |  5  | 20  |
|_________|_______|_____|_____|
| Gloves  |  30   | 30  | 70  |
| Gloves  |  70   | 30  | 70  |
| Gloves  | NULL  | 30  | 70  |

> To handle NULLs we have to use ```ISNULL()``` or ```COALESCE()``` by replacing it with 0 in case of MAX but in case of MIN replace with 99999

```python
      # Task : Find the highest & lowest sales across all orders and
      #        the highest & lowest sales for each product.
      #        Additionally, provide details such as order_id and order_date.

      SELECT
           product,
           order_id,
           order_date,
           sales,
           MAX(sales) OVER() AS highestSales,
           LOW(sales) OVER() AS lowestSales,
           MAX(sales) OVER(PARTITION BY product) AS highestSalesByProduct,
           LOW(sales) OVER(PARTITION BY product) AS lowestSalesByProduct
     FROM Sales.orders
```

```python
     # Task : Show the employees who have the highest salaries.

    SELECT *
    FROM ( 
          SELECT
              employee_id,
              employee_name,
              salary,
              MAX(salary) OVER() as highestSalary
          FROM Sales.customers
    ) WHERE salary = highestSalary
```

Compare to Extreme : Help to evaluate how well a value is performing relative to the extremes(Highest/Lowest)

Distance From Extreme 
- The lower the deviation, the closer the data point is to extreme.
- The higher the deviation, the distant the data point is to extreme.

```python
   # Task : Calculate the deviation of each sale from both the minimun and maximum sales amounts.

     
            SELECT
                sales,
                MAX(sales) OVER() AS high,
                LOW(sales) OVER() AS low,
                sales - MIN(sales) OVER() deviationFromMIN,
                MAX(sales) - sales OVER() deviationFromMAX
          FROM Sales.orders
```
</details>

<details>
 <summary> <b> Running Total & Rolling Total and Moving Averages </b> </summary>

### Analytical Use case of Aggregate Function : Running & Rolling Total

- One of the most important use case of Aggregate function is doing running total & rolling total
- These two concepts are very important for Data Analysis & Reporting that we must know.
- The key use case of these 2 concept is to do Tracking

> Tracking : Tracking current sales with Target sales in our business
> Trend Analysis : Providing insights into historical patterns.

**RUNNING & ROLLING TOTAL**
- They aggregate sequence of members, the aggegation is updated each time a new member is added to sequence. Sequence could be like Time sequence. That's why we call this type **Analysis Over Time**

**What is the difference between the RUNNING TOTAL and ROLLING TOTAL?**

RUNNING TOTAL : Aggregate all values from the begining up to the current point without dropping off older data.

ROLLING TOTAL : Aggregate all values within a fixed time window (e.g. Last 30 days, last 2 months).
- As new data is added, the oldest data point will be dropped.
- In this we will get effect of **Rolling/Shifting window**

RUNNING TOTAL
```python
    # Find the running total of sale for each month.

    SUM(sales) OVER( ORDER BY Month)
    # Default Window Frame : ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
```
| Month | Sales | SUM |
|-------|-------|-----|
| Jan   | 20    | 20  |
| Feb   | 10    | 30  |
| Mar   | 30    | 60  |
| Apr   | 5     | 65  |
| Jun   | 70    | 135 |
| Jul   | 40    | 175 |

ROLLING TOTAL
```python
     # Find the 3 months rolling total of sale for each month.
 
    SUM(sales) OVER( ORDER BY Month ROWS BETWEEN 2 PRECEDING AND CURRENT ROW)
```
| Month | Sales | SUM |
|-------|-------|-----|
| Jan   | 20    | 20  |
| Feb   | 10    | 30  |
| Mar   | 30    | 60  |
| Apr   | 5     | 45  |
| Jun   | 70    | 105 |
| Jul   | 40    | 115 |

So, We can see here, Window functions are very powerful for Data Analytics.

 
### Analytical Use case of Aggregate Function : Moving Average

```python
   # Calculate the moving average of sales for each product over time.

      SELECT
           product,
           order_id,
           order_date,
           sales,
           AVG(sales) OVER( PARTITION BY product) AS avgByProduct,
           AVG(sales) OVER( PARTITION BY product ORDER BY order_date) AS movingAVG
           --Default Frame : ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
      FROM Sales.order
```
> Over time analysis means sorting dates in ascending order.

```python
    # Task : Calculate the moving average of sales for each product over time, including only the next order.

        SELECT
             product,
             order_id,
             order_date,
             sales,
             AVG(sales) OVER(PARITION BY product) avgSalesByProd,
             AVG(sales) OVER(PARITION BY product ORDER BY order_date ROWS BETWEEN CURRENT ROW AND 1 FOLLOWING) movingAvgSalesByProd
       FROM Sales.orders     
```
</details>

<details>
 <summary> <b>  Summary of Window Aggregate Functions </b> </summary>

**Window Aggregate Functions**
- Aggregate set of values and return a single aggregated value
- Here, we don't lose details unlike GROUP BY

**Rules**
- Expression of Aggregate function
  - Numbers (All Functions ```COUNT()```,```SUM()```,```AVG()```,```MAX()```,```MIN()```)
  - Any Data type (```COUNT()```)
- All clauses like ```PARTITION BY```, ```ORDER BY``` or Window Frame are optional inside ```OVER()```

**Use Cases**

-  Overall Total Analysis: Overview of entire data  ```SUM(sales) OVER()```

- Total Per Groups Analysis : Compare categories    ```SUM(sales) OVER(PARTITION BY product)```

- Comparison Analysis
   - Average
   - Extreme : Highest/Lowest

- Part=to-whole Analysis : Running Total |  Rolling Total
  - Running Total Analysis : Progress over time ```SUM(sales) OVER(PARTITION BY product ORDER BY month ROWS BEWTEEN UNBOUNDED PRECEDENT AND CURRENT ROW)```
  - Rolling Total Analysis : Progress over time in specific fixed window ```SUM(sales) OVER(PARTITION BY product ORDER BY month ROWS BEWTEEN  2 PRECEDENT AND CURRENT ROW)```
  - Moving Average
  
- Identify Data Quality Issue by checking Duplicates using Window aggregate function ```COUNT()``` 

- Outlier Detection : Finding out the data points above average, below average  & so on.

> This is amazing how Window Aggregate functions ```ORDER BY```, ```PARTITION BY``` and window frames open door for advanced data analysis.

</details>
<!---------------------------------------------->

## 8.4 Window Ranking Function
Here, We will learn how to rank data using window functions?

<details>
  <summary><b>Window Ranking Function</b></summary>

What are Window Ranking Functions?

Let's say we have data in a table inside database, We want to rank them.

| Products | Sales |
|----------|-------|
| A        | 20    |
| B        | 30    |
| C        | 10    |
| D        |  5    |
| E        | 70    |

```python
   # Task : Rank the products based on their sales

```

In the process of ranking, first step SQL does is sort the data before doing anything.
We have 2 methods in order to rank our data :
1. Integer-based Ranking : Here, SQL assign an integer for each row based on the position of row
- Here, we have distinct integer values.

2. Percentage-based Ranking : Here, SQL assign a percentage to each row compare to all others & then assign a percentage for each row.
- Here, we have values on scale from 0 to 1, between 0 & 1, we have infinite number of data points, this called a normalize scale or continuous value.

| Products | Sales | Int-based Ranking | Per-based Ranking |
|----------|-------|-------------------|-------------------|
| E        | 70    |    1              |   0               |
| B        | 30    |    2              |   0.25            |
| A        | 20    |    3              |   0.50            |
| C        | 10    |    4              |   0.75            |
| D        |  5    |    5              |   1               |


Now the Question is when to use which one? 

On one hand, Percentage-based ranking is used to answer such questions like **Find the Top 20% products based on their sales**
- This method is used to understand the contribution of data value to the oevrall Total.
- This type of analysis is called as ***Distributed Analysis***.

On the other hand Integer-based ranking is used to answers such questions like **Find the Top 3 products**
- This method is not interested about the contribution of each product to the overall total, here we're just interested in the position of value within a list.
- This is also very commonly used analysis or reporting, we call it ***Top/Bottom N Analysis***
 
|  Window Ranking Functions     |    2 Methods of Ranking              | 
|-------------------------------|--------------------------------------|
| Integer-based Ranking         | Percenatge-based Ranking             |
| It is Top/Bottom N Analyis    | It is Distribution Analysis          |
| Find Top N products           | Find Top N% Products                 |
| Here, rank in Discrete Values | Here, rank in Continuous values (0,1)|
| ```ROW_NUMBER()```            |  ```CUME_DIST()```                   |
| ```RANK()```                  |  ```PERCENT_RANK()```                |
| ```DENSE_RANK()```            |                                      |
| ```NTILE()```                 |                                      |

</details>

<details>
    <summary> <b>Syntax of Window Ranking Functions</b> </summary>

```python
    RANK() OVER(PARTITION BY product_id ORDER BY sales)
```
- ```RANK()``` function doesn't takes any argument. So, It must be empty.
- ```PARTITION BY``` is optional.
- ```ORDER BY``` is required. So, It must be there bcuz without sorting how can we rank something.
- Window frame is not allowed in ranking functions.

| Window <br/> Ranking Functions | Expression | Partition Clause | Order Clause | Frame Clause |
|--------------------------------|------------|------------ -----|--------------|--------------|
| ```ROW_NUMBER()```             | Empty      |  Optional        | Required     | Not Allowed  |
| ```RANK()```                   | Empty      |  Optional        | Required     | Not Allowed  |
| ```DENSE_RANK()```             | Empty      |  Optional        | Required     | Not Allowed  |
|  ```CUME_DIST()```             | Empty      |  Optional        | Required     | Not Allowed  |
| ```PERCENT_RANK()```           | Empty      |  Optional        | Required     | Not Allowed  |
| ```NTILE(n)```                 | Number     |  Optional        | Required     | Not Allowed  |

| Window <br/> Ranking Functions | Definition                                                               | Syntax                                   |
|--------------------------------|--------------------------------------------------------------------------|------------------------------------------|
| ```ROW_NUMBER()```             | Assign a unique number to each in a window                               | ```ROW_NUMBER() OVER(ORDER BY sales)```  |
| ```RANK()```                   | Assign a rank to each row in a window, with gaps                         | ```RANK() OVER(ORDER BY sales)```        |
| ```DENSE_RANK()```             | Assign a rank to each row in a window, without gaps                      | ```DENSE_RANK() OVER(ORDER BY sales)```  |
| ```CUME_DIST()```              | Calculates the cumulative distribution of a value within a set of values | ```CUME_DIST() OVER(ORDER BY sales)```   |
| ```PERCENT_RANK()```           | Returns the percentile ranking numbers of a row                          | ```PERCENT_RANK() OVER(ORDER BY sales)```|
| ```NTILE(n)```                 | Divides the rows into a specified number of approximately equal groups   | ```NTILE(n) OVER(ORDER BY sales)```      |

</details>

<details>
 <summary> <b> <code> ROW_NUMBER() </code> </b> </summary>
 
- Assign a unique number to each in a window, without gaps/skipping.
- It doesn't handles ties. Meaning, If you have 2 Rows sharing the same value, they will not share same rank.  
- In the output, you will see unique ranking without gaps/skipping.
- Example Rank will always be like ```1,2,3,4,5``` unique even if the rows share same value. meaning Doesn't handles ties

| Sales |
|-------|
| 80    |
| 20    |
| 80    |
| 50    |
| 100   |
 
```python
         ROW_NUMBER() OVER(ORDER BY sales)
```  

| Sales | Rank |
|-------|------|
| 100   |  1   |
| 80    |  2   |
| 80    |  3   |
| 50    |  4   |
| 20    |  5   |

```python
   # Rank the orders based on the their sales from highest to lowest.

   SELECT
        order_id,
        product_id,
        sales,
        ROW_NUMBER() OVER(ORDER BY sales DESC) SalesRankRow
   FROM Sales.orders
```
| Order_id | product_id | Sales | SalesRankRow |
|----------|------------|-------|--------------|
| 8        | 101        | 90    |   1          |
| 4        | 105        | 60    |   2          |
| 10       | 102        | 60    |   3          |
| 6        | 104        | 50    |   4          |
| 7        | 102        | 30    |   5          |
| 5        | 104        | 25    |   6          |
| 9        | 101        | 20    |   7          |
| 3        | 101        | 20    |   8          |
| 2        | 102        | 15    |   9          |
| 1        | 101        | 10    |   10         |
 
</details>



<details>
 <summary> <b> <code> RANK() </code> </b> </summary>

- Assign a rank to each row in a window.
- It handles ties, Meaning If 2 rows will have same data, sharing same values. then both will share same rank.
- It leaves gaps in ranking, once you have tie.                   
- Rank is not unique, Sharing Ranking, leaving with gaps(skipping)

- Like in Olympics, if 2 athelites tie for a Gold Medal(1st place) then there will be no Silver Medal(2nd Place). Next Medal i.e Bronze (3rd Place) will be given to 3rd place.

```python
         RANK() OVER(ORDER BY sales)
```       
| Sales | Rank |
|-------|------|
| 100   |  1   |
| 80    |  2   |
| 80    |  2   |
| 50    |  4   |
| 20    |  5   |

```python
   # Rank the orders based on their sales from highest to lowest

    SELECT
         order_id,
         product_id,
         sales,
         RANK() OVER( ORDER BY sales DESC) AS SalesRank
    FROM Sales.orders
```
| Order_id | product_id | Sales | SalesRank    |
|----------|------------|-------|--------------|
| 8        | 101        | 90    |   1          |
| 4        | 105        | 60    |   2          |
| 10       | 102        | 60    |   3          |
| 6        | 104        | 50    |   3          |
| 7        | 102        | 30    |   5          |
| 5        | 104        | 25    |   6          |
| 9        | 101        | 20    |   7          |
| 3        | 101        | 20    |   7          |
| 2        | 102        | 15    |   9          |
| 1        | 101        | 10    |   10         |
 
</details>

<details>
 <summary> <b> <code> DENSE_RANK() </code> </b> </summary>

 - Assign a rank to each row in a window.
 - It handles ties.
 - It is very similar to ```RANK()``` but why do we need it? bcuz It doesn't leaves gaps in ranking.
 - Shared Ranking + Leaving no gaps(skipping)
 
 ```python
         DENSE_RANK() OVER(ORDER BY sales)
```
| Sales | Rank |
|-------|------|
| 100   |  1   |
| 80    |  2   |
| 80    |  2   |
| 50    |  3   |
| 20    |  4   |

```python
   # Task : Rank the orders based on their sales from Highest to Lowest

   SELECT
        order_id,
        product_id,
        sales,
        DENSE_RANK() OVER(ORDER BY sales DESC) AS salesDenseRank
   FROM Sales.orders
```
| Order_id | product_id | Sales |salesDenseRank|
|----------|------------|-------|--------------|
| 8        | 101        | 90    |   1          |
| 4        | 105        | 60    |   2          |
| 10       | 102        | 60    |   3          |
| 6        | 104        | 50    |   3          |
| 7        | 102        | 30    |   4          |
| 5        | 104        | 25    |   5          |
| 9        | 101        | 20    |   6          |
| 3        | 101        | 20    |   6          |
| 2        | 102        | 15    |   7          |
| 1        | 101        | 10    |   8          |


```python
   # Task : Rank the orders based on their sales from Highest to Lowest

   SELECT
        order_id,
        product_id,
        sales,
        ROW_NUMBER() OVER( ORDER BY sales DESC) AS SalesRank_Row
        RANK() OVER( ORDER BY sales DESC) AS SalesRank_Rank
        DENSE_RANK() OVER(ORDER BY sales DESC) AS salesRank_Dense
   FROM Sales.orders
```
| Order_id | product_id | Sales | RowNumRank   | Rank | Rank_Dense |
|----------|------------|-------|--------------|------|------------|
| 8        | 101        | 90    |   1          | 1    |  1         | 
| 4        | 105        | 60    |   2          | 2    |  2         |
| 10       | 102        | 60    |   3          | 2    |  2         |
| 6        | 104        | 50    |   4          | 4    |  3         |
| 7        | 102        | 30    |   5          | 5    |  4         |
| 5        | 104        | 25    |   6          | 6    |  5         |
| 9        | 101        | 20    |   7          | 7    |  6         |
| 3        | 101        | 20    |   8          | 7    |  6         |
| 2        | 102        | 15    |   9          | 9    |  7         |
| 1        | 101        | 10    |   10         | 10   |  8         |

</details>

### Integer-based Ranking Comparison

| ```ROW_NUMBER()```  | ```RANK()```     |  ```DENSE_RANK()```| 
|---------------------|------------------|--------------------|
| Unique rank         | Shared Rank      | Shared Rank        |
| Doesn't handle Ties | Handles Ties     | Handles Ties       |
| No Gaps in Rank     | Gaps in Rank     | No Gaps in Rank    |

### Case of ```ROW_NUMBER()```

1. Use Case of ROW_NUMBER() : Top N Analysis

> Top-N Analysis : Analyse the top performers to do targeted marketing.

```python
     # Task : Find the top highest sales for each product

     SELECT *
     FROM (
           SELECT
                order_id,
                product_id,
                sales,
                ROW_NUMBER(PARTITION BY product_id) OVER(ORDER BY sales) AS rankByProductSales
          FROM Sales.orders
     ) WHERE rankByProductSales = 1
```
| Order_id | product_id | Sales | RowNumRank   | 
|----------|------------|-------|--------------|
| 8        | 101        | 90    |   1          | 
| 9        | 101        | 20    |   2          |
| 3        | 101        | 20    |   3          |
| 1        | 101        | 10    |   4          |
|__________|____________|_______|______________|
| 10       | 102        | 60    |   1          | 
| 7        | 102        | 30    |   2          |
| 2        | 102        | 15    |   3          |
|__________|____________|_______|______________|
| 5        | 104        | 25    |   1          |
| 6        | 104        | 50    |   2          | 
|__________|____________|_______|______________|
| 4        | 105        | 60    |   1          | 

Result output :

| Order_id | product_id | Sales | RowNumRank   | 
|----------|------------|-------|--------------|
| 8        | 101        | 90    |   1          | 
| 10       | 102        | 60    |   1          | 
| 5        | 104        | 25    |   1          |
| 4        | 105        | 60    |   1          | 

2. Use Case of ```ROW_NUMBER()``` : Bottom-N Analysis

> Bottom-N Analysis : Helps in analyse the underperfmance to manage risks and to do optimizations.

```python
     # Task : Find the lowest 2 customers based on their total sales

     # Step1 : Total sales for each customers, easy by using GROUP BY
     # Step2 : Lowest 2 customer, so we can use window rank function to rank them, Don't need to partition by customers.

     SELECT *
     FROM (
           SELECT
                customer_id,
                SUM(sales) totalSales,
                ROW_NUMBER() OVER( ORDER BY SUM(sales) ASC) as customerRank
          FROM Sales.orders
          GROUP BY customer_id
     ) WHERE customerRank <= 2

    # Note : Columns used in GROUP BY and Window function must be same.
```

3. Use Case of ```ROW_NUMBER()``` : Generate Unique IDs

<details>
 <summary> <b> <code> CUME_DIST() </code> </b> </summary>

- Calculates the cumulative distribution of a value within a set of values
-  ```CUME_DIST() OVER(ORDER BY sales)```   
</details>

<details>
 <summary> <b> <code> PERCENT_RANK() </code> </b> </summary>
- Returns the percentile ranking numbers of a row 
 ```PERCENT_RANK() OVER(ORDER BY sales)```
 
</details>

<details>
 <summary> <b> <code> NTILE(n)</code> </b> </summary>
- Divides the rows into a specified number of approximately equal groups   
 ```NTILE(n) OVER(ORDER BY sales)```      |
 
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
