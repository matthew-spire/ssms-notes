## Core Info

- T-SQL is the scripting language for talking to your SQL Server (database)
- Query -> Request for something (data) -> Should be as optimized and simplified as possible
  - Keep it simple -> Getting cute with queries and being overly complicated can cause a mess
  - Operates on a table or tables, which could have a million plus records -> When writing queries, think of how the entire database and system (e.g., memory) might be impacted and how that impacts efficiency
  - Ask for information in a way that is efficient for SQL, the database, the system, etc.
- Let SQL (and other tools) do what it is good at
- Keywords
  - SELECT
    - Used to select data from a database
    - Can either fetch all columns using an asterisk (*) or select individual columns by specifying their names -> Should almost always include the unique identifier that identifies the record (in order to refer to it)
      - Primary Key that is an integer that counts up -> Pretty common
      - Microsoft uses GUID -> A GUID (globally unique identifier) is a 128-bit text string that represents an identification (ID) -> Not as performant as IDs
      - Using a unique identifier makes insertion into the database much faster because it inserts based upon the ID in the spot where it should go
    - Can also be used with functions to perform calculations or operations on the data
  - `*`
    - Returns all columns
    - Usage is not necessarily best practice
  - FROM
    - Used in conjunction with SELECT to specify from which table in the database you want to select data
    - Name of the table follows the FROM keyword
    - Subqueries using () -> Be careful how complex this gets
  - WHERE
    - Used to filter records that fulfill a specified condition -> Extracts only those records that fulfill the condition specified by the WHERE clause
    - Not limited to only one condition
      -  Use logical operators like AND, OR, NOT, and comparison operators like =, <, >, >=, <=, <>, LIKE, IN, BETWEEN, etc. to form more complex conditions
  - ORDER BY
    - Used to sort the results of a query in ascending or descending order based on the columns specified
    - Sorts the records in ascending order by default -> If you want to sort the records in descending order, you can use the DESC keyword
    - Can order by a single column, mutliple columns, and can specify how you want the records sorted for one or all columns
  - JOIN
    - Associate rows in one table with rows in another table
    - Used to combine rows from two or more tables based on a related column between them -> Allows you to create a relationship between different tables
    - Keys used to match rows are often primary and foreign keys
    - INNER JOIN -> Returns records that have matching values in both tables
    - LEFT (OUTER) JOIN -> Returns all records from the left table, and the matched records from the right table -> If there is no match, the result is NULL on the right side
    - RIGHT (OUTER) JOIN -> Returns all records from the right table, and the matched records from the left table -> If there is no match, the result is NULL on the left side
    - FULL (OUTER) JOIN -> Returns all records when there is a match in either left or right table records
    - The performance of a query using JOIN can be impacted based on the amount of data, indexes, and the complexity of the JOIN operation -> Always ensure your JOIN operations are optimized for your specific use case
    - Understand the different types of JOIN
    - Can add WHERE, ORDER BY, etc. after the JOIN
  - INSERT
    - A command used to add new rows of data into a table in the database
    - Can be used to insert a single row, multiple rows, or copy rows from one table to another
    - Need to specify what you are inserting (i.e., the column names) and the values that you are inserting
  - UPDATE
    - Used to modify existing records in a table
    - Note: Make sure to include a WHERE clause, otherwise you will update every row in the table
  - DELETE
    - Used to delete existing records in a table
    - Note: Make sure to include a WHERE clause, otherwise you will delete every row in the table
- Create a table
- Stored Procedures
  - Security, speed, and safety

## Extra Stuff

- Understand Primary Key and Foreign Key
  - Primary Key
    - A Primary Key is a column (or set of columns) in a table that uniquely identifies each row in that table
    - Ensures that no duplicate or null values are entered into the entire column (or set of columns) marked as a Primary Key
    - Provides a way to distinguish each record in a table
    - Serves as the reference column for establishing relationships between tables
  - Foreign Key
    - A column (or set of columns) that is used to establish a link between the data in two tables
    - Acts as a cross-reference between tables as it references the primary key of another table, thereby establishing a link between them
    - Main purpose of a Foreign Key is to maintain the integrity of data across tables
    - Prevents actions that would destroy links between tables
  - Difference between PK and FK
    - PK is used to ensure data in the specific column is unique and not null in order to identify each record in the table uniquely
    - FK is a set of one or more columns used to ensure the integrity of data and maintain relationships between data columns and tables
    - PK uniquely identifies a record in a table
    - FK in one table points to a PK in another table
    - Table can only have on PK but can have multiple FK
- If you delete the data with a PK of 3 (i.e., the Id is 3), then you want to leave that gap there and you do not want the data with a PK of 4 to decrement to 3
- Jump in PK could be because the SQL Server was turned off -> SQL Server is not meant to be turned off, but this can occur when running things locally and not on a server
  - Log files and database files -> When you insert data into SQL it actually writes it to a log file first, then takes the log file and puts the data into the database
  - Might still have had information in the buffers (i.e., information in the log file that has not been written to database yet) -> Creates gap b/c does not want there to be a conflict as SQL Server is doing everything while running, including placing the data into the database (i.e., it wants to leave enough room and be safe)
  - Should not expect to see in production because you are not shutting the server off and on
- Nickname a table by specifying the nickname after the table -> E.g., `FROM dbo.People ppl`
- Nickname (rename) a column using as -> E.g., `ppl.FirstName AS GivenName`
- Better to specify columns rather than use a `*` because if, at a later time, a column is added to the table, using `*` changes the query without the user being explicit about changing the query -> May not be ideal, especially if an app expects a certain column order
- Specify that something is a column and not a reserved word by using [] around the column name
- T-SQL can modify objects in your database -> Be careful with this
- Wrap queries in Stored Procedures -> Call Stored Procedures (in your code) to do the work
- In prod, you need to have good backups in case someone makes mistakes

## Sample Queries

```
SELECT *
FROM dbo.People
```
- This will return all columns and all rows from the People database
- No limiter to specify the number of rows

```
SELECT *
FROM dbo.People
WHERE State = 'CA';
```
- This will return all columns and limit the rows to those where the state is CA

```
SELECT FirstName, LastName
FROM dbo.People
```
- This will return the FirstName and LastName columns and all rows from the People database

```
SELECT Id, FirstName, LastName
FROM dbo.People
WHERE LastName = 'Smith'
ORDER BY Id DESC;
```
- This will return the specified columns of people whose last name is Smith in descending order by ID

```
SELECT ppl.Id, ppl.FirstName, ppl.LastName, ppl.[State], addr.City
FROM dbo.People ppl
INNER JOIN dbo.Addresses addr
ON ppl.Id = addr.PersonId;
```
- Will only return records where there is an overlap between the People table and the Addresses table

```
INSERT INTO [dbo].[Addresses] (PersonId, City) VALUES 
(1, 'New York'),
(2, 'Los Angeles'),
(3, 'Chicago'),
(4, 'Houston'),
(5, 'Philadelphia'),
(6, 'Phoenix');
```
- Example of inserting data into the Addresses table

```
UPDATE Addresses
SET City = 'New York'
WHERE Id = 2;
```
- Example of updating the City in the Addresses table
- Note: If you do not include the WHERE clause, then all rows will be updated, so be extra careful

```
DELETE FROM Addresses
WHERE Id = 2;
```
- Example of deleting the row in the Addresses table that has an Id of 2
- Note: If you do not include the WHERE clause, then all rows will be deleted, so be extra careful