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

Not only Date, we can also format Number. <br/>
1234567.89 -> ```FORMAT(N)``` -> 1,234,567.89 <br/>
1234567.89 -> ```FORMAT(C)``` -> $ 1,234,567.89 <br/>
1234567.89 -> ```FORMAT(P)``` -> $ 123,456,789.00% <br/>

> Output result of ```FORMAT()``` is String.

Here, we have seen how the value looks like.
On the other hand 

**Casting** : Changing the data type from one to another. <br/>
String '123' -> ```CAST()``` -> 123 Interger <br/>
Date '2026-01-31' -> ```CAST()``` -> '2026-01-31' String <br/>
String '2026-01-31' ```CAST()``` -> '2026-01-31' Date <br/>


> We can do casting using ```CAST()``` function or ```CONVERT()``` function in order to change the data type.

### ```Format( )```

- Formate a date or the time value

Syntax :
```sql
   FORMAT(value, format, [culture]) 3rd is optional & Default culture is `en-US`

   FORMAT(order_date, 'dd/MM/yyyy')

   FORMAT(order_date, 'dd/MM/yyyy', 'ja-JP') --Japanese Style

   FORMAT(1234.56, 'D', 'fr-FR') --J France Style  
```

```sql
   SELECT
      order_id,
      creation_time,
      FORMAT(creation_time, 'dd') dd                   --01,02,....,31
      FORMAT(creation_time, 'ddd') ddd                 --Sun,Mon,.....Sat
      FORMAT(creation_time, 'dddd') dddd               --Sunday,Monday,.....Saturday
      FORMAT(creation_time, 'MM') MM                   --01,02,......,12
      FORMAT(creation_time, 'MMM') MMM                 --Jan, Feb,.....,Dec
      FORMAT(creation_time, 'MMMM') MMMM               --January,February,.....,December
      FORMAT(creation_time, 'MM-dd-yyy') USA_Format    --'12-31-2026'
      FORMAT(creation_time, 'dd-MM-yyy') EU_Format    --'31-12-2026'
   FROM Sales.orders
```

```sql
   -- Task : Show creation_time using the following format: Day Wed Jan Q1 2026 12:34:56 PM

   SELECT
      creation_time,
      'DAY '+ FORMAT(creation_time'ddd MMM') + ' Q'+DATENAME(quarter, creation_time)+' '+format(creation_time, 'yyyy hh:mm:ss tt') as customFormat
   FROM Sales.orders
```

**Use case of Formatting** : Data Aggregations and Data Standardization

**1. Data Aggregations**

Reports : Sales by Month and 2 digit for year like : Jan 26, Feb 26, Mar 26,.....,Dec <br/>
Once we change the format like Jan 26 and then do data aggregation, we will have a nice report about sales.

```sql
   SELECT
      FORMAT(order_date, 'MMM yy') order_date
      COUNT(*)
      FROM Sales.orders
      GROUP BY FORMAT(order_date, 'MMM yy')
```

**2. Data Standardization**

Data could be store in different technology like .csv file or we can get data by using an API call or data coulde be stored in another database. <br/>
So, what we usually do, go and extract the data from these different sources into one centre storage. <br/>
It could have been you're getting different format for the date. Of course, this is a problem for Analytics we can't present different formats of date. <br/>
So, What we can do here is clean the format into one standard format. means we have to format incoming data into a new format. <br/>
Once, we have one standard format, we can use it in Analytics & Report. <br/>
This is very common use case in data preparation and in data clean up by standardizing the different format into one. 


### ALL FORMATS (Date & Time Format Specifiers)

'2025-08-20 18:55:45'

|   Format   | Description                     |  Result                            |
|------------|---------------------------------|------------------------------------|
| D          | Full day name                   |                                    |
| d          | Day of the month                | 8/20/2025                          |
| dd         | Day of the month (2 digit)      | 20                                 |
| ddd        | Abbreviated day name            | Wed                                |
| dddd       | Full day name                   | Wednesday                          |
| M          | Month number                    | 44044                              |
| MM         | Month number (2 digit)          | 8                                  |
| MMM        | Abbreviated month name          | Aug                                |
| MMMM       | Full month name                 | August                             |
| yy         | Year (2 digit)                  | 25                                 |
| yyyy       | Year (4 digit)                  | 2025                               |
| hh         | Hour (12-hour format, 2 digit)  | 06                                 |
| HH         | Hour (24-hour format, 2 digit)  | 18                                 |
| m          | Minutes                         | August 20                          |
| mm         | Minutes (2 digit)               | 55                                 |
| s          | Seconds                         | 2025-08-20T18:55:45                |
| ss         | Seconds(2 digit)                | 45                                 |
| f          | Fractional seconds (1 digit)    | Wednesday, August 20, 2025 6:55 PM |
| ff         | Fractional seconds (2 digit)    | 00                                 |
| fff        | Fractional seconds (3 digit)    | 000                                |
| tt         | AM/PM designator                | PM                                 | 

Not only dates we can format number as well like 1234.56

