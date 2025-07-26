# Setup the infrastructure from scratch

To get started, simply Configure the host IPs in `inventory.yml`

This infrastructure project is designed to run on hosts with the following requirements:
- ssh enabled on port 22
- non-root user
- passwordless ssh access with a key already installed on your device.

If the remote hosts don't have a dedicated non-root user or don't have proper ssh key access,
run the bootstrap playbook with the instructions described in the next section.

## If you have only root admin password and no ssh key

To run the bootstrap playbook against a remote host with a root admin password:

1. Define a new user in `group_vars/all/main.yml` in the `_admin_name` variable.

2. Run the ssh setup playbook
    ```
    ansible-playbook playbooks/ssh_setup.yml \
      -e "ansible_user=root" \
      --ask-pass
    ```

3. After completion, the playbook will save an ssh key on your system, and you'll be able
   to access the hosts with that key and the user you defined.

## If you already have a non-root account with ssh key access

To run the bootstrap playbook against a remote host that already has an ssh key and non-root user:

1. Make sure the ssh key is stored in your file system. It's good practice to keep all keys in your `~/.ssh/`folder, and manage the host ssh access in your `.ssh/config` file

1. Configure the name of your non-root user in `group_vars/all/main.yml` in the `_admin_name` variable.

1. Run the ssh setup playbook.
  ```
  ansible-playbook playbooks/ssh_setup.yml
  ```

## Continue with the setup

Check that the inventory is reachable after the ssh configuration. If it works,
ansible is successfully connecting to the hosts using your ssh key and the non-root ansible user

```
ansible -i inventory.yml -m ping all
```

Launch the rest of the playbooks, in this order:

```
ansible-playbook playbooks/os_configuration.yml
ansible-playbook playbooks/platform.yml
```
