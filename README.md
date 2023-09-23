# Setting up PostgreSQL and psycopg2 for Django on Linux

```markdown

# Follow these steps to set up PostgreSQL and psycopg2 for your Django project on a Linux system.

## 1. Install PostgreSQL

First, install PostgreSQL and its contrib package:

```bash
sudo apt update
sudo apt install postgresql postgresql-contrib
```

## 2. Install psycopg2 Library for PostgreSQL in Python:

### 2.1 Install `libpq-dev` package

Ensure you have the `libpq-dev` package installed, which is required for psycopg2:
- `libpq-dev` contains libraries and headers for C language frontend development

```bash
sudo apt update
sudo apt install libpq-dev
```

### 2.2 Install psycopg2

You can install psycopg2 using pip:

```bash
pip install psycopg2
```

If you're using Django 3.2 or later, consider using `psycopg2-binary`:

```bash
pip install psycopg2-binary
```

## 3. Create a PostgreSQL Database and User:

### 3.1 Check PostgreSQL Status

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

### 3.2 Access the PostgreSQL shell as the `postgres` user:

```bash
sudo -iu postgres
```

Then, enter the PostgreSQL shell:

```bash
psql
```

### 3.3 Create Database and User

Create a database (replace `dbname` with your desired name):

```sql
CREATE DATABASE dbname;
```

Create a user with an encrypted password (replace `djangouser` and `myPasswordHere`):

```sql
CREATE USER djangouser WITH ENCRYPTED PASSWORD 'myPasswordHere';
```

### 3.4 Connect to the Database

Connect to the newly created database:

```sql
\c dbname;
```

You're now connected to the "dbname" database as the "djangouser."

Create a new schema and set its owner:

```sql
CREATE SCHEMA schema_name AUTHORIZATION djangouser;
```

Modify the search path for the user:

```sql
ALTER ROLE djangouser IN DATABASE dbname SET search_path = schema_name;
```

## 4. Django Database Configuration (settings.py):

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

That's it! If you're a Windows user, you will need to find alternative instructions as I am a Linux user 😄
```
