# Little Lemon Database Capstone Project

![Meta Database Engineer](https://tinypic.host/images/2024/10/26/Meta-Database-Engineer.png)

## Overview

This project is part of the Meta Database Engineer Professional Certificate program. It involves designing and implementing a database system for the fictional restaurant "Little Lemon." The project encompasses database modeling, SQL query creation, and data analysis using Tableau, providing insights into business performance and customer behavior.

## Project Components

1. **Database Design and Implementation**
   - Developed a logical data model representing the key entities and their relationships.
   - Deployed the physical data model in MySQL using the Forward Engineer method in MySQL Workbench.

2. **SQL Queries**
   - Created various SQL queries for data management and summarization, including:
     - Creating views for simplified data access
     - Using JOINs to extract comprehensive data from multiple tables
     - Defining stored procedures for booking management

3. **Data Analytics**
   - Utilized Tableau for data visualization, creating various charts and dashboards that provide insights into sales trends, customer behavior, and cuisine performance.

## Little Lemon Data Model

The data model visually represents the entities involved in the Little Lemon database and their relationships. Key components include:

- **Entities**:
  - **Customers**: Information about the restaurant's patrons.
  - **Orders**: Details of customer orders.
  - **Menus**: Items available for order.
  - **MenuItems**: Specific items within each menu.
  - **Bookings**: Reservations made by customers.

### Data Model Characteristics

- **Attributes**: Each entity has defined attributes that describe its properties.
- **Primary Keys**: Unique identifiers for each record within a table.
- **Foreign Keys**: References that establish relationships between tables, ensuring data integrity.

### ER Diagram

![ER Diagram](https://github.com/Willie-Conway/Meta-Database-Capstone-Project/blob/main/LittleLemonDM.png) <!-- Replace with your actual image path -->

## Deploying the Data Model in MySQL

To deploy the data model in MySQL, follow these steps:

1. **Create a New Model**: Open MySQL Workbench and create a new model.
2. **Forward Engineer**: Use the Forward Engineer feature to generate the SQL schema based on the physical data model.
3. **Execute SQL**: Run the generated SQL script in your MySQL server to create the Little Lemon schema.

## SQL Queries

### Task 1: Create a Virtual Table

To simplify order management, we create a view that focuses on orders with a quantity greater than 2.

```sql
CREATE VIEW OrdersView AS
SELECT OrderID, Quantity, Cost
FROM orders
WHERE Quantity > 2;
```

### Task 2: Extract Customer and Order Information

This query retrieves information about customers with orders exceeding $150, using multiple JOIN clauses to combine data from several tables.

```sql
SELECT customers.CustomerID, customers.FullName, orders.OrderID, orders.Cost, 
       menus.MenuName, menuitems.CourseName
FROM customers
INNER JOIN orders ON customers.CustomerID = orders.CustomerID
INNER JOIN menus ON orders.MenuID = menus.MenuID
INNER JOIN menuitems ON menuitems.MenuItemID = menus.MenuItemsID
WHERE Cost > 150
ORDER BY Cost;

```

### Task 3: Create a Stored Procedure to Get Maximum Quantity

This stored procedure retrieves the maximum quantity ordered in the orders table.

```sql
CREATE PROCEDURE GetMaxQuantity()
BEGIN
    SELECT MAX(Quantity) AS "Max Quantity in Order" FROM orders;
END;

```

### Task 4: Booking Procedures

Several stored procedures were created to manage bookings:

`MakeBooking()`
Inserts a new booking into the database.

```sql
CREATE PROCEDURE MakeBooking(IN booking_id INT, IN customer_id INT, IN table_no INT, IN booking_date DATE)
BEGIN
    INSERT INTO bookings (BookingID, BookingDate, TableNumber, CustomerID)
    VALUES (booking_id, booking_date, table_no, customer_id);
    SELECT "New booking added" AS "Confirmation";
END;

```

`CheckBooking()`
Verifies if a specific table is booked on a given date.

```sql
CREATE PROCEDURE CheckBooking(IN booking_date DATE, IN table_number INT)
BEGIN
    DECLARE bookedTable INT DEFAULT 0;
    SELECT COUNT(*) INTO bookedTable
    FROM Bookings
    WHERE BookingDate = booking_date AND TableNumber = table_number;

    IF bookedTable > 0 THEN
        SELECT CONCAT("Table ", table_number, " is already booked") AS "Booking status";
    ELSE
        SELECT CONCAT("Table ", table_number, " is not booked") AS "Booking status";
    END IF;
END;

```

`UpdateBooking()`
Updates an existing booking’s date.

```sql
CREATE PROCEDURE UpdateBooking(IN booking_id INT, IN booking_date DATE)
BEGIN
    UPDATE bookings 
    SET BookingDate = booking_date 
    WHERE BookingID = booking_id;
    SELECT CONCAT("Booking ", booking_id, " updated") AS "Confirmation";
END;

```

## Data Analytics with Tableau

### Task 1: Customer Sales Bar Chart

Created a bar chart visualizing customer sales for amounts over $70.

### Task 2: Sales Trend Line Chart

Displayed the sales trend from 2019 to 2022, showcasing overall performance changes.

### Task 3: Sales Bubble Chart

Developed a bubble chart representing sales data, with customer names and profit information displayed on hover.

### Task 4: Cuisine Sales Comparison

Compared sales data for Turkish, Italian, and Greek cuisines from 2020 to 2022, illustrating sales and profits.

### Task 5: Interactive Dashboard

Created an interactive dashboard combining the bar and bubble charts, allowing users to filter data dynamically.


## Client Project Setup with Python

### Setup Steps

### 1. Import MySQL Connector

```sql

import mysql.connector as connector

```

### 2. Connect to the Database

```sql

connection = connector.connect(user="mario", password="cuisine")

```

### 3. Create a Cursor

```sql

cursor = connection.cursor()

```

### 4. Set Database Context

```sql

cursor.execute("USE little_lemon")

```

### 5. Execute Join Query Example

This query retrieves booking and order details for orders with a bill amount greater than $60.

```sql
join_query = """
SELECT Bookings.BookingID, Bookings.TableNO, Bookings.GuestFirstName, Orders.BillAmount AS TotalCost
FROM Bookings
LEFT JOIN Orders ON Bookings.BookingID = Orders.BookingID
WHERE Orders.BillAmount > 60
"""
cursor.execute(join_query)
results = cursor.fetchall()
print(cursor.column_names)
print(results)

```

## Conclusion

This capstone project for Little Lemon integrates database design, SQL implementation, and data analytics. The insights derived from this project are valuable for enhancing operational decisions and strategies within the restaurant.

## Acknowledgments

* Thanks to **Meta** for the training and resources provided.
* Special gratitude to mentors and peers for their support and collaboration throughout the project.

