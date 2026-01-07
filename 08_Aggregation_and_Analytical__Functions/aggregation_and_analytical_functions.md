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

<details>
 <summary> Aggregate Functions </summary>

So, Aggregate Functions accepts multiple rows as inputs and result output will be one single value.

| Inputs                     | Aggregate Functions | Result | Explaination                 |
|----------------------------|---------------------|--------|------------------------------|
| ```30 ,10, 50, 20, 40```   |    ```COUNT()```    | 5      | number of rows like 1,2,3,4,5|
| ```30 ,10, 50, 20, 40```   |    ```SUM()```      | 150    | 30+10+50+20+40 = 150         |
| ```30 ,10, 50, 20, 40```   |    ```AVG()```      | 30     | (30+10+50+20+40)/5 = 30      |
| ```30 ,10, 50, 20, 40```   |    ```MAX()```      | 50     | max value = 50               |
| ```30 ,10, 50, 20, 40```   |    ```MIN()```      | 10     | min value = 10               |

</details>

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
| E        | 70    |    1              |   0.2             |
| B        | 30    |    2              |   0.4             |
| A        | 20    |    3              |   0.6             |
| C        | 10    |    4              |   0.8             |
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

**Integer-based Ranking** : ```ROW_NUMBER()```,```RANK()```,```DENSE_RANK()```,```NTILE(n)```

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


 **Integer-based Ranking Comparison**

| ```ROW_NUMBER()```  | ```RANK()```     |  ```DENSE_RANK()```| 
|---------------------|------------------|--------------------|
| Unique rank         | Shared Rank      | Shared Rank        |
| Doesn't handle Ties | Handles Ties     | Handles Ties       |
| No Gaps in Rank     | Gaps in Rank     | No Gaps in Rank    |


</details>


<details> 
 <summary> <b> Use Case of <code> ROW_NUMBER()</code></b> </summary>

| Use case of ```ROW_NUMBER()```                  |
|-------------------------------------------------|
| 1. Top-N Analysis                               |
| 2. Bottom-N Analysis                            |
| 3. Assign Unique IDs                            |
| 4. Quality Checks : Identify & Remove Duplicates|


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

> Assign Unique IDs : Help to assign unique identifier for each row to help paginating.

> Paginating : The process of breaking down a large data into smaller, more managable chunks.
> e.g. Page 1-1000, 1K-2k, by dividing the data we can improve importing/exporting data, fast retrieval for users.

```python
     # Task : Assign unique IDs to the rows of the orders Archive table.

        SELECT
              ROW_NUMBER() OVER(ORDER BY order_id, order_date) uniqueID,
              *
        FROM Sales.ordersArchive 
```

4. Use Case of ```ROW_NUMBER()``` : Indentify Duplicates

> Identify Duplicates : Identify and remove duplicate rows to improve data quality.

- Data Cleansing is essential task for each Data Engineer not only Data Analyst in order to prepare the data before doing Data Analysis.

```python
    # Task : Identify duplicate rows in the table 'ordersArchive' and return a clean result without any duplicates.

    SELECT *
    FROM ( 
           SELECT
                ROW_NUMBER() OVER(PARTITION BY order_id ORDER BY creation_time DESC) rank,
                *
           FROM Sales.ordersArchive
    ) WHERE rank=1
    -- If you want bad data put rank>1
```
</details>

<details>
 <summary> <b> <code> NTILE(n)</code> </b> </summary>
- Divides the rows into a specified number of approximately equal groups(buckets)   
 
```python
        NTILE(n) OVER(ORDER BY sales)

        NTILE(2) OVER(ORDER BY sales DESC)
```
- ```Bucket Size = Num of Rows / Number of Buckets``` So, n=2, means Num of buckets = 2 and Number of Rows=4 Therefore Bucket size -> 4/2 = 2

| Sales | NTILE|
|-------|------|
| 100   |  1   |
| 80    |  1   |
| 80    |  2   |
| 50    |  2   |

What if bucket size is odd, like 
- ```Bucket Size = Num of Rows / Number of Buckets``` So, n=2, means Num of buckets = 2 and Number of Rows=5 Therefore Bucket size -> 5/2 = 2.5 -> 3

