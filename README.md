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

- [Setup your computer to work on the infrastructure](./docs/devenv-setup.md)
- [Setup the infrastructure from scratch](./docs/infra-from-scratch.md)
- [Deploy a new project](./docs/deploy.md)
- [Manage an existing infrastructure](./docs/manage-existing-infra.md)

## Overview

This ansible project will configure your ubuntu server, install docker,
and set up the following folder structure in the home folder of the 
administrator user:

```
home/admin/
├── platform/
│   ├── docker-compose.platform.yml
│   └── docker-compose.traefik.yml
├── project-name1/
│   └── docker-compose.yml
└── project-name2/
    └── docker-compose.yml
```

## Deployment Workflow

the [Deployment guide](./docs/deploy.md) covers several way to deploy your projects.  
If you want to follow the Continuos-Deployment approach, the steps are extremely simple:

- add a docker-compose file to your project repository
- configure the `deploy.yml` file in the infrastructure repository:
  ```
  projects:
    - name: "my-projet"
      git: "https://github.com/username/repository"
      compose_path: "/compose-prod/"
      autodeploy_on_change: true
  ```
- That's it! The system will monitor your project repository, and will automatically deploy 
  your docker-compose when you modify it.  
  If you deployed a web server, it will instantly be availabe online, with a valid TLS 
  certificate.

If you want to manually manage your projects, that's fine too!
Just copy your docker-compose files into the server, and launch them with `docker compose up -d`  
Your project logs and metrics will be available on the Grafana dashboard.

## FAQ

- __Does this scale?__
  Chances are this is not the question you should be asking:  
  No. This Infrastructure will run on a single-server only, and cannot scale to multiple nodes.  
  But most project will never need to scale. This infrastructure running on a `4$` VPS
  can handle more than `10k` cuncurrent users depending on the workload.  
  Know your system: are you running expensive machine learning inference models, or just serving a static web page?
  Most importantly: can you afford downtime? can you run your database on a single node?

- __What happens if the server breaks?__  
  All the `docker-compose` projects you deployed on the server will go
  offline. Since all your infrastructure is stored in git and automated via Ansible, you can get a new server up and running in a few minutes: just run the Ansible scripts again.  
  This is an extremely simple and cheap way to deploy your projects, but it comes at the cost of less availabilty.
  If you cannot afford downtime, `docker-compose` on a single server is not the right way to deploy 
  your projects, and it's probably time to look at more expensive and complicated solutions.

- __Is this [web scale](https://www.youtube.com/watch?v=b2F-DItXtZs)__?  
  yes, it's web scale. And blazing fast.


## Contributing

Contributions are always welcome! The ansible code
in this repository follows an experimental coding style,
you can read more about it in the [code guidelines](./docs/code-guidelines.md)
