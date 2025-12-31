<!---------Chapter 6. Combining Data----------------->
# Chapter 6. Combining Data

Here, We will learn How to combine data from multiple tables.

**```JOINS```**
- **Basic JOINs** : ```INNER JOIN```, ```LEFT JOIN```, ```RIGHT JOIN```, ```FULL JOIN```
- **Advanced JOINs** : ```LEFT ANTI JOIN```, ```RIGHT ANTI JOIN```, ```FULL ANTI JOIN``` and ```CROSS JOIN```

- How to choose the right JOIN?
- How to JOIN multiple Tables?


**```SET``` Operators**
-  ```UNION```
-  ```UNION ALL```
-  ```EXCEPT``` OR ```MINUS```
-  ```INTERSECT```


## 6.1 Combining (joining) the Data

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

**If you want to combine the COLUMNS that means we're talking about joining tables, that's where we use ```JOINs```** <br/>

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

> **If you want to combine the COLUMNs that means we're talking about joining tables, that's where we use ```JOINs```** <br/>

> **If you want to combine the ROWs then we're going to use ```SET OPERATORs```** <br/>

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

### 🔻 **Basic JOINs** ```INNER Join```  ```LEFT Join``` ```RIGHT Join``` ```FULL Join```

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

### 🔻 **Advanced JOINs** : ```LEFT ANTI JOIN```,  ```RIGHT ANTI JOIN```, ```FULL ANTI JOIN``` and ```CROSS JOIN```  


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
| NULL |  NULL     | 1004      | '2026-01-31' |

> We use FULL ANTI JOIN in order to check existence of data (filtering).

</details>

<details>
  <summary> <b>CHALLENGE</b> : Get all students along with their orders, but only for students who have placed an order (Without using INNER JOIN). </summary>

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

```sql
    SELECT *
    FROM students
    CROSS JOIN orders
```

Students

|  id  |  name     | country   | score |
|------|-----------|-----------|-------|
|  1   | Maria     | Germany   | 350   |
|  2   | John      | USA       | 900   |

Orders

|  order_id | order_date | sales | student_id |
|-----------|------------|-------|------------|
| 1001      |'2026-01-01'| 35    |  1         |
| 1002      |'2026-01-05'| 15    |  2         |
| 1003      |'2026-01-11'| 20    |  3         |

Results :

|  id  |  name    | country   | score |  order_id | order_date | sales | student_id |
|------|----------|-----------|-------|-----------|------------|-------|------------|
|  1   | Maria    | Germany   | 350   | 1001      |'2026-01-01'| 35    |  1         |
|  1   | Maria    | Germany   | 350   | 1002      |'2026-01-05'| 15    |  2         |
|  1   | Maria    | Germany   | 350   | 1003      |'2026-01-11'| 20    |  3         |
|  2   | John     | USA       | 900   | 1001      |'2026-01-01'| 35    |  1         |
|  2   | John     | USA       | 900   | 1002      |'2026-01-05'| 15    |  2         |
|  2   | John     | USA       | 900   | 1003      |'2026-01-11'| 20    |  3         |

</details>

<details>
  <summary><b>Use Cases of JOINs</b> : Recombine Data, Data Enrichment & Data Filtering </summary>

|    Usecase                             |    ```JOIN```                                                   |
|----------------------------------------|-----------------------------------------------------------------|
| 1. Recombine Data (to see Big Picture) |    ```INNER```, ```LEFT```, ```FULL```                          |
| 2. Data Enrichment (to get Extra Info) |    ```LEFT```, ```FULL```                                       |
| 3. Check Existence of Data (Filtering) |    ```INNER```, ```LEFT``` + ```WHERE```, ```FULL```+```WHERE```|

</details>

 ### 🔻 How to choose Between JOIN Types?
<details>
  <summary><b> Decision Tree to choose the right JOIN? </b></summary>
- If you want to see results **Only Matching Data** between 2 tables -> **```INNER JOIN```**
- If you want to see everything **All Rows** after joining 2 tables then we take different path :
  - If you want to all data from one table from **one side important** like **Main/Master table** -> **```LEFT JOIN```**
  - If you want to see all data from both tables, **Both sides are important** -> **```FULL JOIN```**
- If you want to see **Only Unmatching Data** there are two path :
  - If you want **Unmatching data from only one table**, **one side is important (Master table)** -> **```LEFT ANTI JOIN```**
  - If you want **Unmatching data from both tables**, **Both sides are important** -> **```FULL ANTI JOIN```**                  

