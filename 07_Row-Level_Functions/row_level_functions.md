<!-----------Chapter 7. Row-Level Functions------------------------>
# SQL Functions

What is exactly a function and why we need it? <br/>
We have data inside our table and we want to do some stuffs on data like change value of your data in **Data Manipulation**  <br/>
or **Aggregation in Data Analysis** (Analyse the data and find insights to build a report) <br/>
or sometime we might find bad data inside the data so we want to Clean bad data by **Data Cleansing** <br/>
or **Data Transformation** in order to solve some SQL Task. <br/>
In order to solve those problems, we have **SQL functions** <br/>

> **SQL FUNCTIONs** is  **a built-in SQL Code** :
>  - accepts an input value
>  - process it
>  - Return an output value

We can group functions into 2 big categories :

**1. Row-Level or Single-Value functions**
- inputs only one value & returns only one value
-  e.g. 'MARIA' --> ```LOWER()``` --> 'maria'

**2. Aggregate & Analytical Functions**
- accepts multiple rows values and returns one value
- e.g. 10,30,20,40 --> ```SUM()``` --> 100 


 <details> 
    <summary><b>Nested Function</b></summary>
Function used inside another function.

e.g. 'Maria' --> ```LEFT(2)``` --> 'Ma' --> ```LOWER()``` --> 'ma' <br/>

```sql
LEN( LOWER( LEFT('Maria',2) ) )
```
 
 </details>
 
<!----------------------------------------------------->


## Types of SQL Functions

In SQL, We have a lot of functions that's why we grouped them into sub-categories.

For Row-Level-Calculations -> Single-Row Functions
- String Functions
- Numeric Functions
- Data & Time Functions
- NULL Functions

For Aggregation -> Multi-Row Functions
- Aggregate functions (Basics)
- Window functions (Advanced)

It is very important to understand these functions, bcuz by using them we can do whatever we want with data.

Single-rows functions are used in order to manipulate and prepare the data for the aggregations.

The Data Engineers prepare the data in SQL using Single-Row Functions like CLeanup,Transform, Manipulate data for Data Analyser. 

The Data Analyst mostly used Aggregate functions in almost every task.
<!----------------------------------------------------->

# 1. Single-Row-Level Functions

Here, we can group Row-Level functions into multiple category

<details>
  
<summary><b> String Functions </b></summary>
  
Since, We have a lot of String Functions, So we are going to divide it into multiple categories based on the purpose.

- Functions that manipulate string values :  ```CONCAT```,  ```UPPER```,  ```LOWER```,  ```TRIM```,  ```REPLACE```
- Functions that does calculation on string values :  ```LEN```
- Functions that extract something from string value :  ```LEFT```,  ```RIGHT```,  ```SUBSTRING```

<!----------------------------------------------------->

**```CONCAT```**
  
- ```CONCAT``` combines multiple strings into one.
- concat first_name and last_name to get full_name

```sql
   --Task : Concatenate first name and country into one column

    SELECT
        first_name,
        last_name,
        CONCAT(first_name, last_name) AS full_name    
    FROM customers

--Not looking well as both are combined without any space, to add space in between.

    SELECT
        first_name,
        last_name,
        CONCAT(first_name, ' ' ,last_name) AS full_name
    FROM customers
```
<!----------------------------------------------------->

**```UPPER```** and **```LOWER```**

- ```UPPER``` converts all characters to uppercase.
- ```LOWER``` converts all characters to lowercase.

```sql
   --Task : Convert the first_name to lowercase.

   SELECT
       first_name,
       LOWER(first_name) AS lowercaseName
   FROM customers

   --Task : Convert the first_name to uppercase.

   SELECT
       first_name,
       UPPER(first_name) AS uppercaseName
   FROM customers

```
<!----------------------------------------------------->

**```TRIM```**
  
