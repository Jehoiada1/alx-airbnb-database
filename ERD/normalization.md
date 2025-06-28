# Database Normalization to 3NF

## Normalized Schema (3NF Compliant)

### Tables

1. **User**
   - `user_id` (PK, UUID)
   - `first_name` (VARCHAR, NOT NULL)
   - `last_name` (VARCHAR, NOT NULL)
   - `email` (VARCHAR, UNIQUE, NOT NULL)
   - `password_hash` (VARCHAR, NOT NULL)
   - `phone_number` (VARCHAR, NULLABLE)
   - `role_id` (FK → UserRole, NOT NULL)
   - `created_at` (TIMESTAMP, DEFAULT NOW())

2. **UserRole**
   - `role_id` (PK, UUID)
   - `role_name` (ENUM: 'guest', 'host', 'admin')
   - `permissions` (JSON)

3. **Property**
   - `property_id` (PK, UUID)
   - `host_id` (FK → User, NOT NULL)
   - `name` (VARCHAR, NOT NULL)
   - `description` (TEXT, NOT NULL)
   - `address_line1` (VARCHAR, NOT NULL)
   - `address_line2` (VARCHAR, NULLABLE)
   - `city` (VARCHAR, NOT NULL)
   - `state` (VARCHAR, NOT NULL)
   - `postal_code` (VARCHAR, NOT NULL)
   - `country` (VARCHAR, NOT NULL)
   - `base_price` (DECIMAL, NOT NULL)
   - `created_at` (TIMESTAMP, DEFAULT NOW())
   - `updated_at` (TIMESTAMP, ON UPDATE NOW())

4. **Pricing**
   - `pricing_id` (PK, UUID)
   - `property_id` (FK → Property, NOT NULL)
   - `start_date` (DATE, NOT NULL)
   - `end_date` (DATE, NOT NULL)
   - `price_per_night` (DECIMAL, NOT NULL)

5. **Booking**
   - `booking_id` (PK, UUID)
   - `property_id` (FK → Property, NOT NULL)
   - `user_id` (FK → User, NOT NULL)
   - `start_date` (DATE, NOT NULL)
   - `end_date` (DATE, NOT NULL)
   - `status` (ENUM: 'pending', 'confirmed', 'canceled')
   - `created_at` (TIMESTAMP, DEFAULT NOW())

6. **Payment**
   - `payment_id` (PK, UUID)
   - `booking_id` (FK → Booking, NOT NULL)
   - `amount` (DECIMAL, NOT NULL)
   - `payment_date` (TIMESTAMP, DEFAULT NOW())
   - `payment_method` (ENUM: 'credit_card', 'paypal', 'stripe')

7. **Review**
   - `review_id` (PK, UUID)
   - `property_id` (FK → Property, NOT NULL)
   - `user_id` (FK → User, NOT NULL)
   - `rating` (INTEGER, CHECK: 1-5, NOT NULL)
   - `comment` (TEXT, NOT NULL)
   - `created_at` (TIMESTAMP, DEFAULT NOW())

8. **Message**
   - `message_id` (PK, UUID)
   - `sender_id` (FK → User, NOT NULL)
   - `recipient_id` (FK → User, NOT NULL)
   - `message_body` (TEXT, NOT NULL)
   - `sent_at` (TIMESTAMP, DEFAULT NOW())

## Normalization Steps

1. **Eliminated Calculated Field**:
   - Removed `total_price` from Booking (can be calculated from Pricing table)

2. **Address Normalization**:
   - Split `location` into atomic address components

3. **Role Normalization**:
   - Created separate UserRole table for role-based permissions

4. **Dynamic Pricing**:
   - Added Pricing table to handle seasonal rates

## Verification of 3NF

- All non-key attributes depend **only on the primary key**
- No transitive dependencies exist
- All tables represent single logical entities

## Indexes Recommended

1. `User(email)`
2. `Property(host_id)`
3. `Pricing(property_id, start_date, end_date)`
4. `Booking(property_id, user_id)`
5. `Message(sender_id, recipient_id)`