|   Format  | Description           |  Query                                  |   Result    |
|-----------|-----------------------|-----------------------------------------|-------------|
| N         | Numeric default       | ```SELECT FORMAT(1234.56,'N')```        | 1234.56     |
| P         | Percentage            | ```SELECT FORMAT(1234.56,'P')```        | 123,456.00% |
| C         | Currency              | ```SELECT FORMAT(1234.56,'C')```        | $1234.56    |
| E         | Scientific notation   | ```SELECT FORMAT(1234.56,'E')```        | 123E+09     |
| F         | Fixed-point           | ```SELECT FORMAT(1234.56,'F')```        | 1234.56     |
| N0        | Numeric no decimal    | ```SELECT FORMAT(1234.56,'N0')```       | 1234        | 
| N1        | Numeric one decimal   | ```SELECT FORMAT(1234.56,'N1')```       | 1234.5      |
| N2        | Numeric two decimals  | ```SELECT FORMAT(1234.56,'N2')```       | 1234.56     |
| N, de_DE  | Numeric (German)      | ```SELECT FORMAT(1234.56,'N','de_DE')```| 1.234.56    |
| N, en_US  | Numeric (US)          | ```SELECT FORMAT(1234.56,'N','en-US')```| 1,234.56    |

<!------------------------------------>

### ```CONVERT( )```

- ```CONVERT()``` converts a date or time value to a different data type & helps in formatting the value.
  
Synatx : 
```sql
       CONVERT(data_type, value, [style])  --[style is optional] & Default value of style is 0

       CONVERT(INT, '124')
       CONVERT(VARCHAR, order_date, '34')  -- convert order_date in VARCHAR using style '34'
```

```sql
SELECT 
     CONVERT (INT, '123') AS [String to Int CONVERT],     --Here, we can use [ ] to take space in your column name.

     CONVERT(DATE, '2026-12-31') AS [String to Date CONVERT],     --Casting 
     creation_time,
     CONVERT(DATE, creation_time) AS [Datetime to Date CONVERT],  --Casting 
     CONVERT(VARCHAR, creation_time, 32) AS [USA Std. Style:32],  --Casting + formating in one function
     CONVERT(VARCHAR, creation_time, 32) AS [EURO Std. Style:34]  --Casting + formating in one function
FROM Sales.orders
```

### ```CONVERT()``` ALL STYLES

Date 

| Style Num |   Format    | Example      |
|-----------|-------------|--------------|
| 1         | mm/dd/yy    | 12/30/25     |
| 2         | yy.mm.dd    | 25.12.31     |
| 3         | dd/mm/yy    | 27/11/2029   |
| 4         | dd.mm.yy    | 31.12.2027   |
| 5         | dd-mm-yy    | 30/12/2026   |
| 6         | dd-Mon-yy   | 31-Dec-25    |
| 7         | Mon dd,yy   | Dec 31, 25   |
| 10        | mm-dd-yy    | 12-30-25     |
| 11        | yy/mm/dd    | 25/12/1930   |
| 12        | yymmdd      | 251230       |
| 23        | yyyy-mm-dd  | 2025-12-30   |
| 31        | yyyy-dd-mm  | 2025-30-12   |
| 32        | mm-dd-yyyy  | 12-30-2025   |
| 33        | mm-yyyy-dd  | 12-2025-30   |
| 34        | dd/mm/yyyy  | 30/12/2026   |
| 35        | dd-yyyy-mm  | 30-2025-12   |
| 101       | mm/dd/yyyy  | 12/30/2025   |
| 102       | yyyy.mm.dd  | 2025.12.30   |
| 103       | dd/mm/yyyy  | 30/12/2026   |
| 104       | dd.mm.yyyy  | 30.12.2026   |
| 105       | dd-mm-yyyy  | 30-12-2026   |
| 106       | dd-Mon-yy   | 30 Dec-25    |
| 107       | Mon dd,yyyy | Dec 30, 2026 |
| 110       | mm-dd-yyyy  | 12-30-2026   |
| 111       | yyyy/mm/dd  | 30/12/2026   |
| 112       | yyyymmdd    | 20261230     |

Time

| Style num | Formnat         | Example      |
|-----------|-----------------|--------------|
| 8         | hh:mm:ss        | 00:38:54     | 
| 14        | hh:mm:ss:nnn    | 00:38:54:840 |
| 24        | hh:mm:ss        | 00:38:54     |
| 108       | hh:mm:ss        | 00:38:54     |
| 114       | hh:mm:ss:nnn    | 00:38:54:840 |

Datetime2  :  '2026-08-20 18:55:45.840'