- ```TRIM``` removes leading and trailing spaces in string. 
- ```'   Rohit'``` this contains leading spaces <br/> ```'Rohit    '``` this contains trailing spaces <br/> ```'  Rohit  '``` this contains both leading and trailing spaces.
- Here, Spaces are evils & you don't know how many spaces user have entered. So, we have to do data cleansing for that we have ```TRIM()```.
- ```TRIM``` cleans all the spaces from starting & ending not in between of a string.

```sql
   -- Task : Find customers whose first name contains leading or trailing spaces.

    SELECT
         first_name
    FROM customers
    WHERE first_name != TRIM(first_name)

   -- Check it with length function

   SELECT
         first_name,
         LEN(first_name) AS length
         LEN(TRIM(first_name)) AS trimmedLength,
         LEN(first_name) - LEN(TRIM(first_name)) AS flag
    FROM customers
    WHERE LEN(first_name) != LEN(TRIM(first_name))
```
<!----------------------------------------------------->

**```REPLACE```**

- ```REPLACE``` function replaces speific character with a new character.
- '123-456-7890' -> ```REPLACE('123-456-7890', '-', '/')``` -> '123/456/7890'
- We can use ```REPLACE``` replace in order to delete something as well by replacing with '' (nothing, just blank). like '123-456-7890' -> ```REPLACE('123-456-7890', '-','')``` -> '1234567890'

```sql
     --Task : Remove dashes (-) from a static value i.e. phone number

     SELECT
          '123-456-7890' AS phone,
           REPLACE('123-456-7890', '-', '') AS clean_phone

    -- Task : Replace File Extence from txt to csv

     SELECT
           'report.txt' AS old_file_name,
           REPLACE('report.txt', '.txt', '.csv') AS new_file_name

```
<!----------------------------------------------------->

**```LEN```**

- ```LEN``` function counts how many character in a string.
- 'Rahul' -> ```LEN()``` -> 5 | 350 -> ```LEN``` -> 3 | 2026-01-01 -> ```LEN``` -> 10

```sql
   -- Task : Calculate the length of each customer's first name.

   SELECT
        first_name,
        LEN(first_name) AS first_name_length
   FROM customers
```
<!----------------------------------------------------->

**```LEFT``` and ```RIGHT```**

- ```LEFT``` function extract specific number of characters from the start.  ```LEFT(value, num_of_characters)```
- ```RIGHT``` function extract specific number of characters from the end.   ```RIGHT(value, num_of_characters)```
- 'Rohit' -> ```LEFT('Rohit', 2)``` -> 'Ro'
- 'Rohit' -> ```RIGHT('Rohit', 2)``` -> 'it'

```sql
     -- Task : Retrieve the first two characters of each first name.

        SELECT
             first_name,
             LEFT(first_name,2) AS first_2_char,
             LEFT(TRIM(first_name),2) AS first_2_chars    --to avoid the leading & trailing spaces
        FROM customers

        -- Task : Retrieve the last two characters of each first name.

        SELECT
             first_name,
             LAST(first_name,2) AS last_2_char
        FROM customers
```

<!----------------------------------------------------->

**```SUBSTRING```**

- ```SUBSTRING``` functions extracts a part of string to a specified position.
- In order to use ```SUBSTRING```, we need 3 things : Value, starting point, length.
- After 2nd char .e. starting from 3rd char extract 3 char : 'Rahul' -> ```SUBSTRING('Rahul', 3, 2)``` -> 'hu' 
- After 2nd char .e. starting from 3rd char extract all char : 'Rahul' -> ```SUBSTRING('Rahul', 3, LEN('Rahul'))``` -> 'hul'

```sql
    --Task : Retrieve a list of customers' first names removing the first character.

     SELECT
         first_name,
         SUBSTRING(first_name, 2, LEN(first_name)) as name_subs
     FROM customers

    SELECT
         first_name,
         SUBSTRING(TRIM(first_name), 2, LEN(first_name)) as name_subs  --if any leading or trailing spaces is there use TRIM
     FROM customers
```

