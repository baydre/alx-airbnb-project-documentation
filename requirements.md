
# 🧾 Backend Requirement Specifications

This document outlines the technical and functional requirements for core backend features of the ALX Airbnb Clone project.

---

## 🧱 Feature: User Authentication

### 1. Description
Provides user registration and login capabilities. Generates tokens to authorize protected routes and track authenticated sessions.

### 2. Functional Requirements
- Users can register with email and password.
- Users can log in to receive a JWT token.
- Only authenticated users can create listings or make bookings.

### 3. API Endpoints

| Method | Endpoint        | Description            | Auth Required |
|--------|------------------|------------------------|----------------|
| POST   | `/api/register` | Register a new user    | No             |
| POST   | `/api/login`    | Log in and get token   | No             |

### 4. Request/Response Examples

#### Register Request

```json
POST /api/register
{
  "email": "user@example.com",
  "password": "strongpassword"
}
```

#### Register Response

```json
{
  "id": 1,
  "email": "user@example.com",
  "token": "jwt_token_here"
}
```

### 5. Validation Rules
- Email must be unique and in valid format.
- Password must be at least 6 characters long.
- Tokens expire after a defined session period (e.g., 24h).

---

## 🧱 Feature: Property Management

### 1. Description
Hosts can add, update, and delete property listings. Guests can view and filter property listings.

### 2. Functional Requirements
- Hosts can create a listing with title, location, price, description, and availability.
- Guests can view and search properties.

### 3. API Endpoints

| Method | Endpoint              | Description               | Auth Required |
|--------|------------------------|---------------------------|----------------|
| POST   | `/api/properties`     | Add new property          | Yes            |
| PUT    | `/api/properties/:id` | Update property info      | Yes            |
| DELETE | `/api/properties/:id` | Remove property listing   | Yes            |
| GET    | `/api/properties`     | List all available props  | No             |

### 4. Request/Response Examples

#### Add Property Request

```json
POST /api/properties
{
  "title": "Ocean View Apartment",
  "description": "2-bedroom with sea view",
  "location": "Lagos, Nigeria",
  "price_per_night": 45.00,
  "available_from": "2025-06-01",
  "available_to": "2025-07-01"
}
```

#### Response

```json
{
  "id": 12,
  "title": "Ocean View Apartment",
  "status": "listed"
}
```

### 5. Validation Rules
- `title`, `location`, `price_per_night` are required.
- `price_per_night` must be > 0.
- `available_from` and `available_to` must be future dates.
- Only the property owner can update or delete the listing.

---

## 🧱 Feature: Booking System

### 1. Description
Allows guests to book available properties, ensuring date availability, payment processing, and booking status management.

### 2. Functional Requirements
- Guests can request bookings on available properties.
- Bookings are not allowed if dates overlap with existing confirmed bookings.
- Payment must be confirmed before booking is marked as confirmed.

### 3. API Endpoints

| Method | Endpoint           | Description            | Auth Required |
|--------|---------------------|------------------------|----------------|
| POST   | `/api/bookings`    | Create a booking       | Yes            |
| GET    | `/api/bookings/:id`| Get booking details    | Yes            |

### 4. Request/Response Examples

#### Booking Request

```json
POST /api/bookings
{
  "property_id": 22,
  "start_date": "2025-06-10",
  "end_date": "2025-06-13"
}
```

#### Booking Response

```json
{
  "booking_id": 47,
  "status": "pending_payment",
  "total_amount": 135.00
}
```

### 5. Validation Rules
- `start_date` must be before `end_date`.
- No overlap with existing bookings.
- Guests cannot book their own property.
- Booking can only be confirmed after successful payment.

---

## ✅ Additional Notes

- All authenticated routes should implement JWT middleware.
- Proper error handling should return `400`, `401`, `403`, or `500` as appropriate.
- Database constraints should enforce uniqueness and relational integrity.

---

> Documentation maintained by Backend Team — ALX Airbnb Clone Project. Last updated: May 2025.
