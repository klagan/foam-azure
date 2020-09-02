# Docker

`Dangling` images or containers are essentially **unused** images or containers.  We can periodically remove them to save on disk space and provide easier management of images and containers by removing this noise.  (remember to remove the containers *before* the images)

## Delete dangling containers

```
docker rm $(docker images -f "dangling=true" -q)
```

## Delete dangling images

```
docker rmi $(docker images -f "dangling=true" -q)
```

simpler command

```
docker system prune
```

## Copy a file from container to host

```
docker cp <containerId>:/file/path/within/container /host/path/target
```

## Copy a file from host to container

```
docker cp my.file <containerId>:/home/dir1
```

## Execute a command in the container

```
docker exec -it <containerId/name> ls
```

## Filter docker images by repository/name

dont forget the forward slash for multi part names. `bit*` is not the same as `bit*`*`

```
docker images --filter=reference='bit*/*:*'
```

[[dotnet.md]]
