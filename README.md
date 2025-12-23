# vehicle-rental-database-system

## DataBase System Overview

This project implements a simplified **Vehicle Rental System** using SQL. The system manages users, vehicles, and bookings, allowing retrieval of information about bookings, available vehicles, and vehicle usage statistics. The task focuses on database design, SQL queries, and relational integrity.

The database schema includes the following tables:

- **Users**: Stores user information including role (Admin or Customer), name, email, password, and phone number. Email is unique for each user.
- **Vehicles**: Stores vehicle information including name, type (car/bike/truck), model, registration number, rental price, and availability status. Registration numbers are unique.
- **Bookings**: Stores booking information including the user who made the booking, the vehicle booked, start and end dates, booking status, and total cost.

The relationships are designed to match real-world scenarios:

- **One-to-Many**: A user can have multiple bookings.
- **Many-to-One**: Multiple bookings can refer to the same vehicle.
- **One-to-One (logical)**: Each booking is linked to exactly one user and one vehicle.

Primary keys and foreign keys are implemented to enforce referential integrity.

---

## Database & Design Tools
- **Database**: PostgreSQL
- **Database Tool**: Beekeeper Studio
- **ERD Design Tool**: DrawSQL


## ERD Design Reference
The full database architecture with defined relationships was developed using DrawSQL.

**Database diagram link**: https://drawsql.app/teams/devworkshop/diagrams/vehicle-rental-system-database-design


## SQL Queries

### Query 1: Retrieve booking information with customer name and vehicle name

**Objective**:
Fetch a list of all bookings along with the customer who made the booking and the vehicle involved.

**How it works**:
This query uses INNER JOIN to merge data from the bookings, users, and vehicles tables. The result provides a complete picture of each booking, including:

- Booking ID
- Name of the customer
- Name of the vehicle
- Start and end dates of the booking
-Current booking status

**Example of Query: JOIN**:

```
SELECT
    b.booking_id,
    u.name AS customer_name,
    v.name AS vehicle_name,
    b.start_date,
    b.end_date,
    b.status
FROM bookings b
INNER JOIN users u ON b.U_ID = u.user_id
INNER JOIN vehicles v ON b.vehicle_id = v.vehicle_id;
```
## Query 2: Find vehicles that have never been booked

**Objective**:
Identify vehicles that have never been booked.

**How it works**:
This query uses NOT EXISTS to check for vehicles that do not have matching entries in the bookings table. Each vehicle is tested for the presence of a booking; if none is found, it appears in the output.

**Example of Query: NOT EXISTS**


```
SELECT *
FROM vehicles v
WHERE NOT EXISTS (
    SELECT 1
    FROM bookings b
    WHERE b.vehicle_id = v.vehicle_id
);
```

## Query 3: Retrieve all available cars

**Objective**:
List all vehicles that are cars and currently available for booking.

**How it works**:
The WHERE clause filters the vehicles table to select only records that meet both conditions:

- The vehicle type is 'car'.

- The vehicle status is 'available'.

This ensures the query only returns vehicles that can be booked immediately.

**Example of Query: WHERE**


```
SELECT *
FROM vehicles
WHERE type = 'car'
  AND status = 'available';
```
## Query 4: Find vehicles with more than 2 bookings

**Objective**:
Find vehicles that have been booked more than twice.

**How it works**:
This query joins vehicles and bookings, groups results by vehicle, and counts the number of bookings per vehicle. Using the HAVING clause, it filters out vehicles with 2 or fewer bookings, showing only the most frequently booked vehicles.

**Example of Query: GROUP BY with HAVING**

```
SELECT
    v.name AS vehicle_name,
    COUNT(b.booking_id) AS total_bookings
FROM vehicles v
INNER JOIN bookings b ON v.vehicle_id = b.vehicle_id
GROUP BY v.vehicle_id, v.name
HAVING COUNT(b.booking_id) > 2;
```

## Notes

- All tables are normalized to avoid data duplication.

- Foreign keys ensure referential integrity between bookings, users, and vehicles.


- The queries demonstrate the use of JOIN, EXISTS, WHERE, GROUP BY, and HAVING as required by the Task.