| Style num | Formnat                       | Example                   |
|-----------|-------------------------------|---------------------------|
| 0         | Mon dd yyyy hh:mm AM/PM       | Dec 30 2026 12:38AM       |
| 9         | Mon dd yyyy hh:mm:ss:nnn AM/PM| Dec 30 2026 12:38:54:840AM|
| 13        | dd Mon yyyy hh:mm:ss:nnn AM/PM| 30 Dec 2026 00:38:54:840AM|
| 20        | yyyy-mm-dd hh:mm:ss           | 2026-12-30 00:38:54       |
| 21        | yyyy-mm-dd hh:mm:ss:nnn       | 2026-12-30 00:38:54.840   |
| 22        | mm/dd/yy hh:mm:ss AM/PM       | 12/30/26 12:38:54 AM      |
| 25        | yyyy-mm-dd hh:mm:ss:nnn       | 2026-12-30 00:39:54.840   |
| 26        | yyyy-dd-mm hh:mm:ss:nnn       | 2026-30-12 00:39:54.840   |
| 27        | mm-dd-yyyy hh:mm:ss:nnn       | 12-30-2026 00:39:54.840   |
| 28        | mm-yyyy-dd hh:mm:ss:nnn       | 12-2026-30 00:39:54.840   |
| 29        | dd-mm-yyyy hh:mm:ss:nnn       | 30-12-2026 00:39:54.840   |
| 30        | dd-yyyy-mm hh:mm:ss:nnn       | 30-2026-12 00:39:54.840   |
| 100       | Mon dd yyyy hh:mm: AM/PM      | Dec 30 2026 12:38AM       |
| 109       | Mon dd yyyy hh:mm:ss:nnn AM/PM| Dec 30 2026 12:38:54:840AM|
| 113       | dd Mon yyyy hh:mm:ss:nnn      | 30 Dec 2026 00:38:54:840  |
| 120       | yyyy-mm-dd hh:mm:ss           | 2026-12-30 00:38:54       |
| 121       | yyyy-mm-dd hh:mm:ss:nnn       | 2026-12-30 00:38:54.840   |
| 126       | yyyy-mm-dd T hh:mm:ss:nnn     | 2026-12-30T00:38:54.840   |
| 127       | yyyy-mm-dd T hh:mm:ss:nnn     | 2026-12-30T00:38:54.840   |
 
<!------------------------------------------------------>

### ```CAST( )```

- ```CAST()``` converts a value to a specified data type.

Syntax :
```sql
     CAST(value AS data_type)

     CAST('123' AS INT)
     CAST('2026-12-31' AS DATE)
```
> No format can be specified.

```sql
      SELECT
           CAST('123' AS INT) AS [String to Int],
           CAST(123 AS VARCHAR) AS [Int to String],
           CAST('2026-12-31' AS DATE) AS [String to Date],
           CAST('2026-12-31' AS DATETIME2) AS [String to Datetime],
           creation_time,
           CAST(creation_time AS DATE) AS [Datetime to Date]
    FROM Sales.orders
```
> ```CAST()``` is an very simple & amazing function. We can use it only to changing the data type from one to another. This doesn't change the format.

<!------------------------------------------------------>

**```FORMAT()``` vs ```CONVERT()``` vs ```CAST()```**

- Using these 3 functions we can do 2 things either casting or formating.

|                |    CASTING              |    Formating                         |
|----------------|-------------------------|--------------------------------------|
| ```CAST()```   | Any type to Any type    |  No Formating                        |
|```CONVERT()``` | Any type to Any type    | Formats only Date & Time, Not Numbers|
| ```FORMAT()``` | Any type to only String | Formats both Date & Time and Numbers |

We have learned how to do formatting & casting on date information.
Now we will see how to do Date calculation or mathematical operations on Dates

<!------------------------------------------------------>

##  DATE & TIME : Calculations -> ```DATEADD()``` & ```DATEDIFF()```

How to do Date calculation or mathematical operations on Dates?

### ```DATEADD()```
- ```DATEADD()``` allows us to adds or subtracts a specific time interval to/from a date.
- '2026-08-20' **Add 2 years** to this date = '**2028**-08-20'
- '2026-08-20' **Add 3 months** to this date = '2026-**11**-20'
- '2026-08-20' **Add 5 days** to this date = '2026-08-**25**'
- '2026-08-20' **Subtract 2 years** to this date = '**2024**-08-20'
- '2026-08-20' **Subtract 3 months** to this date = '2026-**05**-20'
- '2026-08-20' **Subtract 5 days** to this date = '2026-08-**15**'

Syntax :
``sql
     DATEADD(part, interval, date)

     DATEADD(year, 2, '2026-08-20'),  --'2028-08-20'
     DATEADD(month, -4, '2026-08-20'),  --'2026-04-20'
     DATEADD(day, 2, '2026-08-20'),  --'2026-08-22'
```

``sql
    SELECT
        order_id,
        order_date,
        DATEADD(YEAR, 2, order_date)  AS twoYearsLater,
        DATEADD(MONTH, 3, order_date)  AS threeMonthsLater,
        DATEADD(DAY, 5, order_date)  AS fiveDaysLater,
        DATEADD(DAY, -10, order_date)  AS tenDaysBefore,
    FROM Sales.orders
```
### ```DATEDIFF()```
- DIFF stands for Diiference, ```DATEDIFF()``` allows us to find the difference between two dates.
- '2026-05-20' ```DATEDIFF()``` '2027-02-01' in Year is 1
- '2026-05-20' ```DATEDIFF()``` '2027-02-01' in Month is 9
- '2026-05-20' ```DATEDIFF()``` '2027-02-01' in Day is 257

