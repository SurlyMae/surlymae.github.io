---
date: "2019-12-15"
title: Containers (notes)
---

_Let's learn more about these, shall we?_

**Containers:**
- Creates a distributed application that leverages components in isolation
- Run on top of an engine (the Docker engine)
    - Docker is a standard AND a company; leader of the container ecosystem
- Approach software development differently
    - Are about structure and deployment, not about managing the application development process
- Have lower overhead for management and ops
- Are isolated, but can share OS and (where appropriate) bins/libraries
- When to use:
    - they do add cost and complexity (and initial latency in development)
    - to isolate apps for portability
    - to isolate apps for performance
    - to simplify development operations
    - to manage complexity
    - to leverage automation around portability/deployment
    - to provide better governance and security (by placing those services around the containers rather than within)
    - to provide better distributed computing capabilities
    - to provide automation services that can provide optimization and self-configuration
- Hold everything needed for an application to run, similar to a directory
- A container is an instance of an image
    - Images are read-only templates used to create Docker containers
- Registries are stateless, scalable, server-side apps that allow you to store and distribute Docker images (locally or not)
- Think of Docker Hub as GitHub for Docker container images (a central repo where you can store, maintain, retrieve Docker images)
- Data needs to be persisted in Docker for most apps, or the data won't exist after the container is shut down
    - Containers are short-term, and once a container is removed, the data is gone

From [Adrian Mouat](https://blog.container-solutions.com/understanding-volumes-docker):
- Docker file system works like this:
    - Docker images are stored as a series of read-only layers.
    - When a container is started up, Docker takes the read-only image and adds a read-write layer on top
    - if the container modifies an existing file, the file is copied out of the underlying read-only layer and into the top-most read-write layer where the changes are applied
    - The version in the read-write layer hides the underlying file, but doesn't destroy it; it still exists in the underlying layer. 
    - When a container is deleted, relaunching the image will start a fresh container without any of the changes made in the previously running container; those changes are lost.
    - Docker calls this combo of read-only layers w/ read-write layer on top a Union File System.
    - Docker data volumes are directories/files outside of the default Union File System and exist as normal directories/files on the host filesystem

[Cool guide to Kubernetes](https://azure.microsoft.com/en-us/resources/videos/the-illustrated-children-s-guide-to-kubernetes/)

**Lower-level notes:**
- Docker is a DevOps tool for putting your apps into containers
- You can run the containers anywhere
- Everywhere you run a container, the environment will be identical
- A container image is a lightweight, standalone, executable package of a piece of software that includes everything needed to run it.
- Docker helps reduce conflicts between teams running different software on the same infrastructure
- Even within one web application, you can run each service (and all of its particular libraries/dependencies) in its own separate container
- OSs consist of two parts: the kernel and a set of software
    - the kernel is responsible for interacting with the underlying hardware
    - the software makes the OSs different from each other - it includes the UI, compilers, drivers, file managers, dev tools, etc.
    - docker containers share the kernel - so a docker container hosted on an Ubuntu OS can use any flavor of Linux on top of it (Linux is the kernel, Ubuntu is a flavor or Linux and what makes it a flavor is its own set of software), the docker containers have the software part of the Linux OSs in them.
- A container only runs as long as the process that's inside it is alive
- Commands:
    - run: starts a container `docker run nginx`
        - attached: run container in foreground (attached to console/standard out of container), you'll see output of service on screen, can't do anything else in that terminal until container stops `docker run my_container/my_service`
        - detach: run container in background mode so you can get back your terminal prompt `docker run -d my_container/my_service`
        - attach: attach to the container running in background `docker attach my_container_name_or_ID`
        - name: adds name to container `docker run --name my_container_name image_name`
        - tag: run a container using a tag `docker run container_name:tag`
        - interactive: run in interactive mode. maps the standard input of your host to the docker container `docker run -i my_container/my_service` 
        - terminal: run in interactive mode with a terminal where you can see prompts from your app `docker run -it my_container/my_service`
        - port: maps port on docker host to port on docker container `docker run -p 80:5000 my_container/my_service`
        - volume: for data persistence (not losing the container's data when the container ceases to exist), map directory outside of the container but on the docker host to a directory in the container `docker run -v host_directory:container_directory my_container`
    - ps: lists running containers `docker ps`
    - ps -a: lists all containers `docker ps -a`
    - stop: stops a container `docker stop my_container`
    - rm: removes a container `docker rm my_container`
    - images: lists images available on host `docker images`
    - rmi: removes image `docker rmi nginx` (ensure no containers are running off this image before removing)
    - pull: downloads an image, stores it on host `docker pull nginx`
    - exec: execute a command on a running container `docker exec my_container cat /etc/hosts` (prints to the terminal the contents of the etc/hosts file)
    - inspect: returns all details about container `docker inspect my_container_name`
    - logs: view container logs `docker logs my_container_name`
    
- Containerizing your already existing app / creating your own image
    - Start by thinking about every step you'd have to do to deploy the app manually. Example steps: 
        1. Start with an OS
        1. Update source repositories
        1. Install dependencies
        1. Copy source code of app over to a deployment location, like `/opt` folder
        1. Run the web server

[Resource](https://www.youtube.com/watch?v=fqMOX6JJhGo)