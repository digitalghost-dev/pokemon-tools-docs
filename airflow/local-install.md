---
description: >-
  This page describes how Airflow is installed and ran for testing on a local
  Ubuntu Linux machine.
---

# Local Install

This project uses Parallels for virtual machine usage but other providors such as VirtualBox or VMWare will suffice.

1. Ubuntu should come installed with Python. Check this by opening up the terminal and running `python --version` and verify that Python is installed.
   1. If Python is not installed, visit the [downloads](https://www.python.org/downloads/) page on python.org.
2. Once Python is installed, run `python --version` and note the version number.
3. Run `sudo apt-get update` to update any out-of-date packages on the machine. This may be needed for installing `virtualenv` for Python.
4. Once the updates are finished, run `sudo apt install python-3.10-venv` to install the `virtualenv` package.
5. Once the package is installed, clone the repository&#x20;
