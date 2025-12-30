<!---------Chapter 6. Combining Data----------------->
# Chapter 6. Combining Data

Here, We will learn How to combine data from multiple tables.

**```JOINS```**
- Basic Joins
  - ```INNER Join``` 
  - ```LEFT Join```
  - ```RIGHT Join```
  - ```FULL Join```
- Advanced Joins
  - ```LEFT Anti Join```
  - ```RIGHT Anti Join```
  - ```FULL Anti Join```
  - ```Cross Join```

- How to choose the right JOIN?
- How to JOIN multiple Tables?


**```SET``` Operators**
-  ```UNION```
-  ```UNION ALL```
-  ```EXCEPT``` OR ```MINUS```
-  ```INTERSECT```


## 6.1 Combining (joining) Data

<details>
  <summary> Joining the Data of tables using <b>JOINs</b> or <b>SET Operators</b>. </summary>

We have 2 tables Table A and Table B.

**TABLE A** i.e. (LEFT)
|  col1      |   col2     |   col3    |
|------------|------------|-----------|
|     1      |     2      |    3      |
|     4      |     5      |    6      |
|     7      |     8      |    9      |

**TABLE B** i.e. (RIGHT)
|  col1       |   col2     |   col3    |
|-------------|------------|-----------|
|     10      |     11     |    12     |
|     13      |     14     |    15     |
|     16      |     17     |    18     |

How to **combine** these 2 tables?

What do we want exactly? Do we want to combine ROWS or COLUMNS?

### 6.1a Combining data using ```JOINS```

If you want to combine the COLUMNS that means we're talking about joining tables, that's where we use ```JOINs``` <br/>
- Here what Join will do is take columns and rows of Table B (Right) and put side by side with the columns and rows of Table A (Left).
- We're combining the columns side by side using JOIN. Our table is going to be Wider column wise.
- In order to join, we have 4 types of JOIN
1. ```INNER``` Join
2. ```LEFT``` Join
3. ```RIGHT``` Join
4. ```FULL``` Join
- The requirement of combining tables using JOINs is We have to defined the key columns between the two tables. Ther number of columns in both tables can vary.
- In order to join tables requirement is we have to define the key columns between the two tables.

**Combine of Left TABLE A and Right TABLE B** using JOIN
|  col1      |   col2     |   col3    |  col4     |  col5     |    col6     |
|------------|------------|-----------|-----------|-----------|-------------|
|     1      |     2      |    3      |    10     |     11    |    12       |  
|     4      |     5      |    6      |    13     |     14    |    15       |
|     7      |     8      |    9      |    16     |     17    |    18       |

### 6.1b Combining data using ```SET``` Operators

If you want to combine the ROWS that means we're talking about joining tables, that's where we use ```SET``` operators. <br/>
- Here, Both of the tables having same columns, We just want to stack them.
- We're are combining the rows by using SET Operator. Our table is going to be longer row wise only.
- For combining the data using SET methods, we have 4 types-
1. ```UNION```
2. ```UNION ALL```
3. ```EXCEPT``` OR ```MINUS```
4. ```INTERSECTION```
- The requirement of combining tables using SET operators, the tables should have exact same number of columns. There is no need of key in order to combine table.

**Combine of Left TABLE A and Right TABLE B** using SET operator
|  col1      |   col2     |   col3    |
|------------|------------|-----------|
|     1      |     2      |    3      |
|     4      |     5      |    6      |
|     7      |     8      |    9      |
|     10     |     11     |    12     |
|     13     |     14     |    15     |
|     16     |     17     |    18     |

</details>
<!--------6.1--------->


## 6.2  SQL ```JOIN```

<details>
<summary> What is <b>SQL JOIN</b>?</summary>
Let's say we have 2 tables.
  
&nbsp;&nbsp; Table 1
|  Name      |   ID     |  
|------------|----------|
|   Maria    |     1    |
|   John     |     2    |
|   Georg    |     3    |
|   Martin   |     4    |