> We don't use ```RIGHT JOIN``` at all as we have alternative to this which is ```LEFT JOIN```.

> ```LEFT JOIN``` is most used and favorite one. 
</details>

<!------------------------------>

### 🔻 How to join multiple tables?

<details>
  <summary> <b> Multi-Table JOIN and ER Model </b> </summary>

**Multi-Table JOIN** 

While we do Data Analysis, We always have a **starting point** like a topic about Customer, you have a **Master Table** (analysis always starts from here) which is your main table. <br/>
Now **Master table** Table A is not enough for analysis, need some extra data from anothers table (**Secondary Tables**) Table B. <br/>
So, In order to get extra info We can connect master table A to table B using  ```LEFT JOIN```. <br/>
Similar way another extra data is required from Table C. <br/>
Again We join our master table A to another table C using ```LEFT JOIN``` <br/>
And so on... <br/>
If you want to see only matching data then we can control everything using ```WHERE``` by putting condtion to filter data. <br/>

```sql
    SELECT *
    FROM A
    LEFT JOIN B ON A.key = B.key
    LEFT JOIN C ON A.key = C.key
    LEFT JOIN D ON A.key = D.key
    WHERE <condtion>

--Table A is master table, which is a primary source of data
--Table B,C,D are secondary tables is used for getting extra info
--WHERE controls what to keep
```
- We learned  ```JOIN``` in order to combine multiple tables to get a complete big picture about any thing & the data is stored in a table.
  So either we can start with main table and ```LEFT JOIN``` other tables or there is no main table, all the tables are equally important then in that case we can join all those tables using   ```INNER JOIN``` if you're interested in only matching data. There you will get only overlapping between all multiple tables.

```sql
   SELECT *
    FROM A
    INNER JOIN B ON A.key = B.key
    INNER JOIN C ON A.key = C.key
    INNER JOIN D ON A.key = D.key
```

```sql
    --Task : Using Sales DB, retrieve a list of all orders, along with the related customer, product and employee details.
    --For each order, display :
    --  - Order ID
    --  - Customer name
    --  - Product name
    --  - Sales Amount
    --  - Product price
    --  - Salesperson's name
    -- Hint : orders (main table) rest are secondary table

    USE SalesDB

    SELECT
         o.orders,
         o.sales,
         c.name AS customer_name,
         p.product AS product_name,
         p.price,
         e.name AS employee_name
    FROM Sales.orders AS o
    LEFT JOIN Sales.customers AS c
    ON o.customer_id = c.customer_id
    LEFT JOIN Sales.products AS p
    ON o.product_id = p.product_id
    LEFT JOIN Sales.employees AS e
    ON o.SalesPerson_Id = p.employee_id
```


**Entity-Relationship Model (ER Model)** <br/>
> 1. If we work on big project, we are going to have lot of tables. <br/>
> 2. Out of 100s tables, Exloring each of them is really hard. <br/>
> 3. So, Usually a good project / database has **Entity-Relationship Model (ER Model)**. <br/>
> 4. Here, you can find easily the relationship between the tables (**Master table** to **Secondary tables**) which is very important specially if you want to ```JOIN``` tables. <br/>
> 5. We have an orders(Master table) and other secondary tables (customers, employees, products) <br/>
> 6. Inside orders table, Order_Id column is **```Primary Key (PK)```** and there is a column Customer_Id which is a **```Foreign Key (FK)```** to the Customer_Id of customers table(secondary table). <br/>
> 7. So that means If we want to connect orders table to customers table, we have to use that customer_id. <br/>
> This pictorial representation of this in a diagram is known as **ER Diagram**

|Orders  |     Table      |
|--------|----------------|
|**PK**  | **Order_Id**   |
| FK     | Product_Id     |
| FK     | Customer_Id    |
| FK     | SalesPerson_Id |
|        | Order_Date     |
|        | Order_Status   |
|        | Quantity       |
|        | Sales          |

|Products |     Table      |
|---------|----------------|
|**PK**   | **Product_Id** |
|         |   Product      |
|         |   Category     |
|         |   Price        |

|Customers |     Table       |
|----------|-----------------|
|**PK**    | **Customer_Id** |
|          |   Name          |
|          |   Country       |
|          |   Score         |

| Employees |     Table      |
|----------|-----------------|
|**PK**    | **Employee_Id** |
|          |   Name          |
|          |   Department    |
|          |   Salary        |
|          |   Manager_ID    |

