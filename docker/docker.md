# Docker Concepts

![Docker-Logo](images/Docker-Logo.png)

## Why use Compose?

Simplified control, while trying to set up a Minikube cluster I ran into some 
issues that's why i decided to use Docker Compose as an alternative. Since the 
applications dockerfiles are already set up it takes less time to learn how 
to manage multi-container applications through a single YAML file. Additionally,
Compose its specially well suited for testing environments. Although Minikube
is also quite useful, i think docker better fits our needs here.

## Simple operations with docker

Build a container image from the present Dockerfile.

`docker build --detach -t nodejs-hello-world:1.0.0 .`

Run a container with the container image mapping localhost port 80 to the 3000 
from the container.

`docker run -p 80:3000 --detach nodejs-hello-world:1.0.0`

Pull a container image available on Docker Hub.

`docker image pull alpine:3.18.2`

Save the container image to a specific file.

`docker save -o alpine:3.18.2.tar alpine:3.18.2`

Load a container image from a file.

`docker load -i alpine:3.18.2.tar`