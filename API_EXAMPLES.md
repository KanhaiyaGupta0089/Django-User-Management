# API Usage Examples

This document provides practical examples for using the UserManagement API.

## Base URL
```
http://localhost:8000/api/users/
```

## 1. Create a New User

```bash
curl -X POST http://localhost:8000/api/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe"
  }'
```

**Expected Response (201 Created):**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "date_joined": "2026-01-22T05:42:00Z"
}
```

## 2. List All Users

```bash
curl http://localhost:8000/api/users/
```

**Expected Response (200 OK):**
```json
{
  "count": 1,
  "next": null,
  "previous": null,
  "results": [
    {
      "id": 1,
      "username": "johndoe",
      "email": "john@example.com",
      "first_name": "John",
      "last_name": "Doe",
      "date_joined": "2026-01-22T05:42:00Z"
    }
  ]
}
```

## 3. Filter Users by Username

```bash
curl "http://localhost:8000/api/users/?username=john"
```

## 4. Filter Users by Email

```bash
curl "http://localhost:8000/api/users/?email=example.com"
```

## 5. Retrieve a Specific User

```bash
curl http://localhost:8000/api/users/1/
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe",
  "date_joined": "2026-01-22T05:42:00Z"
}
```

## 6. Update a User (Full Update - PUT)

```bash
curl -X PUT http://localhost:8000/api/users/1/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "email": "john.doe@example.com",
    "first_name": "John",
    "last_name": "Doe"
  }'
```

## 7. Partial Update a User (PATCH)

```bash
curl -X PATCH http://localhost:8000/api/users/1/ \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "Jane"
  }'
```

**Expected Response (200 OK):**
```json
{
  "id": 1,
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "Jane",
  "last_name": "Doe",
  "date_joined": "2026-01-22T05:42:00Z"
}
```

## 8. Delete a User

```bash
curl -X DELETE http://localhost:8000/api/users/1/
```

**Expected Response (204 No Content)**

## Error Examples

### Duplicate Email Error

```bash
curl -X POST http://localhost:8000/api/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "janedoe",
    "email": "john@example.com",
    "first_name": "Jane",
    "last_name": "Doe"
  }'
```

**Expected Response (400 Bad Request):**
```json
{
  "email": ["A user with this email already exists."]
}
```

### Duplicate Username Error

```bash
curl -X POST http://localhost:8000/api/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "email": "new@example.com",
    "first_name": "New",
    "last_name": "User"
  }'
```

**Expected Response (400 Bad Request):**
```json
{
  "username": ["A user with this username already exists."]
}
```

### Missing Required Fields

```bash
curl -X POST http://localhost:8000/api/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "first_name": "John"
  }'
```

**Expected Response (400 Bad Request):**
```json
{
  "username": ["This field is required."],
  "email": ["This field is required."]
}
```

## Using Python requests Library

```python
import requests

BASE_URL = "http://localhost:8000/api/users/"

# Create a user
response = requests.post(BASE_URL, json={
    "username": "johndoe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe"
})
print(response.json())

# List all users
response = requests.get(BASE_URL)
print(response.json())

# Get a specific user
response = requests.get(f"{BASE_URL}1/")
print(response.json())

# Update a user
response = requests.patch(f"{BASE_URL}1/", json={
    "first_name": "Jane"
})
print(response.json())

# Delete a user
response = requests.delete(f"{BASE_URL}1/")
print(response.status_code)  # 204
```

## Using JavaScript (Fetch API)

```javascript
const BASE_URL = 'http://localhost:8000/api/users/';

// Create a user
fetch(BASE_URL, {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    username: 'johndoe',
    email: 'john@example.com',
    first_name: 'John',
    last_name: 'Doe'
  })
})
.then(response => response.json())
.then(data => console.log(data));

// List all users
fetch(BASE_URL)
  .then(response => response.json())
  .then(data => console.log(data));

// Get a specific user
fetch(`${BASE_URL}1/`)
  .then(response => response.json())
  .then(data => console.log(data));

// Update a user
fetch(`${BASE_URL}1/`, {
  method: 'PATCH',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    first_name: 'Jane'
  })
})
.then(response => response.json())
.then(data => console.log(data));

// Delete a user
fetch(`${BASE_URL}1/`, {
  method: 'DELETE'
})
.then(response => console.log(response.status)); // 204
```
