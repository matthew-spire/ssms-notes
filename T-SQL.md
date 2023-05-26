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
  - *
    - Returns all columns
    - Usage is not necessarily best practice
  - FROM
    - Used in conjunction with SELECT to specify from which table in the database you want to select data
    - Name of the table follows the FROM keyword
  - WHERE
    - Used to filter records that fulfill a specified condition -> Extracts only those records that fulfill the condition specified by the WHERE clause
    - Not limited to only one condition
      -  Use logical operators like AND, OR, NOT, and comparison operators like =, <, >, >=, <=, <>, LIKE, IN, BETWEEN, etc. to form more complex conditions

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