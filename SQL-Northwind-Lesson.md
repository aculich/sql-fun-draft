# SQL Hands-on Workshop: Northwind Database

## Section 1: Basic SELECT Queries ( 5 mins )

### Concepts
The SELECT statement is used to retrieve data from a single table.

**Basic Syntax:**
```sql
SELECT column1, column2, ... | *
FROM table_name;
```

**Examples:**
```sql
-- Get all columns
SELECT * FROM Customers;

-- Get specific columns
SELECT ProductName, UnitPrice FROM Products;
```

Using the AS keyword, you can give your columns an alias or a custom name

### Practice Questions

1. **Question**: Display all columns and rows from the Products table.
<!-- ```sql
SELECT * FROM Products;
``` -->

2. **Question**: Show only the company name and contact name from the Customers table.
<!-- ```sql
SELECT CompanyName, ContactName 
FROM Customers;
``` -->

3. **Question**: Show the product name, unit price, and units in stock from the Products table.
<!-- ```sql
SELECT ProductName, UnitPrice, UnitsInStock 
FROM Products;
``` -->

## Section 2: Filtering with WHERE Clause ( 15 mins )

### Concepts
The WHERE clause is used to filter records based on specified conditions.

**Basic Syntax:**
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;

-- Common operators:
-- =, >, <, >=, <=, <> (not equal)
-- AND, OR, NOT
-- BETWEEN, IN, LIKE
```
The LIKE keyword although usually slow in processing is a useful one when it comes to string matching. It uses the % to represent multiple character matches and _ to represent a single character match. For instance "%AB%", "%AB%TG%", "AB%" etc 

**Examples:**
```sql
-- Simple condition
SELECT * FROM Products WHERE UnitPrice > 20;

-- Multiple conditions
SELECT * FROM Customers 
WHERE Country = 'Germany' AND City = 'Berlin';

-- Pattern matching
SELECT * FROM Products WHERE ProductName LIKE 'Ch%';

-- IN 
IN ('VALUE1', 'VALUE2')

-- BETWEEN
BETWEEN VALUE1 AND VALUE2

-- NOT (Used to negate a condition)
NOT BETWEEN
NOT IN 

```

### Practice Questions

1. **Question**: Find all products that cost $20 or more. Query the Products table for this and the cost is determined by the UnitPrice column. (Ans: 38 rows)
<!-- ```sql
SELECT ProductName, UnitPrice 
FROM Products 
WHERE UnitPrice >= 20;
``` -->

2. **Question**: From the Customers table, list all customers from Germany. (Ans: 11 rows)
<!-- ```sql
SELECT CompanyName, ContactName, City 
FROM Customers 
WHERE Country = 'Germany';
``` -->

3. **Question**: From the Products table, find all products that cost between $15 and $25 using BETWEEN. (Ans: 25 rows)
<!-- ```sql
SELECT ProductName, UnitPrice 
FROM Products 
WHERE UnitPrice BETWEEN 15 AND 25;
``` -->

4. **Question**: From the Customers table, list all customers from either Paris or London using IN (Ans: 8 rows)
<!-- ```sql
SELECT CompanyName, City, Country 
FROM Customers 
WHERE City IN ('Paris', 'London');
``` -->

5. **Question**: From the Products table, show all products that start with the letter 'C' using the LIKE keyword (Ans: 9 rows)
<!-- ```sql
SELECT ProductName 
FROM Products 
WHERE ProductName LIKE 'C%';
``` -->
6. **Question**: From the Employees table, show all the employees that have "an" in their firstname and were hired before "2014-02-01" (Ans: 3 rows)

<!-- ```sql
SELECT FirstName, LastName, HireDate
FROM Employees
WHERE FirstName LIKE  '%an%' 
AND HireDate < "2014-02-01"
``` -->

7. **Question**: List all customers from the Customers table who have a fax number and live outside the USA. (Ans: 60 rows)
<!-- ```sql
SELECT CustomerID, CompanyName, Fax, Country
FROM Customers
WHERE Fax IS NOT NULL
AND Country <> 'USA';
``` -->

## Section 3: Sorting with ORDER BY ( 10 mins )

### Concepts
The ORDER BY clause is used to sort the result set.

**Basic Syntax:**
```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column1 [ASC|DESC];
```

**Examples:**
```sql
-- Sort by price ascending (default)
SELECT ProductName, UnitPrice 
FROM Products 
ORDER BY UnitPrice;