Syntax :
```sql
     DATEDIFF(part, start_date, end_date)

     DATEDIFF(year, '2026-05-20', '2027-02-01')
     DATEDIFF(year, order_date, ship_date)
     DATEDIFF(day, order_date, ship_date)
```

```sql
   --Task : Calculate the age of an employee

    SELECT
         employee_id,
         birth_date,
         DATEDIFF(year, birth_date, GETDATE()) AS age
    FROM Sales.employees

  --Task : Find the average shipping duration in days for each month

   SELECT
        order_id,
        order_date,
        ship_date, 
        DATEDIFF(day, order_date, ship_date) AS dayToShip,
   FROM Sales.orders

   SELECT
        MONTH(order_date) as Months,  --MONTHNAME(order_date) 
        AVG( DATEDIFF(day, order_date, ship_date) ) AS avgShip,
   FROM Sales.orders
   GROUP BY MONTH(order_date)
```

> ```DATEDIFF()``` is very strong function in order to do Data Analytics using the dates information.

```sql
   -- Time Gap Analysis : Done using Window Function LAG() and DATEDIFF()
   -- Task : Find the number of days between each order and previous order.

   SELECT
         order_id,
         order_date current_order_date,
         --in order to get previous order_date from order_date is use window function LAG() which access a value from the previous row.
         LAG(order_date) OVER(ORDER BY Order_date) previous_order_date,
         DATEDIFF(day, previous_order_date, order_date) AS numOfDaysBetweenOrderDateAndPrevOrderDate
   FROM Sales.orders
```

<!------------------------------------------------------->

##  DATE & TIME : Validation ```ISDATE()```

- ```ISDATE()``` checks if a value is a date.
  - Return 1 if the string value is a valid date.
  - Return 0 if the string value is not a valid date.

Syntax :
```sql
       ISDATE(value)

       ISDATE('2026-01-31')   --True 1
       ISDATE(2026')          --True 1
```

```sql
       SELECT
               ISDATE('123') dateCheck1,         --0
               ISDATE('2026-01-31') dateCheck2,  --1
               ISDATE('31-01-2026') dateCheck3,  --0 bcuz SQL doesn't understand the all formats of date 
               ISDATE('2026') dateCheck4,        --1
               ISDATE('08') dateCheck5,          --0 bcuz SQL only allow to check year not month
```

```sql
-- Cast the value of string into Date datatype of a corrupt data.

SELECT
      order_date,
      CAST(order_date AS DATE) new_order_date
FROM
(      SELECT '2026-08-20' as order_date
       UNION
       SELECT '2026-08-21'
       UNION
       SELECT '2026-08-23'
       UNION
       SELECT '2026-08'     --Here, we have data quality problem
)

-- SQL failed to do so.
-- How to handled this?
-- We can solve this prolem using ISDATE()

SELECT
      order_date,
      ISDATE(order_date), 
      CASE WHEN ISDATE(order_date) = 1 THEN CAST(order_date AS DATE) END new_order_date
FROM
(      SELECT '2026-08-20' as order_date
       UNION
       SELECT '2026-08-21'
       UNION
       SELECT '2026-08-23'
       UNION
       SELECT '2026-08'     --Here, we have data quality problem
)

--That's done successfully!
-- If you want to see all the data that are corrupted use WHERE ISDATE(order_date) = 0
-- And if you want the other value instead of NULL, Put ELSE in case condtion like
-- CASE WHEN ISDATE(order_date) = 1 THEN CAST(order_date AS DATE)
-- ELSE '9999-01-01'
-- END new_order_date
```

**Summary of DATE & TIME Functions :**

We have learned 13 different date & time functions. <br/>
How to extract date parts using **Part Extraction** & when to use which one. <br/>
How to change date format from one to another as well as data type of one from another using **Format & Casting** <br/>
How to do **mathematical calculations** on dates? <br/>
How to validate a value whether date or not using **Validation** <br/>
Part Extraction : ```DAY()```, ```MONTH()```, ```YEAR()```, ```DATEPART()```, ```DATENAME()```, ```DATETRUNC()```, ```EOMONTH()``` <br/>
Format & Casting : ```FORMAT()```, ```CONVERT()```, ```CAST()``` <br/>
Calculations : ```DATEADD()```, ```DATEDIFF()``` <br/>
Validation : ```ISDATE()``` <br/>

<!------------------------------------------------------->

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

Here, We will learn about NULL Functions in order to handle NULLs inside the tables.

Let's talk about a Registration Form. There we have many fields in which some are required* mandatory and some are optional.
So when we insert those data into database, so if optional field is not provided by user then we save it as NULL in the database.
This is what NULL in SQL.

