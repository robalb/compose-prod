# Compose prod

A template for deploying your containerized projects
on a single-node server.

Features:

- Deploy your projects by defining a simple `docker-compose.yml` file
- automatic TLS certificate generation, powered by traefik.
- A grafana control panel to monitor the state of all your services, and their logs.  
  powered by pre-configured Prometheus, Loki, node-exporter, Container-advisor.
- Ready out of the box: Buy a VPS, fork this repository, and follow the [guide](#guide) to
  have your project running in a few minutes.

## Guide

- [setup your computer to work on the infrastructure](./docs/devenv-setup.md)
- [setup the infrastructure from scratch](./docs/infra-from-scratch.md)
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

## FAQ

Is this [web scale](https://www.youtube.com/watch?v=b2F-DItXtZs)?
yes, it's web scale. And blazing fast