-- Sort by multiple columns
SELECT ProductName, UnitPrice 
FROM Products 
ORDER BY CategoryID ASC, UnitPrice DESC;
```

### Practice Questions

1. **Question**: From the products table, list all products sorted by price (unit price) from lowest to highest. (min: 2.5, max: 263.5)
<!-- ```sql
SELECT ProductName, UnitPrice 
FROM Products 
ORDER BY UnitPrice ASC;
``` -->

2. **Question**: From the customers table, show all customers sorted alphabetically by company name. (first: Alfreds Futterkiste, last: Wolski  Zajazd)
<!-- ```sql
SELECT CompanyName, ContactName, Country 
FROM Customers 
ORDER BY CompanyName;
``` -->

3. **Question**: From the products table, list products that cost more than $50, sorted by price from highest to lowest. (first: 263.5, last:53)
<!-- ```sql
SELECT ProductName, UnitPrice 
FROM Products 
WHERE UnitPrice > 50 
ORDER BY UnitPrice DESC;
``` -->

4. **Question**: From the products table, list all products with less than 20 units in stock, sorted by units in stock ascending. (first: 0, last:19)
<!-- ```sql
SELECT ProductName, UnitsInStock 
FROM Products 
WHERE UnitsInStock < 20 
ORDER BY UnitsInStock ASC;
``` -->

## Section 4: Aggregate Functions ( 10 mins )

### Concepts
Aggregate functions perform calculations on a set of values and return a single value.

**Common Functions:**
- COUNT(): Counts rows
- SUM(): Adds values
- AVG(): Calculates average
- MAX(): Finds maximum value
- MIN(): Finds minimum value

**Basic Syntax:**
```sql
SELECT COUNT(*) FROM table_name;

SELECT aggregate_function(column_name) FROM table_name;
```

### Practice Questions

1. **Question**: From the Products, what is the average price of all products? (Ans: 28.8663636363636)
<!-- ```sql
SELECT AVG(UnitPrice) as AveragePrice 
FROM Products;
``` -->

2. **Question**: Within the same query what is the lowest and highest product price? Make use of aliases (low:2.5, high: 263.5)
<!-- ```sql
SELECT 
    MIN(UnitPrice) as LowestPrice,
    MAX(UnitPrice) as HighestPrice 
FROM Products;
``` -->

3. **Question**: From the products table, find the total(sum) value of products in stock (UnitPrice * UnitsInStock) for products costing more than $20. (ans: 50157.0)
<!-- ```sql
SELECT SUM(UnitPrice * UnitsInStock) as TotalValue 
FROM Products 
WHERE UnitPrice > 20;
``` -->

4. **Question**: Find the most expensive and least expensive product price for products starting with 'C'. (12.5 and 263.5)
<!-- ```sql
SELECT 
    MIN(UnitPrice) as MinPrice,
    MAX(UnitPrice) as MaxPrice 
FROM Products 
WHERE ProductName LIKE 'C%';
``` -->


## Section 5: GROUP BY and HAVING ( 15 mins )

### Concepts
GROUP BY groups rows that have the same values. HAVING filters groups based on conditions.

**Basic Syntax:**
```sql
SELECT column1, aggregate_function(column2)
FROM table_name
GROUP BY column1
HAVING condition;
```

**Examples:**
```sql
-- Count orders per customer
SELECT CustomerID, COUNT(*) as OrderCount 
FROM Orders 
GROUP BY CustomerID;

