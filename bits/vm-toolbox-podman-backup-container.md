---
title: "Vm Toolbox Podman Backup Container"
created: 2021-09-25 07:23:04
tags: #devops
keywords: podman, virtual, backup, containers, toolbox
---

# Vm Toolbox Podman Backup Container

[Backup toolbox container using podman](https://fedoramagazine.org/backup-and-restore-toolboxes-with-podman/)

## Creating a backup

To backup a toolbox container you will need it’s name and container ID which can be gotten by using toolbox list. For this example I am going to backup my golang development toolbox container, imaginatively named go.

```bash
toolbox list


CONTAINER ID  CONTAINER NAME  CREATED      STATUS   IMAGE NAME
00ff783a102f  go              5 weeks ago  exited   registry.fedoraproject.org/f32/fedora-toolbox:32
```

If the container’s status shows as running , you should stop it using podman container stop container_name. Although the commit command has a -p for pause option, make sure that the Toolbox is not running, which helps it initialize correctly when restored from backup.

```bash
podman container stop go
```

To create an image of the toolbox container use
podman container commit -p container_ID backup-image-name
Depending on the complexity of the Toolbox, this can take a little while.

```bash
podman container commit -p 00ff783a102f  go-backup
```

Now to confirm the image has been created type…

```bash
toolbox list

# You should get output similar to what is below…

IMAGE ID IMAGE NAME CREATED
cfcb13046db7 localhost/go-backup:latest About a minute ago

CONTAINER ID CONTAINER NAME CREATED STATUS IMAGE NAME
00ff783a102f go 5 weeks ago exited registry.fedoraproject.org/f32/fedora-toolbox:32

```

Now to save the backup image to a tar archive file using podman save -o backup-filename.tar backup-image-name.

```bash
podman save -o go.tar go-backup
```

Confirm the archive file, our toolbox container backup, was created.

```bash
ls

go.tar
```

Do some tidying up, remove the backup image and, if needed, remove the original Toolbox.

```bash
podman rmi go-backup

$ toolbox rm go
```

## Restore a backup

To create an image from the backup file that was made above, you do it with the command `podman load -i backup_filename`.

```bash
podman load -i go.tar
```

Then you can confirm the image was created with…

```bash
toolbox list

IMAGE ID      IMAGE NAME                  CREATED
cfcb13046db7  localhost/go-backup:latest  17 minutes ago
```

Now create a toolbox container from the restored image, with toolbox create ––container container_name ––image image_name, specifying the full repository and version tag as the image name.

```bash
toolbox create --container go --image localhost/go-backup:latest
```

Confirm that the toolbox was created.

```bash
toolbox list

IMAGE ID      IMAGE NAME                CREATED
cfcb13046db7 localhost/go-backup:latest 20 minutes ago

CONTAINER ID CONTAINER NAME CREATED STATUS IMAGE NAME
34cef6b7e28d go 21 seconds ago configured localhost/go-backup:latest
```

Finally, you can test that the restored Toolbox works…

```bash
toolbox enter --container go
```

If you can enter the newly created toolbox container, you will see the toolbox prompt and will have successfully backed up and restored your Pet toolbox container.