> The name of Foreign Key of master table to Primary Key of secondary table not necessarily same, their data must me matched. Hence SalesPerson_Id which is a FK of Orders is conected to PK of Employees.

</details>
<!------------------------------------->

## 6.3 SQL ```SET Operators```

**How to combine ROWs from multiple tables?** <br/> 

We have learned in order to combine 2 tables. We have 2 methods. 
1. To combine COLUMNs use ```JOIN``` and
2. To combine ROWs use ```SET Operators```

> **4 Types of SET Operators : ```UNION```, ```UNION ALL```, ```MINUS```/```EXCEPT``` and ```INTERSECT```** <br/>

<details>
<summary> Syntax & Rules of <b> SET Operators</b>? </summary>

**Syntax of SET Operators** :

```sql
   SELECT
       first_name,
       last_name
   FROM customers
   JOIN Clause
   WHERE Clause
   GROUP BY Clause

   UNION

    SELECT
       first_name,
       last_name
   FROM employees
   JOIN Clause
   WHERE Clause
   GROUP BY Clause

   ORDER BY first_name    --ORDER BY can be used only at the end to sort the final result.
```

**Rules of SET Operators** :

**Rule 1.** SET Operator can be use almost in all clauses ( ```WHERE```, ```JOIN```, ```GROUP BY```, ```HAVING```) except ***```ORDER BY```***. Because ```ORDER BY``` is allowed only once at the end of the entire query.

**Rule 2.** The **number of columns** in SELECT of each query must be the same.

**Rule 3.** The **data types of columns** in each query must be campatible (matching).

**Rule 4.** The **order of columns** in each query must be same.

**Rule 5.** The **name of columns** in the output result set are determined by the column name specified in the first query. <br/>
-Meaning, First Query is responsible for naming the columns in output.  <br/>
-So, If we want to change the column names in output result, we always do ALIAS in first query that's enough, No need to do ALIAS in each query bcuz SQL ingnores it.

**Rule 6.** Even if all rules are met and SQL shows no errors, the result may be incorrect.  <br/>
-Bcuz Inccorect column selection leads to inaccurate results.

> SQL doesn't know to content of data, SQL does only mapping of number of columns and its datatype.
  
</details>
<!------------------------------>

<details>
  <summary> <b> UNION </b> </summary>

- Returns all distincts rows from both queries.
- Remove duplicates rows from the result. 
- Here, Order of the query does not effect the final result.

```sql
  Task : Combine the data from  employees and customers into one table.

  SELECT
       first_name,
       last_name
  FROM Sales.customers 
  UNION
  SELECT
       first_name,
       last_name
  FROM Sales.employees 

```

How SQL do combine the data internally using UNION ? <br/>
-First step SQL does is go & takes column_names from first query to the results, then takes all the rows from first query table and checks whether there is any duplicates, if found deletes it, then next step is start adding from the second query table very carefully without generating any duplicates. if row is already present in result, it ignores it and then next row, if it doesn't exist in result add it to result. 

Employees

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Vikash   | Dubey   |
| Vipin    | Patel   |

Customers

|first_name|last_name|
|----------|---------|
| Ganesh   | Bhatt   |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Navin    | Patil   |
| Guddu    | Pandit  |

Output Result :

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Vikash   | Dubey   |
| Vipin    | Patel   |
| Ganesh   | Bhatt   |
| Navin    | Patil   |
| Guddu    | Pandit  |

</details>
<!----------------------------------------->

<details>
  <summary> <b> UNION ALL </b> </summary>

- Returns all rows from both queries, including duplicates.
  
- Here, Order of the query does not effect the final result.

> ```UNION ALL``` is only SET Operator that does not remove any duplicates rows. It shows all the rows as it is.

- ```UNION ALL``` vs ```UNION```

> ```UNION ALL``` is generally way faster than ```UNION```. That's bcuz ```UNION ALL``` doesn't perform additional steps like removing duplicates what ```UNION``` does.

> If you know that there are no duplicates, use ```UNION ALL```. It gives you better performance.


```sql
  Task : Combine the data from  employees and customers into one table including duplicates.

  SELECT
       first_name,
       last_name
  FROM Sales.customers 
  UNION ALL
  SELECT
       first_name,
       last_name
  FROM Sales.employees 
```

How SQL do combine the data internally using ```UNION ALL```? <br/>
-First step SQL does is go & takes column_names from first query to the results, and then SQL will take all rows of first query table and put it in output result without checking anything.
-Now SQL will take all the rows of second query table and appended it to output results.

