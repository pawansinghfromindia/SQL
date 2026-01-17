<!-----------Chapter 10. Performance Optimization---------->
# Chapter 10. Performance Optimization
- Indexes
- Partitions
- Performance Tips
<!-----------Chapter 10. Performance Optimization---------->

This chapter is about performance, as we start writing complex and multiple queries on multiple tables, views in the database.
One thing to notice is the performance  of the query are really slow.

Here, in this chapter we will learn different techniques on how to increase the performence like twice by using
techniques Indexes and partitions.

## 10.1 Indexes

<details>
  <summary> <b>What is an Index ? </b> </summary>

> **An index is a data structure that provides a quick access to the row (data), to improve(optimizing) the speed of the queries.**

Index is like a guide for our database in order to speed up the process of searching for data especially if we have like big tables..

In order to understand indexes, imagine we have a book and we want to find a specific Topic inside a chapter.
So, instead of flipping every single page to search for the topic. We can use the index of the Book chapters in the initial page 
in order to jump straight to the right page. <br/>
That's exactly Index does for our data in database.

Another analogy to understand indexes is think about the indexes as a big hotel.
In hotel if we don't have any guide and we have to find the room number 101.
Now there you have to find the room floor by floor and checking each room until we find our room.
But instead of that, Thankfully hotels have a numbering system and we can ask for a map from a reception in order to understand 
in which building, on which floor, we can find our room.
There we have maps & signs to very quickly to locate & find the room in such big hotel. <br/>
That's exactly what each database needs. It needs an index in order to help the database in finding & locating the right data without
having to scan everything.

Let's say we have a big table. <br/>
Now we would like to speed up queries using indexes. <br/>
Question :
- What're we exactly going to do with the table?
- Are we using the table to search for text ?
- or Are we doing complex analysis with tables ?

Reason to ask these question is that We have different indexes in database for different purposes.
</details>

<details>
  <summary> <b>Indexes Types</b> </summary>

We divides the **Indexes** in database in 3 categories :
1. **Structure** : How the database is organizing and referencing the data? They're of 2 types :
  - **Clustered Index**
  - **Non-Clustered Index**

2. **Storage** : How the data is stored physically in the database?. They're of 2 types :
  - **RowStore Index**
  - **ColumnStore Index**

3. **Functions** : How the database function? They're of 2 types :
   - **Unique Index** 
   - **Filtered Index** 

<img width="350" height="150" alt="image" src="https://github.com/user-attachments/assets/59652c38-50ac-4b1f-8f58-371b62205ba5" />

Each index type has its own strings but it's always a trade off.
Some might improve their read(Select) performance other one might improve writing(Insert, update) performance.
So, It's all about choosing the right type of index for the job.

> Trade-off
> - Some Indexes are better for reading.
> - Others for writing performance.
> - Choose the right one for the job.

Let's deep dive into each of the type in order to understand :
How they work? and
How we can create them?

</details>

<details>
  <summary> <b> Clustered Index & Non-Clustered Index </b> </summary>

Before we dive into How indexes work in database? <br/>

Let's understand first, What happens to database tables if we don't use any index?
- We create a new table in database. e.g. Customers table where we have 20 customers inside this table.
- What we see at client side is like spreadsheet (a table, rows & columns) but behind the scene the database stores it bit differently.
- Database stores the data in a data file on the disk and inside this file the data can be stored inside the blocks called **pages**.
- **Pages** are not like **rows & columns** 

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/1c5ded8c-9be2-4349-95e9-d1ccf6ce5536" />

**What is a Page ?**
> A page is the smallest unit of data storage in a database (8kb)
> - It is a fixed size of 8 kilobytes where SQL database can store anything inside it.
> - It stores anything (Data like table:rows & columns, Metadata, Indexes etc)
> - So, Everytime we interact with data, the SQL is reading and writing on those pages.

As we can see, SQL is not storing the data inside database like table : rows & columns. 
So, if we're running a query then SQL not like selecting a specific column. SQL always fetch a data page in order to read the rows 
inside the this page.

There are mainly 2 types of Pages :
1. **Data Page**
2. **Index Page**

How the **Data Page** looks like? </br?
It is divided into multiple sections.

**Header**
- 1st section is Page **Header**, where database stores key information about the metadata like Page ID.
- It has following format :
  - It starts with the File ID and then have a unique number for each page. like File Id is 1 and Page Number is 150```1 : 150```
- The page header is a fixed size of 96 bytes.

| Header   File ID : Page Number  |
|---------------------------------|
| Header         1 : 150          |

**Rows Section**
- This section have a variable size, this is where our data is stored.
- The actual data is going to be stored in this section.
- SQL try to fits as many rows as it can in one single page.
- Of course this depends on size of each row. So if we have a large table in which rows are really big then
  SQL can fit only few rows in one single page.

| Rows  | Columns Data  |
|-------|---------------|
| Row 1 | 1, David, USA |
| Row 2 | 1, Mike, UK   |
| Row 3 | 1, John,  CA  |
| ..... | ............. |
| Free  | Spaces        |
| Free  | Spaces        |

**Offset Array**
- The section of Data page is  Offset Array. This is like a quick index for the rows stored inside this page
- It keeps track of where each row begins so that the SQL can easily locate a specific rows
  without having scanning the entire page in order to find a row.

| Offset [address of rows] |
|--------------------------|
| 1, 2, 3 <br/> [row1, row2, row3]|

This is the Structure of a Data Page. This is exactly how SQL stores the data inside the databases.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/c3ef9f81-6bca-481d-852d-ae3c1be19765" />

Let's get back to example of our table Customers where we have 20 rows.
Let's see How SQL will creates those pages for the table customers.

- So, If we're not using any index on the table Customers what is going to happen is : SQL is going to insert the data inside those pages
as we're inserting the data inside the customers.

- like may be we are inserting the customers like 12, 5, 6, 7 and SQL is going to insert it to the Data pages exactly like that only.
So, SQL is just inserting the data as we insert it to the table.

- So Let's say each data page is fitting only five rows. So after inserting 5 customers, 
SQL will create another data page for that next rows. Once it again load 5 customers, It will create another data page and so on
until covered all the rows that are being inserted.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/849dc3f0-a2ee-4065-baaf-9140ffddb11e" />

- Now if we check customers inside those 4 pages. We can see that they're not sorted at all. That's bcuz in this scenario we're not
  using any index.
- We call this Structure as **Heap Structure**.

> Heap table = Table without Clustered Index
> - that means the Rows are stored randomly without any particular order.
> - This is not very bad, bcuz It's going to be very quick to insert the data inside the table.
> - But of course the finding something from this table is going be slow.

This is the first trade-off. If you have a very fast writes but very slow reads.
It is like We're throwing all our papers in a drawer without organizing them. So, we can put a paper very quickly but if we search for
a specific paper later, It is going to very long time taking process until we find it. Bcuz things are no it order.

<img width="350" height="200" alt="image" src="https://github.com/user-attachments/assets/60ce4214-f8c9-4f51-84fa-41ada666083e" />

**Heap Structure**



</details>

<!---------------Indexes------------------>



## 10.2 Partitions
<details>
  <summary> <b>Partitions </b> </summary>
</details>
<!----------------Partitions----------------->


# 10.3 Performance Tips

<details>
  <summary> <b>Performance Tips </b> </summary>
</details>
<!---------------Performance Tips------------------>







