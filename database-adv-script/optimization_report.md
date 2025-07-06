# Query Optimization Report

**Author:** Yodahe Eshetu  
**Date:** *(Add date here)*

---

## Objective

Optimize the query that retrieves all bookings with user, property, and payment details to improve execution performance.

---

## Initial Query

```sql
SELECT 
    b.id AS booking_id,
    b.date AS booking_date,
    u.id AS user_id,
    u.name AS user_name,
    u.email AS user_email,
    p.id AS property_id,
    p.name AS property_name,
    p.location AS property_location,
    pay.id AS payment_id,
    pay.amount AS payment_amount,
    pay.status AS payment_status
FROM 
    bookings b
JOIN 
    users u ON b.user_id = u.id
JOIN 
    properties p ON b.property_id = p.id
LEFT JOIN 
    payments pay ON pay.booking_id = b.id
ORDER BY 
    b.date DESC;
