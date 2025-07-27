We assume that you already followed the steps to:
- [configured your computer](./devenv-setup.md) to work on the infrastructure
- [setup the infrastructure](./infra-from-scratch.md) on a remote server, connected to the internet, with a public IP.

Now that you have a working infrastructure, maybe with some [projects](./deploy.md) deployed on it, you might want to perform some maintenaince tasks on it.

---

This part of the documetation is still in progress.
Generally speaking, this is the right page to talk about:

- updating system (no downtime)
- updating docker (downtime)
- docker volumes housecleaning

## investigating disk usage

These commands can be useful when you start to run out of disk space 
on the server:

Inspect the disk space used by docker:
```
$ docker system df
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          43        12        6.382GB   4.655GB (72%)
Containers      12        12        25.84kB   0B (0%)
Local Volumes   8         5         656.6MB   206.3MB (31%)
Build Cache     0         0         0B        0B
```

Inspect the disk usage of specific volumes:
```
docker system df -v
```

Inspect the disk usage of docker logs:
```
sudo du -h $(docker inspect --format='{{.LogPath}}' $(docker ps -qa))
```




