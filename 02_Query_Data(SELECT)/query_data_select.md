<!-------Chapter 2. Query Data (SELECT)------------------>
# Chapter 2. Query Data (SELECT)

Here in this chapter, we will see - How to query our data?

Query Data (SELECT)
  - ```SELECT```
  - ```FROM```
  - ```WHERE```
  - ```ORDER BY```
  - ```GROUP BY```
  - ```HAVING```
  - ```DISTINCT```
  - ```TOP```
  - ```JOIN```
  - Query Order and Execution

<!--------------------------------->

## 2.1 Query Data (SELECT)

Understanding What is an SQL Query? 

Normally data is inside the table, and table is inside the database.

Now you have questions from the business like -
  - What is the total sales?
  - What is the total number of customers?

So, We ask for data, data is in the database and we have to retrieve data from the database, in order to do that we have to talk to the database using the language
that database understands that is SQL.

We will write query something called ```SELECT``` statement, with that we're asking our database for data. 
Once you execute SQL Query, the database is going process your query and fetch your data and then it prepares a result to send back to you.

So Here, we are like reading our data from the database.
The queries will not modify anything, no data is changed inside table or no structure is changes of the database.

> Use ```SELECT``` statement only in order to read something from the database.
You will retrieve data from the database.

This is what we mean with a Query!

<!--------------------------------->

## 2.2 SQL Query Clauses

Each SQL Query has usually different sections, different components. We call the **Clauses**.

Clauses are amazing tools to write a query that matches any question that you have about your data.

Let's start with two clauses that makes the simplest query in SQL : ```SELECT``` and ```FROM```.

- **```SELECT *```** : Retrieve all columns(Everything) from your table. We can also specify which column to select.
- **```FROM```** : Tells SQL where to find your data. So, specify your table.

**SELECTE *(ALL) data from table***

Syntax :
```sql
   SELECT * FROM table_Name;
```
Once you execute the SQL Command, what's going to happen is First SQL execute ```FROM``` clause, then ```SELECT``` which columns to keep in results.

Important Note : Make sure you're connected to the correct database, otherwise your queries won't work. We also have a command for that is :

```sql
   USE database_Name;
   SELECT * FROM table_Name;
```

<!--------------------------------------->
**COMMENTS** :
  - Comments are like notes to explain code, and ignored by SQL Engine during the execution.
  - There are two ways to comment

  1. Inline Comments
  ```sql
        -- This is inline comments
  ```

  2. Multiple line Comments
  ```sql
            /* This
                 is a
                   multi-line comments.
            */
   ```
<!----------------------->

SQL Task : Retrieve All Customer Data (All Rows & Columns)
```sql
      SELECT *
      FROM table_name;
```

**SELECTE Few COlumns from table*** : Choose Columns

If we don't want to see all the columns, we want to see only specific column or columns.
```sql SELECT Column1, Column2 FROM table_name```

Syntax
```sql
     SELECT
           name,
           rollNumber,
           score
     FROM Students;
```
The result will be only those columns in which are mentioned in exactly same order.

Note : No comma (,) after the last column otherwise syntax error.

<!------------------------------------->

## ```WHERE``` Clause | filter the data

We use ```WHERE``` Clause in order to filter our data based on condition. 
The data which fulfill the given condition are going to stay in fetched output result and the data which don't meet the condition will be filter out from the results.

Syntax :
```sql
       SELECT *
       FROM table_name
       WHERE Condition
```

```sql
       SELECT *
       FROM Students
       WHERE Score > 500 
```

```sql
       SELECT
           firsr_name,
           country
       FROM Students
       WHERE Score > 500 and Country = 'India'
```

<!---------------------------------->

##  ```ORDER BY``` Clause | Sort the data

We can used ```ORDER BY``` clause in order to sort our data.

