---
description: >-
  This page describes how Airflow is installed and ran for testing on a local
  Ubuntu Linux machine.
---

# Virtual Machine Install

This project uses Parallels for virtual machine usage but other providors such as VirtualBox or VMWare will suffice.

## Installs

_There are a few installs needed before setting up Airflow._

### **Updates**

* Firstly, run `sudo apt-get update` to update any out-of-date packages on the machine.

### **Python**

* Ubuntu should come installed with Python. Check this by opening up the terminal and running `python --version` and verify that Python is installed. This project uses Python `v3.12.0`
* If Python is not installed, visit the [downloads](https://www.python.org/downloads/) page on python.org.
* Once Python is installed, run `python3 --version` to ensure it's installed.

### **git**

* `Run` `sudo apt install git-all` to install git. This is needed to clone the project's GitHub repository.

### **virtualenv**

* Run `sudo apt install python-3.12-venv` to install the `virtualenv` package.

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

2. Once the `airflow_db` and `airflow_user` are created and the user has its privileges assigned, everything is ready to switch the default SQLite database to the hosted database. Get the connection string from the **Overview** page on the database cluster. The `airflow_db` and the `airflow_user` should be specified in the string.

```bash
# Example:
postgresql+psycopg2://airflow_user:<PASSWORD>@my-database-cluster-do-user-123456789-0.h.db.ondigitalocean.com:25060/airflow_db?sslmode=require 
```

3. In the Ubuntu machine, change into the `airflow/` directory and then edit the `airflow.cfg` file to update the `sql_alchemy_conn` variable with the PostgreSQL connection string.

```bash
nano airflow.cfg
```

4. With `nano`, you can search for the variable with `CTRL + W` and search for the variable name. Hit enter and the cursor will be placed in front of the variable.  Once the variable is found, replace the default SQLite connection with the PostgreSQL connection string. Below is an example of what the variable looks like in the `airflow.cfg` file.

```toml
# The SqlAlchemy connection string to the metadata database.
# SqlAlchemy supports many different database engines.
# More information here:
# http://airflow.apache.org/docs/apache-airflow/stable/howto/set-up-database.html#database-uri
#
# Variable: AIRFLOW__DATABASE__SQL_ALCHEMY_CONN
#
sql_alchemy_conn = sqlite://....
```

5. While still in the `airflow.cfg` file, edit the `executor` variable from `SequentialExecutor` to `LocalExecutor`. Read more about Executors in [Airflow's documentation](https://airflow.apache.org/docs/apache-airflow/2.7.3/core-concepts/executor/index.html).

```toml
# The executor class that airflow should use. Choices include
# ``SequentialExecutor``, ``LocalExecutor``, ``CeleryExecutor``, ``DaskExecutor``,
# ``KubernetesExecutor``, ``CeleryKubernetesExecutor``, ``LocalKubernetesExecutor`` or the
# full import path to the class when using a custom executor.
#
# Variable: AIRFLOW__CORE__EXECUTOR
#
executor = LocalExecutor
```

6. Also while in the `airflow.cfg` file, edit the `load_examples` variable to remove the example dags by setting it to `False`.

```toml
# Whether to load the DAG examples that ship with Airflow. It's good to
# get started, but you probably want to set this to ``False`` in a production
# environment
#
# Variable: AIRFLOW__CORE__LOAD_EXAMPLES
#
load_examples = False
```

7. Then, run the `airflow db migrate` command to initialize the new connection.
8. Create a user for logging into the Airflow UI. Any name, password, and email will work. In keeping with the theme of the project, the admin user is based off of the franchise.

```bash
airflow users create \
    --username admin \
    --firstname Ash \
    --lastname Ketchum \
    --password airflow \
    --role Admin \
    --email spiderman@superhero.org
```

8. Once the user is created, run the web server.

```bash
airflow webserver --port 8080
```

9. In a web browser, visit `localhost:8080` and you will be taken the sign-in page for Airflow.
10. Login with the user's username and password created in step 8 and you'll see Airflow's dashboard.
