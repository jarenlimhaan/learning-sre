# Container operations

## Images and container lifecycle

```bash
# Download an image without starting a container
docker pull <image>:<tag>

# List locally available images
docker image ls

# Start a named detached container and publish a host port to a container port
docker run --detach --name <name> --publish <host-port>:<container-port> <image>:<tag>

# Start a container with a persistent named volume
docker run --detach --name <name> --volume <volume>:<container-path> <image>:<tag>

# List running containers
docker container ls

# Include stopped containers
docker container ls --all

# Show detailed low-level container configuration and state
docker inspect <name-or-id>

# Follow a container's logs
docker logs --follow <name-or-id>

# Run an interactive shell inside a running container
docker exec --interactive --tty <name-or-id> /bin/sh

# Stop and restart a container
docker stop <name-or-id>
docker start <name-or-id>

# Remove a stopped container
docker rm <name-or-id>

# Remove an unused image
docker image rm <image>:<tag>

# List named volumes
docker volume ls
```

Containers package an application and its userspace dependencies, but they share the host kernel. Persist state in explicit volumes rather than in a container's writable layer.

Membership in the `docker` group is effectively root-equivalent on a typical Docker host. Prefer `sudo docker ...` or rootless container tooling unless that access is intentional.
