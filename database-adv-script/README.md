# SQL Joins Practice

This project contains SQL queries to help understand and practice different types of **SQL JOINs**, including:

---

## 📁 File: `joins_queries.sql`

This file includes:

### 1️⃣ INNER JOIN – Bookings with Users

> Retrieves all bookings along with the user information of those who made them.

```sql
SELECT 
    bookings.id AS booking_id,
    bookings.date,
    users.id AS user_id,
    users.name,
    users.email
FROM 
    bookings
INNER JOIN 
    users ON bookings.user_id = users.id;