> SQL Rule : Larger Group Come first, So first Bucket will contain 3 rows & Second bucket will contain 2 rows.   

| Sales | NTILE|
|-------|------|
| 100   |  1   |
| 80    |  1   |
| 80    |  1   |
| 50    |  2   |
| 30    |  2   |

> That's why called as Almost(approximately) equal groups/buckets.

| order_id | sales|
|----------|------|
| 1        | 10   |
| 2        | 15   |
| 3        | 20   |
| 4        | 60   |
| 5        | 25   |
| 6        | 50   |
| 7        | 30   |
| 8        | 90   |
| 9        | 20   |
| 10       | 60   |

```python
        SELECT
            order_id,
            sales,
            NTILE(1) OVER(ORDER BY sales DESC) oneBucket,
            NTILE(2) OVER(ORDER BY sales DESC) twoBucket,
            NTILE(3) OVER(ORDER BY sales DESC) threeBucket
            NTILE(4) OVER(ORDER BY sales DESC) fourBucket
        FROM Sales.orders
```
- First SQL will sort the data, then calculate the buckets which will be 10/1 = 10 & rank them in one bucket.
|  Bucket  | Size | 10/1 = 10 | 10/2 = 5  | 10/3=3.33 ~4| 10/4=2.25 ~3|  
| order_id | sales| oneBucket | twoBucket | threeBucket | fourBucket  |
|----------|------|-----------|-----------|-------------|-------------|
| 8        | 90   | 1         | 1         | 1           |  1          |     
| 4        | 60   | 1         | 1         | 1           |  1          |
| 10       | 60   | 1         | 1         | 1           |  1          |
| 6        | 50   | 1         | 1         | 1           |  2          |
| 7        | 30   | 1         | 1         | 2           |  2          |
| 5        | 25   | 1         | 2         | 2           |  2          |
| 3        | 20   | 1         | 2         | 2           |  3          |
| 9        | 20   | 1         | 2         | 3           |  3          |
| 2        | 15   | 1         | 2         | 3           |  4          |
| 1        | 10   | 1         | 2         | 3           |  4          |

> As we have seen, if bucket size will be approx like 10/3 = 3.33 ~ 3 then larger group comes first like 4, 3, 3 kind of.

**Why dow we need bucket? What is the use case of ```NTILE(n)```?**

</details>

<details> 
 <summary> <b> Use Case of <code> NTILE(n)</code></b> </summary>

There is 2 use cases of ```NTILE(n)``` function in data projects.

One hand, A Data Analyst uses ```NTILE(n)``` function in order to segment data.<br/>
On the other hand, A Data Engineer uses ```NTILE(n)``` function in order to do ETL process (Equalizing Load Process) and Load Balancing.

1. Use Case of ```NTILE(n)``` : Data Segmentation
- Segmentation is a very nice way to understand data. So we can segment our data into different buckets or groups.

For Example : Doing segamentation for the customers. So, here we can group customers depends on their behaviour like total sales, total orders & based on that we can make VIP section, Medium & low.

> Divides a dataset into distinct subsets based on certain criteria.

```python
   # Task : Segment all orders into 3 categories : high, medium & low sales

   SELECT *,
   CASE WHEN bucket = 1 THEN 'High'
        WHEN bucket = 2 THEN 'Medium'
        ELSE 'Low'
   END AS salesSegmentation
   FROM (
        SELECT
          order_id,
          sales,
          NTILE(3) OVER(ORDER BY sales DESC) as bucket
       FROM Sales.orders
   )
```
 
| order_id | sales| oneBucket |
|----------|------|-----------|
| 8        | 90   | 1         |     
| 4        | 60   | 1         |
| 10       | 60   | 1         |
| 6        | 50   | 1         |
| 7        | 30   | 2         |
| 5        | 25   | 2         |
| 3        | 20   | 2         |
| 9        | 20   | 3         |
| 2        | 15   | 3         |
| 1        | 10   | 3         |          

2. Use Case of ```NTILE(n)``` : Equalizizng Load
- As a Data Engineer, we can use ```NTILE(n)``` function in order to do load balancing in ETL(Extract, Transform, Load).
In a scenario, we have 2 databases, And we want to move 1 big table from Database A to Database B.
So, In this case we're doing something called full load Means Loading all the table rows from one database to another database.
If we are doing it in one go, It will take long time like hours or may be you get newtwrok error bcuz we have stressed the network between databases.
So, we might lose the data & have to start again.