</details>
<!------------------------------------------->

<details>
  <summary><b> Number Functions </b></summary>

**```ROUND```** : returns the round value of a number.

```sql
      SELECT 3.516,                              --3.516
             ROUND (3.516, 2) AS round_2,        --3.520
             ROUND (3.516, 1) AS round_1,        --3.500
             ROUND (3.516, 0) AS round_0         --4.000     
```

**```ABS```**  returns the absolute (positive) value of a number, removing any negative sign.
```sql
    SELECT -10,         -- -10
            ABS(-10),   --  10
            ABS(10),    --  10
```

</details>
<!------------------------------------------->

<details>
  <summary><b> Date & Time Functions </b></summary>
 
We have functions to manipulate date & time in SQL.

**What is date?**
- Calendar which contains series of dates in an order. 
- Like your BirthDay or Exam Date or Project DeadLine all of them are date. 
- Example -> '2026-12-31' : It has 3 components,
  - 4 digits number indicates **Year** 1999,2000,2001,.........2026,...........2099,3000,...so on
  - Next 2 digits indicates **Month** ranges between 1 to 12 as Jan, Feb,....,Dec
  - next 2 digit indicates **Day** ranges between 1 to 31 as 1,2,3,........,31
- So, in database this structure is called as **Date**
- We can have different formats of Date like 'YYYY-MM-DD' or 'DD/MM/YYYY' etc

**What is Time?**
- Time refers to specific time within a day.
- Example -> '18 : 55 : 45' It also has 3 components
  - First 2 digits indicate **Hours** ranges between 0 to 23 as [1 Day = 24 hours]
  - Second 2 digits indicate **Minutes** ranges between 0 to 59 as [1 hour = 60 minutes]
  - Second 2 digits indicate **Seconds** ranges between 0 to 59 as [1 minute = 60 seconds]
 
**What is TimeStamp?**
- If we combine both Date and Time together & put them side by side, We will get a new structure known as **Timestamp**.
- '2026-12-31 18:55:45.0000000' is Timestamp, after seconds we have fractions of seconds like mili seconds etc so on.
- This **Timestamp** is used in many databases like ORACLE, MySQL, Postgres But in SQL Server, we call it **Datetime2**

```sql
      SELECT
           order_id,
           order_date,
           ship_date,
           creation_time
      FROM Sales.orders
    
```
<!--------------------------------------------------------->

**Date & Time Values**

We have 3 different sources in order to query the dates.

1. Date Column from a table
- Dates which are stored in database table like order_date, shipment_date etc
```sql
    SELECT 
        order_date,
        ship_date
    FROM Sales.orders 
```

2. Hardcoded Constant String value
- Sometime in query, we define a date to do calculation.
- It is not stored in database, this is just added in query which is hardcoded.

```sql
    SELECT 
        order_date,
        ship_date,
        '2026-01-01'  AS hardcoded_date
    FROM Sales.orders
```

3. ```GETDATE()``` Function
- We can source date using the funtion ```GATDATE()```, is most important function used in SQL
- Returns the current date and time at the moment when the query is executed.

```sql
    SELECT 
        order_date,
        ship_date,
        '2026-01-01'  AS hardcoded_date,
        GETDATE() AS today
    FROM Sales.orders       
```
<!-------------------------------------------->

**Date & Time Function Overview**

We have a date ```2026-10-25```. <br/>
- We can extract different parts of the date like we need only Year ```2026``` or Month ```10``` or Day ```25```. <br/>
- We can also change the date format ```2026-10-25``` to ```2026/10/25``` or ```2026.10.25``` or ```25 Oct 2026```
- We can also do date calculations like Add or Diff of dates  ```2026-10-25``` + 3 Years = ```2029-10-15``` | ```2026-10-25``` - ```2026-10-15``` = 10
- We can also do validation of date by putting it in conditions and get True or False 

