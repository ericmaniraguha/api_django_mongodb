# Django MongoDB CRUD API Project

This guide explains how to set up a Django project with MongoDB as the database backend, including REST API functionality and file upload capabilities.

## Environment Setup

First, set up your Python environment and install the necessary packages:

```bash
# Install pip if not already installed
sudo apt install python3-pip

# Create a virtual environment
python3 -m venv env_django

# Activate the virtual environment
source env_django/bin/activate

# Install required packages
pip install django==4.2.10  # Use a specific version for compatibility
pip install djangorestframework==3.14.0  # Must be compatible with Django version
pip install django-cors-headers
pip install djongo  # Django MongoDB connector
pip install pymongo==3.11.4  # Specific version for compatibility with djongo
pip install dnspython  # For MongoDB Atlas connections
```

## Project Creation

Create your Django project and apps:

```bash
# Create a new Django project
django-admin startproject mongodb_crud_api_project
cd mongodb_crud_api_project

# Create Django apps
python manage.py startapp EmployeeApp
python manage.py startapp UploadFilesApp
```

## Project Configuration

### Update settings.py

Add your apps and required middleware in `mongodb_crud_api_project/settings.py`:

```python
INSTALLED_APPS = [
    # Default Django apps...
    
    # Third party apps
    'rest_framework',
    'corsheaders',
    
    # Your apps
    'EmployeeApp.apps.EmployeeappConfig',
    'UploadFilesApp.apps.UploadfilesappConfig',
]

MIDDLEWARE = [
    # Add this before other middleware
    'corsheaders.middleware.CorsMiddleware',
    # Other middleware...
]

# CORS settings (not recommended for production)
CORS_ORIGIN_ALLOW_ALL = True
```

### Configure MongoDB Connection

Set up the MongoDB database connection in `settings.py`:

```python
DATABASES = {
    'default': {
        'ENGINE': 'djongo',  # Use 'djongo' not 'django'
        'NAME': 'EmployeeDB',  # Your database name
        'CLIENT': {
            'host': 'mongodb+srv://username:password@crudapiproject.uxn5cn6.mongodb.net/',
            'authMechanism': 'SCRAM-SHA-1',  # For MongoDB Atlas
        }
    }
}
```

## MongoDB Setup

1. Install MongoDB Compass (GUI tool for MongoDB)
2. Create a MongoDB Atlas account and set up a cluster
3. Connect to your cluster from MongoDB Compass
4. Create a database named 'EmployeeDB'

## Creating Models and Migrations

Define your models in `EmployeeApp/models.py` and `UploadFilesApp/models.py`, then create and apply the migrations:

```bash
# Generate migration files
python manage.py makemigrations EmployeeApp
python manage.py makemigrations UploadFilesApp

# Apply migrations to the database
python manage.py migrate EmployeeApp
python manage.py migrate UploadFilesApp
```
![make migration & migrate our DB](<Screenshot from 2025-05-12 23-58-12.png>)
## Running the Development Server

Start the Django development server:

```bash
python manage.py runserver
```

Your API will be available at http://127.0.0.1:8000/

![check connection from mongod](<Screenshot from 2025-05-13 00-26-38.png>)

## Common Issues and Troubleshooting

1. **Version Compatibility**: Ensure your Django and Django REST framework versions are compatible
   - Django 3.1.x works with DRF 3.12.x
   - Django 4.2.x works with DRF 3.14.x

2. **MongoDB Connection Issues**: 
   - Verify your connection string is correct
   - Check that network access is enabled for your IP in MongoDB Atlas
   - Ensure you've installed `dnspython` for Atlas connections

3. **Import Errors**:
   - If you see `ImportError: cannot import name 'parse_header_parameters'`, this indicates version mismatch between Django and Django REST framework

4. **Python Environment**:
   - Always ensure your virtual environment is activated before running Django commands
   - Your prompt should show `(env_django)` when the environment is active

## Project Structure

```
mongodb_crud_api_project/
├── manage.py
├── mongodb_crud_api_project/
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py  # Configure DB connection here
│   ├── urls.py      # Configure main URL routing
│   └── wsgi.py
├── EmployeeApp/
│   ├── models.py    # Define Employee data models
│   ├── views.py     # API views for Employee data
│   ├── serializers.py # DRF serializers
│   └── urls.py      # API endpoint URLs
└── UploadFilesApp/
    ├── models.py    # Define File data models
    ├── views.py     # API views for file operations
    ├── serializers.py # DRF serializers
    └── urls.py      # File API endpoint URLs
```

## Next Steps

1. Define serializers for your models
2. Create API views
3. Configure URL routing
4. Test your API endpoints using tools like Postman or curl