To handle this, what we can do instead of loading all rows at a time. We can split all rows into fractions(packets).
Like one big table into 4 small small table using ```NTILE(n)``` function.
Now we can move those smaller tables one after another from one database to another database without stressing the network. It will increase the chances of loading the table in time.
Once all those smaller tables successfully loaded into target database. 
Now, Of course we can merge them into one single table using ```UNION``` of all those tables.

```python
   # Task : In order to export the data, divide the orders into 2 groups

    SELECT 
         NTILE(2) OVER(ORDER BY order_id) AS bucket,
         *
    FROM Sales.order
```
</details>

<details>
 <summary> Recap of Percentage-based Ranking </summary>

 **Percentage-based Ranking** : ```PERCENT_RANK()``` ,```CUME_DIST()``` 

In percetange-based ranking, SQL calculates a relative position as a percentage and assign it for each row.
So, Rank will be continuos value from scale of 0 to 1. <br/>
This is amazing in order to do distribution analysis. <br/>
Calculation is focus on overall total(whole size of dataset) which helps us to find out the contribution of each value to the overall total. <br/>
In SQL, in order to generate percenatge we have 2 different formulas :
1. CUME_DIST = Position of Num / Number of Rows
2. PERCENT_RANK = Position Num - 1 / Number of Rows - 1 

</details>

<details>
 <summary> <b> <code> CUME_DIST() </code> </b> </summary>

- Cumulatative Distribution calculates the distribution of data points within a window. 
- Calculates the cumulative distribution of a value within a set of values
- ```CUME_DIST = Position of Num / Number of Rows```

```python
         CUME_DIST() OVER(ORDER BY sales DESC)
```
| Sales | CUME_DIST |
|-------|-----------|
| 100   |  1/5 ~ 0.2|
| 80    |  3/5 ~ 0.6|
| 80    |  3/5 ~ 0.6|
| 50    |  4/5 ~ 0.8|
| 30    |  5/5 ~ 1  |

> **Tie Rule** : The position of the last occurance of the same value in case of ```CUME_DIST()```.
  
</details>

<details>
 <summary> <b> <code> PERCENT_RANK() </code> </b> </summary>

- calculates the relative position of each row within a window.
- Returns the percentile ranking numbers of a row 
- ```PERCENT_RANK = Position Num - 1 / Number of Rows - 1 ```
 
 ```python
       PERCENT_RANK() OVER(ORDER BY sales DESC)
```
| Sales | PERCENT_RANK   |
|-------|----------------|
| 100   |  1-1/5-1 = 0   |
| 80    |  2-1/5-1 = 0.25|
| 80    |  3-1/5-1 = 0.50|
| 50    |  4-1/5-1 = 0.75|
| 30    |  5-1/5-1 = 1   |

> **Tie Rule** : The position of the first occurence of the same value in case of ```PERCENT_RANK()```
 
</details>

<details> 
    <summary> <b> Use cases of <code>CUME_DIST()</code> and <code>PERCENT_RANK()</code> </b></summary>

Use case of ```CUME_DIST()```  and ```PERCENT_RANK()``` 

If you want to focus on the distribution of your data points go with ```CUME_DIST()``` but if you want to focus on relative position of each row then go with ```PERCENT_RANK()``` 

| ```CUME_DIST()```                         | ```PERCENT_RANK()``` |
|-------------------------------------------|-----------------------------------------------------|
| CUME_DIST = Position Num / Number of Rows | PERCENT_RANK = Position Num - 1 / Number of Rows -1 |
| Inclusive (The current row is included)   | Exclusive (The current row is excluded)             |
| Tie Rule : The position of last occurence | Tie Rule : The position of first occurence          |
| focus on the distribution of your data points | focus on relative position of each row          |