**DATE and TIME Functions**

|  Part Extraction | Format & Casting | Calculations    |  Validation |
|------------------|------------------|-----------------|-------------|
| ```DAY```        |  ```FORMAT```    | ```DATEADD```   | ```ISDATE```|
| ```MONTH```      |  ```CONVERT```   | ```DATEDIFF```  |             |
| ```YEAR```       |  ```CAST```      |                 |             |
| ```DATEPART```   |                  |                 |             |
| ```DATENAME```   |                  |                 |             |
| ```DATETRUNC```  |                  |                 |             |
| ```EOMONTH```    |                  |                 |             |

<!--------------------------------------------->
##  DATE & TIME : Part Extraction

**DAY() | MONTH() | YEAR()** Part Extraction
- ```DAY()``` returns the day from a data.
- ```MONTH()``` returns the month from a data.
- ```YEAR()``` returns the year from a data.

```sql
    SELECT
           DAY('2026-12-31') AS day,       --31
           MONTH('2026-12-31') AS month,   --12
           YEAR('2026-12-31') AS year      --2026

    SELECT
        order_id,
        creation_time,
        YEAR(creation_time) AS year,
        MONTH(creation_time) AS month,
        DAY(creation_time) AS day
    FROM Sales.order
```

<!--------------------------------------->

**DATEPART()** Part Extraction
- ```DATEPART()``` returns a specific part of a date as a number.
- Like Date stores Week, Quarter we can't see it as value but we can extract it but there is no dedicated functions for that bcuz they are not commonly used like year, month and day but still we can extract those information using the ```DATEPART()```
- ```DATEPART(week,'2025-08-20')``` -> 34 as this was the 34th week
- ```DATEPART(quarter,'2025-08-20')``` -> 3 as this was the 3rd Quarter

Syntax of DATEPART()
```sql
     DATEPART(part, date)
```
```sql
SELECT 
     DATEPART(month, order_date),   --12
     DATEPART(mm, order_date),      --12
FROM Sales.order
```

```sql
SELECT
        order_id,
        creation_time,
        -- DAY MONTH YEAR Examples
        YEAR(creation_time) AS year,
        MONTH(creation_time) AS month,
        DAY(creation_time) AS day,
        --DATEPART Examples
        DATEPART(year, creation_date) year_dp,
        DATEPART(month, creation_date) month_dp,
        DATEPART(day, creation_date) day_dp,
        DATEPART(hour, creation_date) hour_dp,
        DATEPART(minute, creation_date) minute_dp,
        DATEPART(week, creation_date) week,
        DATEPART(quarter, creation_date) quarter,
    FROM Sales.order
```

> ```INT``` is the datatype of output of all the date functions like ```DAY()```, ```MONTH()```, ```YEAR()``` and ```DATEPART()```

<!-------------------------------------------------->

**DATENAME()** Part Extraction
- ```DATENAME()``` returns the name of a specific part of a date.
- Using ```DATEPART()``` we extacted everything in number like months (8) but how to get months as 'August' or 20 as 'Tuesday' Here we use ```DATENAME()```

```sql
   DATEPART(part, date)   -- return in INT
   DATENAME(part, date)   -- return in STRING
```

```sql
SELECT
        order_id,
        creation_time,
                   -- DAY MONTH YEAR Examples
        YEAR(creation_time) AS year,
        MONTH(creation_time) AS month,
        DAY(creation_time) AS day,
                   --DATEPART Examples
        DATEPART(year, creation_date) year_dp,
        DATEPART(month, creation_date) month_dp,
        DATEPART(day, creation_date) day_dp,
        DATEPART(hour, creation_date) hour_dp,
        DATEPART(minute, creation_date) minute_dp,
        DATEPART(week, creation_date) week,
        DATEPART(quarter, creation_date) quarter,
                  --DATENAME Examples
        DATENAME(year, creation_date) as year_dn,     --2027 but datatype is string as DATENAME return String output
        DATENAME(month, creation_date) as month_dn,   --January
        DATENAME(day, creation_date) as day_dn,       --1 as This is the day of the month & SQL has no name for that
                                                        --It has number but datatype is String only.
        DATENAME(weekday, creation_date) as weekday_dn,       --Monday
    FROM Sales.order
```


