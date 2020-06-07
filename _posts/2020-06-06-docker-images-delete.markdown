---
layout: post
title: 'Docker images, containers and volumes - list and delete commands'
date: 2020-06-06
tags:
  - terminal
keywords:
  - Docker delete images
  - images, containers, volumes, docker
  - Docker list images
dont_show_excerpt_separator: true
---

Here is compiled list of commands to list and delete images, containers and volumes.

## Images

List:

```bash
docker images -a
docker images -f dangling=true
```

Delete:

```bash
docker rmi Image
### Remove dangling
docker images purge
### Remove all
docker rmi $(docker images -a -q)
```

## Containers

List:

```bash
docker ps -a
### List all exited containers
docker ps -a -f status=exited
```

Remove:

```bash
docker rm ID_or_Name
### Remove all exited containers
docker rm $(docker ps -a -f status=exited -q)
### Stop containers
docker stop $(docker ps -a -q)
```

## Volumes

List:

```bash
### All
docker volume ls
### Dangling
docker volume ls -f dangling=true
```

Remove:

```bash
docker volume rm volume_name
### Remove dangling
docker volume prune
### Remove container and its volume at the same time
docker rm -v container_name
```
