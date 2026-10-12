# Custom sandbox image

Default snapshot layered on `ghcr.io/nightona-co/sandbox:0.6.0` (adds the desktop wallpaper; extra packages go in the `Dockerfile`).

Also adds LibreOffice with UNO bindings, multilingual fonts (Noto incl. CJK, Nanum, emoji), the `lo-uno` helper (`/usr/local/bin/lo-uno`), and agent instructions in `/home/nightona/AGENTS.md` (symlinked as `CLAUDE.md`).

## Build and push to the local registry

```bash
docker compose -f docker/docker-compose.yaml up -d registry

TAG=0.6.0-custom.1   # bump on every change
docker build -t localhost:6000/nightona/custom-sandbox:$TAG images/custom-sandbox
docker push localhost:6000/nightona/custom-sandbox:$TAG
```

Then set `DEFAULT_SNAPSHOT=registry:6000/nightona/custom-sandbox:$TAG` in `docker/docker-compose.yaml` and restart the `api` service.

**Always use a new tag.** The API registers `DEFAULT_SNAPSHOT` only if no snapshot with that name exists yet, so re-pushing an existing tag is ignored.
