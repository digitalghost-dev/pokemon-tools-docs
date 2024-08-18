---
description: >-
  This page describes how Airflow is installed and ran for testing on a local
  Ubuntu Linux machine.
---

# Local Install

This project uses Parallels for virtual machine usage but other providors such as VirtualBox or VMWare will suffice.

## Installs

_There are a few installs needed before setting up Airflow._

### **Updates**

* Firstly, run `sudo apt-get update` to update any out-of-date packages on the machine.

### **Python**

* Ubuntu should come installed with Python. Check this by opening up the terminal and running `python --version` and verify that Python is installed. This project uses Python `v3.10.12`
* If Python is not installed, visit the [downloads](https://www.python.org/downloads/) page on python.org.
* Once Python is installed, run `python3 --version` to ensure it's installed.

### **git**

* `Run` `sudo apt install git-all` to install git. This is needed to clone the project's GitHub repository.

### **virtualenv**

* Run `sudo apt install python-3.10-venv` to install the `virtualenv` package.

***

## Airflow Setup

### Install

1. Once the above are installed, choose a directory on the Ubuntu machine where the repository will be cloned. This project uses the `Documents/` directory.  After cloning the repository, a file is prepared to handle the install of Airflow.

```bash
# Change directories:
$ cd Documents/

# Clone the project's repository:
$ git clone https://github.com/digitalghost-dev/pokemon-data-engineering.git

# Change into the cloned directory:
$ cd pokemon-data-engineering/

# Create a .venv directory:
$ python3 -m venv .venv

# Activate the virtual env:
$ source .venv/bin/activate

# Change into the etl directory where the airflow_install.sh file lives:
$ cd etl/

# Make the file executable:
$ chmod +x airflow_install.sh

# Run the airflow_install.sh file:
$ source airflow_install.sh
```

### Database

Visit the [Airflow docs ](https://airflow.apache.org/docs/apache-airflow/2.7.3/howto/set-up-database.html#setting-up-a-postgresql-database)for more information on setting up a database.

1. Before setting the backend database in Airflow, a user with permissions needs to be set in the database first.
   1. Connect to the database cluster and run the following in the console:

```sql
-- Create a new database, this can also be done in the
-- Digital Ocean UI under the Users & Databases page.
CREATE DATABASE airflow_db;

-- Create a new user with anyone username and/or password:
CREATE USER airflow_user WITH PASSWORD 'airflow_pass';

-- Grant privilages to the newly created user:
GRANT ALL PRIVILEGES ON DATABASE airflow_db TO airflow_user;

-- Switch to the database and grant additional privilages:
GRANT ALL ON SCHEMA public TO airflow_user;
```

2. Once the `airflow_db` and `airflow_user` with it's added priviliges have beed created, everything is ready to switch the default SQLite database to the hosted database. Get the connection string from the **Overview** page on the database cluster.