```python
   # Task : Find the product that fall within the highest 40% of prices.

      SELECT
           *,
           CONCAT(distRank * 100, '%') AS DistRankPercentage
      FROM (
            SELECT
                product_id,
                prices,
                CUME_DIST() OVER(ORDER BY price DESC) distRank
            FROM Sales.products
      ) WHER distRank <= 0.4


    # OR
     SELECT
           *,
           CONCAT(distRank * 100, '%') AS DistRankPercentage
      FROM (
            SELECT
                product_id,
                prices,
                PERCENT_RANK() OVER(ORDER BY price DESC) distRank
            FROM Sales.products
      ) WHER distRank <= 0.4
```
</details>
<details>
  <summary><b> Summary of Window Rank Functions</b></summary>

**Window Rank Functions**
- Assign a Rank for each row within a window.

- 2 Types of Ranking
  - Integer-based Ranking : Assign a number an integer for a row : ```ROW_NUMBER()```,```RANK()```,```DENSE_RANK()```,```NTILE(n)```
  - Percentage-based Ranking : Calculate a rank & assign it for each row : ```CUME_DIST()```, ```PERCENT_RANK()```

- Rules of Ranking Functions:
  - Expression -> Empty means No arguments pass in the function
  - ```ORDER BY``` -> To sort data in ascending or descending
  - ```PARTITION BY``` -> To divide the data
  - Window Frame is Not Allowed

- Use Cases of Ranking Functions
  - Top-N Analysis
  - Bottom-N Analysis
  - Identify & Remove Duplicates
  - Assign Unique IDs & Pagination
  - Data Segmentation
  - Data Distribution Analysis
  - Equalizing Load Processing of ETL
</details>
 Next we will learn about Window Value Functions, how to access another records.

<!----------------------------------------------->

## 8.5 Window Value Functions
<details>
  <summary><b>Window Value Function</b></summary>

We also call it Analytics Functions

What are Window Value Functions?

We have a data in a table inside database.

| Months | Sales |
|--------|-------|
| Jan    | 20    |
| Feb    | 10    |
| Mar    | 30    |
| Apr    |  5    |
| Jun    | 70    |
| Jul    | 40    |

Now, We can use the Window Value Function to **Access a value from another row**

Suppose, SQL is processing the months, we are currently at March. <br/>
Now we want to access the value from the previous month from feb. <br/>
So, in order to do that, we can use ```LAG()``` function to get the value of Feb which is 10. <br/>
With that we have in the same row we have the value of March which is current sales value & Feb which is the previous sales value. <br/>
May be in another case, we want to get sales from next month in the current sales. <br/>
In order to do that we can use ```LEAD()``` function to get the value of Apr which is 5 in the same current row. <br/>
With that we can compare current month, previous month & next month sales. <br/>
Now, If you want to get the sales of first month Jan which is 20, in order to get the value of first month we can use ```FIRST_VALUE()``` to get the value of the first month sales value. <br/>
Similarly for the last month sales we can use ```LAST_VALUE()``` function. <br/>

This is exactly the purpose of Window Value functions, that we can get acces to values from another rows. <br/>

The Value functions is like Ranking function, we have to use ```ORDER BY``` in order to sort data in order to understand what is the first row and the last row. In the above example data is sorted by months. <br/>

The Window Value functions are really important for analytics, we can use it in order to access a value from other rows in order to do comparison.

**Syntax of Window Value functions**

| Window <br/> Value Functions      | Expression     | Partition Clause | Order Clause | Frame Clause  |
|-----------------------------------|----------------|------------ -----|--------------|---------------|
| ```LEAD(expr, offset, default)``` | All Data types |  Optional        | Required     | Not Allowed   |
| ```LAG(expr, offset, default)```  | All Data types |  Optional        | Required     | Not Allowed   |
| ```FIRST_VALUE(expr)```           | All Data types |  Optional        | Required     | Optional      |
|  ```LAST_VALUE(expre)```          | All Data types |  Optional        | Required     | Should be used|

| Window <br/> Value Functions      | Definition                             |  Syntax                                           |
|-----------------------------------|----------------------------------------|---------------------------------------------------|
| ```LEAD(expr, offset, default)``` | Returns the value from a previous row  | ```LEAD(sales,2,10) OVER(ORDER BY order_date)```  |
| ```LAG(expr, offset, default)```  | Returns the value from a subsequent row| ```LAG(sales,2,0) OVER(ORDER BY order_date)```    |
| ```FIRST_VALUE(expr)```           | Returns the first value in a window    | ```FIRST_VALUE(sales) OVER(ORDER BY order_date)```|
|  ```LAST_VALUE(expre)```          | Returns the last value in a window     | ```LAST_VALUE(sales) OVER(ORDER BY order_date)``` |
  
