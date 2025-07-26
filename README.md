# Compose prod

A template for deploying your containerized projects
on a single-node server.

- [setup your computer to work on the infrastructure](./docs/devenv-setup.md)
- [build the infrastructure from scratch](./docs/infra-from-scratch.md)
- [Work on an existing infrastructure](#Work-on-an-existing-infrastructure)

# Work on an existing infrastructure

Quick commands for an existing infrastructure:
```
# Check the host connection
ansible -m ping all

# Run a playbook
ansible-playbook playbooks/playbookname.yml

# View vault secrets
ansible-vault view group_vars/all/vault.yml
ansible-vault view group_vars/staging/vault.yml

# Edit vault secrets
ansible-vault edit group_vars/all/vault.yml
ansible-vault edit group_vars/staging/vault.yml
```

## guidelines

An extension of:
https://redhat-cop.github.io/automation-good-practices/

- variables should be defined in two places only: In the `defaults/main.yml` section of a role,
  and in the `group_vars/<name>/main.yml` section.
- prefix all variables used by a role with the role name. Optionally define a default value define it in `defaults/main.yml`.
- prefix all generic variables defined in `group_vars/<name>/main.yml` with `_`, and only use them to explicity override role variables:
  ```
  #group_vars/all/main.yml
  vars:
    _replicas: 1
    rolename__replicas: {{ _replicas }}
    database__replicas: {{ _replicas }}
  ```
- you can define temporary variables in a role using `set_facts`, but only when it's part of the role logic. Use any name you want, but avoid [reserved names](https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html#special-variables) or names that start with `_`.

Why these strict rules? Ansible variables are untyped, and extremely flexible. You can define them everywhere:
in a playbook, in a role, in a task, in an inventory, in group_var/, in host_vars/, you can even manually define them from the command line when 
running a playbook.

Because of this, managing variables can become extremely hard. When you meet a variable, how do you know where
it's defined? when you change a variable, how do you know what roles will be affected?

With these additional rules, following variables becomes verbose, but simple:
- when you change a vatialbe in `group_vars`, what role does it affect? just look at the variable prefix.
  if the variable is called `rolename__password` for example, the affected role is `rolename`.
- when you meet a variable in a role, where is it defined? the default value is in `rolename/defaults/main.yml`.
  it can be overridden in `group_vars/groupname/main.yml`. Ansible will not 
  start if the variable is not defined in one of those two places.
  Remember the [variable lookup precendence](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_variables.html#understanding-variable-precedence).
  variables defined in group_vars will override defaults values.


## FAQ

Is this [web scale](https://www.youtube.com/watch?v=b2F-DItXtZs)?
yes, it's web scale. And blazing fast

