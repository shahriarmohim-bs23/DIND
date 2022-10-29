
# Docker in docker

Docker in Docker (also known as dind) is, as the name implies, running Docker on top of a Docker container.


## Method 1: Docker in Docker Using dind
This method actually creates a child container inside a container. If you really want to, you can use “real” Docker in Docker, that is nested Docker instances that are completely encapsulated from each other. You can do this with the dind (Docker in Docker) tag of the docker image, as follows


```bash
 docker run --privileged -d --name dind docker:dind
 docker exec -it dind /bin/ash
```


## Method 2: Docker in Docker Using Dockerfile
Build the image:
```bash
 docker build -t dind .
```
Run Docker-in-Docker and get a shell where you can play, and docker daemon logs to stdout:

```bash
 docker run --privileged -t -i dind
```
Run Docker-in-Docker and get a shell where you can play, but docker daemon logs into /var/log/docker.log:
```bash
 docker run --privileged -t -i -e LOG=file dind
```
Run Docker-in-Docker and expose the inside Docker to the outside world:
```bash
 docker run --privileged -d -p 4444 -e PORT=4444 dind
```
Note: when started with the PORT environment variable, the image will just the Docker daemon and expose it over said port. When started without the PORT environment variable, the image will run the Docker daemon in the background and execute a shell for you to play.


## Daemon configuration
You can use the DOCKER_DAEMON_ARGS environment variable to configure the docker daemon with any extra options:
```bash
 docker run --privileged -d -e DOCKER_DAEMON_ARGS="-D" dind
```
 
