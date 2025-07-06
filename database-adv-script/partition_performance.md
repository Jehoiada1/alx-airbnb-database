# Table Partitioning Performance Report

**Author:** Yodahe Eshetu  
**Date:** *(Add date here)*

---

## Objective

Implement table partitioning on the large `bookings` table to improve query performance, particularly for queries filtering on the `start_date` column.

---

## Partitioning Implementation

The `bookings` table was partitioned by **range** on the `start_date` column.

### Partitioning SQL (saved in `partitioning.sql`):

```sql
-- Create partitioned bookings table
CREATE TABLE bookings_partitioned (
    id SERIAL PRIMARY KEY,
    user_id INT NOT NULL,
    property_id INT NOT NULL,
    start_date DATE NOT NULL,
    end_date DATE NOT NULL,
    -- other columns...
) PARTITION BY RANGE (start_date);

-- Create partitions for each year (example)
CREATE TABLE bookings_2023 PARTITION OF bookings_partitioned
    FOR VALUES FROM ('2023-01-01') TO ('2024-01-01');

CREATE TABLE bookings_2024 PARTITION OF bookings_partitioned
    FOR VALUES FROM ('2024-01-01') TO ('2025-01-01');

-- Additional partitions can be added as needed
