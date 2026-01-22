# UserManagement Django Project

A Django REST Framework (DRF) application for managing users with full CRUD operations, field validations, and optional gRPC integration.

## Features

- ✅ Django REST Framework integration
- ✅ User model with fields: id, username, email, first_name, last_name, date_joined
- ✅ DRF serializers with field validations (unique email and username)
- ✅ Complete CRUD API endpoints (Create, Read, Update, Delete)
- ✅ ViewSets and Routers for clean endpoint management
- ✅ Pagination support
- ✅ Optional gRPC request integration (bonus feature)
- ✅ Comprehensive documentation

## Project Structure

```
UserManagement/
├── UserManagement/          # Main project directory
│   ├── __init__.py
│   ├── settings.py          # Django settings
│   ├── urls.py              # Main URL configuration
│   ├── wsgi.py              # WSGI configuration
│   └── asgi.py              # ASGI configuration
├── users/                   # Users application
│   ├── __init__.py
│   ├── models.py            # User model
│   ├── serializers.py       # DRF serializers
│   ├── views.py             # ViewSets
│   ├── urls.py              # API URLs
│   ├── admin.py             # Django admin configuration
│   └── management/
│       └── commands/
│           └── grpc_request.py  # gRPC management command
├── grpc_requests/           # gRPC related documentation
│   └── README.md            # gRPC setup and usage
├── manage.py                # Django management script
├── requirements.txt         # Python dependencies
├── README.md                # This file
└── SCALABILITY.md           # Scalability documentation
```

## Prerequisites

- Python 3.8 or higher
- pip (Python package installer)
- Virtual environment (recommended)

## Installation and Setup

### 1. Clone the Repository

```bash
git clone <repository-url>
cd app
```

### 2. Create and Activate Virtual Environment

```bash
# Create virtual environment
python3 -m venv venv

# Activate virtual environment
# On Linux/Mac:
source venv/bin/activate

# On Windows:
# venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Run Migrations

```bash
# Create database migrations
python manage.py makemigrations

# Apply migrations
python manage.py migrate
```

### 5. Create Superuser (Optional)

```bash
python manage.py createsuperuser
```

This allows you to access the Django admin interface at `http://localhost:8000/admin/`.

### 6. Run the Development Server

```bash
python manage.py runserver
```

The API will be available at `http://localhost:8000/api/`.

## API Endpoints

The API uses Django REST Framework's router, which automatically generates the following endpoints:

### Base URL
```
http://localhost:8000/api/
```

### Available Endpoints

#### 1. List Users (GET)
```
GET /api/users/
```
Returns a paginated list of all users.

**Query Parameters:**
- `username`: Filter users by username (case-insensitive partial match)
- `email`: Filter users by email (case-insensitive partial match)
- `page`: Page number for pagination

**Example:**
```bash
curl http://localhost:8000/api/users/
curl http://localhost:8000/api/users/?username=john
curl http://localhost:8000/api/users/?email=example.com
```

#### 2. Create User (POST)
```
POST /api/users/
```
Creates a new user.

**Request Body:**
```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe"
}
```

**Example:**
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

#### 3. Retrieve User (GET)
```
GET /api/users/{id}/
```
Returns details of a specific user.

**Example:**
```bash
curl http://localhost:8000/api/users/1/
```

#### 4. Update User (PUT)
```
PUT /api/users/{id}/
```
Fully updates a user (all fields required).

**Request Body:**
```json
{
  "username": "johndoe",
  "email": "john@example.com",
  "first_name": "John",
  "last_name": "Doe"
}
```

**Example:**
```bash
curl -X PUT http://localhost:8000/api/users/1/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "johndoe",
    "email": "john@example.com",
    "first_name": "John",
    "last_name": "Doe"
  }'
```

#### 5. Partial Update User (PATCH)
```
PATCH /api/users/{id}/
```
Partially updates a user (only provided fields).

**Request Body:**
```json
{
  "first_name": "Jane"
}
```

**Example:**
```bash
curl -X PATCH http://localhost:8000/api/users/1/ \
  -H "Content-Type: application/json" \
  -d '{"first_name": "Jane"}'
```

#### 6. Delete User (DELETE)
```
DELETE /api/users/{id}/
```
Deletes a user.

