---
title: "rsync a switches"
created: 2021-06-14 06:38:58
tags: backups
keywords: rsync, copy, synchronizing
---

# rsync for synch backup example

```bash
# for d03 synch
rsync -avPx --delete /data/ /run/media/ronh/d03/ronh/2021/synch/data/
rsync -avPx --delete /home/ronh/ /run/media/ronh/d03/ronh/2021/synch/ronh/
sudo rsync -avPx --delete /etc /run/media/ronh/d03/ronh/2021/synch/etc/
# for d02 synch
rsync -avPx --delete /data/ /run/media/ronh/d02/ronh/2021/synch/data/
rsync -avPx --delete /home/ronh/ /run/media/ronh/d02/ronh/2021/synch/ronh/
rds: backup rsync
---
sudo rsync -avPx --delete /etc/ /run/media/ronh/d02/ronh/2021/synch/etc/
```

## rsync for  backup example

```bash
# for d03 synch
rsync -avPx  /data/ /run/media/ronh/d03/ronh/2021/backup/data/
rsync -avPx /home/ronh/ /run/media/ronh/d03/ronh/2021/backup/ronh/
sudo rsync -avPx /etc /run/media/ronh/d03/ronh/2021/backup/etc/
# for d02 synch
rsync -avPx /data/ /run/media/ronh/d02/ronh/2021/backup/data/
rsync -avPx /home/ronh/ /run/media/ronh/d02/ronh/2021/backup/ronh/
sudo rsync -avPx /etc/ /run/media/ronh/d02/ronh/2021/backup/etc/
```

# rsync `a` switches

```text
provides these switches: rlptgoD (no -H,-A,-X)
```

## __IMPORTANT__  when backing up a directory, to keep from crossing mount points use

__-x__, __--one-file-system__
: avoid crossing filesystem boundary when recursing,  Also will not cross symlinks to directories on another filesystem

__-r__, __--recursive__
:   recurse into directories

__-l__, --links
: Copy symlinks as symlinks

__-p__,  __--perms__
: Preserve permissions

__-t__, __--times__
: Preserve modification times

__-g__, __--group__
: Preserve group

__-o__, __--owner__
: Preserve owner (super user only)

__-D__
: __--devices__: preserve device files(super user only)
: __--specials__: preserve special files

## Additional flags that are useful

```bash
rsync -avPx --delete
```

__-a__
: Explained above (-rlptgo)

__-x__
: Explained above (do not cross file boundaries)

__-P__
: --progress
: --partial

--delete
: Used to delete files during synch of target that do not exist on source
: Good to use for backups so you do not restore everything
: Do a backup with --delete to ./synch folder
: Do not use --delete when backing up to ./backup folder
