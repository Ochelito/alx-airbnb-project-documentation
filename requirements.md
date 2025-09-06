# Backend Requirements Specifications

This document defines the functional and technical requirements for three key backend features of the Airbnb Clone: User Authentication, Property Management, and Booking System.

---

## 1. User Authentication

### Functional Requirements
- Users must be able to register using an email and password.
- Users must be able to log in securely using JWT (JSON Web Token) authentication.
- OAuth login (Google, Facebook) should be supported.
- Users should be able to update their profile information (name, photo, contact details).

### API Endpoints
- `POST /api/v1/auth/register` → Register new users.
- `POST /api/v1/auth/login` → Authenticate users and return JWT.
- `GET /api/v1/auth/profile` → Fetch user profile (requires JWT).
- `PUT /api/v1/auth/profile` → Update user profile.

### Input/Output
**Register Input:**
```json
{
  "email": "user@example.com",
  "password": "securePassword123",
  "role": "guest"
}
```
**Register Output:**
```json
{
  "id": "uuid",
  "email": "user@example.com",
  "role": "guest",
  "created_at": "2025-09-06T10:00:00Z"
}
```

### Validation Rules
- Email must be unique and valid format.
- Password must have at least 8 characters, one uppercase letter, one number, and one symbol.
- Role must be either "guest" or "host".

### Performance Criteria
- Authentication requests must complete in under 200ms.
- JWT tokens must expire after a configurable duration (e.g., 24 hours).

---

## 2. Property Management

### Functional Requirements
- Hosts must be able to create, edit, and delete property listings.
- Listings must include title, description, location, price, amenities, and availability dates.
- Guests should be able to view listings with search and filters.

### API Endpoints
- `POST /api/v1/properties` → Create a new property (host only).
- `GET /api/v1/properties` → Retrieve all properties with optional filters (location, price, amenities).
- `GET /api/v1/properties/{id}` → Retrieve a single property by ID.
- `PUT /api/v1/properties/{id}` → Update property details (host only).
- `DELETE /api/v1/properties/{id}` → Remove a property (host only).

### Input/Output
**Create Property Input:**
```json
{
  "title": "Cozy Apartment",
  "description": "A nice 2-bedroom apartment",
  "location": "Lagos, Nigeria",
  "price": 75.50,
  "amenities": ["WiFi", "Pool", "Air Conditioning"],
  "availability": ["2025-09-10", "2025-09-15"]
}
```

**Create Property Output:**
```json
{
  "id": "uuid",
  "title": "Cozy Apartment",
  "host_id": "uuid",
  "created_at": "2025-09-06T10:15:00Z"
}
```

### Validation Rules
- Title max length: 100 characters.
- Price must be positive.
- Availability dates must be valid and not overlap.

### Performance Criteria
- Property search results should return within 300ms for up to 10,000 listings.

---

## 3. Booking System

### Functional Requirements
- Guests can create bookings for properties if available.
- Double bookings must be prevented.
- Guests and hosts can cancel bookings based on policy.
- Booking statuses must be tracked: pending, confirmed, canceled, completed.

### API Endpoints
- `POST /api/v1/bookings` → Create a booking (guest only).
- `GET /api/v1/bookings` → Retrieve all bookings for a user.
- `PUT /api/v1/bookings/{id}/cancel` → Cancel a booking.
- `GET /api/v1/bookings/{id}` → Retrieve booking details.

### Input/Output
**Create Booking Input:**
```json
{
  "property_id": "uuid",
  "guest_id": "uuid",
  "check_in": "2025-09-12",
  "check_out": "2025-09-15"
}
```

**Create Booking Output:**
```json
{
  "id": "uuid",
  "property_id": "uuid",
  "guest_id": "uuid",
  "status": "pending",
  "created_at": "2025-09-06T10:30:00Z"
}
```

### Validation Rules
- Booking dates must not overlap with existing confirmed bookings.
- Check-in must be before check-out.
- Only the guest who created the booking or the host can cancel.

### Performance Criteria
- Booking creation must process in under 400ms.
- The system must handle at least 100 concurrent booking requests.

---

## Summary
This requirements document ensures clear technical specifications for the Airbnb Clone backend, covering authentication, property management, and booking workflows with security, performance, and validation requirements.