**What is ```NULL```?**
- ```NULL``` means nothing, unknown.
- ```NULL``` is not equal to anything.
  - ```NULL``` is not Zero (0)
  - ```NULL``` is not empty string ('')
  - ```NULL``` is not blank space (' ')
  - ```NULL``` is not string ('NULL')

**```NULL``` Functions**

```NULL()``` is a special SQL function in order to handle the NULLs indside the database. <br/>

In a scenaria where a table contains NULL, we have NULLs inside table. <br/>
We want to remove NULL, and replace it with a new value like 40. <br/>
In order to do that we have 2 functions : <br/>
1. ```ISNULL()``` 
2. ```COALESCE()```

 NULL -> ```ISNULL()```  or ```COALESCE()``` -> 40

In another scenaria, we have a value 40 and we want to make it as NULL. <br/>
Here, We're doing opposite, we are replacing the value 40 with NULL. <br/>
For that we have a SQL Function ```NULLIF()```. <br/>

40 -> ```NULLIF()```  -> NULL

> In order to manipulate data from NULL to Value or Value to NULL we have these 3 SQL functions ```ISNULL()```  or ```COALESCE()``` and ```NULLIF()```.


In other scenarios, where we don't want to manipulate anything. <br/>
We just want to check whether the value is NULL or not. <br/>
For that we have function ```IS NULL()``` return TRUE and ```IS NOT NULL()``` return FASLE <br/>

|                   |               | NULL Functions                            |
|-------------------|---------------|-------------------------------------------|
| Replace Values  : | NULL to Value | ```ISNULL()``` or ```COALESCE()```         |
| Replace Values  : | Value to NULL | ```NULLIF()```                            |
| Check for NULLS : | Value to NULL | ```IS NULL()``` and   ```IS NOT NULL()```  |


**```ISNULL()```**
- ```ISNULL()``` replaces `NULL` with a specified value.
Syntax :
```sql
    ISNULL(value, replacement_value)

    ISNULL(shipping_address, 'N/A')       --Here, we're replacing NULL with a static value
  
    ISNULL(shipping_address, billing_address) --Here, we're replacing NULL with another column which is second address
```
How it works? <br/>
Here, We're checking whether the value is NULL if it is Yes then get value from replacement and if value is not NULL then show the va;ue itself.

Scenario 1:

| order_id | shipment_adderess|  ->  | ```ISNULL()``` : Result |
|----------|------------------|------|--------|
| 1        | abc              |  ->  | abc    |
| 2        | NULL             |  ->  | N/A    |

Scenario 2 :

| order_id | shipment_adderess| billing_address |  ->  | ```ISNULL()``` : Result |
|----------|------------------|-----------------|------|--------|
| 1        | abc              |  xyz            |  ->  | abc    |
| 2        | NULL             |  pqr            |  ->  | pqr    |
| 3        | NULL             |  NULL           |  ->  | NULL   |


**```COALESCE()```**
- ```COALESCE()``` returns the first non-null value from a list.
Syntax :
```sql
      COALESCE(value1, value2, value3, .......)

     COALESCE(shipping_address, 'N/A')
     COALESCE(shipping_address, billing_address)
     COALESCE(shipping_address, billing_address, 'N/A')
```

How it works? <br/>

| order_id | shipment_adderess| billing_address |  ->  | ```COALESCE()``` : Result |
|----------|------------------|-----------------|------|---------------------------|
| 1        | abc              |  xyz            |  ->  | abc    |
| 2        | NULL             |  pqr            |  ->  | pqr    |
| 3        | NULL             |  NULL           |  ->  | N/A    |

**```ISNULL()``` vs ```COALESCE()```***

| ```ISNULL()```                             | ```COALESCE()```         |
|--------------------------------------------|--------------------------|
| Limited to two values                      | Unlimited                |
| Faster                                     | Slower                   |
| Different keywords for different databases | same for every databases |
| SQL Server -> ```ISNULL()```               |    ```COALESCE()```      |
| Oracle     -> ```NVL()```                  |    ```COALESCE()```      |
| MySQL      -> ```IFNULL()```               |    ```COALESCE()```      |

> Always use  ```COALESCE()```  instead of  ```ISNULL()``` bcuz  ```COALESCE()``` is available in all databases.

### **Use cases of  ```COALESCE()``` and  ```ISNULL()```**

We mainly use ```COALESCE()``` and  ```ISNULL()``` in order to handle NULL before doing any SQL task.

**1. Handle the NULL before doing data aggregations**

If we have 3 cells one contains 15, second 25 and third NULL. If we aggregate it with function like ```AVG()``` SQL will calculate like (15+25)/2 = 20 which should be (15+25+0)/3 = 13.33 similar way ```SUM()``` or ```COUNT(sales)``` OR ```MIN()``` OR ```MAX()``` except we are doing ```COUNT(*)```

In scenario where business understand NULL as 0 , then you can have problem with the result of Analysis if you don't handle the NULLs. So, what we have to do is handle the NULLs before aggregation. So, we will replace NULL with 0 either by using ```ISNULL()``` or ```COALESCE()```.