</details>

<details>
 <summary> <b> <code>LEAD()</code> and <code>LAG()</code> Window Value Functions</b> </summary>

- ```LEAD()``` access a value from the next row within a window.
- ```LAG()``` access a value from the previous row within a window, 

```python
        LEAD(sales,2,10) OVER(PARTITION BY product_id ORDER BY order_date)

        LAG(sales,2,10) OVER(PARTITION BY product_id ORDER BY order_date)
```
- ```LEAD()``` requires an expression (any datatype) like sales is here
- 2 is offset here, which is optional : Number of rows forward or backward from current row, default offset is 1.
- 10 is Default value, which is also optional : Returns default value if next/previous row is not available, dafault Default is NULL.
- ```OVER()``` clause in which we define the values
- ```PARTITION BY``` is optional here, which divide the data
- ```ORDER BY``` is required to sort the data other SQL doesn't know prev & next row data

| Months | Sales |
|--------|-------|
| Jan    | 20    |
| Feb    | 10    |
| Mar    | 30    |
| Apr    |  5    |

```python
   # Find the sales of next the month
   # Find the sales of prev the month

        LEAD(sales) OVER(ORDER BY month)
        LAG(sales) OVER(ORDER BY month)
```

| Months | Sales | LEAD | LAG |
|--------|-------|------|-----|
| Jan    | 20    | 10   | NULL|
| Feb    | 10    | 30   | 10  |
| Mar    | 30    |  5   | 30  |
| Apr    |  5    | NULL |  5  |

```python
   # Find the sales for the two months ahead
   # Find the sales for the two months ago
   

        LEAD(sales,2) OVER(ORDER BY month)
        LAG(sales,2) OVER(ORDER BY month)
```

| Months | Sales | LEAD | LAG |
|--------|-------|------|-----|
| Jan    | 20    | 30   | NULL|
| Feb    | 10    |  5   | NULL|
| Mar    | 30    | NULL | 20  |
| Apr    |  5    | NULL | 10  |

```python
   # Find the sales for the two months ahead
   # Find the sales for the two months ago

        LEAD(sales,2,0) OVER(ORDER BY month)
        LAG(sales,2,0) OVER(ORDER BY month)
```

| Months | Sales | LEAD | LAG |
|--------|-------|------|-----|
| Jan    | 20    | 30   |  0  |
| Feb    | 10    |  5   |  0  |
| Mar    | 30    |  0   | 20  |
| Apr    |  5    |  0   | 10  |

</details>
<details>
 <summary> <b>Use cases of <code>LEAD()</code> and <code>LAG()</code> </b></summary>

1. Use case of ```LEAD()``` and ```LAG()``` : Month-Over-Month(MoM) Analysis
- Here, We will do comparison Analysis

**Time Series Analysis**

The process of analyzing the data to understand patterns, trends and behaviors over time. 

Do **year over year(YoY) Analysis** : Help us to analyzr the long-term overall growth/decline of the business's performances over time

and Do **month over month(MoM) Analysis** : Helps us to analyze short-trends and discover patterns in seasonality.

```python
   # Task : Analyze the month-month-month(MoM) performance by finding the percentage
   #        change in sales between the current & previous month

        SELECT
              *,
              currentMonthSales - PrevMonthSales AS MoM_Change,
              ROUND(CAST( (currentMonthSales - PrevMonthSales) AS FLOAT)/PrevMonthSales * 100, 2) as MoM_Perc
        FROM (
               SELECT
                   MONTH(order_date) orderMonth,
                   SUM(sales) currentMonthSales,
                   LAG(SUM(sales)) OVER(ORDER BY MONTH(order_date)) PrevMonthSales
              FROM Sales.orders
              GROUP BY MONTH(order_date)
        )
```
> ```MONTH()``` : extracts and returns the month from a given date.