&nbsp;&nbsp; Table 2
|   ID  |   Country   |  
|-------|-------------|
|   1   |   Germany   |
|   2   |   USA       |
|   4   |   Germany   |
|   5   |   USA       |

Now, We want both of those information Names and Countries. <br/>
In order to query those 2 tables in one query, first we have to connect them. <br/>
In order to connect 2 tables, we need a key column that exists in both tables. <br/>
That common key column is ID column. <br/>
Now, once we connect those IDs, We will be able to query those tables together. And SQL starts matching those IDs and gives the result based on matched IDs. <br/>


|   ID  |  Name    |   Country   |  
|-------|----------|-------------|
|   1   |  Maria   |   Germany   |
|   2   |  John    |   USA       |
|   4   |  Martin  |   Germany   |  
</details>
<details>
  <summary> When to use <b>SQL JOINs</b>? </summary>

Why do we actually need SQL JOINs?

Reason 1. **Recombine Data** to see **Big Picture**<br/>

Usually in databases, the data about something could be stored in multiple tables based on requirements. <br/>
Like Data about students can be stored in multiple tables.
- Students

|   ID  |  Name    |   Country   |  
|-------|----------|-------------|
|   1   |  Maria   |   Germany   |
|   2   |  John    |   USA       |
|   4   |  Martin  |   Germany   |  

- Addresses

|   ID  | Post_Code  |  
|-------|-------------|
|   1   |   10161     |
|   2   |   20005     |
|   4   |   64321     |  

- Reviews

|   Order_ID  |  Product |   Comments  |  
|-------------|----------|-------------|
|   101       |  Book1   |   'abc'     |
|   112       |  Book3   |   'xyz'     |
|   221       |  Book5   |   'pqr'     |  

- Orders

|   Student_ID  |  Order_ID   | Order_Date     |  Product_ID  | 
|---------------|-------------|----------------|--------------|
|   1           |  101        | '2026-01-15'   |  A1001B      |
|   2           |  112        | '2026-01-17'   |  A3001B      |
|   4           |  221        | '2026-01-21'   |  A5001B      |

Now, If we want to see all the data about students in one result. (Complete Big Picture about Students)
So, What we can do is connect those 4 tables using the SQL JOIN.
Once that's done, In one query, we will able to combine the results in one table. (All details about Students)

Reason 2. **Data Enrichment** Getting **Extra Info** <br/>
We have a master table that contains all the data, Now we want few extra information from another table that may be Reference Table.
In order to get extra data information, we use JOIN.
Getting extra data for main master table from source reference table.

- Master Table : Students

|   ID  |  Name    |   Country   |  
|-------|----------|-------------|
|   1   |  Maria   |   Germany   |
|   2   |  John    |   USA       |
|   3   |  Peter   |   UK        |
|   4   |  Martin  |   Germany   |  

- Reference Table : Zip Codes

|   ID  | Zip_Code    |  
|-------|-------------|
|   1   |   10001     |
|   2   |   20005     |
|   3   |   30005     |
|   4   |   60002     |  

- Extra Info : Enhanced Students Table

|   ID  |  Name    |   Country   |  Zip_Code  |
|-------|----------|-------------|------------|
|   1   |  Maria   |   Germany   |  10001     |
|   2   |  John    |   USA       |  20005     |
|   4   |  Martin  |   Germany   |  60002     |

Reason 3. **Check for Existence** of one table data in another table if it exists or not. <br/>
**Filtering** the data based on JOIN.

- Students : Data has to be checked

|   ID  |  Name    |   Country   |  
|-------|----------|-------------|
|   1   |  Maria   |   Germany   |
|   2   |  John    |   USA       |
|   3   |  Peter   |   UK        |
|   4   |  Martin  |   Germany   |  

- Orders : Lookup Table

|   Student_ID  |  Order_ID   | Order_Date     |  Product_ID  | 
|---------------|-------------|----------------|--------------|
|   1           |  101        | '2026-01-15'   |  A1001B      |
|   2           |  112        | '2026-01-17'   |  A3001B      |
|   4           |  221        | '2026-01-21'   |  A5001B      |