-- Find customers with more than 10 orders
SELECT CustomerID, COUNT(*) as OrderCount 
FROM Orders 
GROUP BY CustomerID 
HAVING COUNT(*) > 10;
```

### Practice Questions

1. **Question**: How many products are in each category? Use the categoryID column to identify. (Ans: 8 rows)
<!-- ```sql
SELECT CategoryID, COUNT(*) as ProductCount 
FROM Products 
GROUP BY CategoryID;
``` -->

2. **Question**: Using the Customers table, get the number of customers in each country. (Ans: 22 rows)
<!-- ```sql
SELECT Country, COUNT(*) as CustomerCount 
FROM Customers 
GROUP BY Country;
``` -->

<!-- 3. **Question**: Calculate the average price of products for each supplier.
```sql
SELECT SupplierID, AVG(UnitPrice) as AveragePrice 
FROM Products 
GROUP BY SupplierID;
``` -->

3. **Question**: From the customers table, find the countries that have more than 5 customers. Sort the result by customer count. (Ans: 5 rows)
<!-- ```sql
SELECT Country, COUNT(*) as CustomerCount 
FROM Customers 
GROUP BY Country 
HAVING COUNT(*) > 5 
ORDER BY CustomerCount DESC;
``` -->


<!-- 3. **Question**: List suppliers who supply more than 5 products with the average price per supplier.
```sql
SELECT SupplierID, COUNT(*) as ProductCount, AVG(UnitPrice) as AvgPrice 
FROM Products 
GROUP BY SupplierID 
HAVING COUNT(*) > 5;
``` -->

4. **Question**: From the employees table, get the number of employees in each City and display only those cities with more than 2 employees. (ans: 1 row)
<!-- ```sql
SELECT City, COUNT(*) AS NumberOfEmployees 
FROM Employees 
GROUP BY City 
HAVING COUNT(*) > 2;
``` -->


## Section 6: Joins ( 20 mins )

### Concepts
Joins combine rows from two or more tables based on a related column.

**Types of Joins:**
- INNER JOIN or JOIN: Returns only matching rows
- LEFT JOIN: Returns all records from left table
- RIGHT JOIN: Returns all records from right table
- FULL JOIN: Returns all records from both tables

**Basic Syntax:**
```sql
SELECT columns
FROM table1
JOIN table2 ON table1.column = table2.column;
```

### Practice Questions

1. **Question**: Join the products and categories table. Show product names and their respective category names. (77 rows)
<!-- ```sql
SELECT p.ProductName, c.CategoryName 
FROM Products p
JOIN Categories c ON p.CategoryID = c.CategoryID;
``` -->

2. **Question**: Join the Orders and Customers table. List all orders with customer company names. (16282 rows)
<!-- ```sql
SELECT o.OrderID, c.CompanyName, o.OrderDate 
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID;
``` -->

3. **Question**: Show all customers and their total number of orders, including customers with no orders. Use Customers and Orders table (Hint: use left join + group by) (92 rows)
<!-- ```sql
SELECT c.CompanyName, COUNT(o.OrderID) as OrderCount 
FROM Customers c
LEFT JOIN Orders o ON c.CustomerID = o.CustomerID 
GROUP BY c.CompanyName;
``` -->

4. **Question**: From the Products and Suppliers table, list products with their supplier company names and category names. (77 rows)
<!-- ```sql
SELECT p.ProductName, s.CompanyName as Supplier, c.CategoryName 
FROM Products p
JOIN Suppliers s ON p.SupplierID = s.SupplierID
JOIN Categories c ON p.CategoryID = c.CategoryID;
``` -->



## Section 7: Subqueries (If time persists)

### Concepts
A subquery is a query nested inside another query.

**Basic Syntax:**
```sql
SELECT column
FROM table
WHERE column operator (SELECT column FROM table WHERE condition);
```

### Practice Questions

1. **Question**: Find products more expensive than the average price.
```sql
SELECT ProductName, UnitPrice 
FROM Products 
WHERE UnitPrice > (SELECT AVG(UnitPrice) FROM Products);
```

2. **Question**: List customers who have never placed an order.
```sql
SELECT CompanyName 
FROM Customers 
WHERE CustomerID NOT IN (SELECT DISTINCT CustomerID FROM Orders);
```

3. **Question**: Show products that have a price higher than all products in category 1.
```sql
SELECT ProductName, UnitPrice 
FROM Products 
WHERE UnitPrice > (SELECT MAX(UnitPrice) FROM Products WHERE CategoryID = 1);
```

4. **Question**: Find employees who have handled more orders than the average.
```sql
SELECT e.FirstName, e.LastName, COUNT(*) as OrderCount 
FROM Employees e
JOIN Orders o ON e.EmployeeID = o.EmployeeID 
GROUP BY e.FirstName, e.LastName
HAVING COUNT(*) > (
    SELECT AVG(OrderCount) 
    FROM (
        SELECT EmployeeID, COUNT(*) as OrderCount 
        FROM Orders 
        GROUP BY EmployeeID
    ) as AvgOrders
);
```

5. **Question**: List products that have been ordered more than average order quantity.
```sql
SELECT p.ProductName, AVG(od.Quantity) as AvgQuantity 
FROM Products p
JOIN "Order Details" od ON p.ProductID = od.ProductID 
GROUP BY p.ProductName
HAVING AVG(od.Quantity) > (
    SELECT AVG(Quantity) 
    FROM "Order Details"
);
```
