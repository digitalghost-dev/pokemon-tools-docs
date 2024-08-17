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

## Virtual Environment and Airflow Install

Once the above are installed, choose a directory on the Ubuntu machine where the repository will be cloned. This project uses the `Documents/` directory.

```bash
# change directory:
$ cd Documents/

# clone the project's repository:
$ git clone https://github.com/digitalghost-dev/pokemon-data-engineering.git

# change into the cloned directory:
$ cd pokemon-data-engineering/

# create a .venv directory:
$ python3 -m venv .venv

# activate the virtual env:
$ source .venv/bin/activate

# change into the etl directory where the airflow_install.sh file lives:
$ cd etl/

# make the file executable:
$ chmod +x airflow_install.sh

# run the airflow_install.sh file:
$ bash airflow_install.sh


```

