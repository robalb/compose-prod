# deploy a new project

We also assume that you already followed the steps to:
- [configured your computer](./devenv-setup.md) to work on the infrastructure
- [setup the infrastructure](./infra-from-scratch.md)

Now that your infrastructure is ready, you can deply your projects to it

## create a docker-compose

It's important that you add the `traefik-proxy` network,
and the appropriate traefik tags if you want to deploy a website.
This is a complete example of docker-compose.yml:

```yml
#docker-compose.yml

# Always add this block, if you want to deploy a website
networks:
  traefik-proxy:
    external: true
  default: null

services:
  my-blog:
    image: ghcr.io/username/project
    # Always add this line, to make sure that your service will 
    # come back online when the host server restart
    restart: always
    # If you want traefik to send web traffic to this service,
    # add these labels. remember to configure properly the domain
    # name, and change `myblog` in each line with the name of your service.
    labels:
      - traefik.enable=true
      - traefik.http.routers.myblog.rule=Host(`myblog.example.com`)
      - traefik.http.routers.myblog.entrypoints=websecure
      - traefik.http.routers.myblog.tls=true
      - traefik.http.routers.myblog.tls.certresolver=myresolver
    # Always add this block, if you want to deploy a website
    networks:
      - traefik-proxy
    # Always add this block to set a limit on the resources 
    # your container is allowed to use.
    deploy:
      resources:
        limits:
          cpus: '0.2'
          memory: 900M
```


