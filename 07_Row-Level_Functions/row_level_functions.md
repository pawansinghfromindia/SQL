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

# 1. Single-Row-Level Functions

Here, we can group Row-Level functions into multiple category

<details>
  
<summary><b> String Functions </b></summary>
  
Since, We have a lot of String Functions, So we are going to divide it into multiple categories based on the purpose.

- Functions that manipulate string values :  ```CONCAT```,  ```UPPER```,  ```LOWER```,  ```TRIM```,  ```REPLACE```
- Functions that does calculation on string values :  ```LEN```
- Functions that extract something from string value :  ```LEFT```,  ```RIGHT```,  ```SUBSTRING```

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

***```UPPER```** and **```LOWER```**

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

**```TRIM```**
  
- ```TRIM``` removes leading and trailing spaces in string. 
- '   Rohit' this contains leading spaces <br/> 'Rohit    ' this contains trailing spaces <br/> '  Rohit  ' this contains both leading and trailing spaces.
- Here, Spaces are evils & you don't know how many spaces user have entered. So, we have to do data cleansing for that we have ```TRIM()```.
- ```TRIM``` cleans all the spaces in a string.

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

**```LEFT``` and ```RIGHT```

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

  
</details>

<!------------------------------------------->
<details>
  <summary><b> Date & Time Functions </b></summary>

  
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

|  Manipulation | Calculation | String Extraction |
|---------------|-------------|-------------------|
| ```CONCAT```  |  ```LEN```  | ```LEFT```        |
| ```UPPER```   |             | ```RIGHT```       |
| ```LOWER```   |             | ```SUBSTRING```   |
| ```TRIM```    |             |                   |
| ```REPLACE``` |             |                   |
<!-----------Chapter 7. Row-Level Functions------------------------>