```sql
   --Task : Find the average score of the customers.
    --We should also have to consider the NULL scores as 0 if it is there in that case below

    SELECT
        customer_id,
        score,
        COALESCE(score,0) score2,
        AVG(score) OVER() avgScore                            --window function
        AVG(COALESCE(score,0)) OVER() avgScoreWithNull
    FROM Sales.customers
    GROUP BY customer_id
```

**2. Handle the NULL before doing any mathematical operations.**

e.g. :
 ```
      1 + 5 = 6
     0  + 5 = 5
   NULL + 5 = ?
   NULL + anything = NULL

    'A'  +  'B' = 'AB'
    ''   +  'B' = 'B'
    NULL +   B   = NULL
```

```sql
   -- Task : Display the full name of the customers in a single field by merging their first and last names,
   --       and add 10 bonus points to each customer's score.

   SELECT
        first_name,
        last_name,
        first_name + ' ' + last_name AS fullname,          --CONCAT(first_name, ' ' ,last_name) 
        score,
        score + 10 AS new_score
   FROM Sales.customers

   --This will do the job but if the any value is found NULL then it will be result into NULL
   -- To handle this, we have to use ISNULL() or COALESCE() in order to replace NULL with 0 or ''

   SELECT
        first_name,
        COALESCE(first_name, '') AS first_name2,
        last_name,
        COALESCE(last_name, '') AS last_name2
        COALESCE(first_name, '') + ' ' + COALESCE(last_name, '') AS fullname,          --CONCAT(first_name, ' ' ,last_name) 
        score,
        COALESCE(score, 0) AS score2,
        COALESCE(score, 0) + 10 AS score_WithBonus
   FROM Sales.customers

```

**3. Handling the NULLs before joining(JOINs) tables**

This is kind of advanced use case. <br/>
Let's say we have 2 tables Table A & Table B. <br/>
Now, we have to combine these 2 tables using ```JOINs```. <br/>
In order to ```JOIN```, we have to specify the keys between the tables to join on it. <br/>
If those keys don't have NULL inside it and all the data are filled then ```JOIN``` works perfectly and you will get expected result. <br/>
But if there are some cases where keys are NULLs that means the value is missing & this is a problem, bcuz in the output you will get unexpected result as some records will be missing. <br/>
So, To handle the NULLs inside the keys before doing the JOIN. <br/>
We use ```ISNULL()``` or ```COALESCE()```. 

Table 1 & Table 2

| Year | Type | orders |   | Year | Type | Sales|
|------|------|--------|---|------|------|------|
|2025  | a    | 30     |   | 2024 | a    | 100  |
|2025  | NULL | 40     |   | 2024 | NULL | 200  |
|2026  | b    | 50     |   | 2025 | b    | 300  |
|2026  | NULL | 60     |   | 2025 | NULL | 200  |

```sql
     SELECT 
         a.year, a.type, a.order, b.sales
     FROM table1 a
     JOIN table2 b
     ON a.year = b.year AND a.type = b.type
```
Result :

| Year | Type | orders | Sales |
|------|------|--------|-------|
|2025  | a    | 30     |  100  |
|2026  | b    | 50     |  300  |

Even though keys are identical & matching but we're getting only 2 rows this is bcuz SQL can't compare NULL.
So, here we are losing data. 

> SQL can't compare NULL

```sql
     SELECT 
         a.year, a.type, a.order, b.sales
     FROM table1 a
     JOIN table2 b
     ON ISNULL(a.year, '') = ISNULL(b.year, '') AND ISNULL(a.type, '') = ISNULL(b.type, '')
```
Result :

| Year | Type | orders | Sales |
|------|------|--------|-------|
|2025  | a    | 30     |  100  |
|2025  | NULL | 40     |  200  |
|2026  | b    | 50     |  300  |
|2026  | NULL | 60     |  200  |


**4. Handling the NULLs before Sorting data**

If we have 3 cells one contains 15, second NULL and third 25. <br/>
If we sort the data by the cells in ascending(lowest to highest) then SQL will show the NULLs at start. that's not bcuz NULL is the lowest value bcuz NULL has no value. <br/>
SQL ORDER BY ASC  : NULL < 15 < 25      -> It's not bcuz NULL has the lowest value, bcuz NULL has no value. <br/>
SQL ORDER BY DESC :   25 < 15  < NULL   -> It's not bcuz NULL has the lowest value, bcuz NULL has no value. <br/>

```sql
    --Task : Sort the customers from lowest to highest scores with NULLs appearing last.

     SELECT
          customer_id,
          score
     FROM Sales.orders
     ORDER BY scores ASC

   --Handling NULLs in sorting

  --Lazy way by keeping Null as a static 99999 value
    SELECT
          customer_id,
          score,
          COALESCE(score, 99999) as score2
     FROM Sales.orders
     ORDER BY COALESCE(score, 99999) ASC

  --Professional way 
   SELECT
          customer_id,
          score,
          --CASE WHEN score IS NULL THEN 1 ELSE 0 END AS flag
     FROM Sales.orders
     ORDER BY CASE WHEN score IS NULL THEN 1 ELSE 0 END AS flag, score ASC
```