| order_id | order_date | Sales | month |
|----------|------------|-------|-------|
| 1        | 2026-01-01 | 1     | 10    |
| 2        | 2026-01-05 | 1     | 15    |
| 3        | 2026-01-10 | 1     | 20    |
| 4        | 2026-01-20 | 1     | 60    |
| 5        | 2026-02-01 | 2     | 25    |
| 6        | 2026-02-05 | 2     | 50    |
| 7        | 2026-02-15 | 2     | 30    |
| 8        | 2026-02-18 | 2     | 90    |
| 9        | 2026-03-10 | 3     | 20    |
| 10       | 2026-03-15 | 3     | 60    |

| Month | currentMonthSales |
|-------|-------------------|
| 1     | 105               |
| 2     | 195               |
| 3     |  80               |

| Month | currentMonthSales | PrevMonthSales |
|-------|-------------------|----------------|
| 1     | 105               |   NULL         |
| 2     | 195               |   105          |
| 3     |  80               |   195          |

- Here, we can see magic. It's very simple, we can use ```LEAD()``` and ```LAG()``` functions in order to access another values from another rows without doing any complicated JOINs & so on.

| Month | currentMonthSales | PrevMonthSales | MoM_Change |
|-------|-------------------|----------------|------------|
| 1     | 105               |   NULL         |   NULL     |
| 2     | 195               |   105          |     90     |
| 3     |  80               |   195          |   -115     |

| Month | currentMonthSales | PrevMonthSales | MoM_Change | MoM_Perc |
|-------|-------------------|----------------|------------|----------|
| 1     | 105               |   NULL         |   NULL     |  NULL    |
| 2     | 195               |   105          |     90     |  85.7    |
| 3     |  80               |   195          |   -115     |  -59     |

2. Use case of ```LEAD()``` and ```LAG()``` : Customer Retention Analysis

**Customer Retention Analysis**
Measure customer's behavior and loyalty to help businesses build strong relationship with customers.

Let's see how ```LEAD()``` and ```LAG()``` functions in order to do customer retention analysis.

```python
     # Task : Analyze customer loyalty by ranking customers based on the average number of days between their orders.
     # step 1. Simple select query on orders table
     # step 2. days between their orders : We can do joins but we have LEAD() and LAG() functions
     # step 3. Define the window OVER() and Partition by bcuz we are analyzing each customers and ORDER BY order_date
     # Step 4. We have current_order_date and next_order_date, now we can subtract them in order to get date between them using DATEDIFF()
     # Step 5. Calculte Average of those dates in order to do this we have to use sub-query
     # Step 6. Rank the customer based on average_days based on inc order
     # Step 7. Replace the NULL with longest value

     SELECT
           customer_id,
           AVG(DaysUntilNextOrder) AS average_Days,
           RANK() OVER(ORDER BY COALESCE(AVG(DaysUntilNextOrder), 999999) ASC) as AvgRank
     FROM (
           SELECT
                 customer_id,
                 order_id, 
                 order_date current_Order,
                 LEAD(order_date) OVER(PARTITION BY customer_id ORDER BY order_date) AS next_order,
                 DATEDIFF(day, order_date, LEAD(order_date) OVER(PARTITION BY customer_id ORDER BY order_date)) AS DaysUntilNextOrder
          FROM Sales.orders 
          ORDER BY customer_id, order_date
       )
    GROUP BY customer_id
```
| order_id | customer_id | current_order |  next_order |
|----------|-------------|---------------|-------------|
| 3        | 1           | 2026-01-10    | 2026-01-20  |
| 4        | 1           | 2026-01-20    | 2026-02-15  |
| 7        | 1           | 2026-02-15    | NULL        |
| 1        | 2           | 2026-01-01    | 2026-02-01  |
| 5        | 2           | 2026-02-01    | 2026-03-10  |
| 9        | 2           | 2026-03-10    | NULL        |
| 2        | 3           | 2026-01-05    | 2026-02-05  |
| 6        | 3           | 2026-02-05    | 2026-03-15  |
| 10       | 3           | 2026-03-15    | NULL        |
| 8        | 4           | 2026-02-18    | NULL        |

> ```DATEDIFF()``` : Calculates the difference between two date values