- Students : Filters the data based on JOIN whether Students orders or the students which didn't order anything.

|   ID  |  Name    |   Country   |  
|-------|----------|-------------|
|   1   |  Maria   |   Germany   |
|   2   |  John    |   USA       |
|   4   |  Martin  |   Germany   | 

</details>

<details>
   <summary> <b>JOIN Types</b></summary>

There are a lot of different possibility on How to JOIN tables.
Table A(Left), Table B(Right) are 2 tables depicted through 2 circle.
If we combine these 2 tables, Both Circles overlaped, there are 3 possibilities :
- through only overlap area, we will get common data of two table : **Matching Data**
- considered all the data of Left table which do also contains the data of right table as those common data of two tables : **All Data**
- considered all the data of left table which do not match or common in right table : **UnMatching Data**

|                            Basic Joins                                              |                       Advanced Join                                                    |
|-------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| No Join     :  Two Results seperately, No Common data between 2 tables              | Cross Join : All the data of left table cross multiply to all the data of right tables | 
| Inner Join  :  Only Matching Common data of both Left and Right Tables              | Full Anti Join : Only Unmatching Data                                                  |
| Left Join   :  All Rows(Everything) of Left table A + Only Matching of Right Table B| Left Anti Join : Only Unmatching Rows of left table A + Nothing from right table B     |
| Right Join  :  All Rows(Everything) of Right table B + Only Matching of Left Table A| Right Anti Join : Only unmatching Rows of right table B + Nothing from left table A    |
| Full Join   :  Everything of left & right, includes matching and unmatching ata     |                                                                                        |

**NO JOIN**

Returns Data from Tables without combining them.

All Rows of Table A + All Rows of Table B

Two Results No need to combine.

Of course, since we're not combining the data, so there will be no ```JOIN``` in the syntax.

Syntax :
```sql
    SELECT *
    FROM A;

    SELECT *
    FROM B;
```
```sql
--Task 1 : Retrieve all the data from students and orders as seperate results.

      SELECT *
      FROM students;

      SELECT *
      FROM orders;

--Two Results and data is not combined at all.
```

</details>

**Basic Joins** ```INNER Join```  ```LEFT Join``` ```RIGHT Join``` ```FULL Join```

<details>
  <summary> <b> INNER JOIN </b> </summary>
  
-Returns only the matching Rows from both tables. means, We will see the output only matching rows between Table A and Table B. <br/>
-Only common Data <br/>
-Since we're combining the data of two tables, so we will use a keyword called ```JOIN``` in order to take 2 tables data in one query. <br/>
-Since we have different types of JOIN in SQL, we can specify the type of JOIN before the keyword ```JOIN```. <br/>

> If you don't specify anything the default type is ```INNER JOIN```. <br/>

> Best practice is always mention the type of ```JOIN```, bcuz in project may be not everyone aware of defaults. <br/>

-Now We have to specify the conditions like key columns (common columns) on which data is combining for that we have another keyword ```ON```

Syntax :
```sql
    SELECT *
    FROM table1_name
    [TYPE] JOIN table2_name
    ON <condition>

    SELECT *
    FROM A
    INNER JOIN B
    ON A.key = B.key
```

> While you do JOIN, you must have to consider the order of tables in your query.

> But in ```INNER JOIN```, the order of the tables doesn't really matter bcuz here we will get only common row bwteen two tables.

```sql
     SELECT *
     FROM A
     INNER JOIN B
     ON A.key = B.key

     --Order of tables doesn't matter in case of INNER JOIN

     SELECT *
     FROM B
     INNER JOIN A
     ON A.key = B.key
```
How SQL Engine runs INNER JOIN internally is first load the table A and table B, then check the condition, if condition like key column ID is matched then only the data is added into result else not.
So first ID which is a column of row1 of table A, will going to check with id which is also key column of row1 in table B, if matched the added to result, if not next is same id row1 table A will be checked with id of row2 of table B and the row3 row4...and so on.