> ```STRING``` is the datatype of output of function ```DATENAME()```.

The most important thing about ```DATENAME()``` is to present in a way that is easy to read and human readable information to the users. <br/>
Like You're preparing a report of sales by months. and you are showing months as number 1,2,3,4,5,6,7,8,9,10,11,12 instead of January, February, March,....December.
This is okay, but Through Names of Months It is way more nicer & readable.

<!------------------------------------------------------>

**DATETRUNC()** Part Extraction
- ```DATETRUNC()``` truncates the date to the specific part.
```sql
       DATEPART(part, date),
       DATENAME(part, date),
       DATETRUNC(part, date)
```
- '2026-08-29 18:55:45' It is very precise we know exact second for this information. So, the level of detail here is very high. So, ```DATETRUNC()``` allows us to change the level of detail by specifying the level of details.

- DATETRUNC(minute, date) -> So after minute, everything is going to reset and all other prev info details are kept as it is. '2026-08-29 18:55:00' 
- DATETRUNC(hour, date) -> So after hour, everything is going to reset and all other prev info details are kept as it is. '2026-08-29 18:00:00' 
- DATETRUNC(day, date) -> So after day, everything is going to reset and all other prev info details are kept as it is. '2026-08-29 00:00:00' 
- DATETRUNC(month, date) -> So after month, everything is going to reset and all other prev info details are kept as it is. '2026-08-01 00:00:00' 
- DATETRUNC(year, date) -> So after month, everything is going to reset and all other prev info details are kept as it is. '2026-01-01 00:00:00' 

> **Date part is reset to 1**, **Month part is reset to 1** and **Time part is reset to 0**

```sql
SELECT
    order_id,
    creation_time,
    DATETRUNC(minute, creation_date),    --'2026-08-29 18:55:00'
    DATETRUNC(hour, creation_date),      --'2026-08-29 18:00:00'
    DATETRUNC(day, creation_date),       --'2026-08-29 00:00:00'
    DATETRUNC(month, creation_date),     --'2026-08-01 00:00:00'
    DATETRUNC(year, creation_date),      --'2026-01-01 00:00:00'
FROM Sales.orders

```

**Why ```DATETRUNC()``` is an amazing function for Data Analysis?**

- In Data Analytics, We can go and quickly change the granuality & level of aggregation details by simply defining the level of details in ```DATETRUNC()```.
- It allows us to do aggregation in zooming in and zooming out. minute < hr < day < month < year 

```sql
SELECT
     creation_time,
     count(*)
FROM Sales.orders
GROUP BY creation_time
-- Here, we will get count 1 in every rows as creation_time is detailed info of date in seconds as well.
```

```sql
-- By using DATETRUNC() we can easily check count month wise, day wise or year wise so easily.

SELECT
     DATETRUNC(month, creation_time) creation,
     count(*)
FROM Sales.orders
GROUP BY DATETRUNC(month, creation_time)

--Now we want highest level of aggregation like year wise
SELECT
     DATETRUNC(year, creation_time) creation,
     count(*)
FROM Sales.orders
GROUP BY DATETRUNC(year, creation_time)
```
<!--------------------------------------->

**```EOMONTH()```** Part Extraction
- End of the Month
- Return the last day of a month.
- It will change day to the last day of the month.
  - '2026-01-21' -> ```EOMONTH()``` -> '2026-01-31'
  - '2026-03-31' -> ```EOMONTH()``` -> '2026-03-31'
  - '2026-02-01' -> ```EOMONTH()``` -> '2026-02-28'

