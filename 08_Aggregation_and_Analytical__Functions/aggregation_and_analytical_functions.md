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

**```COUNT()``` Window Aggregate Functions**
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


- 4. Data Quality Issue : Duplicates lead to inaccuracies in analysis. ```COUNT()``` can be used to identify deuplicates.

```python
   # Task : Check whether the table orders contains any duplicate rows
   # To solve this 1st thing is to check & understand what is the primary key of table orders

   SELECT
        order_id,
        COUNT(*) OVER(PARTITION BY order_id) TotalRows,  --Divides the data by order_id which is a Primary key
        COUNT(order_id) OVER() TotalOrderID
```
  

**```SUM()``` Window Aggregate Functions**




**```AVG()``` Window Aggregate Functions**



**```MAX()``` Window Aggregate Functions**



**```MIN()``` Window Aggregate Functions**
 
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