```sql
--Task 1 : Get all students along with their orders, but only for the students who have placed the order.

      SELECT *
      FROM students
      INNER JOIN orders
      ON id = student_id

-- This is not a good practice, here SELECT * takes all the columns of both the tables.
-- Select only those columns which are required for you.

     SELECT
            id,
            name,
            order_id,
            order_date
      FROM students
      INNER JOIN orders
      ON id = student_id

--Column name Ambiguity : Add the table name before the column to avoid confusion in JOIN with same-named columns.
     SELECT
            students.id,
            students.name,
            orders.order_id,
            orders.order_date
      FROM students
      INNER JOIN orders
      ON students.id = orders.student_id

-- Annoying to add table_name before each column in JOIN in that as we can assign ALIAS to the table name
      SELECT
            s.id,
            s.name,
            o.order_id,
            o.order_date
      FROM students AS s
      INNER JOIN orders AS o
      ON s.id = o.student_id

```
> We can use ```INNER JOIN``` either for recombine data for big picture of multiple tables or for filtering purposes (check existence of rows) 
</details>

<details>
    <summary> <b>LEFT JOIN</b> </summary>

-Returns **all the Rows(Everything) from the left table** and **only the matching from the right table**. <br/>
-Here, Left table has more priority, **Left table is Primary source of data**. We can't miss anything from Left table. <br/>
-But, **Right table is Secondary source of data**. We're joining it **only to get additional data**. <br/>
-We want the data only that matched to left table.

Syntax:
```sql
    SELECT *
    FROM A
    LEFT JOIN B
    ON A.key = B.key
```

> Order of the Table is very important in ```LEFT JOIN```

```sql
-- Task : Get all students along with their orders, including those without orders.
-- Here order is only for additional information.

    SELECT
        s.id,
        s.name,
        o.order_id,
        o.order_date
    FROM students AS s
    LEFT JOIN orders AS o
    ON s.id = o.student_id   

-- NULL will be assigned to students which didn't have order for the right table columns order_id,order_date.

-- If you switch the table order, you will get completely different result. 
```

How SQL Engine runs LEFT JOIN internally is first without checking the condition It will put all the data(mentioned columns of row1) from left table and then it will check condition if condition is matched it takes the data (metioned columns of row 1) from right table. if conditioned is not matched It will put the value NULL on the those rows.

Students

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |
|  3   | Georg     | USA       | 750   |
|  4   | Martin    | Germany   | 500   |
|  5   | Peter     | USA       | 100   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |
| 1004      |'2026-01-31'| 10    |  6         |

Results

|  id  |  name     | order_id  | order_date   |
|------|-----------|-----------|--------------|
|  1   | Maria     | 1001      | '2026-01-01' |
|  2   | John      | 1002      | '2026-01-05' |
|  3   | Georg     | 1003      | '2026-01-11' |
|  4   | Martin    | NULL      |    NULL      |
|  5   | Peter     | NULL      |    NULL      |

> We can use LEFT JOIN in order to combine the data to see big picture as well as to get extra info (Data Enrichment). Ans with twist we can use it to filter as well.

</details>

<details>
  <summary> <b> RIGHT JOIN </b> </summary>
  
```RIGHT JOIN``` is Exact opposite of ```LEFT JOIN``` <br/>
Returns **all the Rows from right table** and **only matching from left table**. <br/>
Here, main table is right table B, focus is on right table B, but from left table B we will get only matching data.
Here, primary table is right table B, which is main source of data, and left table is secondary as we're joining it just to get additional data.

Syntax:
```sql
    SELECT *
    FROM A
    RIGHT JOIN B
    ON A.key = B.key
```

```sql
--Task : Get all students along with their orders, including orders without matching students. 
      SELECT 
         s.id,
         s.name,
         o.order_id,
         o.order_date
      FROM students AS s
      RIGHT JOIN orders AS o
      ON s.id = o.student_id

-- Solve the same task using LEFT JOIN
      SELECT
         s.id,
         s.name,
         o.order_id,
         o.order_date
      FROM orders AS o
      LEFT JOIN students AS s
      ON s.id = o.student_id
```

