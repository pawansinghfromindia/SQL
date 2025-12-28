<!---------------Chapter 4. Data Manipulation Language (DML)-------------------->
## Chapter 4. Data Manipulation Language (DML)

After leraning DDL. Here, We're going to learn DML.
How to manipulate(modify) data inside the database.

- ```INSERT```
- ```UPDATE```
- ```DELETE```



##  4.1 ```INSERT``` DML Command
<details>
<summary>  </summary>

Let's say there is a Table inside Database.
Table is empty, means Tabble don't have any data, in order to add data into the table, we use ```INSERT``` DML command
So, Insert is going to add a new row into the table (empty table as well as tables with data).

In order to insert new data to the target table. There are 2 methods :


### 1. First & classical way in order to insert new data, we can use ```INSERT``` command and manually specify the values that should be inserted into the targeted table.

Here in this process we are manually inserting new values to the table i.e. **Manual Entry (Values)**

Syntax of INSERT:
```sql
    INSERT INTO table_name (column1, column2, column3)
    VALUES (value1, value2, value3)
```

> Note : **OPTIONAL** if ***no columns*** are specified, SQL expects ***values for all columns***
> bcuz sometime we don't want to insert values for each column. So, we can skip few columns while inserting just via specifying the column.

**RULE or CAUTION**

> Number of columns and the number of values must be matched.

> Matching Data Types, Column Counts & Constraints.

> Columns and Values must be in the same order.

While inserting the data, the number of columns and the number of values must be matched. Also the order should be same otherwise data will be wrong column.

```sql
     INSERT INTO table_name
     VALUES (value1, value2, value3, value4, value5)
```

|  ID  |     Name     |    Country    |  Score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |

Insert one row at at time.
```sql
      INSERT INTO students (id, name, country, score)
      VALUES (6, 'Andy', 'USA', NULL)
```

Insert multiple row at at time.
```sql
      INSERT INTO students (id, name, country, score)
      VALUES
            (7, 'Sam', NULL, 100),
            (8, 'Steve', 'Germany', 600),
        --  ( NULL, 'Andy', 'USA', NULL), -- Can't insert as Primary Key can't be NULL.
        --  ( 'Jeff', 10, 'USA', 750), -- Can't insert as ID can't be VARCHAR.
```

|  ID  |     Name     |    Country    |  Score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |
|  6   |  Andy        |  USA          |   NULL  |
|  7   |  Sam         |  NULL         |   100   |
|  8   |  Steve       |  Germany      |   600   |

```sql
      INSERT INTO students (id, name, country, score)
      VALUES (9, 'USA','Andrew', 550)
```
Data is inserted here BUT It's in wrong way. So, before insert make sure order of columns and values must be in same order.

|  ID  |     Name     |    Country    |  Score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |
|  6   |  Andy        |  USA          |   NULL  |
|  7   |  Sam         |  NULL         |   100   |
|  8   |  Steve       |  Germany      |   600   |
|  9   |  USA         |  Andrew       |   550   |

You can skip the columns if you insert values for every column.
```sql
    INSERT INTO students
    VALUES (10, 'Matt', 'France', NULL)
```
> Tips : Always lists columns explicitly for clarity and maintainability 

```sql
    INSERT INTO students (id, name)
    VALUES (11, 'Marsh')
```
> Note : Columns not included in ```INSERT``` become NULL (unless a default or constraint exists)

```sql
    INSERT INTO students (name)
    VALUES ('Marsh');  -- Not allowed as Contraint on ID exists which can't be NULL
   
    INSERT INTO students (id)
    VALUES (12) --This is fine
```

|  ID  |     Name     |    Country    |  Score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |
|  6   |  Andy        |  USA          |   NULL  |
|  7   |  Sam         |  NULL         |   100   |
|  8   |  Steve       |  Germany      |   600   |
|  9   |  USA         |  Andrew       |   550   |
|  10  |  Matt        |  France       |   NULL  |
|  11  |  Marsh       |  NULL         |   NULL  |
|  12  |  NULL        |  NULL         |   NULL  |

### 2.  ```INSERT``` using ```SELECT```

Instead of inserting the data manually, We can insert data using another table.

Imagine a scenario where we have already existing a table with data i.e. Source Table (source of data).

We have another table that is empty and we want to insert the data to this target table from the source table without manually writing values.

So, here we're moving the data from one table(Source table) to another(Target table).

In order to do that we need to do 2 steps:
- First Select the data from Source table
- Then insert the selected data into target table.

Syntax of INSERT with SELECT:
```sql
      INSERT INTO target_table_name
      SELECT
          column1,
          column2,
          column3,
          column4
      FROM source_table_name;

     --OR specify columns for clarity

     INSERT INTO target_table_name (column1, column2, column3, column4)
      SELECT
          column1,
          column2,
          column3,
          column4
       FROM source_table_name;
```

Que : Insert data from students table into persons table.
Source Table : students
|  id  |     name     |    country    |  score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |
|  6   |  Andy        |  USA          |   NULL  |
|  7   |  Sam         |  NULL         |   100   |
|  8   |  Steve       |  Germany      |   600   |
|  9   |  USA         |  Andrew       |   550   |
|  10  |  Matt        |  France       |   NULL  |
|  11  |  Marsh       |  NULL         |   NULL  |
|  12  |  NULL        |  NULL         |   NULL  |

Target table is persons.
iD is primary_key constraints, can't be NULL, birth_date can be NULL, mobile is VARCHAR & can't be NULL.
|  id  | person_Name  |   birth_date  |  mobile  |
|------|--------------|---------------|---------|

