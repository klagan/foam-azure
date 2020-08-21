
## Error starting userland proxy: listen tcp 0.0.0.0:53: bind: address already in use.

I just disabled the service to make it work.  Luckily I dont have enough knowledge to know the implications of what I have done.  It just works for now.

```
sudo systemctl disable systemd-resolved.service
sudo systemctl stop systemd-resolved
```

## Change password

`pihole -a -p`

or if running in docker:

`docker exec -it <name of container> pihole -a -p`

## Ad lists

`https://firebog.net/`
