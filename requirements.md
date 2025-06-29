
# 📄 Airbnb Clone - Backend Feature Requirements

This document outlines the **technical** and **functional** requirements for three key backend features of the Airbnb Clone project:

- User Authentication  
- Property Management  
- Booking System  

---

## 1. 🔐 User Authentication

### ✅ Functional Requirements
- Users can register as either **Guest** or **Host**
- Users can log in using:
  - Email and Password
  - OAuth (Google, Facebook)
- Sessions are managed using secure JWTs (JSON Web Tokens)
- Passwords are securely hashed using bcrypt

### 🌐 API Endpoints

#### `POST /api/auth/register`
- **Description**: Register a new user
- **Input**:
  ```json
  {
    "email": "user@example.com",
    "password": "securepassword",
    "role": "guest", // or "host"
    "full_name": "John Doe"
  }
  ```
- **Output**:
  ```json
  {
    "message": "User registered successfully",
    "token": "<JWT>"
  }
  ```
- **Validation Rules**:
  - `email`: must be valid and unique
  - `password`: minimum 8 characters
  - `role`: either `guest` or `host`

---

#### `POST /api/auth/login`
- **Description**: Log in a user
- **Input**:
  ```json
  {
    "email": "user@example.com",
    "password": "securepassword"
  }
  ```
- **Output**:
  ```json
  {
    "token": "<JWT>",
    "user": {
      "id": "uuid",
      "role": "guest"
    }
  }
  ```

---

### 🧪 Performance Criteria
- Login response time ≤ 300ms
- JWT expires after 24 hours
- Rate limiting: max 5 failed login attempts per 15 minutes per IP

---

## 2. 🏡 Property Management

### ✅ Functional Requirements
- Hosts can create, update, and delete property listings
- Properties include:
  - Title, Description, Price, Location
  - Availability range, Amenities, Images

### 🌐 API Endpoints

#### `POST /api/properties`
- **Authentication**: Required (Host)
- **Input**:
  ```json
  {
    "title": "Modern Loft in NYC",
    "description": "A cozy loft with skyline views.",
    "price": 120,
    "location": "New York, NY",
    "amenities": ["WiFi", "AC"],
    "available_from": "2025-07-01",
    "available_to": "2025-08-31",
    "images": ["url1", "url2"]
  }
  ```
- **Output**:
  ```json
  {
    "message": "Property created successfully",
    "property_id": "uuid"
  }
  ```

- **Validation Rules**:
  - `price`: must be a positive number
  - `available_from` and `available_to`: valid and `from < to`
  - Only authenticated hosts can create properties

---

#### `PUT /api/properties/:id`
- **Description**: Update a property (Host only)

#### `DELETE /api/properties/:id`
- **Description**: Delete a property (Host only)

---

### 🧪 Performance Criteria
- Searchable properties should respond in ≤ 500ms
- Image uploads must be stored securely (e.g., S3 or local dev file storage)
- Handle 1000+ listings efficiently (pagination support)

---

## 3. 📅 Booking System

### ✅ Functional Requirements
- Guests can book available properties
- Prevent overlapping/double bookings
- Track booking statuses:
  - `pending`, `confirmed`, `cancelled`, `completed`

### 🌐 API Endpoints

#### `POST /api/bookings`
- **Authentication**: Required (Guest)
- **Input**:
  ```json
  {
    "property_id": "uuid",
    "start_date": "2025-07-05",
    "end_date": "2025-07-10"
  }
  ```
- **Output**:
  ```json
  {
    "message": "Booking request submitted",
    "booking_id": "uuid",
    "status": "pending"
  }
  ```

- **Validation Rules**:
  - Property must exist and be available
  - No overlapping bookings allowed for the same property
  - `start_date` must be before `end_date`
  - A guest cannot book their own property

---

#### `GET /api/bookings/:id`
- **Description**: View booking details (Guest or Host)

#### `DELETE /api/bookings/:id`
- **Description**: Cancel a booking (subject to policy)

---

### 🧪 Performance Criteria
- Booking creation time ≤ 500ms
- Use database-level locks or transactions to avoid race conditions
- Booking history must be paginated and filterable

---

## 📌 Notes
- All endpoints must follow RESTful standards
- JSON is the required format for all requests and responses
- Use HTTPS for secure transport
- Error responses must include appropriate status codes and error messages

---

## 📂 File Location
Save this file as:

```
/alx-airbnb-project-documentation/requirements.md
```

---

## ✅ Git Commands to Commit

```bash
cd alx-airbnb-project-documentation

git add requirements.md
git commit -m "Add detailed technical and functional requirements for key backend features"
git push origin main
```

---