---
layout: default
title: Deploy
grand_parent: SMDRM
parent: Get Started
nav_order: 2
---

# Prerequisites

A Docker registry is required to distribute images to all of Docker Engines participating in a swarm.

You can use the Docker Hub or [maintain your own](https://docs.docker.com/engine/swarm/stack-deploy/#set-up-a-docker-registry).

# Build the Docker Images

Build the Docker images and push them to the Docker registry.

{: .warning}
Building a Docker image for the first time can take several minutes to complete... ☕ 

1. Build the SMDRM base image.

{: .note}
> Environemnt Variables:
> 1. `POWERTRACK_PASSWORD=<changeme>` enables interactions with the Twitter Powertrack API,
> 2. `DOCKER_HUB=<url>` references your Docker registry,
> 3. `http_proxy=<url>` and `https_proxy=<url>` enable proxy authentication for requests outside the Docker build environment,
> 4. `VERSION=x.y.z` of Docker base image i.e. export VERSION=$(./tools/version.sh)

```shell
docker compose build
```

# Push to the Docker Registry

```shell
# docker login $DOCKER_HUB if your registry is not local
docker compose push
```

# Deploy the Stack

```shell
# add --with-registry-auth if you use a Docker registry with authentication
docker stack deploy --compose-file compose.yaml --compose-file compose.staging.yaml smdrm
```

# Swagger UI

Navigate to `http://<HOST_IP_ADDRESS>/api/v1/docs` to reach the API swagger UI to test the service.

# What's Next?

After successfully deploying the stack, you can [create your first data collection]({{ site.baseurl }}/smdrm/examples).
