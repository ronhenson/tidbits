---
title: "Postgresql Podman Practical Guide"
created: 2021-10-03 17:49:56
tags: #tags
keywords: template
---

# Postgresql Podman Practical Guide

Link
: [https://linuxiac.com/postgresql-docker/](https://linuxiac.com/postgresql-docker/)

## Pull a PostgreSQL Server Docker Image

```bash
podman pull postgres:latest
```

I didn't have to do the following since I had a postgresql database `/home/ronh/data/psql/data`.  I changed the ownership of the database.

```bash
sudo docker volume create postgres-volume
```

Now that you have PostgreSQL image on your machine and a volume to persist the data, you can deploy a container with:

```bash
podman run -d --name=postgre14 -p 5432:5432 -v postgres-volume:/home/ronh/data/pgsql/data -e POSTGRES_PASSWORD='Apple!0#' 5861c038d674
```

Let’s break down this syntax. Here is what each parameter in that command means:

- `-d` will run this container in detached mode so that it runs in the background.
- `--name` assigns the name “postgres13” to your container instance.
- `-p` will bind the PostgreSQL container port 5432 to the same port on your host machine. You’ll be able to connect to localhost:5432 using PostgreSQL clients (psql) running on your host.
- `-v` option bind that data folder inside the container volume (/var/lib/postgresql) to the local Docker volume (postgres-volume) you created in the previous step.
- `-e` sets an environment variable. In this case, the PostgreSQL root password.
    postgres is the name of the image we use to create the container.

You can check whether the container is running by listing the running containers:

```bash
podman ps
```

```bash
CONTAINER ID  IMAGE                              COMMAND     CREATED      STATUS          PORTS                   NAMES
476a890687bf  docker.io/library/postgres:latest  postgres    2 hours ago  Up 2 hours ago  0.0.0.0:5432->5432/tcp  postgre14
```

```bash
sudo docker logs postgre14
```

## Connect to the PostgreSQL Server

```bash
podman exec -it postgre14 psql -U postgres
```

or connect by

```bash
podman -h localhost -U postgres
```