Students

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |
|  3   | Georg     | USA       | 750   |
|  4   | Martin    | Germany   | 500   |
|  5   | Peter     | USA       | 100   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |
| 1004      |'2026-01-31'| 10    |  6         |

Results

|  id  |  name     | order_id  | order_date   |
|------|-----------|-----------|--------------|
|  1   | Maria     | 1001      | '2026-01-01' |
|  2   | John      | 1002      | '2026-01-05' |
|  3   | Georg     | 1003      | '2026-01-11' |
| NULL | NULL      | 1004      | '2026-01-31' |

> If you just switch the table order in LEFT JOIN(B,A), you will get exact result of RIGHT JOIN of (A,B). That's why we rarely used RIGHT JOIN. Now you know why LEFT JOIN is way more famous than RIGHT JOIN.

</details>

<details>
  <summary> <b> FULL JOIN </b> </summary>

-Returns everything all the Rows from both tables. <br/>
-The matching, unmatching all the data from left and right table. <br/>
-Here, We the keyword for FULL JOIN is ```FULL JOIN```

The order of table is not important in FULL JOIN. So there is no main(primary) or secondary table.
Both of the tables are important.

Syntax
```sql
     SELECT *
     FROM A
     FULL JOIN B
     ON A.key = B.key

     -- Get exact same result

     SELECT *
     FROM B
     FULL JOIN A
     ON A.key = B.key
```
```sql
--Task : get all the students and all orders even if there is no match

        SELECT
             s.id,
             s.name,
             o.order_id,
             o.order_date
         FROM students AS s
         FULL JOIN orders AS o
         ON s.id = o.student_id
```
Students

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |
|  3   | Georg     | USA       | 750   |
|  4   | Martin    | Germany   | 500   |
|  5   | Peter     | USA       | 100   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |
| 1004      |'2026-01-31'| 10    |  6         |

Results

|  id  |  name     | order_id  | order_date   |
|------|-----------|-----------|--------------|
|  1   | Maria     | 1001      | '2026-01-01' |
|  2   | John      | 1002      | '2026-01-05' |
|  3   | Georg     | 1003      | '2026-01-11' |
|  4   | Martin    | NULL      |   NULL       |
|  5   | Peter     | NULL      |   NULL       |
| NULL | NULL      | 1004      | '2026-01-31' |

- If there is matching data we get it, if not we gets NULL.

How SQL Engine executes FULL JOIN internally is :
It identifies the columns that we want to see in results so id, name from students table, order_id and order_date from orders table
Now since it started with students (left). It will take everything from left table and present it into output.
Now start searches for matching from the orders(right) table.
So first student_id=1 is matched so in includes the data into result similiarly for id=2,id=3, id=4 not matched it put NULL to order_id and order_dates then id=5 again not matched put NULL to order_id and order_date.
Now SQL is not stop here, otherwise till here it is LEFT JOIN Effect
Now SQL starts looking at Orders(Right) to find any order that is not in the output.
So, It sees order_id=1001 which is present in output skip it then order_id=1002 then order_id=1003 then order_id=1004 which is not present in output so SQL will get the data of order_id=1004 where order_id=1003 and order_date='2026-01-31' and put it in output.
and shows NULL for id and name column of left table.

> We can use FULL JOIN in order to Recombine data from multiple tables if you don't want to miss anything from all tables whether matching data or unmatching data. Usually don't used in Data Enrichment. But in the data filtering we use full join to check exitence with little twist.

Now we have learned basic JOINs ```INNER JOIN```, ```LEFT JOIN```, ```RIGHT JOIN``` and ```FULL JOIN```. These are the classical join on how to combine two tables.

</details>

**Advanced Joins** ```CROSS Join```, ```LEFT Anti Join```,  ```RIGHT Anti Join``` and ```FULL Anti Join```  