Employees

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Vikash   | Dubey   |
| Vipin    | Patel   |

Customers

|first_name|last_name|
|----------|---------|
| Ganesh   | Bhatt   |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Navin    | Patil   |
| Guddu    | Pandit  |

Output Result :

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Vikash   | Dubey   |
| Vipin    | Patel   |
| Ganesh   | Bhatt   |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Navin    | Patil   |
| Guddu    | Pandit  |
  
</details>
<!----------------------------------------->

<details>
  <summary> <b> EXCEPT </b> or <b> MINUS </b> </summary>

```EXCEPT``` and ```MINUS``` both are same used in different different server.

- Returns all distinct rows from the first query that are not found in the second query.
  
- It is the only ```EXCEPT``` or ```MINUS``` where the order of queries affect the final result.

```sql
   -- Task : Find the employees who are not customers at the same time.

          SELECT
                 first_name,
                 last_name
          FROM Sales.customers 
           EXCEPT                                  -- EXCEPT == MINUS
          SELECT
                 first_name,
                 last_name
          FROM Sales.employees 
```

How SQL do combine the data internally using ```EXCEPT```? <br/>
-First step SQL does is go & takes column_names from first query to the results, now SQL will going to put  row1 of first query table data in output and then SQL is going to use second query table only as a check. So SQL will not put any row from second query table. and again row2, row3,..... of first query table data.

Employees

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Vikash   | Dubey   |
| Vipin    | Patel   |

Customers

|first_name|last_name|
|----------|---------|
| Ganesh   | Bhatt   |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Navin    | Patil   |
| Guddu    | Pandit  |

Output Result of Employees EXCEPT Customers:

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Vikash   | Dubey   |
| Vipin    | Patel   |

Output Result after switching the order of Table Customers EXCEPT Employees:

|first_name|last_name|
|----------|---------|
| Ganesh   | Bhatt   |
| Navin    | Patil   |
| Guddu    | Pandit  |
    
</details>
<!----------------------------------------->

<details>
  <summary> <b> INTERSECT </b> </summary>

- Returns only rows that are common in both queries.
- It is similar to the ```INNER JOIN``` but row wise.
- It also removes duplicates.

```sql
   -- Task : Find the employees who are also customers at the same time.

          SELECT
                 first_name,
                 last_name
          FROM Sales.customers 
           INTERSECT
          SELECT
                 first_name,
                 last_name
          FROM Sales.employees 
```

How SQL do combine the data internally behind the scene using ```INTERSECT```? <br/>
-First step SQL does is go & takes column_names from first query to the results, SQL is going row by row, first row1 data of the first query table and check with the second query table data if it is present that means common so include the row1 into result else skip it. Again row2 of first query table complare it with second query table, similiarly row3, row4 and so on.

Employees

|first_name|last_name|
|----------|---------|
| Rohit    | Sharma  |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Vikash   | Dubey   |
| Vipin    | Patel   |

Customers

|first_name|last_name|
|----------|---------|
| Ganesh   | Bhatt   |
| Rahul    | Kumar   |
| Vishal   | Singh   |
| Navin    | Patil   |
| Guddu    | Pandit  |

Output Result of Employees INTERSECT Customers:

|first_name|last_name|
|----------|---------|
| Rahul    | Kumar   |
| Vishal   | Singh   |
  
</details>
<!----------------------------------------->

<details> 
 <summary>Use Cases of <b> SET Operators </b> </summary>

|                 Use Cases                                                               |    SET Operators             |
|-----------------------------------------------------------------------------------------|------------------------------|
|1. Combine similar information before analysing the data.                                | ```UNION```, ```UNION ALL``` |
|2. Identifying the differences or changes (Delta Detection) between two batches of data. | ```EXCEPT``` or ```MINUS```  |
|3. To compare tables to detect discrepancies between databases (Data Completeness Check) | ```EXCEPT``` or ```MINUS```  |

**1. Combine similar information before analysing the data.** <br/>
-Let's say we have 4 tables (Employees, Customers, Suppliers, Students) <br/>
-As we can all of the 4 tables are sharing same kind of information they hold data about persons. <br/>
-Now we have to generate a reports that requires all of the individual in the organisation in the database. <br/>
-So, we will write 4 queries, one for each table and then merge all the results into final report. <br/>
-Now the issue with this setup is that You're having a lot of similar queries and if you modifies in one or two and forget to rest then you will get inaccurate inconsistent data, you have to make changes in every query. <br/>
-Instead what we can do is use SET Operator in order to combine all those tables in one big table. <br/>
-Employees ```UNION``` Customers ```UNION``` Suppliers ```UNION``` Students to make new big table **Persons** which contains all the rows from all the tables. <br/>
-Now, we can write a single SQL query in order to analyse the new big table which holds data about every Person of an Organisation and make report.

