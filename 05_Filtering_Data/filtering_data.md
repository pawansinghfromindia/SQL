<!---------Chapter 5. Filtering Data----------------->
# Chapter 5. Filtering Data

Here, We're going to learn how to filter the data using different operators with ```WHERE``` clause.

- **Comparision Operators** : ```=```, ```<>``` or ```!=```, ```>```, ```>=```, ```<```, ```<=``` <br/>
   It is used to compares 2 values.

- **Logical Operators** : ```AND```, ```OR```, ```NOT``` <br/>
  It is used to combine multiple operators.

- **Range Operator** : ```BETWEEN``` <br/>
  It is used in order to check whether a value falls within a specific range.

- **Membership Operator** : ```IN```, ```NOT IN```<br/>
  It is used to check whether a value is in a list or not.

- **Search Operator** : ```LIKE``` <br/>
  It is used in order to search for a specific thing in the text.

## 5.1 Comparison Operators : ```=```, ```<>``` or ```!=```, ```>```, ```>=```, ```<```, ```<=```

<details>
  <summary> Comparison opertors <b>Compare two things</b>.  </summary>

| Expression | Operator | Expression |    
|------------|----------|------------|

This is what we call it as ***Condition*** <br/>

Comparison opertors **Compare two things**.

- **```=```** : Checks if two values are ***equal***.
- **```<>```** or **```!=```**: Checks if two values are ***not equal***. 
- **```>```** : Checks if a value is ***greater than*** another value. 
- **```>=```** : Checks if a value is ***greater than or equal to*** another value.
- **```<```** : Checks if a value is ***less than*** another value. 
- **```<=```** : Checks if a value is ***less than or equal to*** another value. 

|  Name     | Country   | Score |
|-----------|-----------|-------|
| Maria     | Germany   | 350   |
| John      | USA       | 900   |
| Georg     | UK        | 750   |
| Martin    | Germany   | 500   |
| Peter     | USA       | 100   |

```sql
--Task 1 : Retrieve all students from Germany.
    SELECT *
    FROM students
    WHERE country = 'Germany';

--Task 2 : Retrieve all students who are not from Germany.
    SELECT *
    FROM students
    WHERE country != 'Germany'; --Both country != 'Germany' and country <> 'Germany' are same. 

--Task 3 : Retrieve all students with a score greater than 500.
    SELECT *
    FROM students
    WHERE score > 500;

--Task 4 : Retrieve all students with a score of 500 or more.
    SELECT *
    FROM students
    WHERE score >= 500;

--Task 5 : Retrieve all students with a score less than 500.
    SELECT *
    FROM students
    WHERE score < 500;

--Task 6 : Retrieve all students with a score of 500 or less.
    SELECT *
    FROM students
    WHERE score <= 500;
```
So, We can compare a lot of things together in SQL like compare one column to another column, column to a value, function to a value, expression to a value and even subquery result to a value.

 Column1  = Column2 <br/>
  e.g. ```first_name = last_name``` 
  
 Column1  = Value <br/>
   e.g. ```first_name = 'John'```
 
 Function  = Value <br/>
   e.g. ```UPPER(first_name)  = 'JOHN'```

 Expression = Value <br/>
   e.g.  ```(Price * Quantity) = 1000 ```

 Subquery = Value   (Advanced Topic) <br/>
   e.g.  ```(SELECT SUM(slaes) FROM order) = 1000```

</details>
<!------------------------>

## 5.2 Logical Operator : ```AND```, ```OR```, ```NOT```
<details>
  <summary>  Logical opertors <b>Combine multiple operators</b>. </summary>
  
- **```AND```** : All conditions must be ***TRUE***.
- **```OR```** : At least one of the condition must be ***TRUE***.
- **```NOT```** : **Reverse** the result, makes ***TRUE*** to ***FALSE*** and ***FALSE*** to ***TRUE***.
   - It is going to Excludes the matching values. <br/>
   - ```NOT``` is not like ```AND``` or ```OR```. It is not going to combine two conditions. <br/>
   - We use ```NOT``` with only one condition. <br>
   - If the condition result is TRUE and we apply not to it then in the result all the rows will come which is not satisfying the condition. <br/>
   - Basically ```NOT (<Condition>)```.


|  Name     | Country   | Score |
|-----------|-----------|-------|
| Maria     | Germany   | 350   |
| John      | USA       | 900   |
| Georg     | UK        | 750   |
| Martin    | Germany   | 500   |
| Peter     | USA       | 100   |

```sql
--Task 1 : Retrieve all students who are from USA AND have a score greater than 500.
    SELECT *
    FROM students
    WHERE country = 'USA' AND score > 500;

--Task 2 : Retrieve all students who are either from USA OR have a score greater than 500.
    SELECT *
    FROM students
    WHERE country = 'Germany' OR score > 500;

--Task 3 : Retrieve all students with a score NOT less than 500.
    SELECT *
    FROM students
    WHERE NOT score < 500;  --Both score >= 500 and NOT score < 500 are same.
```

</details>
<!------------------------>

## 5.3 Range Operator : ```BETWEEN```
<details>
  <summary> Range operator <b> checks if a value is within a range</b>. </summary>

- **```BETWEEN```** : Checks if a value is within a ***range***. <br/>

   For any range we need two things, **lower boundary** and **upper boundary**
  and everything between those two boundaries is ***TRUE*** and everything outside those two boundaries is ***FALSE*** <br/>

  Boundaries are **inclusive**.
  
  e.g range(100, 500) : It includes 100 and 500 as well, from 100 upto 500. <br/>
  ```100,101,102,......................,499,500```