<details>
  <summary> <b>LEFT ANTI JOIN</b> </summary>

-Return Rows from the left table that has NO MATCH in the right table. <br/>
-Only Unmatching Rows from A + Nothing from B, So there is matching data we don't see. <br/>
-The only source of your data left table (Primary:Main source of data) and we're just doing a check to filter data from right table (Lookup or Filter : Not for data, just a filter)

> Since, we don't have any special type called ```LEFT ANTI JOIN``` but still can create this effect using ```LEFT JOIN``` and to remove the common data we use ```WHERE``` filter that is what we call ```LEFT ANTI JOIN```

> The order of Tables is very important.

Syntax :
```sql
      SELECT *
      FROM A
      LEFT JOIN B
      ON A.key = B.key
      WHERE B.key IS NULL; -- As we have to exclude common(matched) data
```

```sql
   -- Task : Get all the students who haven't place any order.
   -- There are different ways to solve this.

   SELECT
        s.id,
        s.name,
        o.order_id,
        o.order_date
   FROM students AS s
   LEFT JOIN orders AS o
   ON s.id = o.student_id
   WHERE o.student_id IS NULL;
```

Students

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |
|  3   | Georg     | USA       | 750   |
|  4   | Martin    | Germany   | 500   |
|  5   | Peter     | USA       | 100   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |
| 1004      |'2026-01-31'| 10    |  6         |

Results

|  id  |  name     | order_id  | order_date   |
|------|-----------|-----------|--------------|
|  4   | Martin    | NULL      |   NULL       |
|  5   | Peter     | NULL      |   NULL       |

> We use LEFT ANTI JOIN (LEFT JOIN + WHERE) for filtering the data to check data existence.

</details>

<details>
  <summary> <b>RIGHT ANTI JOIN</b> </summary>

-RIGHT ANTI JOIN is exact opposite of LEFT ANTI JOIN. Switch the table order LEFT ANTI JOIN (B,A) == RIGHT ANTI JOIN(A,B) <br/>
-Nothing from the left table A + Only unmatching Rows of right table B <br/>
-Returns Rows from the right table that has No match in left table. <br/>
-Here, right table (Primary : Main source of data) is important, and left table (Filter : Not for data, just a filter) is just for filtering the data (Lookup)

> Since, we don't have any special type called ```RIGHT ANTI JOIN``` but still can create this effect using ```RIGHT JOIN``` and to remove the common data we use ```WHERE``` filter.

> The order of Tables is very important.

> The order of Tables is very important.

Syntax :
```sql
      SELECT *
      FROM A
      RIGHT JOIN B
      ON A.key = B.key
      WHERE A.key IS NULL; -- As we have to exclude common(matched) data
```

```sql
   -- Task : Get all the orders without matching students. (Basically those orders which don't have student)
   -- There are different ways to solve this.

   SELECT
        s.id,
        s.name,
        o.order_id,
        o.order_date
   FROM students AS s
   RIGHT JOIN orders AS o
   ON s.id = o.student_id
   WHERE s.id IS NULL;

-- Solve this task using Without using RIGHT JOIN.
-- Here we can do it using LEFT JOIN

    SELECT
       s.id,
       s.name,
       o.order_id,
       o.order_date
    FROM orders AS o
    LEFT JOIN students AS s
    ON o.student_id = s.id
    WHERE s.id IS NULL
```

Students

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |
|  3   | Georg     | USA       | 750   |
|  4   | Martin    | Germany   | 500   |
|  5   | Peter     | USA       | 100   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |
| 1004      |'2026-01-31'| 10    |  6         |

Results

|  id  |  name     | order_id  | order_date   |
|------|-----------|-----------|--------------|
| NULL |  NULL     | 1004      | '2026-01-31' |

</details>

<details>
  <summary> <b>FULL ANTI JOIN</b> </summary>

-Only unmatching Rows of left table A + only unmatching Rows of right table B <br/>
-Returns Only Rows that don't match in either table. <br/>
-FULL ANTI JOIN (Only Unmatching data) i.e. Exactly opposite of INNER JOIN (Only matching data)