**```NULLIF()```**
- ```NULLIF()``` compares two expressions returns :
  - NULL if they are equal.
  - First value, if they are not equal.

Syntax:
```sql
     NULLIF(value1, value2)

     NULLIF(shipping_address, 'unknown')              -- returning a static value
     NULLIF(shipping_address, billing_addres)         -- returning another column
```

> ```NULLIF()``` takes only 2 arguments, unlike ```COALESCE()``` which can takes a list of arguments.

```sql
      NULLIF(price, -1)
```

|order_id | price |   | ```NULLIF()``` Result |
|---------|-------|---|-----------------------|
|  1      |  90   |   |        90             |
|  2      |  -1   |   |        -1             |
|  3      |       |   |       NULL            |

- Here, we're replacing a real value to NULL.

```sql
     NULLIF(original_price, discount_price)
```

|order_id | original price |discount_price|   | ```NULLIF()``` Result |
|---------|----------------|--------------|---|-----------------------|
|  1      |  150           |    50        |   |        150            |
|  2      |  250           |    250       |   |        NULL           |
|  3      |  100           |    100       |   |        NULL           |


Why do we need ```NULLIF()``` ? 
- In order to highlight or flag special cases in our data.

**Use cases of ```NULLIF()```**

1. Preventing the error of Division By Zero

```sql
   --Task : FInd the sales price for each order by dividing the sales by the quantity.

         SELECT
              order_id,
              qty,
              sales,
              sales / qty AS sales_price           --Error as Division by Zero
         FROM Sales.orders

        --To handle the Division by zero bcuz qty can be 0, we have to use NULLIF()

         SELECT
              order_id,
              qty,
              sales,
              sales / NULLIF(qty, 0) AS sales_price      -- bcuz NULLIF makes o as NULL & If you do any operation with NULL it will be NULL only.
         FROM Sales.orders
```

> It's better to get NULL than having an error of Division by zero.


**```IS NULL``` and ```IS NOT NULL```**

- ```IS NULL``` returns **TRUE** if the value is **NULL** otherwise it returns **FALSE**
  
- ```IS NOT NULL``` returns **TRUE** if the value is not **NULL** otherwise it return **FALSE**

Syntax :
```sql
       value IS NULL
       value IS NOT NULL

       shipping_address IS NULL
       shipping_address IS NOT NULL
```

```sql
       price IS NULL
```

How it works?

| order_id | price  |   | ```IS NULL``` Result | ```IS NOT NULL``` Result|
|----------|--------|---|----------------------|-------------------------|
|    1     |  90    |   |   FALSE              |   TRUE                  |
|    1     |  NULL  |   |   TRUE               |   FALSE                 |


**Use cases of ```IS NULL``` and ```IS NOT NULL```** : Filtering Data ,ANTI JOINs

1. Searching for missing information or searching for NULL & may be after that clean up data by removing NULLs from datasets.

```sql
    -- Task : Identify the customers who have no scores.

       SELECT *
       FROM Sales.customers
       WHERE score IS NULL

   -- Task : List all cusomers who have scores

       SELECT *
       FROM Sales.customers
       WHERE score IS NOT NULL
```

2. Findinding the unmatched rows between two tables : LEFT ANTI JOIN | RIGHT ANTI JOIN

> **LEFT ANTI JOIN** : All rows from the left table without matches the right table.

```sql
     --Task : List all details for customers who have not placed any order

      SELECT
           c.customer_id,
           c.customer_name,
           o.order_id 
      FROM Sales.customers c
      LEFT JOIN Sales.orders o
      ON c.customer_id = o.customer_id
      WHERE o.order_id IS NULL

     --All rows from the left table (customers) without matches the right table(orders).
```

### NULL ```NULL``` vs Empty ```''``` vs Space ```' '```

This is something which is very confusing for a lot of Developers and anyone who is working with data in databases in SQL.

- ```NULL``` NULL means nothing, unknown.
  
- ```''``` Empty, String value that has zero character, Here we know the value that is nothing which means empty string.

- ```' '``` Space, string value that has one space character or more than one depends on how long we press the spacebar.

```sql

     WITH orders AS (
        SELECT 1 Id, A category
        UNION
        SELECT 2, NULL
        UNION
        SELECT 3, ''
        UNION
        SELECT 4, ' '      --one space
        UNION
        SELECT 5, '  '     --two space
     )
    SELECT *, DATALENGTH(category) as length
    FROM orders
```

| Id  | category | length |
|-----|----------|--------|
| 1   |   A      |   1    |
| 2   |  NULL    |  NULL  |
| 3   |          |   0    |
| 4   |          |   1    |
| 5   |          |   2    |

- It's really hard to detect the data quality issue when it is empty string or space/spaces by just looking at the result. So, To detect we can check length of the value.

