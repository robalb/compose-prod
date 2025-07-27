# Developer environment setup

## Requirements:

All the commands and instructions in this documentation only work for a Linux environment.
The infrastructure development has only been tested on Ubuntu

If you have windows, we recommend to set up WSL for linux, with ubuntu. see: [install wsl on windows](https://learn.microsoft.com/en-us/windows/wsl/install).

Once installed you will have an ubuntu terminal inside windows, which you can use to perform all the commands described in this documentation

## 0. Clone this repository

[fork this repository](https://github.com/robalb/compose-prod/fork) if you haven't already.

Then [Clone the repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository) and navigate into it:

```
git clone https://github.com/YOUR-USERNAME/compose-prod
cd compose-prod
```

## 1. Install Ansible

   This repository includes a pinned version of Ansible and all its dependencies.
   it can be installed with:
   ```
   pip install -r requirements.txt
   ```
   either globally 
   in your system or [in a virtualenv](#Install-ansible-in-venv)


## 2. Configure Ansible-vault

1. Create the vault password file `touch vault_password`
2. enter the vault password in the file, it should be provided to you by the infra maintainer

You should now be all set. You can [continue with the documentation](../README.md) to either set up the infrastructure from scratch, or work on an existing infrastructure.


---

## Install ansible in venv

To install ansible inside a python environment:

Init the virtualenv:
```
python3 -m venv venv
```

Activate the virtual environment:
```
source venv/bin/activate
```

Install the package dependencies
```
pip install --upgrade pip
pip install -r requirements.txt
```
