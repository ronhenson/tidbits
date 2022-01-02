---
title: "Toolbox Create Distribution"
created: 2021-12-20 07:25:48
tags: container
keywords: toolbox, podman, distributions, create, remove, containers
---

# Toolbox Create Distribution


- [Getting Started With Toolbox On Fedora Silverblue](https://ostechnix.com/getting-started-with-toolbox-on-fedora-silverblue)

## archlinux distribution

create image

```bash
toolbox --image docker.io/library/archlinux [container-name]
```

## debian distribution

create image

```bash
toolbox --image docker.io/library/debian [container-name]
```

## remove image example

```bash
podman container list --all
CONTAINER ID  IMAGE                                         COMMAND               CREATED       STATUS                   PORTS                   NAMES
476a890687bf  docker.io/library/postgres:latest             postgres              2 months ago  Exited (0) 2 months ago  0.0.0.0:5432->5432/tcp  postgre14
3f6ff0d249c8  registry.fedoraproject.org/fedora-toolbox:35  toolbox --log-lev...  6 weeks ago   Up 12 hours ago                                  f35-django
b3ae6c1a571a  registry.fedoraproject.org/fedora-toolbox:35  toolbox --log-lev...  6 weeks ago   Up 12 hours ago                                  f35-apps

# --------------
podman container stop [container-name]

podman images --all
REPOSITORY                                 TAG         IMAGE ID      CREATED       SIZE
docker.io/library/archlinux                latest      64c6abec2fa0  7 days ago    395 MB
registry.fedoraproject.org/fedora-toolbox  35          ab8bc106d4a7  6 weeks ago   497 MB
localhost/f34-backup_2021-09-25            latest      c16f7a7d7c42  2 months ago  7.17 GB
docker.io/library/postgres                 latest      5861c038d674  2 months ago  379 MB
docker.io/dpage/pgadmin4                   latest      4d0211d377df  3 months ago  260 MB
registry.fedoraproject.org/fedora-toolbox  34          daf770cfb9ee  4 months ago  518 MB
k8s.gcr.io/pause                           3.5         ed210e3e4a5b  9 months ago  690 kB

# --------------
toolbox list images
IMAGE ID      IMAGE NAME                                    CREATED
c16f7a7d7c42  localhost/f34-backup_2021-09-25:latest        2 months ago
ab8bc106d4a7  registry.fedoraproject.org/fedora-toolbox:35  6 weeks ago
daf770cfb9ee  registry.fedoraproject.org/fedora-toolbox:34  4 months ago

CONTAINER ID  CONTAINER NAME  CREATED      STATUS   IMAGE NAME
b3ae6c1a571a  f35-apps        6 weeks ago  running  registry.fedoraproject.org/fedora-toolbox:35
3f6ff0d249c8  f35-django      6 weeks ago  running  registry.fedoraproject.org/fedora-toolbox:35

# ---------------
toolbox rmi localhost/f34-backup_2021-09-25:latest
```
