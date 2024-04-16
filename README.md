# Docker Octane
[![Deploy Octane Image](https://github.com/codebarista/docker-octane/actions/workflows/main.yml/badge.svg)](https://github.com/codebarista/docker-octane/actions/workflows/main.yml)

Latest PHP Alpine Docker image to run Laravel Octane with Swoole or FrankenPHP.


## Setup

Install [Go-Task](https://taskfile.dev)

```shell
brew install go-task
```

## Config

Copy `.env.example` to `.env` and set your env vars:

```dotenv
DOCKER_IMAGE_TAG=codebarista/octane:latest
DOCKER_PLATFORM=linux/amd64,linux/arm64
DOCKER_LOCAL_PLATFORM=linux/arm64
DOCKER_REGISTRY=docker.io
```

## Load

Build and load image to local registry.

```shell
task load
```

## Push

Build and push image to remote registry.

```shell
task push
```

## Fork

Fork and set GitHub secrets for custom push.

```dotenv
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

## Specials

- including xdg-utils and Node.js for Laravel Nova component development
- including build essentials and fonts for Node canvas
- including those fonts and canvas to support [laravel-echarts](https://github.com/codebarista/laravel-echarts) :) 
