---
layout: default
title: Deploy
grand_parent: SMDRM
parent: Get Started
nav_order: 2
---

# Prerequisite

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
> 4. `VERSION=latest` of Docker base image set via the VERSION file i.e. export VERSION=$(<./VERSION)

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

# What's Next?

After successfully deploying the stack, you can [create a data collection]({{ site.baseurl }}/smdrm/data_collection).