| order_id | customer_id | current_order |  next_order | DaysUntilNextOrder |
|----------|-------------|---------------|-------------|--------------------|
| 3        | 1           | 2026-01-10    | 2026-01-20  |  10                |
| 4        | 1           | 2026-01-20    | 2026-02-15  |  26                |
| 7        | 1           | 2026-02-15    | NULL        |  NULL              |
| 1        | 2           | 2026-01-01    | 2026-02-01  |  31                |
| 5        | 2           | 2026-02-01    | 2026-03-10  |  37                | 
| 9        | 2           | 2026-03-10    | NULL        |  NULL              |
| 2        | 3           | 2026-01-05    | 2026-02-05  |  31                |
| 6        | 3           | 2026-02-05    | 2026-03-15  |  38                |
| 10       | 3           | 2026-03-15    | NULL        |  NULL              |
| 8        | 4           | 2026-02-18    | NULL        |  NULL              |

This is the magic of ```LEAD()``` and ```LAG()``` functions, we can very easily access any information we need in same row in order to do analysis.
With very simple query without joining stuffs.

| customer_id | average_Days |
|-------------|--------------|
|   1         |  18          |
|   2         |  34          |
|   3         |  34          |
|   4         |  NULL        |

| customer_id | average_Days | Avg_Rank |
|-------------|--------------|----------|
|   4         |  NULL        |  1       |
|   1         |  18          |  2       |
|   2         |  34          |  3       |
|   3         |  34          |  4       |

> ```COALESCE()``` : Replaces NULLs by returning the first non-NULL value from a list.

| customer_id | average_Days | Avg_Rank |
|-------------|--------------|----------|
|   1         |  18          |  1       |
|   2         |  34          |  2       |
|   3         |  34          |  3       |
|   4         |  NULL        |  4       |

</details>


<details>
 <summary> <b> <code>FIRST_VALUE(expr)</code> and <code> LAST_VALUE(expre) </code></b> </summary>

- ```FIRST_VALUE(expr)``` allows us to access a value from the first row within a window. 
- ```LAST_VALUE(expr)``` allows us to access a value from the last row within a window. 

```python
       FIRST_VALUE(sales) OVER(ORDER BY month)
       LAST_VALUE(sales) OVER(ORDER BY month)
```

Default Frame : ```RANGE BETWEEN UNBOUNDED  PRECEDING AND CURRENT ROW```

| Months | Sales | FIRST_VALUE | LAST_VALUE |
|--------|-------|-------------|------------|
| Jan    | 20    | 20          | 20         |
| Feb    | 10    | 20          | 10         |
| Mar    | 30    | 20          | 30         |
| Apr    |  5    | 20          | 5          |

Here, We can see, ```FIRST_VALUE()``` is working fine with default frame But It is really useless to use ```LAST_VALUE()``` with Default frame bcuz the value is same as sales.
So with ```LAST_VALUE()```, we should not use Default Frame, the frame effect in order to get last value correctly.

```python
       LAST_VALUE(sales) OVER(ORDER BY month ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING)
```
| Months | Sales | LAST_VALUE | 
|--------|-------|------------|
| Jan    | 20    | 5          |
| Feb    | 10    | 5          |
| Mar    | 30    | 5          |
| Apr    |  5    | 5          |

```python
   # Task : Find the lowest & Highest sales for each product

   SELECT
        order_id,
        product_id,
        sales,
        FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales) AS lowestSales,
        LAST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS highestSales,
        FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales DESC) AS highestSales2,
   FROM Sales.orders
```
| order_id | product_id |sales | Lowest | Highest | 
|----------|------------|------|--------|---------|
|  1       | 101        | 10   |  10    |  90     |
|  3       | 101        | 20   |  10    |  90     |
|  9       | 101        | 20   |  10    |  90     |
|  8       | 101        | 90   |  10    |  90     |
|__________|____________|______|________|_________|
|  2       | 102        | 15   |  15    |  60     |
|  7       | 102        | 30   |  15    |  60     |
|  10      | 102        | 60   |  15    |  60     |
|__________|____________|______|________|_________|
|  5       | 104        | 25   |  25    |  50     |
|  6       | 104        | 50   |  50    |  50     |
|__________|____________|______|________|_________|
|  4       | 105        | 60   |  60    |  60     |

Note : We can use ```FIRST_VALUE()``` in order to find last value by sorting the data in DESCENDING Order.

