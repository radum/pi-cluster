# Docker Media Center

> Light version based on:
> - https://github.com/jarrad-roam/bolt-media
> - https://github.com/danshilm/Tatooine-380

## How to run

Make sure in your hosts file you have `127.0.0.1	home.media`.

```shell
# Make sure you are in this folder
docker compose -f docker-compose.yml --project-name media up
```

Then open:

- http://localhost:9090/ Traefik
- http://home.media:9117/UI/Dashboard - Jacket
- http://home.media:8989/ Sonarr
- http://home.media:9091/ Transmission
- http://home.media:6767/ Bazarr

# TODO

- [ ] Set Traefik to handle the docker containers routing (this will fix using local 192. ips inside the apps of each container)
- [ ] Transmissions is crashing very often for some reason