**Example:**
```bash
curl -X DELETE http://localhost:8000/api/users/1/
```

## Field Validations

The API includes the following validations:

- **Email**: Must be unique across all users
- **Username**: Must be unique across all users
- **Email Format**: Must be a valid email address
- **Required Fields**: username and email are required

### Validation Error Response

If validation fails, the API returns a 400 Bad Request with error details:

```json
{
  "email": ["A user with this email already exists."],
  "username": ["A user with this username already exists."]
}
```

## gRPC Integration (Bonus Feature)

The project includes a Django management command for making gRPC requests.

### Running the gRPC Request Command

```bash
python manage.py grpc_request
```

### Command Options

- `--host`: Specify the gRPC server host (default: `grpcbin.org`)
- `--port`: Specify the gRPC server port (default: `9000`)

**Example:**
```bash
python manage.py grpc_request --host example.com --port 50051
```

For detailed information about the gRPC integration, see [grpc_requests/README.md](grpc_requests/README.md).

## Testing the API

### Using curl

```bash
# List all users
curl http://localhost:8000/api/users/

# Create a user
curl -X POST http://localhost:8000/api/users/ \
  -H "Content-Type: application/json" \
  -d '{
    "username": "testuser",
    "email": "test@example.com",
    "first_name": "Test",
    "last_name": "User"
  }'

# Get a specific user
curl http://localhost:8000/api/users/1/

# Update a user
curl -X PATCH http://localhost:8000/api/users/1/ \
  -H "Content-Type: application/json" \
  -d '{"first_name": "Updated"}'

# Delete a user
curl -X DELETE http://localhost:8000/api/users/1/
```

### Using Django REST Framework Browsable API

Visit `http://localhost:8000/api/users/` in your browser to access the browsable API interface, which allows you to interact with the API directly from the browser.

### Using Postman or Similar Tools

Import the following collection or manually create requests:

- **Base URL**: `http://localhost:8000/api/users/`
- **Headers**: `Content-Type: application/json`
- **Methods**: GET, POST, PUT, PATCH, DELETE

## Django Admin Interface

Access the Django admin interface at `http://localhost:8000/admin/` to manage users through the web interface.

You'll need to create a superuser first:
```bash
python manage.py createsuperuser
```

## Scalability Documentation

For detailed information on how to scale this Django application, see [SCALABILITY.md](SCALABILITY.md). The document covers:

- Database optimization techniques
- Caching strategies
- Asynchronous processing with Celery
- Load balancing and horizontal scaling
- Efficient use of serializers and querysets

## Dependencies

- **Django**: 5.0.1 - Web framework
- **djangorestframework**: 3.14.0 - REST API framework
- **grpcio**: 1.60.0 - gRPC library
- **grpcio-tools**: 1.60.0 - gRPC tools for protocol buffer compilation
- **protobuf**: 4.25.1 - Protocol buffer library

## Development

### Running Tests

```bash
python manage.py test
```

### Creating Migrations

```bash
python manage.py makemigrations
```

### Applying Migrations

```bash
python manage.py migrate
```

## Production Deployment Considerations

1. **Security**: Change `SECRET_KEY` in `settings.py` and set `DEBUG = False`
2. **Database**: Use PostgreSQL or MySQL instead of SQLite
3. **Static Files**: Configure static file serving (e.g., using WhiteNoise or a CDN)
4. **Caching**: Set up Redis or Memcached for caching
5. **WSGI Server**: Use Gunicorn or uWSGI instead of the development server
6. **Reverse Proxy**: Use Nginx as a reverse proxy
7. **HTTPS**: Configure SSL/TLS certificates
8. **Environment Variables**: Use environment variables for sensitive settings

## Troubleshooting

### Common Issues

1. **ModuleNotFoundError**: Ensure the virtual environment is activated and dependencies are installed
2. **Database errors**: Run migrations with `python manage.py migrate`
3. **Port already in use**: Change the port with `python manage.py runserver 8001`
4. **gRPC connection errors**: The default gRPC server may not be available; this is expected for demonstration purposes

## License

This project is created for educational purposes.

## Author

Created as part of a Django project assessment assignment.

## Contributing

This is an assessment project. For questions or issues, please refer to the assignment guidelines.
# Django-User-Management