```sql
    INSERT INTO persons (id, person_name, birth_date, mobile)
    SELECT
      id,
      name,
      NULL,       --since no birth_Date in source table But we can keep it NULL
      'Unknown'   --since no mobile in source table & also can't keep it NULL. That's why providing static value
   FROM students
```
It will the insert the data from students table to persons table.

|  id  | person_Name  |   birth_date  |  mobile   |
|------|--------------|---------------|-----------|
|  1   |  Maria       |  NULL         |  Unknown  |
|  2   |  John        |  NULL         |  Unknown  |
|  3   |  Georg       |  NULL         |  Unknown  |
|  4   |  Martin      |  NULL         |  Unknown  |
|  5   |  Peter       |  NULL         |  Unknown  |
|  6   |  Andy        |  NULL         |  Unknown  |

</details>
<!------------------------------>


##  4.2 ```UPDATE``` DML Command

<details>
<summary> </summary>

We have learned how to insert data to table in database.
  
Now we don't have to add a new row but we have to update the content of the already existing row values in table.
  
So, in order to update(change) content of already existing rows we use ```UPDATE``` DML Command.

Unlike ```INSERT``` which insert completely new data. ```UPDATE``` change the data of already existing row.

Syntax of ```UPDATE```
```SQL
     UPDATE table_name
        SET column1 = value1,
            column2 = value2
         WHERE <condition>
```
> Note : ***Always use*** ```WHERE``` to avoid ***updating all rows*** unintentionally.

Que : Change the score of students with ID 6 to 0.
```sql
    UPDATE students
     SET score = 0
     WHERE id = 6  --Caution : without a WHERE, all rows will be updated!

   /*  SELECT *
     FROM students
     WHERE id = 6  */
```

> Best Practice : Check with **```SELECT```** before running **```UPDATE```** to avoid updating the wrong data.

Que : Change the score of students with ID 10 to 0 and update the country to UK
```sql
      UPDATE students
       SET
          score = 0,
          country = 'UK'
       WHERE id = 10
  /*
      SELECT *
      FROM students
      WHERE id = 10   */ --check before update
```

Que : Update all students with a NULL score by setting their score to 0
```sql
      UPDATE students
       SET score = 0
       WHERE score IS NULL
  /*
      SELECT *
      FROM students
      WHERE score IS NULL   */ --check with SELECT before update
```

Result Output:
|  id  |     name     |    country    |  score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |
|  6   |  Andy        |  USA          |   0     |
|  7   |  Sam         |  NULL         |   100   |
|  8   |  Steve       |  Germany      |   600   |
|  9   |  USA         |  Andrew       |   550   |
|  10  |  Matt        |  UK           |   0     |
|  11  |  Marsh       |  NULL         |   0     |
|  12  |  NULL        |  NULL         |   0     |

</details>
<!------------------------------>

##  4.3 ```DELETE``` DML Command
<details>
<summary> </summary>

We have leaned how to insert a new row in the table and also how to update the content of already existing rows, last thing we can do to data inside the table is remove rows from the table.
We can do this using command ```DELETE```

```DELETE``` will remove already existing rows inside the table.

Syntax of ```DELETE```
```sql
      DELETE FROM table_name
      WHERE <condition>
```

> Note : Always use **```WHERE```** to avoid ***deleting all rows*** unintentionally

Que : Delete all students with an ID greater than 5.
```sql
      DELETE FROM students
      WHERE id > 5         --Caution : without a WHERE, all rows will be deleted!

  /*    SELECT *
        FROM students
        WHERE id > 5;   */ --check with SELECT before delete
```
Result Output:
|  id  |     name     |    country    |  score  |
|------|--------------|---------------|---------|
|  1   |  Maria       |  Germany      |   350   |
|  2   |  John        |  USA          |   900   |
|  3   |  Georg       |  UK           |   750   |
|  4   |  Martin      |  Germany      |   500   |
|  5   |  Peter       |  USA          |   100   |

Que : Delete all data from the table persons.

```sql
    DELETE FROM persons;

    --both are exactly doing the same thing, even TRUNCATE is way faster than DELETE in case of large size table

    TRUNCATE  persons; 

```

Here, comes another interesting command ```TRUNCATE```, If you want to delete everything from the table

> **```TRUNCATE```** clears the whole table at once without checking or logging.

We have learned the basics on How to manipulate the table data in the database through DML commands. 

</details>
<!------------------------------>
<!---------------Chapter 4. Data Manipulation Language (DML)-------------------->

**Summary of all the 4 Chapters:** 
- Intro - Data , SQL
- Server > Database > Schema > Table > Rows(records) > Colums(fields) Database Structure where Data is stored.
- Database Types - Relational DB, Key-Value DB, Column-Based DB, Graph DB, Document DB
- DBMS & SQL Server, Why SQL? 
- Setup SQL Environment
- Datatype - INT, DECIMAL, BIGINT, CHAR, VARCHAR, DATE, TIME & static(fixed) value
- Query Data using Clauses (select, select distinct, top/limit, from, where, group by, order by, having),
- Aggregate functions (sum(),count(),avg(),min(),max())
- DDL(create, alter, drop) and 
- DML(insert, update, delete, truncate) commands

That's all Beginner-Level completed!
🎉🎊Congratulations!!🎊🎉

Next is Intermediate-Level.

[DDL](https://github.com/pawansinghfromindia/SQL/blob/main/03_DDL/data_definition_language.md) [Filtering Data]()
