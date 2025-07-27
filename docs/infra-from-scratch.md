# Setup the infrastructure from scratch

## Prerequisites

We assume that you already have an ubuntu VPS - you can get one for less than 4€/ month on [hetzner](https://www.hetzner.com/cloud), [DigitalOcean](https://www.digitalocean.com/pricing/droplets#basic-droplets), or [OVH](www.ovhcloud.com)
Make sure to get a server with:
- At least `1Gb` of memory. More is better
- `Ubuntu` as the operative system
- Internet access, and an `IPv4` associated to the machine

We also assume that you already [configured your computer](./devenv-setup.md) to work on the infrastructure.

## Getting started

To get started, you need to configure the IP of your server in `inventory.yml`

This is an example configuration

```yml
all:
  # all servers, listed by their identifier name.
  # Once you add a server here, you can declare its
  # role (for example: staging, production, test)
  # by referencing its name in the children section
  hosts:
    hetzner_ubuntu_frankfurt_1:  #a descriptive name of your server
      ansible_host: 192.88.99.42 #the ip of your server

  # logical grouping of the existing servers.
  # each group defined here can have dedicated
  # variables defined in /group_vars/.
  children:
    staging:
      hosts:
        # TODO: add staging servers here
    production:
      hosts:
        hetzner_ubuntu_frankfurt_1:


```

This infrastructure project is designed to run on hosts with the following requirements:
- ssh enabled on port 22
- non-root user
- passwordless ssh access with a key already installed on your device.

If this is not how you currently access your server, don't worry!
the next paragraph include the necessary steps to setup your ssh access in the proper way.

## If you have a only root admin password and no ssh key

To run the bootstrap playbook against a remote host with a root admin password:

1. Define the name of a new administrator user in `group_vars/all/main.yml` in the `_admin_name` variable. By default it's `admin`

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
