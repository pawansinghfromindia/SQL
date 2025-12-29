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

|   ID  |  Order_ID   | Order_Date     |  Product_ID  | 
|-------|-------------|----------------|--------------|
|   1   |  101        | '2026-01-15'   |  A1001B      |
|   2   |  112        | '2026-01-17'   |  A3001B      |
|   4   |  221        | '2026-01-21'   |  A5001B      |

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

|   ID  |  Order_ID   | Order_Date     |  Product_ID  | 
|-------|-------------|----------------|--------------|
|   1   |  101        | '2026-01-15'   |  A1001B      |
|   2   |  112        | '2026-01-17'   |  A3001B      |
|   4   |  221        | '2026-01-21'   |  A5001B      |

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

|                            Basic Joins                                          |                       Advanced Join                                                    |
|---------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| No Join     :  No Common data between 2 tables                                  | Cross Join : All the data of left table cross multiply to all the data of right tables | 
| Inner Join  :  Only Common data of 2 Tables                                     |                                                                                        |
| Left Join   :  all data of Left table, which includes common data of right table| Left Anti Join : all data of Left table, which excludes common data of right table     |
| Right Join  :  all data of right table, which includes common data of left table| Right Anti Join : all data of right table, which excludes common data of left table    |
| Full Join   :  all the data of left & right which includes common data of both  | Full Anti Join : all the data of left & right which includes common data of both       |


</details>

<!------------------------------>

## 6.3 SQL ```SET Operators```

<details>
<summary> What is <b>SQL JOINs</b>? </summary>
  
</details>

<!------------------------------>
<!---------Chapter 6. Combining Data----------------->

