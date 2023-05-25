# SQL Server Management Studio (SSMS) Notes

## Overview of SSMS
- SSMS is a tool to work with Microsoft SQL Server, similar to Visual Studio.
- Everything in SSMS and SQL Server is built off T-SQL.
- SSMS doesn't come with the database; it's a separate tool for working with the database.

## Setting up SSMS

- Use Visual Studio to install database tools and functionality.
  - Go to View > SQL Server Object Explorer for database options.
  - Tools > Get Tools and Features > Data storage and processing to add database tools if needed.
- `localdb` is a database file used to work with SSMS or T-SQL.
  - Can be used like a real database.
  - Great for testing and small deployments.
- For bigger database requirements, consider using SQL Server on Azure, SQL Server at the edge, or SQL Server on-prem.
  - Download the Developer free specialized edition for full features.
  - SQL Server consumes considerable resources, designed to optimize database speed.
- SQL Server (2019) can also be run in a Docker container to manage system resources more effectively.

## Using SSMS

1. Open SSMS and click "Connect to Server".
    - Server name will depend on whether you're using SQL Server or `localdb`.
    - Use "Windows Authentication" for local operations and "SQL Server Authentication" for non-local operations.
    - Click "Connect" to establish a connection to the server.

2. In the "Object Explorer", you'll see a list of folders showing connected SQL Servers.

## Working with Databases in SSMS

- Under the "Databases" folder, you can see the available databases or create a new one.
- When you create a database it creates two types of files: a data file and a log file.
- Right-click > "New Database..." to create a new database.

### Database Structure

- Each database contains multiple folders like "Tables", "Views", "Programmability" etc.
- The "Tables" folder is where we store data in the database.
  - You can create a new table here.
- "Views" allow you to create queries that can join multiple tables.
- "Programmability" > "Stored Procedures" for action queries like INSERT, UPDATE, DELETE.

### Interacting with a Database

- Right-click on a database to access different options.
- Design: Look at and change the design of the database.
- Select Top 1000 Rows: See what data is present in the table.
- Edit Top 200 Rows: Make changes to the data in those rows (should not be used in production).
- Scripting: CREATE To and DROP To scripts can be used to manipulate the database.

## Writing and Running Queries

- Ensure you are using the correct database for your query (check the upper left of the screen).
- Use `dbo` (the owner) to specify the path in your queries.
- You can query from different databases at the same time.
- Be careful when running DELETE queries, especially with a WHERE clause.
- Options like Display Estimated Execution Plan can offer insights into query efficiency.

## Sample Queries

```
-- Select the database to use for the subsequent statements
USE [MattTest]
GO

-- Start of object creation script for Table [dbo].[People]
/****** Object:  Table [dbo].[People]    Script Date: 5/25/2023 12:00:57 PM ******/

-- Enforce the SQL-92 standard requiring that an equals (=) or not equal to (<>) comparison against a null value returns unknown
SET ANSI_NULLS ON
GO

-- Causes SQL Server to follow the SQL-92 rules regarding how to interpret double quotation marks 
SET QUOTED_IDENTIFIER ON
GO

-- Begin the creation of the table
CREATE TABLE [dbo].[People](
	[Id] [int] IDENTITY(1,1) NOT NULL, -- IDENTITY sets Id as auto-incrementing
	[FirstName] [nvarchar](50) NOT NULL, -- First name cannot be null
	[LastName] [nvarchar](50) NOT NULL, -- Last name cannot be null
	[State] [nvarchar](50) NOT NULL, -- State cannot be null
PRIMARY KEY CLUSTERED 
(
	[Id] ASC -- Id is the primary key and is clustered, meaning physical ordering of rows in the table is based on this
)WITH (
    PAD_INDEX = OFF, -- Index pages aren't padded to a certain factor
    STATISTICS_NORECOMPUTE = OFF, -- Statistics are recomputed on index columns when necessary
    IGNORE_DUP_KEY = OFF, -- Duplicate keys aren't allowed
    ALLOW_ROW_LOCKS = ON, -- Row-level locking is allowed
    ALLOW_PAGE_LOCKS = ON, -- Page-level locking is allowed
    OPTIMIZE_FOR_SEQUENTIAL_KEY = OFF -- Optimization for sequential key access is off
    ) ON [PRIMARY] -- Specifies the filegroup for the index
) ON [PRIMARY] -- Specifies the filegroup for the table
GO -- End of statement delimiter
```

```
INSERT INTO [dbo].[Employees] (FirstName, LastName, State)
VALUES ('John', 'Doe', 'CA'),
       ('Jane', 'Smith', 'ME'),
       ('Bob', 'Johnson', 'NY');
```

```
SELECT *
FROM dbo.People -- Could just use People, but using dbo. specifies the owner
```
- Does a Clustered Index Scan -> Looks through all records (simple query on a small amount of data, so no need to be super efficient)
- Clustered Index Seek -> Go to particular record using the PK