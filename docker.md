# Docker

Cheat sheet

```
docker container prune
docker system prune
docker ps --filter "status=exited" | grep 'weeks ago' | awk '{print $1}' | xargs --no-run-if-empty docker rm

```
