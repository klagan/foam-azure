# Docker

`Dangling` images or containers are essentially **unused** comages or containers.  We can periodically remove them to save on disk space and provide easier management of images and containers by removing this noise.

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
docker system prune -a
```

[[dotnet.md]]