|  Name     | Country   | Score |
|-----------|-----------|-------|
| Maria     | Germany   | 350   |
| John      | USA       | 900   |
| Georg     | UK        | 750   |
| Martin    | Germany   | 500   |
| Peter     | USA       | 100   |

```sql
--Task 1 : Retrieve all students whose score falls in the RANGE between 100 and 500.
    SELECT *
    FROM students
    WHERE score BETWEEN 100 AND 500;  -- Both score BETWEEN 100 AND 500 and score >=100 AND score <=500 are same.
```

>  Both score BETWEEN 100 AND 500 and score >=100 AND score <=500 are same. <br/>
>  It's upto you to use BETWEEN or Explicit camparison (as it clearly shows that both boundaries are included.)

</details>
<!------------------------>

## 5.4 Membership Operators : ```IN```, ```NOT IN```
<details>
  <summary> Membership Operators <b>check if a value exists in a list or not</b>  </summary>

- **```IN```** : checks if a value exists in a list.
- **```NOT IN```** : checks if a value doesn't exist in a list.

In order to use Membership Operators, all we have to do is ***define a list of members*** and use ```IN``` and ```NOT IN``` operators.
If the value is available in the list it returns ***TRUE*** else returns ***FALSE***.

|  Name     | Country   | Score |
|-----------|-----------|-------|
| Maria     | Germany   | 350   |
| John      | USA       | 900   |
| Georg     | UK        | 750   |
| Martin    | Germany   | 500   |
| Peter     | USA       | 100   |

```sql
--Task 1 : Retrieve all students from either Germany or USA.
    SELECT *
    FROM students
    WHERE country IN ('Germany','USA');
    --Both country = 'Germany' OR country = 'USA' and country IN ('Germany','USA') are same.

--Task 2 : Retrieve all students from neither Germany nor USA.
    SELECT *
    FROM students
    WHERE country NOT IN ('Germany','USA');
    --Both country != 'Germany' OR country != 'USA' and country NOT IN ('Germany','USA') are same.
```

> Tips : Use ```IN``` instead of ```OR``` for multiple values in the same column to simplify SQL. 

```sql
       SELECT *
       FROM students
       WHERE country = 'Germany' OR country = 'USA' OR country = 'UK' OR country = 'France';

       --Both are same, but the second one is simplified.

       SELECT *
       FROM students
       WHERE country IN ('Germany','USA','UK','France');
```

</details>
<!------------------------>

## 5.5 Search Operator : ```LIKE```
<details>
  <summary> Search Operator <b>searches for a pattern in text</b>.  </summary>

- **```LIKE```** : searches for a pattern in text.

How to build and search a ***pattern*** in the text using ```LIKE``` operator?

Define a pattern.

In SQL, in order to build a pattern, we have 2 special character ```%``` (percentage) and ```_``` (underscore).

1️⃣ **```%```** is ***Anything***. <br/>

If we are using ```%```, that means we are saying Anything. It could be no character at all or only one character or many characters.
- 0
- 1
- Many

2️⃣ **```_```** is ***Exact 1***. <br/>

If we are using ```_```, that means we are saying Exact one thing. like exactly one character.

- 1
  
```M%``` means first character must be ```M``` and then Anything. <br/>
```%in``` means It could start with anything but the last two characters must be ```in```. <br/>
```%r%``` means ```r``` could be anywhere whether at begining or at the end or in between. <br/>
```__b%``` means There must be one character at first position, there must be one character at second position and then third position must me character ```b``` 
after that it could be anything.


| M%           |  %in           |  %r%             |    __b%        |
|--------------|----------------|------------------|----------------|
|     M Any    |     Any  in    |      Any r Any   |      12b Any   |
| ✅ Maria    |  ✅  Martin    | ✅     Maria     | ✅  Albert    |
| ✅ Ma       |  ✅     Vin    | ✅   Peter       | ✅  Rob       |
| ✅ M        |  ✅      in    | ✅       Rayn    | ❌  Abel      |
| ❌ Emma     |  ❌  Jasmine   | ✅       R       | ❌  An        |
|             |                 | ❌     Alice     |                |

> Note : **```%```** is way more famous than **```_```**. We rarely use **```_```**.

|  Name     | Country   | Score |
|-----------|-----------|-------|
| Maria     | Germany   | 350   |
| John      | USA       | 900   |
| Georg     | UK        | 750   |
| Martin    | Germany   | 500   |
| Peter     | USA       | 100   |

```sql
--Task 1 : Find all students whose name starts with 'M'.
    SELECT *
    FROM students
    WHERE name LIKE 'M%';

--Task 2 : Find all students whose name ends with 'n'.
    SELECT *
    FROM students
    WHERE name LIKE '%n';

--Task 3 : Find all students whose name contains with 'r'.
    SELECT *
    FROM students
    WHERE name LIKE '%r%'; --'%value%' is most used to search a value in the database.

--Task 4 : Find all students whose name has 'r' in the third position.
    SELECT *
    FROM students
    WHERE name LIKE '__r%';
```

</details>
<!------------------------>

We have seen all the different operators (```=```, ```!=``` or ```<>```, ```<``` , ```<=```, ```>```, ```>=```, ```AND```, ```OR```, ```NOT```, ```BETWEEN```,
```IN```, ```NOT IN```, ```LIKE```) that we use inside the ```WHERE``` clause in order to filter the tables's data inside database.

<!---------Chapter 5. Filtering Data----------------->

Next, We are going to learn is about How to combine data from multiple tables using ```JOINS``` and ```SET``` operator.

[DML](https://github.com/pawansinghfromindia/SQL/blob/main/04_DML/data_manipulation_language.md) [Combining Data]()
