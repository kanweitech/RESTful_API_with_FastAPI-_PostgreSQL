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
We will now create a file called `database.py` which will hold the necessary boiler plate code to establish a connection to our local PostgreSQL database and establish session management.

It imports the create_engine module which helps to create a database engine and establish connection to the database.

It also imports the sessionmaker module which helps in creating the database session objects. Also, declarative_base module is imported which helps in creating ORM models.
Next, we configure the database URL and create the database engine from the database connection.
Then we are creating a SessionLocal with `autocommit=false` means that commits will have to be done manually explicitly and `autoflush=false` means automatic flushing of changes to the database before queries.
Note: This SessionLocal will be instantiated later to access the database. Finally we create a Base class from which all ORM models will inherit.