```python
   SELECT
        order_id,
        product_id,
        sales,
        FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales DESC) AS highestSales2
   FROM Sales.orders
```
| order_id | product_id |sales | Highest | 
|----------|------------|------|---------|
|  8       | 101        | 90   |  90     |
|  3       | 101        | 20   |  90     |
|  9       | 101        | 20   |  90     |
|  1       | 101        | 10   |  90     |
|__________|____________|______|_________|
|  10      | 102        | 60   |  60     |
|  7       | 102        | 30   |  60     |
|  2       | 102        | 15   |  60     |
|__________|____________|______|_________|
|  6       | 104        | 50   |  50     |
|  5       | 104        | 25   |  50     |
|__________|____________|______|_________|
|  4       | 105        | 60   |  60     |

We can do same task by using ```MAX()``` and ```MIN()```
```python
   # Task : Find the lowest & Highest sales for each product

   SELECT
        order_id,
        product_id,
        sales,
        MIN(sales) OVER(PARTITION BY product_id) AS lowestSales,
        MAX(sales) OVER(PARTITION BY product_id) AS highestSales
   FROM Sales.orders
```

```python
    # Task : Find the lowest & Highest sales for each product
    # We can solve this using 3 different functions.

   SELECT
        order_id,
        product_id,
        sales,
        FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales) AS lowestSales,
        LAST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS highestSales,
        FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales DESC) AS highestSales2,
        MIN(sales) OVER(PARTITION BY product_id) AS lowestSales3,
        MAX(sales) OVER(PARTITION BY product_id) AS highestSales3
   FROM Sales.orders
```
 
</details>

<details>
 <summary>Use Case of <code> FIRST_VALUE() </code> and <code> LAST_VALUE() </code> </summary>

**Use Case of ```FIRST_VALUE()``` and ```LAST_VALUE()```**

1. Use Case of ```FIRST_VALUE()``` and ```LAST_VALUE()```  : Compare to Extremes

- Compare to Extremes :How well a value is performing relative to extremes (lowest/highest)

```python
     # Task : Extend the previous task,
     #        Find the difference in sales between the current and the lowest sales

     SELECT
        order_id,
        product_id,
        sales currentSales,
        FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales) AS lowestSales,
        sales - FIRST_VALUE(sales) OVER(PARTITION BY product_id ORDER BY sales) AS SalesDifference
   FROM Sales.orders    
```

This is very important analysis in order to do comparison analysis.

</details>

<details>
 <summary> <b> Summary of Window Value Functions</b> </summary>

- Window Value functions are also know as Analytical Functions

- Allows access to specific value from another row, this helps in order to do complex calculations with very simple SQL query without having joining table together like Self Joins.

- Four Types of Value Functions :
  - ```LAG()``` : Allows to access previous value
  - ```LEAD()``` : Allows to access the next value
  - ```FIRST_VALUE()``` : Allows to access the first value in a subset  
  - ```LAST_VALUE()``` : Allows to access the last value in a subset  

- Rules of Window Value Functions :
  - Expressions : We can use Any Data Type (Number, String , Date)
  - ```ORDER BY``` : must bcuz in order to sort the data
  - ```PARTITION BY``` : is Optional.
  -  Window Frame : Window Frame is optional, but for ```LAST_VALUE()``` It is recommended.

- Use cases of Window Value Function :
  - Time Series Analysis : YoY and MoM are classical & always first question in Data Analysis in order to measure growth/decline in businesses.
  - Time Gap Analysis : Customer Retention, Customer behaviour like average days bwteen 2 orders
  - Comaprison Analysis : Extreme Values | Lowest and Highest with current values

</details>
<!------------------------------------------------>

We have covered everything about How to aggregate data using SQL and these Window functions are very important tool in How to do Data Analytics in SQL especially if you're a Data Scientist or Data Analyst.

Intermediated Level is completed here!!
How to filter Data?
How to Combine Data?
Functions in SQL

Next we will see the Advanced Level 
<!--------Chapter 8. Aggregation and Analytical Functions--------->

[< Row-Level Functions](https://github.com/pawansinghfromindia/SQL/blob/main/07_Row-Level_Functions/row_level_functions.md) | 
**Aggregations and Analytical Functions** | [Advanced SQL Techniques >]()