> Database Developers divide the data into multiple tables to optimized performance and archive old data. <br/>
e.g. There is a big table Orders contains huge data, so Developers divide this order table into multiple tables in order to optimize performance like orders_2026, orders_2025, orders_2024, orders_2023, orders_2022. Here, We can combine all these tables into one tables using ```UNION``` and write a single query to generate a report.

```sql
  -- Task : Orders are stored in a seperate tables (orders and orderArchive). Combine all orders into one report without duplicates.

     SELECT *
     FROM Sales.orders
     UNION
     SELECT *
     FROM SalesorderArchive

  -- * is not recommended to use while combining the tables

  -- If you want to see the columns of a table, you can do this by either DESC TABLE table_name;
  -- or go to database object explorer > table > right click SELECT 100 copy from there.


     SELECT
          'order' AS sourceTable,
          o.order_id,
          o.product_id,
          o.customer_id,
          o.order_date,
          o.order_status,
          o.salesPerson_id
     FROM Sales.orders AS o
     UNION
     SELECT
          'orderArchive' AS sourceTable,
          oa.order_id,
          oa.product_id,
          oa.customer_id,
          oa.order_date,
          oa.order_status,
          oa.salesPerson_id
     FROM Sales.orderArchive AS oa

     ORDER BY order_id;
```

> Best Practice : Never use * (asterisk) to combine tables, instead always use list of columns. <br/>
Because using column name makes more clear to read query and also if you want to switch order you can as per your use. And also It is not necessary that both tables should have equal number of columns etc etc.

> Add a static string in order to differentiate the table data.

**2. Identifying the differences or changes (Delta Detection) between two batches of data** <br/>
- e.g. Data Engineers build Data pipeline in order to load daily new data from **Source System** to **Data Warehouse or Data Lake**.
- In those Data Pipelines, We build a logic in order to identify what are the new data that is generated from the source system in order to insert it into Data Warehouse or Data Lake.
- One way is to use ```SET Operator``` in order to compare the current data with previous load. like We received 100 rows on Day 1, we inserted the data into Data Warehouse or Data Lake. On Day 2, we received 120 rows out of which 100 were already present, so in order to load only the data which is not present in our Data Warehouse or Data Lake we use ```EXCEPT```, by using the ```EXCEPT``` between those two sets we can identify the new data that's existing in Source System table.
- So if we do ```EXCEPT``` between Day2 and Day1 then we will get only new records.

> ```EXCEPT``` SET Operator is very powerful in order to compare two sets. Not only for Data Analysis, we can use it in Data Engineering in order to identify what is the new data that is generated from the source to insert into Data Warehouse or Data Lake.

**3. To compare tables to detect discrepancies between databases (Data Completeness Check)** <br/>
- We have a scenario where we are doing Data Migration between two databases, and we want to move a table from DatabaseA to new DatabaseA.
- It is very important is to check whether all the records did move from DatabaseA to DatabaseB, we're not missing anything even one record.
- So, Basically we want to do Data Completeness Check, And there are many methods to do this test. One is to use SET Operator ```EXCEPT```.
- We can do an ```EXCEPT``` between table from DatabaseA and table from DatabaseB in order to find if any records which is not migrated from DatabaseA to DatabaseB.
- If we get result is empty that means all the rows from DatabaseA to DatabaseB and then Table from DatabaseB EXCEPT DatabaseA this also gives empty that means we migrated data successfully.
  
</details> 

**Summary of SET Operators** <br/>
- Combines the results of multiple queries into a single result set.
- SET Operator Types : ```UNION```, ```UNION ALL```, ```EXCEPT/MINUS``` and ```INTERSECT```
- Rules :
  - Same num of columns, datatypes and order of columns.
  - 1st Query controls column names.   
- Use Cases :
  - Combine Information (```UNION``` + ```UNION ALL```)
  - Delta Detection (```EXCEPT```)
  - Data Completeness Check (```EXCEPT```) 


<!---------Chapter 6. Combining Data----------------->

[Filtering Data](https://github.com/pawansinghfromindia/SQL/blob/main/05_Filtering_Data/filtering_data.md) | [Row-Level functions]()
