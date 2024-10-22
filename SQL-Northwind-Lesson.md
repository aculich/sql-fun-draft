# SQL Workshop with the Northwind Database

## Introduction

Welcome to the SQL Workshop! In this two-hour session, we'll dive into the world of SQL using the Northwind database—a classic, professionally curated dataset that simulates a small business's operations. You'll gain hands-on experience querying a real-world database using **SQLite Browser**, a graphical user interface tool that allows you to interact with SQLite databases outside of Jupyter Notebooks.

## Learning Objectives

By the end of this workshop, you will be able to:

- Install and set up SQLite Browser and the Northwind database.
- Navigate and understand the structure of a relational database.
- Write SQL queries to select, filter, and sort data.
- Perform complex joins between tables to retrieve related data.
- Use aggregate functions to summarize data.
- Apply real-world problem-solving skills to extract meaningful insights from the data.

## Prerequisites

- Basic understanding of SQL syntax (e.g., `SELECT`, `FROM`, `WHERE` clauses).
- Familiarity with relational database concepts.

---

## Setup Instructions

### 1. Install SQLite Browser

**SQLite Browser** is a free, open-source tool that allows you to create, design, and edit SQLite database files.

- **For Windows:**
  - Download the installer from the [SQLite Browser Download Page](https://sqlitebrowser.org/dl/).
  - Run the installer and follow the on-screen instructions.

- **For macOS:**
  - Install via Homebrew:
    ```bash
    brew install --cask db-browser-for-sqlite
    ```
    *Or*

  - Download the `.dmg` file from the [SQLite Browser Download Page](https://sqlitebrowser.org/dl/).
  - Open the `.dmg` file and drag the app to your Applications folder.

- **For Linux:**
  - Install via your package manager. For example, on Ubuntu:
    ```bash
    sudo apt-get install sqlitebrowser
    ```

### 2. Download the Northwind Database

- **Download Link:** [Northwind SQLite Database](https://github.com/jpwhite3/northwind-SQLite3/blob/master/Northwind_small.sqlite.zip)
- Extract the downloaded `.zip` file to obtain `Northwind_small.sqlite`.
- Save the database file in a convenient location on your computer.

### 3. Open the Database in SQLite Browser

- Launch **SQLite Browser**.
- Go to `File` > `Open Database`.
- Navigate to where you saved `Northwind_small.sqlite` and open it.

---

## Workshop Exercises

### Exercise 1: Explore the Database Schema

**Objective:** Familiarize yourself with the structure of the Northwind database.

- **Tasks:**
  - Click on the **"Database Structure"** tab in SQLite Browser.
  - Browse through the list of tables (e.g., `Customers`, `Orders`, `Products`, `Employees`, `Suppliers`).
  - For each table, view the schema to understand the columns and data types.
- **Questions:**
  - How many tables are in the database?
  - What are the primary keys of the `Customers` and `Orders` tables?
  - Identify foreign key relationships between tables.

### Exercise 2: Basic SELECT Queries

**Objective:** Practice retrieving data using `SELECT` statements.

- **Tasks:**
  - List all records from the `Customers` table.
    ```sql
    SELECT * FROM Customers;
    ```
  - Retrieve the `ProductName` and `UnitPrice` from the `Products` table.
    ```sql
    SELECT ProductName, UnitPrice FROM Products;
    ```
  - Display the `FirstName` and `LastName` of all employees.
- **Questions:**
  - What is the `CompanyName` of the customer with `CustomerID` 'ALFKI'?
  - How many products are listed in the `Products` table?

### Exercise 3: Filtering Data with WHERE Clause

**Objective:** Use the `WHERE` clause to filter query results.

- **Tasks:**
  - Find all customers located in Germany.
    ```sql
    SELECT * FROM Customers WHERE Country = 'Germany';
    ```
  - List products that are out of stock (`UnitsInStock` = 0).
  - Retrieve orders placed after January 1, 2018.
    ```sql
    SELECT * FROM Orders WHERE OrderDate > '2018-01-01';
    ```
- **Questions:**
  - How many suppliers are from the USA?
  - Which employees have the title "Sales Representative"?

### Exercise 4: Sorting Data with ORDER BY

**Objective:** Learn how to sort query results.

- **Tasks:**
  - Display all products sorted by `UnitPrice` in descending order.
    ```sql
    SELECT * FROM Products ORDER BY UnitPrice DESC;
    ```
  - List customers sorted by `CompanyName` alphabetically.
  - Retrieve the top 5 most expensive products.
- **Questions:**
  - What is the cheapest product available?
  - Which customer comes first when sorted by `CompanyName`?

### Exercise 5: Joining Tables

**Objective:** Combine data from multiple tables using joins.

- **Tasks:**
  - List all orders along with the `CompanyName` of the customer who placed each order.
    ```sql
    SELECT Orders.OrderID, Customers.CompanyName
    FROM Orders
    INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID;
    ```
  - Retrieve a list of products along with their supplier's `CompanyName`.
  - Find all orders processed by the employee with `LastName` 'Fuller'.
- **Questions:**
  - How many orders has each customer placed?
  - Which products are supplied by "Exotic Liquids"?

### Exercise 6: Aggregate Functions and GROUP BY

**Objective:** Use aggregate functions to summarize data.

- **Tasks:**
  - Calculate the total number of orders.
    ```sql
    SELECT COUNT(*) AS TotalOrders FROM Orders;
    ```
  - Find the average `UnitPrice` of all products.
  - Determine the number of customers in each country.
    ```sql
    SELECT Country, COUNT(*) AS NumberOfCustomers
    FROM Customers
    GROUP BY Country;
    ```
- **Questions:**
  - Which country has the most customers?
  - What is the total revenue generated from all orders? *(Hint: You'll need to join `OrderDetails` and `Orders` tables and use `SUM`.)*

### Exercise 7: Advanced Joins and Subqueries

**Objective:** Solve complex queries using advanced SQL techniques.

- **Tasks:**
  - List employees and the total number of orders they have processed.
    ```sql
    SELECT Employees.LastName, COUNT(Orders.OrderID) AS NumberOfOrders
    FROM Employees
    LEFT JOIN Orders ON Employees.EmployeeID = Orders.EmployeeID
    GROUP BY Employees.LastName;
    ```
  - Find products that have never been ordered.
  - Retrieve customers who have not placed any orders in 2019.
- **Questions:**
  - Which employee has processed the most orders?
  - Are there any products supplied by multiple suppliers?

### Exercise 8: Using Views (Optional)

**Objective:** Understand and create SQL views for recurring queries.

- **Tasks:**
  - Create a view that lists the `ProductName`, `CategoryName`, and `SupplierName` for all products.
    ```sql
    CREATE VIEW ProductDetails AS
    SELECT Products.ProductName, Categories.CategoryName, Suppliers.CompanyName AS SupplierName
    FROM Products
    INNER JOIN Categories ON Products.CategoryID = Categories.CategoryID
    INNER JOIN Suppliers ON Products.SupplierID = Suppliers.SupplierID;
    ```
  - Query the `ProductDetails` view to find all products in the "Beverages" category.
- **Questions:**
  - What are the benefits of using views in SQL?
  - Can you update data through a view?

### Exercise 9: Modifying Data (Caution Advised)

**Objective:** Practice inserting, updating, and deleting data.

- **Tasks:**
  - **Insert:** Add a new customer to the `Customers` table.
    ```sql
    INSERT INTO Customers (CustomerID, CompanyName, ContactName, Country)
    VALUES ('NEWID', 'New Company', 'John Doe', 'USA');
    ```
  - **Update:** Change the `ContactTitle` of 'John Doe' to 'Sales Manager'.
    ```sql
    UPDATE Customers
    SET ContactTitle = 'Sales Manager'
    WHERE CustomerID = 'NEWID';
    ```
  - **Delete:** Remove a discontinued product from the `Products` table.
    ```sql
    DELETE FROM Products
    WHERE Discontinued = 1;
    ```
- **Caution:** These changes are permanent in your local database. Consider making a backup before performing these operations.

### Exercise 10: Real-World Problem Solving

**Objective:** Apply your SQL skills to solve business problems.

- **Scenario:** The company wants to offer a discount to customers who have placed more than 10 orders.

- **Tasks:**
  - Identify customers with more than 10 orders.
  - Calculate the total sales amount for each of these customers.
  - Suggest a discount rate based on their total sales (e.g., 5% for sales over $10,000).
- **Questions:**
  - How would offering discounts impact customer loyalty?
  - What other strategies can the company use to increase sales?

---

## Conclusion

Congratulations on completing the workshop! You've gained practical experience working with a real-world database and learned how to perform various SQL operations, from basic queries to complex joins and data manipulation. These skills are invaluable for working with databases in a professional setting.

---

## Additional Resources

- **SQLite Documentation:** [https://www.sqlite.org/docs.html](https://www.sqlite.org/docs.html)
- **SQL Tutorial:** [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)
- **SQLite Browser Wiki:** [SQLite Browser GitHub Wiki](https://github.com/sqlitebrowser/sqlitebrowser/wiki)
- **Northwind Database Schema:** [Schema Diagram](https://github.com/jpwhite3/northwind-SQLite3/blob/master/Northwind_ERD.png)

---

Feel free to explore further and don't hesitate to ask questions. Happy querying!
