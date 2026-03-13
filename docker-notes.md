# Docker Notes

## Why Docker
- Reproducible environments across machines
- Isolated dependencies and processes
- Faster onboarding and deployments

## Core Ideas
- Image: Read-only template with app + dependencies
- Container: Running instance of an image
- Dockerfile: Build recipe for an image
- Registry: Remote store for images (Docker Hub, GHCR)
- Volume: Persistent data storage outside containers
- Network: Virtual network for container communication

## Install (Windows/Mac/Linux)
- Docker Desktop includes:
  - Docker Engine (daemon)
  - Docker CLI
  - Docker Compose plugin
- Linux: install `docker` and `docker-compose-plugin` packages

## CLI Basics
```bash
docker version
docker info
docker help
```

## Images
```bash
docker images
docker pull ubuntu:24.04
docker build -t myapp:1.0 .
docker rmi myapp:1.0
```

## Containers
```bash
docker run -it ubuntu:24.04 bash
docker run --rm alpine:3.19 echo "hello"
docker ps
docker ps -a
docker stop <container_id>
docker rm <container_id>
```

## Common `docker run` Flags
- `-it` interactive terminal
- `--rm` auto-remove container on exit
- `-d` detached (background)
- `-p HOST:CONTAINER` port mapping
- `-e KEY=VALUE` environment variables
- `--name` assign a name
- `-v HOST:CONTAINER` bind mount

## Volumes
```bash
docker volume ls
docker volume create mydata
docker run -v mydata:/data ubuntu:24.04 bash
docker volume rm mydata
```

## Bind Mounts
```bash
docker run -v ${PWD}:/app -w /app node:20 npm test
```

## Dockerfile Basics
```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3000
CMD ["node", "server.js"]
```

### Build and Run
```bash
docker build -t myapp:1.0 .
docker run -p 3000:3000 myapp:1.0
```

## Multi-Stage Builds
```dockerfile
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=build /app/dist ./dist
CMD ["node", "dist/server.js"]
```

## Networking
```bash
docker network ls
docker network create appnet
docker run -d --name db --network appnet postgres:16
docker run -d --name api --network appnet -e DB_HOST=db myapi:1.0
```

## Logs and Exec
```bash
docker logs <container_id>
docker logs -f <container_id>
docker exec -it <container_id> bash
```

## Cleaning Up
```bash
docker system df
docker system prune
docker image prune
docker container prune
docker volume prune
```

## Docker Compose (Quick Start)
`docker-compose.yml` example:
```yaml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - dbdata:/var/lib/postgresql/data
  api:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db
volumes:
  dbdata:
```

Run:
```bash
docker compose up -d
docker compose logs -f
docker compose down
```

## Troubleshooting
- Permission errors on Linux:
  - Add user to `docker` group, then re-login
- Port already in use:
  - Change host port in `-p` or stop the other service
- Slow builds:
  - Use `.dockerignore` and multi-stage builds

## `.dockerignore` Example
```
node_modules
dist
.git
.env
```

## Quick Reference
```bash
docker pull <image>
docker build -t <name>:<tag> .
docker run -d -p 8080:80 <image>
docker ps -a
docker logs <id>
docker exec -it <id> sh
docker stop <id>
docker rm <id>
```
