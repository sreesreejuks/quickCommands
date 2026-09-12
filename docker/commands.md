# Docker Quick Commands

Use this section for container lifecycle and cleanup tasks.

## Containers

Run an ephemeral, interactive Ubuntu container (auto-removed on exit):
```bash
docker run -it --rm ubuntu
```

Stop all running containers whose name contains a given substring
(`--filter "name=..."` matches on container name):
```bash
docker stop $(docker ps -q --filter "name=<name-filter>")
```

List running containers only (default):
```bash
docker ps
```

List all containers, running and stopped:
```bash
docker ps -a
```

List only exited/stopped containers:
```bash
docker ps -a --filter "status=exited"
```

List only unhealthy containers:
```bash
docker ps --filter "health=unhealthy"
```

List only healthy containers:
```bash
docker ps --filter "health=healthy"
```

List just the IDs of running containers:
```bash
docker ps -q
```

List just the IDs of exited containers (handy for cleanup, e.g. piping into `docker rm`):
```bash
docker ps -a -q --filter "status=exited"
```
