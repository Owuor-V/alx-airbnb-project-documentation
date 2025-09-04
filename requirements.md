# Backend Features Requirements – ALX Airbnb Project

This document defines the **technical** and **functional requirements** for three key backend features of the Airbnb Clone: **User Authentication**, **Property Management**, and **Booking System**.

---

## 1. User Authentication

### Description
Provides secure registration, login, and session management for users. Ensures only verified users can access protected resources.

### API Endpoints
- **POST** `/api/auth/register` – Register a new user  
- **POST** `/api/auth/login` – Authenticate user and issue JWT token  
- **GET** `/api/auth/profile` – Retrieve logged-in user profile (JWT required)

### Input/Output Specifications

**Register**

- Input:
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "P@ssword123"
  }
Output (Success):

json
Copy code
{
  "message": "User registered successfully",
  "userId": "12345"
}
Login

Input:

json
Copy code
{
  "email": "john@example.com",
  "password": "P@ssword123"
}
Output (Success):

json
Copy code
{
  "token": "jwt-token-here",
  "expiresIn": 3600
}
Validation Rules
Email

Must be unique and valid format.

Password

Minimum 8 characters.

At least 1 uppercase, 1 lowercase, 1 digit, and 1 special character.

Failure Handling

Return 400 Bad Request with details if validation fails.

Performance Criteria
Authentication requests must complete in ≤ 500 ms under normal load.

Token validation should be stateless (via JWT).

2. Property Management
Description
Allows hosts to create, update, and manage their property listings (apartments, houses, rooms).

API Endpoints
POST /api/properties – Create a new property listing (Host only)

GET /api/properties/{id} – Retrieve property details

PUT /api/properties/{id} – Update property details (Host only)

DELETE /api/properties/{id} – Delete a property (Host only)

GET /api/properties – Browse all available properties with filters

Input/Output Specifications
Create Property

Input:

json
Copy code
{
  "title": "Cozy Studio Apartment",
  "description": "Modern studio in the city center",
  "location": "Nairobi, Kenya",
  "pricePerNight": 50,
  "amenities": ["WiFi", "Air Conditioning", "Kitchen"]
}
Output (Success):

json
Copy code
{
  "message": "Property created successfully",
  "propertyId": "prop-001"
}
Validation Rules
Title must not exceed 100 characters.

Price must be a positive number.

Amenities must be provided as a list of strings.

Only authenticated hosts can create/update/delete properties.

Performance Criteria
Must support filtering by location, price, availability, and amenities.

Property retrieval requests should return results in ≤ 1 second for queries ≤ 1000 records.

3. Booking System
Description
Enables users (guests) to book properties, manage bookings, and check availability.

API Endpoints
POST /api/bookings – Create a new booking (Guest only)

GET /api/bookings/{id} – Get booking details

PUT /api/bookings/{id}/cancel – Cancel a booking

GET /api/bookings/user/{userId} – Retrieve all bookings for a user

Input/Output Specifications
Create Booking

Input:

json
Copy code
{
  "propertyId": "prop-001",
  "checkInDate": "2025-09-10",
  "checkOutDate": "2025-09-15",
  "guests": 2
}
Output (Success):

json
Copy code
{
  "message": "Booking confirmed",
  "bookingId": "book-123",
  "totalPrice": 250
}
Validation Rules
Dates must be valid and checkOutDate > checkInDate.

Property must be available for the selected date range.

Guests must not exceed property’s max capacity.

Performance Criteria
Booking confirmation must complete in ≤ 800 ms.

Handle concurrent bookings with transaction safety (prevent double-booking).

General Requirements
All endpoints must use RESTful conventions.

API communication must use HTTPS + JSON.

Error responses should follow a consistent format:

json
Copy code
{
  "error": "Validation Error",
  "details": "Email is already in use"
}
System must support at least 500 concurrent users with response times under 2 seconds.