|               | NULL           | Empty String       |  Blank Space               |
|---------------|----------------|--------------------|----------------------------|
| Represntation | NULL           |    ```''```        | ```' '```                  |
| Meaning       | Unknown        | Known, Empty value | Known, space value         |
| Data type     | Special Marker | String, length=0   | String (length 1 or more)  |
| Storage       | Very minimal   | Occupies memory    | Occupies memory each space |
| Performance   | Best           | Fast               | Slow                       |
| Comparison    | ```IS NULL```  | ```=''```          | ```=' '```                 |

### Handling NULL Data Policies

Why do we have to understand the differences between those stuffs like NULL, Empty string, space etc everything like similar?
- In a project, we will be working on source data that has bad data quality & we might encounter these scenario.
- If we don't do any Data preparation like cleaning the data by handling all those scenario and bring standards to your data, if you directly jump to source data you will providing inaccurate results in your report analysis which leads to a wrong business decision.
- So, Preparing data before doing Analysis is very important step.

Together with the stakeholders and users of reports & analyis, We have to define a clear Data Policy.

**Data Policy** : Set of rules that defines how data should be handled.

**Rule 1. Data POlicy : Only use NULLS and empty strings, but avoid blank spaces.**

> ```TRIM()``` function remove unwanted leading & trailing spaces from a string.

```sql
     WITH orders AS (
        SELECT 1 Id, A category
        UNION
        SELECT 2, NULL
        UNION
        SELECT 3, ''
        UNION
        SELECT 4, ' '      --one space
        UNION
        SELECT 5, '  '     --two space
     )
    SELECT *, TRIM(category) policy1, 
    FROM orders

   -- Now we are very sure either category will be NULL or empty string
   -- Using only one single TRIM() function, cleaning up data & bringing standards.
```

**Rule 2. Data POlicy : Only use NULLS and avoid using empty strings and blank spaces.**

```sql
     WITH orders AS (
        SELECT 1 Id, A category
        UNION
        SELECT 2, NULL
        UNION
        SELECT 3, ''
        UNION
        SELECT 4, ' '      --one space
        UNION
        SELECT 5, '  '     --two space
     )
    SELECT *,
           TRIM(category) policy1,              --avoiding blank spaces(leading & trailing)
           NULLIF(TRIM(category), '') policy2  --avoiding both empty string as well as blank spaces(leading & trailing)
    FROM orders
```

**Rule 3. Data Policy : Use the default value 'unknown' and avoid using nulls, empty string and blank spaces**

```sql
     WITH orders AS (
        SELECT 1 Id, A category
        UNION
        SELECT 2, NULL
        UNION
        SELECT 3, ''
        UNION
        SELECT 4, ' '      --one space
        UNION
        SELECT 5, '  '     --two space
     )
    SELECT *,
           TRIM(category) policy1,                                    --avoiding blank spaces(leading & trailing)
           NULLIF(TRIM(category), '') policy2,                        --avoiding both empty string as well as blank spaces(leading & trailing)
           ISNULL(category, 'unknown') policy                         --avoiding even NULL, instead using default static string unknown using ISNULL()
           COALESCE(NULLIF(TRIM(category), ''), 'unknown') policy3    --avoiding even NULL, instead using default static string unknown using COALESCE()
    FROM orders
```

- These are three different ways to clean up data & bring data to it standards before reports & analysis.
- which one should we use in project, It's really depends on business but try to avoid policy1, So we left with policy2 and policy3. We use both of them in different scenario. We normally go with policy2 bcuz It consumes less storage space as well as performance is fast.  

Use case of Data Policy 2
- replacing empty strings and blank spaces with NULL during the data preparation before inserting into a database to optimize storage & performance.

Use case of Data Policy 3
- Replacing empty string, blanks and NULL with the default value during data preparation before using it in reporting to improve readability and reduce confusion.

### Summary of NULLs
- NULLs are special markers in SQL in order to say no value or missing value or unknown.
- Using NULLs can optimise storage save some spaces and provide strong performance in query.
- In SQL, we have different functions in order to handle NULL
  - ```COALESCE()``` | ```ISNULL()``` : replaces NULL to Value 
  - ```NULLIF()``` : replaces Value to NULL
  - ```IS NULL``` | ```IS NOT NULL``` : Check whether value is NULL or not & returns TRUE & FALSE.
- Use cases of NULLs
  - Handles NULL - Before Data Aggregation [ SUM(), AVG(), COUNT(), MAX(), MIN() ]
  - Handles NULL - Before Mathematical Operations [+, -, *, / ]
  - Handles NULL - Before Joining Tables
  - Handles NULL - Before Sorting Data
  - Finding unmatched Data - LEFT ANTI JOIN (```LEFT JOIN + IS NULL``` ) | RIGHT ANTI JOIN (```RIGHT JOIN + IS NULL``` )
  - Data Policies [NULLs, Default Value = 'unknown']

</details>

<!------------------------------------------->
<details>
  <summary><b> CASE Statement </b></summary>

This is very important tool in order to do Data Transformation
  
</details>
<!------------------------------------------->


<!-----------Chapter 7. Row-Level Functions------------------------>
