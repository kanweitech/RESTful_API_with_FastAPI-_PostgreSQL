# RESTful_API_with_FastAPI-_PostgreSQL
FastAPI is a modern, high-performance Python web framework that uses Python type hints and Pydantic for data validation and auto-generates OpenAPI documentation. Pairing it with PostgreSQL, a robust and feature-rich relational database, yields a powerful solution for building scalable and maintainable APIs.

## Project Setup
Run the following commands sequentially to have the project directory and environment setup accomplished.

It first creates a directory using mkdir (stands for make directory) command and then navigates into the created folder. Next, it creates a virtual environment and activates the scripts on it.

The below script is for Windows compatible machine. For Linux/Unix/Mac, try running the last command as: _source venv/bin/activate_

```
mkdir <your_project>
cd <your_project>
python -m venv venv
venv\Scripts\activate
```

## Installing the necessary dependencies
Run the following command to download and install the required dependencies.

```
pip install fastapi uvicorn sqlalchemy psycopg2-binary python-dotenv
```

### Database Configuration
Create a file called `database.py` which will hold the necessary boiler plate code to establish a connection to our local PostgreSQL database and establish session management.

It imports the create_engine module which helps to create a database engine and establish connection to the database.

It also imports the sessionmaker module which helps in creating the database session objects. Also, declarative_base module is imported which helps in creating ORM models.
Next, we configure the database URL and create the database engine from the database connection.
Then creating a SessionLocal with `autocommit=false` means that commits will have to be done manually explicitly and `autoflush=false` means automatic flushing of changes to the database before queries.

Note: This SessionLocal will be instantiated later to access the database. Finally create a Base class from which all ORM models will inherit.

_Make sure to replace the username and password in the DATABASE_URL section._

### Defining Models & Schemas
First, create a `model.py` file. This will create a table called Users. 

Import the Base class (created with declarative_base()) in the previous boiler plate code, which is the base class for all ORM models.
Next, define a new ORM model class named User that inherits from Base.

__tablename__ = "users" : Specifies the name of the database table this model maps to — "users".
Next, define the columns with their datatype and constraints mentioned.

Next, create a `schemas.py` file.

import the `BaseModel from Pydantic`, which is a powerful library for data validation and settings management.

Next, define a base Pydantic model UserBase with three fields. This model ensures these fields are validated when input data is received.
Next, define UserCreate model that inherits from UserBase. This is used for user creation requests (same fields as UserBase). No additional fields or behavior are added here.
Finally, define a User model that extends the UserBase with an additional id field (as integer).

**Note:** orm_mode = True enables compatibility between SQLAlchemy instances and Pydantic schemas.

### Create FastAPI App with CRUD Endpoints
The following code implements full CRUD functionality for User resources. The code has `GET`, `PUT`, `POST`, `DELETE` API endpoints to operate on the User model.

In the below code, we can see that across all the endpoint implementations, we are primarily doing the following:

```
from fastapi import FastAPI, Depends, HTTPException
```
_FastAPI: Used to create the web app.
Depends: Handles dependency injection (e.g., for database session).
HTTPException: Used to return error responses (e.g., 404 Not Found)._
```
from typing import List
```
_List: Used for type hinting when returning a list of users._
```
from sqlalchemy.orm import Session
```
_Session: A SQLAlchemy session used to interact with the database._
```
import models, schemas
```
_models: Contains SQLAlchemy models (i.e., ORM representations of your tables).
schemas: Contains Pydantic models used for request/response validation._
```
from database import SessionLocal, engine, Base
```
_SessionLocal: Function to create a new database session.
engine: The SQLAlchemy engine connected to your DB.
Base: The base class for all SQLAlchemy models (from which they inherit).
`Base.metadata.create_all(bind=engine)`
This tells SQLAlchemy to create all tables defined in models by looking at Base subclasses, using the engine connection._
`app = FastAPI()`
_Initializes the FastAPI application object (app) which you use to define routes._
```
def get_db():
    db = SessionLocal()  # Create new session
    try:
        yield db  # Provide it to the request
    finally:
        db.close()
```
_# Ensure it's closed after request finishes_
_This function is used as a dependency to get a DB session.
yield allows FastAPI to inject it into endpoints, and finally ensures proper cleanup.
POST Endpoint understanding to create an user:_

```
@app.post("/users/", response_model=schemas.User)
def create_user(user: schemas.UserCreate, db: Session = Depends(get_db)):
```
_Defines a `POST` route to /users/.
Takes user as input, validated against UserCreate schema.
Injects a DB session using Depends(get_db)._