> Since, we don't have any special type called ```FULL ANTI JOIN``` but still can create this effect using ```FULL JOIN``` and to remove the common data we use ```WHERE``` filter.

> The order of Tables is very important.

Syntax :
```sql
      SELECT *
      FROM A
      FULL JOIN B
      ON A.key = B.key
      WHERE A.key IS NULL OR B.Key IS NULL; 
```

```sql
   -- Task : Find students without orders and orders without customers
   -- There are different ways to solve this.

   SELECT
        s.id,
        s.name,
        o.order_id,
        o.order_date
   FROM students AS s
   FULL JOIN orders AS o
   ON s.id = o.student_id
   WHERE s.id IS NULL OR o.student_id IS NULL;
```

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |
|  3   | Georg     | USA       | 750   |
|  4   | Martin    | Germany   | 500   |
|  5   | Peter     | USA       | 100   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |
| 1004      |'2026-01-31'| 10    |  6         |

Results

|  id  |  name     | order_id  | order_date   |
|------|-----------|-----------|--------------|
|  4   | Martin    | NULL      |   NULL       |
|  5   | Peter     | NULL      |   NULL       |
| NULL |  NULL     | 1004      | '2026-01-31' |

> We use FULL ANTI JOIN in order to check existence of data (filtering).

</details>

<details>
  <summary> <b> CROSS JOIN </b> </summary>
  
- ```CROSS JOIN``` is totally different than all other joins we have seen before.
- ```CROSS JOIN``` Combines Every Row from Left table A with Every Row from the right table B
- Using ```CROSS JOIN```, we will be able to see All Possible Combination from both table, called **Cartersian Join**
- If we have 2 rows in left table A and 3 rows in right table B then we will get 6 combinations. Like
  -  A-row1 to B-row1, A-row1 to B-row2, A-row1 to B-row3
  -  A-row2 to B-row1, A-row2 to B-row2, A-row2 to B-row3

> Be careful, when you're using ```CROSS JOIN```, you will get huge number of records like combination = Rows in table A * Rows in table B which cause database slow to search records.

Syntax :
```sql
      SELECT *
      FROM A
      CROSS JOIN B
```

> Note : Since, we don't care at all about data is matching or not, just want to see all possible combination that's why no need to put any condition. 

> The order of table doesn't matter.
</details>


## **Use Cases of ```JOINs```**

|    Usecase                             |    ```JOIN```                                                   |
|----------------------------------------|-----------------------------------------------------------------|
| 1. Recombine Data (to see Big Picture) |    ```INNER```, ```LEFT```, ```FULL```                          |
| 2. Data Enrichment (to get Extra Info) |    ```LEFT```, ```FULL```                                       |
| 3. Check Existence of Data (Filtering) |    ```INNER```, ```LEFT``` + ```WHERE```, ```FULL```+```WHERE```|


<details>
  <summary> <b>CHALLENGE : Get all students along with thier orders, but only for students who have placed an order (Without using INNER JOIN).</b> </summary>

- In order to solve anything, Don't rush. Go step by step. Since, here we can't use INNER JOIN so first thing we can do is get all data from students using LEFT and then apply filter to get those records which must have orders (How to check this? : In orders your key column (student_id) must be not NULL then only you can say the order is of any student)

```sql
  --CHALLENGE : Get all students along with thier orders, but only for students who have placed an order (Without using INNER JOIN).
       SELECT
            s.id,
            s.name,
            o.order_id,
            o.order_date
       FROM students AS s
       LEFT JOIN orders AS o
       ON s.id = o.student_id
       WHERE o.student_id IS NOT NULL;
```
  
</details>



<!------------------------------>


## 6.3 SQL ```SET Operators```

<details>
<summary> What are SQL <b>SET Operators</b>? </summary>
  
</details>

<!------------------------------>
<!---------Chapter 6. Combining Data----------------->