Syntax: 

```sql
     DAY(date),
     MONTH(date),
     YEAR(date),
     EOMONTH(date)
```
```sql
SELECT
     order_id,
     creation_time,
     EOMONTH(creation_time)  endOfMonth
     --DATETRUNC(month, creation_time)  startOfMonth
     CAST(DATETRUNC(month, creation_time) AS DATE)  startOfMonth
FROM Sales.orders
```
<!------------------------------------------------------------------>

**Use Case of Part Extraction : Data Aggregation & Data Filtering**

Why do we need extract those day, month, year, etc parts from a Date?

1. Doing Data Aggregation and Reporting
- Sometime we build report based on data, but sometime we need to aggregate data by specific time unit.
- Reports of Sales
  - Sales by Year : 2024, 2025, 2026
  - Sales by Quarter : Q1, Q2, Q3, Q4
  - Sales by Month : Jan, Feb, Mar, Apr
  - Sales by Day : 1,2,3,4

```sql
--Task : How many orders were place each year?

    SELECT
        YEAR(order_date),
        COUNT(*) numOfOrders
    FROM Sales.orders 
    GROUP YEAR(order_date)

--Task : How many orders were place each months?

    SELECT
        MONTH(order_date),
        COUNT(*) numOfOrders
    FROM Sales.orders 
    GROUP MONTH(order_date)

   SELECT
        DATENAME(month, order_date),
        COUNT(*) numOfOrders
    FROM Sales.orders 
    GROUP DATENAME(month, order_date)
```
2. Data Filtering
- We use part extraction in order to filter data as well.
```sql
-- Task : Show all orders that were placed during the month of february.

   SELECT *
   FROM  Sales.orders
   WHERE MONTH(order_date) = 2;

```
> Best Practice : Filtering Data using Integer is faster than using String.

> Best Practice : Avoid using ```DATENAME()``` for filtering data, instead use ```DATEPART()```.

<!--------------------------------------------------------------------------------------->

**Part Extraction Functions Comparison**

| Functions                                                | Return Type    |
|----------------------------------------------------------|----------------|
|```DAY()```, ```MONTH()```, ```YEAR()```, ```DATEPART()```|  ```INT```     |
|                                          ```DATENAME()```|  ```STRING```  |
|                                         ```DATETRUNC()```|  ```DATETIME```|
|                                           ```EOMONTH()```|  ```DATE```    |

<!----------------------------------------------------------------------------->

**How to decide when to use which function?** <br/>

First, ask yourself ***which part?*** you want to extract. ->

- Day or Month or Year -> ```YEAR()```
  - Day or Month in Numeric -> ```DAY()```,```MONTH()```
  - Day or Month in String -> ```DATENAME()```

- Other parts like Week, Quarter -> ```DATEPART()``` 
<!----------------------------------------------------->

**Part Extraction ALL DATE PARTS**

Date = '2025-08-20 09:38:54:840'

| Part         |  Abbre       | ```DATEPART()``` : INT | ```DATENAME()``` : STRING | ```DATETRUNC()``` : DATETIME2 |
|--------------|--------------|------------------------|---------------------------|-------------------------------|
| year         | yy, yyyy     | 2025                   |    2025                   |  2025-01-01 00:00:00          |
| quarter      | qq, q        | 3                      |    3                      |  2025-07-01 00:00:00          |
| month        | mm, m        | 8                      |    August                 |  2025-08-01 00:00:00          |
| dayofyear    | dy, y        | 232                    |    232                    |  2025-08-20 00:00:00          |
| day          | dd, d        | 20                     |    20                     |  2025-08-20 00:00:00          |
| weekday      | dw           | 4                      |    Wednesday              |  Not Supported                |
| week         | wk, ww       | 34                     |    34                     |  2025-08-17 00:00:00          |
| iso_week     | ns           | 34                     |    34                     |  2025-08-18 00:00:00          |
| hour         | hh           | 9                      |    9                      |  2025-08-20 09:00:00          |
| minute       | mi, n        | 45                     |    45                     |  2025-08-20 09:45:00          |
| second       | ss, s        | 21                     |    21                     |  2025-08-20 09:45:21          |
| millisecond  | ms           | 0                      |    0                      |  2025-08-20 09:45:21          |
| microsecond  | msc          | 0                      |    0                      |  2025-08-20 09:45:21          |
| nanosecond   | ns           | 0                      |    0                      |  Not Supported                |
| iso_week     | isowk, isoww | 0                      |    +00:00                 |  Not Supported                |

