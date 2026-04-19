# **Docker Notes**

#### Dockerfile

A Dockerfile is a text-based document that's used to create a container image. It provides instructions to the image builder on the commands to run, files to copy, startup command, and more.

##### Common Instructions:

FROM <image> - Specifies the base image that the build will extend

WORKDIR <path> - Sets the working directory where files will be copied and commands executed

COPY <host-path> <image-path> - Copies files from the host into the container image

ADD <host-path> <image-path> - Like COPY but can also handle URLs and auto-extract archives

RUN <command> - Executes commands during image build

ENV <name> <value> - Sets environment variables for the container

EXPOSE <port-number> - Indicates which ports the container will listen on

USER <user-or-uid> - Sets the default user for subsequent instructions

CMD \["<command>", "<arg1>"] - Sets the default command for the container (only one CMD per Dockerfile)

ENTRYPOINT \["<command>", "<arg1>"] - Sets the main executable (can be combined with CMD)

ARG <name>\[=<default>] - Defines build-time variables (different from ENV)

VOLUME \["<path>"] - Creates a mount point for external volumes

##### Important Notes:

CMD vs ENTRYPOINT:

CMD provides default arguments to ENTRYPOINT

If ENTRYPOINT exists, CMD becomes its arguments

If no ENTRYPOINT, CMD is the command to run

COPY vs ADD: Prefer COPY unless you need ADD's extra features

RUN vs CMD: RUN executes during build, CMD executes at container runtime

#### Building and Running Containers

1\. Build an Image

docker build -t image-name:tag .

docker build -t myapp:v1 . # With tag

docker build -f Dockerfile.dev . # Custom Dockerfile name

2\. Run a Container

docker run image-name

docker run -p 8080:80 image-name # Port mapping

docker run -d image-name # Detached mode

docker run --name mycontainer image-name # Named container

docker run -it image-name /bin/bash # Interactive with terminal

3\. Container Management

bash

\# List containers

docker ps # Running containers

docker ps -a # All containers

docker ps -l # Last created container

\# Container lifecycle

docker start <container> # Start stopped container

docker stop <container> # Stop container

docker restart <container> # Restart container

docker rm <container> # Remove container

docker rm $(docker ps -aq) # Remove all containers

\# Container inspection

docker logs <container> # View logs

docker logs -f <container> # Follow logs

docker exec -it <container> /bin/bash # Execute command in running container

docker inspect <container> # Detailed container info

docker stats # Resource usage

4\. Image Management

bash

\# List images

docker images

docker image ls

\# Remove images

docker rmi <image>

docker rmi $(docker images -q) # Remove all images

\# Clean up

docker system prune -a # Remove unused data

\# Image inspection

docker history <image> # Show image layers

docker inspect <image> # Image details

Common Docker Run Options

bash

-d # Run in background

-p <host>:<container> # Port mapping

-v <host>:<container> # Volume mounting

--name <name> # Container name

-e VAR=value # Set environment variable

--network <network> # Connect to network

--restart always # Auto-restart policy

--rm # Remove after exit

-it # Interactive terminal

### DOCKER COMPOSE