Of course in order to sort the data, we have to decide on 2 mechanism either **ascending order** (lowest value to highest value) and **descending order** (Highest to lowest value.

```sql
      SELECT *
      FROM table_name
      ORDER BY column ASC
```

If you don't specify the mechanism, the default is **ASC**. But It is recommended to specify for clarity.

```sql
      SELECT *
      FROM Students
      ORDER BY rollNumber DESC
```

<!------------------------------------>

## **(Nested) ```Order BY``` | Sort the data**

We can sort out data using multiple columns and we call it Nested Sorting.

```sql
      SELECT *
      FROM Students
      ORDER BY Country ASC
```
The data is not sorted completely as we sorted it on only one column, to sort it in more better way we can sort it on multiple columns

```sql
      SELECT *
      FROM table_name
      ORDER BY Col1,Col2 DESC
```
```sql
      SELECT *
      FROM Students
      ORDER BY Country ASC, Score DESC
```
Here, first Country will be in ascending then Score in ascending.

Important Note : Columns order in ORDER BY is crucial, as sorting is sequential. 

```sql
      SELECT *
      FROM Student
      ORDER BY Score DESC, Country ASC
```
Here, first Score will be Sorted descending then Country in ascending.

We have learned How to sort data using ORDER BY.
Now let's learn how to aggregate and group up your data using GROUP BY.
GROUP BY is put in between WHERE and ORDER BY bcuz in the order of query GROUP BY comes between WHERE and ORDER BY.

<!------------------------------------>

## ```GROUP BY``` | Aggregate or group your data

It is going to combine row with the same values.
It aggregates a column by another column.
e.g. Total score by country

Syntax of GROUP BY
```sql
     SELECT
           Country,       --Category
           SUM(score) AS total_score     --aggregate function is SUM(),Count(),AVG()
      FROM table_name
      GROUP BY Country
```
For each country we're going to see aggregation. <br/>
GROUP BY Country, SQL is going to combine and smash them together in only one row, so each value of the country must exist at maximum once. <br/>
We have scores, now SQL is going to check the Aggregation function i.e SUM() and gives total result.

> **```AS```** : Alias, only like a name that lives inside your query.

|  ID  |    Name    |   Country  | Score |
|------|------------|------------|-------|
| 1    |   Maria    |  Germany   | 350   |
| 2    |   John     |  USA       | 900   |
| 3    |   George   |  UK        | 750   |
| 4    |   Martin   |  Germany   | 500   |
| 5    |   Peter    |  USA       | 100   |

Find the total score for each country.
```sql
     SELECT
         country,
         SUM(score) as total_score
     FROM students
     GROUP BY country
```

Output Result :
|   Country  | total_score |
|------------|-------|
| Germany    | 850  | 
| USA        | 1000 | 
| UK         | 750  | 


**Important Note : ```GROUP BY``` RULE** 
All the columns in the ```SELECT``` must be either aggregated or included in the ```GROUP BY```.

```sql
      SELECT
           country,
           name,
           SUM(score) as total_score
           FROM students
           GROUP BY country,name;
```
The result of ```GROUP BY``` is determined by the unique values of the grouped columns.
Output Result :
|   Country  | name   |total_score |
|------------|--------|------------|
|   Maria    |  Germany   | 350   |
|   John     |  USA       | 900   |
|   George   |  UK        | 750   |
|   Martin   |  Germany   | 500   |
|   Peter    |  USA       | 100   |


Find the total score and total number of students for each country.
```sql
  SELECT
       country,
       SUM(score) as total_score,
       COUNT(id) as total_students
   FROM students
   GROUP BY country     
```
Output Result :
|   Country  | total_score | total_students |
|------------|-------------|----------------|
| Germany    |    850      |     2          |
| USA        |    1000     |     2          |
| UK         |    750      |     1          |


**SQL Aggregate Functions**
- An aggregate function is a function that performs a calculation on a set of values, and returns a single value.

SQL aggregate functions are:
- **MIN()** - returns the smallest value within the selected column
- **MAX()** - returns the largest value within the selected column
- **COUNT()** - returns the number of rows in a set
- **SUM()** - returns the total sum of a numerical column
- **AVG()** - returns the average value of a numerical column

> Aggregate functions ignore null values (except for COUNT(*)).

We have learned How to group up data using ```GROUP BY```.
Next we will see another technique How to filter your data using ```HAVING``` clause.

<!------------------------------------>

## ```HAVING``` Clause | Filter Data after Aggregation

We use ```HAVING``` in order to filter but after the aggregation, that means We can use the ```HAVING``` only after using ```GROUP BY```.

Syntax of HAVING:
```sql
     SELECT
           Country,       
           SUM(score) AS total_score     
      FROM table_name
      GROUP BY Country
      HAVING SUM(score)>800   --Condition
```
Output Result :
|   Country  | total_score |
|------------|-------|
| Germany    | 850  | 
| USA        | 1000 | 


Question ? 
We have used the ```WHERE``` clause to filter the data, then why another one ```HAVING```? can't we use ```WHERE```?
In SQL, there are different ways on how to filter data based on the scenario.

```sql
     SELECT
           Country,       
           SUM(score) AS total_score     
      FROM table_name
      WHERE score>400
      GROUP BY Country
      HAVING SUM(score)>800   --Condition
```

The difference between ```WHERE``` and ```HAVING``` is It is when the filter is happening.
If you want to filter your data ***before aggregation*** then use **```WHERE```** clause But
If you want to filter your data **after aggregation*** then use **```HAVING```** clause.

Output Result :
 WHERE will filter the data before aggregation

|   Country  |    score    |
|------------|-------------|
| Germany    |   500       |  
| USA        |   900       | 
| UK         |   750       |  

After aggregation HAVING will filter the data, so final result is 

|   **Country**  | **total_score** |
|----------------|-----------------|
| USA            |       900       |  

Find the average score for each country considering only students with a score not equal to 0 and return only those countries with an average score greater than 430.
```sql
      SELECT
           country,
           AVG(score) as avg_score
      FROM students
      WHERE score != 0
      GROUP BY country
      HAVING AVG(score) > 430;
```
Output Result :
WHERE filters before aggregation
After aggregation 
|   Country  |    avg_score    |
|------------|-----------------|
| Germany    |   425           |  
| USA        |   900           | 
| UK         |   750           |

After aggregation HAVING filters applied, which removed Germany bcuz avg_score>430 condition is false.

|   Country  |    avg_score    |
|------------|-----------------|  
| USA        |   900           | 
| UK         |   750           |

We have learned how to filter data after aggregation using HAVING.
Next is We use keyword ```DISTINCT``` exactly after the SELECT.

<!-------------------------------------->

## ```DISTINCT```  | Remove Duplicates

```DISTINCT``` removes Duplicates (repeated values) in your data.
It makes sure, each value appears only once in the results.
It is used exactly after the ```SELECT```

Syntax of Distinct :
```sql
      SELECT DISTINCT
             Column_name
      FROM table_name;
```

First thing SQL do is get the data FROM, then SELECT the specified columns then DISTINCT values rows will be only considered in results.

Return Unique list of all countries.

```sql
      SELECT DISTINCT
             country
      FROM students;
```

| **Country** |
|---------|
| Germany |
| USA     |
| UK      |

> Note : Don't use ```DISTINCT``` unless it is necessary; It can slow down your query. bcuz It doesn't make sense to apply distinct on unique column.

For example : ```sql SELECT ID FROM students``` == ```sql SELECT DISTINCT ID FROM students``` , Both will give same result.

> Use ```DISTINCT``` only when you see duplicates.

We have learned how to removed diplicates using ```DISTINCT```.
Next is another keyword ```TOP``` in order to limit your data.

<!------------------------------------------------->

## ```TOP``` | Limit your data

```TOP``` or in some databases, we call it ```LIMIT```
It is also used in a kind of filtering to **restrict the number of rows** returned in the result.
We can control how many rows we want to see in the result using TOP or LIMIT.

Syntax
```sql
      SELECT TOP 3
         *
       FROM table_name;
```

How ```TOP``` works?
For each row in database, we have row numbers (row 1, row 2, row 3, .......). It has nothing to do with your ID column.
Those row numbers are not your actual data. It is something technical from the database.

Retrieve only three students.

```sql
      SELECT TOP 3
         *
       FROM students;
```

|  ID  |    Name    |   Country  | Score |
|------|------------|------------|-------|
| 1    |   Maria    |  Germany   | 350   |
| 3    |   George   |  UK        | 750   |
| 4    |   Martin   |  Germany   | 500   |

This type of filtering is not based on a condition. It is based on internal row number of database.


Retrieve only three students with the Highest score.
First ORDER BY works
```sql
     SELECT *
     FROM students
     ORDER BY score DESC
```
And then at last TOP
```sql
     SELECT TOP 3 *
     FROM students
     ORDER BY score DESC
```
|  ID  |    Name    |   Country  | Score |
|------|------------|------------|-------|
| 2    |   John    |  USA        | 900   |
| 3    |   George   |  UK        | 750   |
| 4    |   Martin   |  Germany   | 500   |


Retrieve the lowest two students based on the score.
```sql
     SELECT TOP 2 *
     FROM students
     ORDER BY score ASC
```
|  ID  |    Name    |   Country  | Score |
|------|------------|------------|-------|
| 5    |   Peter    |  USA       | 100   |
| 1    |   Maria    |  Germany   | 350   |


| Order_ID | Customer_ID |  Order_Date | Sales |
|----------|-------------|-------------|-------|
| 1001     |  1          |'2025-01-11' | 35    |
| 1002     |  2          |'2025-04-05' | 15    |
| 1003     |  3          |'2025-06-18' | 20    |
| 1004     |  4          |'2025-08-31' | 10    |

Get the two most recent orders. Most recent basically means order by dates.

```sql
   SELECT TOP 2 *
   FROM  orders
   ORDER BY Order_Date DESC
```

| Order_ID | Customer_ID |  Order_Date | Sales |
|----------|-------------|-------------|-------|
| 1004     |  4          |'2025-08-31' | 10    |
| 1003     |  3          |'2025-06-18' | 20    |

This is how we limit our data using ```TOP```.
Till here, we learned basics, like all the Clauses.

## Coding Order Vs Execution Order

**Execution order in SQL Engine** in which order SQL run the clauses not in which we write the code.

```sql
      SELECT DISTINCT TOP 2
         Column1,
         Column2,
         SUM(Column3) as total
      FROM table_name
      WHERE Col1 = 10
      GROUP BY Column1, Column2
      HAVING SUM(Column2) > 100
      ORDER BY Column1         
```
Coding Order of Query:
| 1. SELECT     | 2. DISTINCT  | 3. TOP   |  4. FROM  | 5. WHERE   | 6. GROUP BY  | 7. HAVING   | ORDER BY   |
|---------------|--------------|----------|-----------|------------|--------------|-------------|------------|
|  1            |  2           |  3       |  4        |  5         |     6        |     7       |   8        |

Execution Order of Query :
| 1. FROM     | 2.  SELECT   | 3.  WHERE   | 4. GROUP BY   | 5. HAVING    | 6.  DISTINCT        | 7. ORDER BY   | 8. TOP     |   
|-------------|--------------|-------------|---------------|--------------|---------------------|---------------|------------|
| Priority 1  | Priority 2   | Priority 3  | Priority 4    | Priority 5   |     Priority 6      | Priority 7    | Priority 8 |

5 Different types of filter in SQL :
- Filters Coulmns using ```SELECT```
- Filters Duplicates using ```DISTINCT```
- Filters final Result Rows using ```TOP```
- Filters Rows Before Aggregation using ```WHERE```
- Filters Rows After Aggregation using ```HAVING```

> Coding Order is completely different than Execution Order in SQL.

<!--------------------------------------------------->

## Multi-Queries

We have seen, One Query -> One result
```sql
    SELECT *
    FROM table_name;
```
But In SQL, We can have multiple queries and multiple results in one go.
Multiple Query -> Multiple result in one go.

```sql
   SELECT *
   FROM students;

   SELECT *
   FROM orders;
```

> To run multiple Query Semicolon ```;``` is must in SQL.

You will set two Result Grids

|  ID  |    Name    |   Country  | Score |
|------|------------|------------|-------|
| 1    |   Maria    |  Germany   | 350   |
| 2    |   John     |  USA       | 900   |
| 3    |   George   |  UK        | 750   |
| 4    |   Martin   |  Germany   | 500   |
| 5    |   Peter    |  USA       | 100   |


| Order_ID | Customer_ID |  Order_Date | Sales |
|----------|-------------|-------------|-------|
| 1001     |  1          |'2025-01-11' | 35    |
| 1002     |  2          |'2025-04-05' | 15    |
| 1003     |  3          |'2025-06-18' | 20    |
| 1004     |  4          |'2025-08-31' | 10    |

<!------------------------------------->

## Static (Fixed) Values

What if we don't show data from the table rather show a static value in the Query like constant or fixed value.

```sql
     SELECT 1 as static_num;

     SELECT 'Hi ' as static_string;

     SELECT 3.14 as PI_Value;
```

```sql
    SELECT
        id,
        name,
        'New Name' as name_type
     FROM students;
```

<!----------------------------------->

## Highlight & Execute

Sometime you don't want to execute the whole thing, you want to execute only part of the query

```sql
      SELECT *
      FROM students
      WHERE country = 'Germany'
```
So, If we want to execute this query without using the WHERE filter, and keeping the WHERE filter as it is in query.
What we can do is just highlight the Query part you want to execute. It will give you result of only highlighted Query.

|  ID  |    Name    |   Country  | Score |
|------|------------|------------|-------|
| 1    |   Maria    |  Germany   | 350   |
| 2    |   John     |  USA       | 900   |
| 3    |   George   |  UK        | 750   |
| 4    |   Martin   |  Germany   | 500   |
| 5    |   Peter    |  USA       | 100   |

<!------------------------------------>

With that we have learned basics about SQL  Query, like basic components etc.
Next chapter we will learn about how to define the structure of our database. i.e. DDL.

<!-------Chapter 2. Query Data (SELECT)------------------>

[<-Introduction](https://github.com/pawansinghfromindia/SQL/blob/main/01_Introduction/intro.md) [DDL->]()