<!----------------------------------------------------->

##  DATE & TIME : Format & Casting ```FORMAT()```, ```CONVERT()``` and ```CAST()```

How to do formatting and casting for the date information in SQL?

**What is DATE FORMAT?** <br/>

```
 'year' 'Month' 'Day' 'Hours' 'Minutes' 'Seconds' Milliseconds
 '2025-08-20 09:45:21'
 'yyyy-MM-dd HH:mm:ss`    <-- Format Specifier, case sensitive MM = Month and mm = minute
```
- This is called **Date & Time Format**
- In the world, there are different representation on how to represent date.
- International Standard (ISO 8601) is 'yyyy-MM-dd'
- USA Standard  is 'MM-dd-yyyy'
- European Standard  is 'dd-MM-yyyy'

> SQL Server follows ISO 8601 'yyyy-MM-dd'


**What are FORMATING and CASTING?** 

**Formatting** : Changing the format of a value from one to another. Meaning, Changing how the data looks. <br/>
'2026-08-20' -> ```FORMAT()``` -> '08/20/2026' or 'Aug 2026' by providing the required format.

In SQL, there is another function that helps us to format data i.e. ```CONVERT()``` Here, we don't provide format, but provide style number.
if style num = 6 -> 20 Aug 25
if style num = 112 -> 20250820

Not only Date, we can also format Number.
1234567.89 -> ```FORMAT(N)``` -> 1,234,567.89
1234567.89 -> ```FORMAT(C)``` -> $ 1,234,567.89
1234567.89 -> ```FORMAT(P)``` -> $ 123,456,789.00%

> Output result of ```FORMAT()``` is String.

Here, we have seen how the value looks like.
On the other hand 

**Casting** : Changing the data type from one to another.

<!------------------------------------------------------>
**String Functions**

|  Manipulation | Calculation | String Extraction |
|---------------|-------------|-------------------|
| ```CONCAT```  |  ```LEN```  | ```LEFT```        |
| ```UPPER```   |             | ```RIGHT```       |
| ```LOWER```   |             | ```SUBSTRING```   |
| ```TRIM```    |             |                   |
| ```REPLACE``` |             |                   |

**Number Functions**

|  Manipulation | 
|---------------|
| ```ROUND```   | 
| ```ABS```     | 


**DATE and TIME Functions**

|  Part Extraction | Format & Casting | Calculations    |  Validation |
|------------------|------------------|-----------------|-------------|
| ```DAY```        |  ```FORMAT```    | ```DATEADD```   | ```ISDATE```|
| ```MONTH```      |  ```CONVERT```   | ```DATEDIFF```  |             |
| ```YEAR```       |  ```CAST```      |                 |             |
| ```DATEPART```   |                  |                 |             |
| ```DATENAME```   |                  |                 |             |
| ```DATETRUNC```  |                  |                 |             |
| ```EOMONTH```    |                  |                 |             |


</details>

<!------------------------------------------->
<details>
  <summary><b> NULL Functions </b></summary>

  
</details>

<!------------------------------------------->
<details>
  <summary><b> CASE Statement </b></summary>

  
</details>
<!------------------------------------------->


<!-----------Chapter 7. Row-Level Functions------------------------>
