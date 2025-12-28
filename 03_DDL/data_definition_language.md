<!-------Chapter 3. Data Definition Language (DDL)---------------->

# Chapter 3. Data Definition Language (DDL)

Here, We're going to learn the DDL Commands.

- ```CREATE```
- ```ALTER```
- ```DROP```

## 3.1 Data Definition Language DDL ```CREATE```

- To create a new table in the database, the DDL command we have is ```CREATE```. Name itself describe it.
  
Usually, we have database but It is empty, So what we have to do is define the structure of our data.
So, First thing that we do is : Create a new Table for that we have ```CREATE``` Command.
What ```CREATE``` does is create new Object inside the database. like new Table is created in the database. Of course the newly created table is empty without any data.

Syntax of CREATE Command:

```sql
     CREATE TABLE table_name(
          column1_name datatype,
          column2_name datatype,
          column3_name datatype
          CONSTRAINT primary_key_name PRIMARY KEY (Column which contains only unique values & not null)
     )
```
We should have a primary key in each table, in order to make sure the table has an integrity or to connect with other tables.
So at last we add CONSTRAINT at last column, there primary_key_name is only visible for the database.(internal to db)

Que : Create a new table called persons with the columns: id, person_name, birth_date and mobile.
```sql
     CREATE TABLE persons(
          id INT NOT NULL,
          person_name VARCHAR(50) NOT NULL,
          birth_date DATE,
          mobile VARCHAR(12) NOT NULL
          CONSTRAINT pk_persons PRIMARY KEY (id) 
     )
```

Once query executed, It will creaate a new table with the name persons having no data which means table is empty.
```sql
    SELECT *
    FROM persons;
```

One important thing is that go and save that SQL Query of Table creation bcuz that contains information about Table, bcuz may be later on you have to redefine this table.
But It's okay if you lost the Script of created table, bcuz we can check that through trick, Go to database > Table > Script Table as > CREATE To > New Query Editor
So, You will see, what happened was database read metadata information about the table created and created DDL query with many extra stuffs which we haven't done. That's the template database
used to create table internally. But you can see the CREATE TABLE Query which you created.
So, This is how you can get back your DDL command.
But It is recommended to store your DDL commands in repository and keep it updated.


<!------------------------------------->

## 3.2 Data Definition Language DDL ```ALTER```

If we have already a table exists and we have modify the structure of the table.

- ```ALTER``` the name itself means to edit or modify or change. 

How can we do that :
 - Create a new table with desired new structure, copy the data into the new table and then drop the old table. We can do this but it is not recommended.
 - For this we have ```ALTER``` DDL command which modifies, Edits, changes the existing table's structure.


So to edit the definition of the existing table like adding a new column or may be changing the datatype of column or anything.

Syntax of ```ALTER```
```sql
      ALTER TABLE table_name
      ADD column1 datatype
```

Que : Add a new column called emain to the persons table.
```sql
     ALTER TABLE persons
     ADD email VARCHAR(50) NOT NULL
```
After the execution of ALTER command, the strucure of persons table has been changed added a new column named email at the end.
```sql
       SELECT *
       FROM persons;
```

> ADDING COLUMNS : The new columns are appeneded at the end of the table by default.

But if you want to add columns in between or at any specific place, well you can do that by creating completely new table using ```CREATE``` from scratch and insert the old data
into newly table and then drop the old table.

Another way is by specifying the column but It is not supported everywhere.

```sql
       ALTER TABLE table_name
       ADD COLUMN new_column_name datatype
       AFTER existing_column_name;   
```

Que : Remove the column mobile from the persons table.
```sql
      ALTER TABLE persons
      DROP COLUMN mobile;
```
It will drop the column mobile from the table persons.
```sql
       SELECT *
       FROM persons
```

> Before droping the column inside table, be careful because you will lose the data of that column inside the table.

This is how we edit, modify or change the structure of table.

<!-------------------------------------->

## 3.3 Data Definition Language DDL ```DROP```

So far what we have done is Created something the new table in the database, also we have changed definition the structure of table and last one is we can drop something the table from 
the database.

- ```DROP``` the name itself says It will drop something .

```DROP``` command is used in order to remove table completely from the database. 

- Be careful before dropping the table from the database, you will lose your data and can't recover.

Syntax of DROP :
```sql
      DROP TABLE table_name;
```

Que : Delete the table persons from the database.
```sql
      DROP TABLE persons;
``` 

> ✨ Destroying things is way easier than Creating/Building it. <br/> Example : How simple is to drop a table than to create a table.

<!--------------------------------------->


These ```CREATE```, ```ALTER``` and ```DROP``` commands are used in order to define the structure of tables in the database known as **DDL commands**

[SQL Clauses](https://github.com/pawansinghfromindia/SQL/blob/main/02_Query_Data(SELECT)/query_data_select.md) [DML Commands]()

<!-------Chapter 3. Data Definition Language (DDL)---------------->
