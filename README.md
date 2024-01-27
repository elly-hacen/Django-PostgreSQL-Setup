## Setting up PostgreSQL and psycopg2 for Django on Linux

### Install [PostgreSQL](https://www.postgresql.org/download/)

1. First, install PostgreSQL and its contrib package:

    ```bash
    sudo apt install postgresql postgresql-contrib
    ```
2. Install ```libpq-dev``` package

     `libpq-dev` contains libraries and headers for C language frontend development
    ```bash
    sudo apt install libpq-dev
    ```
3. Install psycopg2 Library for PostgreSQL in Python:

    ```bash
    pip3 install psycopg2
    ```
4. If you're using Django 3.2 or later, consider using `psycopg2-binary`:

    ```bash
    pip3 install psycopg2-binary
    ```

## Create a PostgreSQL Database and User:

1. Check PostgreSQL Status

    Check if PostgreSQL is running:
    
    ```bash
    systemctl status postgresql
    ```

    If it's not running, start it:
    
    ```bash
    sudo systemctl start postgresql
    ```

    Enable automatic startup at boot:
    
    ```bash
    sudo systemctl enable postgresql
    ```

### Access the PostgreSQL shell as the `postgres` user:

```bash
sudo -iu postgres
```

Then, enter the PostgreSQL shell:

```bash
psql
```

### Create Database and User

1. Create a database (replace `dbname` with your desired name):

```sql
CREATE DATABASE dbname;
```

2. Create a user with an encrypted password (replace `djangouser` and `myPasswordHere`):

```sql
CREATE USER djangouser WITH ENCRYPTED PASSWORD 'myPasswordHere';
```

### Connect to the Database

1. Connect to the newly created database:

```sql
\c dbname;
```

You're now connected to the "dbname" database as the "djangouser."

2. Create a new schema and set its owner:

```sql
CREATE SCHEMA schema_name AUTHORIZATION djangouser;
```

3. Modify the search path for the user:

```sql
ALTER ROLE djangouser IN DATABASE dbname SET search_path = schema_name;
```

### Django Database Configuration (settings.py):

In your Django project's `settings.py` file, configure the database settings as follows:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'dbname',          # Replace with your database name
        'USER': 'djangouser',      # Replace with your database user
        'PASSWORD': 'myPasswordHere',  # Replace with your database user's password
        'HOST': '127.0.0.1',       # Set the host to '127.0.0.1' for a local PostgreSQL server
        'PORT': '5432',            # Set the port to 5432 for the default PostgreSQL port
    }
}
```

Remember to replace `'dbname'`, `'djangouser'`, and `'myPasswordHere'` with your actual database name, username, and password set during database creation.

That's it! If you're a Windows user, you will need to find alternative instructions as I am a Linux user ( i use arch btw ⚜️ ). 😄
