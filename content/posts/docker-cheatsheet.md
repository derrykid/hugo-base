---
title: "Docker Cheat sheet, a general cheat sheet for all use case"
date: 2023-01-08T17:32:01Z
tags: ["linux", "command", "docker", "docker compose", "development", "container", "network", "java", "web", "server"]
categories: ["technology and development"]
---

# Docker general

## Help - `docker COMMAND --help` 

This is for each command's detail

For example, `docker run --help`

## TL;DR

- `docker run <image>` - to initialize a new container
- `docker start <container_name>` start running the "stopped" container
- `docker stop <container_name>`

## `docker run` - Initialize a container 

> Run a command in a new container

`Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

Basic form to initialize a container, or to run a command
```bash
$ docker run <imageName>

              <image>  <-----COMMAND------->
                |         |
                v         v
$ docker run alpine /bin/echo 'Hello World'
Hello World
$
```

### Initialize a alpine Linux and use the command prompt
For example, this initialize a *alpine Linux.* 
```bash
$ docker run alpine
$   # nothing here
```

But this container stopped right after it's initialized. There's no command prompt. To get it running and use the command prompt:
```bash
$ doccker run --interactive --tty alpine

# shorthand
$ docker run -it alpine
```

You might've created 2 containers. If you initialize the first container and run another `docker run` command. 

---
Use `docker run -i -t alpine`

If run `docker run -t alpine`, there might be error, e.g. can't exit. Add `-i` will solve the issue.

```bash
# only -i
echo hi
hi

# only -t
$ exit       # can't exit
```

### detach (to daemon)

- `-d` is to let it running in daemon, **not mean detach.** 

When `docker run <image>`, you're using and reading the output from the console. This makes us unavailable to do anything else. We can use detach to make it run in the background.

```bash
docker run -d someName/simple-webapp
```

### set environment variable

`docker run -e VARIABLE=MY_VAR <imageName>`

### `docker run -p HOST:CONTAINER`port

`Usage: docker run -p [external]:[Internal]`

Example, this will open in http://localhost:8181 to container port 80)
```bash
docker run --name mynginx -p 8181:80 -d nginx
```

## `docker start` - Restart a container

> Start one or more stopped containers

`Usage:  docker start [OPTIONS] CONTAINER [CONTAINER...]`

```bash
docker start -i <container_name>
```


## Attach to a running container - `docker attach`

`Usage:  docker attach [OPTIONS] CONTAINER`

When we want to attach:
```bash
docker attach <ID>
```

### PORT mapping

Docker host address locally: `http://192.168.1.5`

```bash
docker run derry/webApp
```

The running port might be `5000`

You can map the port by the command. Many containers can run in different port:
```bash
docker run -p 80:5000 derry/webApp
docker run -p 1000:3306 mysql
```

### `docker run --name` to set custom name

using `--name` flag
```bash
docker run -dp 8080:8080 --name custom_naming java-docker
```

Map port 300 of the host to port 80 in the container. So in practice, you open your browser at `http://localhost:300/` to access to docker container. And the container inside is open port at `80`.

### run a specific version by tag

By default, the docker runs the latest(also a tag) ver.

If you'd run old one:
```bash
docker run redis:<tagVersion>
```

## Environment variables

Use `-e` to export
```
docker run -e APP_COLOR=blue <imageName>
```

Use `inspect` and in Config section, it shows the env variables.


## images - to see all images available locally

```bash
docker images
```


### build and tag an image

To build your image, run `docker build SOURCE_LOCATION`
```bash
docker build --tag java-docker .
```

Optionally, use the `--tag` to tag an image. It's in format of `name:tag`. If do not pass a tag, it uses **latest** as default.

After run `docker build --tag java-docker . `, we list our images by `docker images`
```bash
docker images

REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
java-docker              latest    b78dea468a3f   3 minutes ago   559MB
```

### remove the images - `docker rmi`

The image files in system might take up MBs
```bash
docker rmi <imageName>
```

### Tag image after build
Alternatively, you can tag it after build

`docker tag NAME:TAG NAME:NEW_TAG`
```bash
docker tag java-docker:latest java-docker:v22_3
```

And list the images:
```bash
REPOSITORY               TAG       IMAGE ID       CREATED         SIZE
java-docker              latest    b78dea468a3f   5 minutes ago   559MB
java-docker              v22_3     b78dea468a3f   5 minutes ago   559MB
```

### remove the image by `rmi`

`docker rmi <name>:<tag>`
```bash
$ docker rmi java-docker:v22_3 
Untagged: java-docker:v22_3    // output message
```

### pull - download an image

```bash
docker pull <imageName>
```

### Create your own image

Create a `Dockerfile`
It's consists of `<INSTRUCTION> <ARGUMENT>`
```
FROM Ubuntu     // it's either a base OS or another image

RUN apt-get update
RUN apt-get install python

RUN pip install flask
// more commands

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```

Once have your `Dockerfile`, run command `docker build --file Dockerfile -t <imageName>`, e.g. `docker build --file Dockerfile -t derry:myImage` to build your image. It will create an image locally. If you want to push, use `docker push <imageName>`

## docker process

### `docker ps` - list running containers

`Usage:  docker ps [OPTIONS]`

List all running containers. Example:
```bash
[derry@Man-x1 spring-petclinic]$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS             PORTS                                                  NAMES
c79d572de49a   java-docker    "./mvnw spring-boot:…"   5 seconds ago   Up 4 seconds       0.0.0.0:300->8080/tcp, :::300->8080/tcp                gifted_wiles
```

#### `docker ps -a` to list all containers either running or stop

```bash
docker ps -a
```

### `docker stop <name>` - stop a container

The container or we say the process is stopped. However, it's not removed. You can restart it by `docker restart <container_name>`

```bash
docker stop <container_name>
docker stop <container_id>

// example
docker stop trusting_beaver
```

### `docker restart <name>` - restart a stopped container

There might be few containers that are stopped but not removed:
```bash
[derry@Man-x1 ~]$ docker ps -a
CONTAINER ID   IMAGE                    COMMAND                  CREATED          STATUS                       PORTS                                                  NAMES
a5bd20f08edc   java-docker              "./mvnw spring-boot:…"   3 minutes ago    Up 3 minutes                 0.0.0.0:8080->8080/tcp, :::8080->8080/tcp              nervous_snyder
ab55379adc06   java-docker              "./mvnw spring-boot:…"   5 minutes ago    Exited (143) 3 minutes ago                                                          naughty_volhard
5820d5e693e2   java-docker              "./mvnw spring-boot:…"   7 minutes ago    Exited (143) 5 minutes ago                                                          confident_feynman
c79d572de49a   java-docker              "./mvnw spring-boot:…"   10 minutes ago   Exited (143) 7 minutes ago                                                          gifted_wiles
2a0736d59236   java-docker              "./mvnw spring-boot:…"   20 minutes ago   Exited (1) 11 minutes ago                                                           quirky_dijkstra
834f35b8fe81   docker/getting-started   "/docker-entrypoint.…"   2 hours ago      Exited (0) 2 hours ago                                                              clever_spence
7fbda5eef9b4   docker/getting-started   "/docker-entrypoint.…"   2 hours ago      Created                                                                             relaxed_poincar
```

restart it simply by:
```bash
docker restart <name>
```

### remove a container - `docker rm`

to remove and free the memory

```bash
docker rm <name>
```

## execute a command on running container `docker exec`

`docker exec <NAMES> command`
```bash
// example
docker exec <NAMES> cat /etc/hosts

// open the terminal of container, i - interactive, t - tty
docker exec -it <name> bash
```

### STDIN - when needs input for the task

If we write a script that asks for user name and prints out the name:

```bash
// run the script
Welcome! Enter your name: Derry

// output
Hello Derry!
```

When we run the docker image, it won't ask for our input
```bash
docker run <imageName>

// output
Hello !
```

We have to add `-i` flag to make it interactive
```bash
docker run -i <imageName>
```

However, it won't show the terminal prompt, we have to add `-t` flag to add a terminal
```bash
docker run -it <imageName>
```


## supervise the container

### container info in json `docker inspect`
```bash
docker inspect <name>
```

Output sample:
```json
[
    {
        "Id": "952899189ac4ab6fab4ddcc057884a0dde4665c338f01326e438c5e1039686ca",
        "Created": "2022-12-28T15:12:07.480095938Z",
        "Path": "/bin/sh",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 267801,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-12-28T15:12:08.008346283Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:d7d3d98c851ff3a95dbcb70ce09d186c9aaf7e25d48d55c0f99aae360aecfd53",

```

### log

`docker logs <container_name>`
- `-f` flag for following

```bash
docker logs -f my-container
```

### history

Sees the history of a specify docker Dockerfile layer
```
docker history <imageName>
```


# volume - data persistent

## Create volume by `docker run -v`
If you want data persistent even after you destroy the image/container, use `volume`

`docker run -v <pathInHost>:<pathInContainer> <imageName>`

```bash
docker run -v /home/derry/playground:/storage alpine
```
This will result in creating a directory in container root: `/storage`. It links to host directory `/home/derry/playground`. You can find the files from host in container as well.

## Search for volume location

```bash
docker run -v /home/derry/playground:/storage alpine
```
This will create a directory in container root named `storage`. To find the directory in host OS, use this command:
```bash
docker inspect -f '{{.Mounts}}' <containerId>
```
This will give a path to the volume directory. Usually at `/var/lib/docker/volumes/`

### Create by `docker volume` 

`Usage:  docker volume COMMAND`

```
docker volume create data_volume
docker run -v data_volume:<data_path> <imageName>
```

Prefer style instead of `-v`
```
docker run --mount type=bind,source=/data/mysql,target=<data_path> <imageName>
```

### By `Dockerfile`
```bash
FROM ubuntu
RUN mkdir /myvol
RUN echo "hello world" > /myvol/greeting
VOLUME /myvol
```

This `Dockerfile` once build, it will use ubuntu image and create a directory at root`/myvol`. In the /myvol, create a file `greeting` with `hello world` text. The `VOLUME /myvol` means the directory in container will persist. To find where the data is saved, go to host OS and find it by:
```bash
docker inspect -f '{{.Mounts}}' <containerId>
```

# Dockerfile

*Blueprint to create an image. That's it.* 

> A Dockerfile is a text document that contains the instructions to assemble a Docker image. When we tell Docker to build our image by executing the `docker build` command, Docker reads these instructions, executes them, and creates a Docker image as a result.

- `FROM` - set base image
- `RUN` - execute the command in **building image process.** 
- `ENV` - set environment variable
- `WORKDIR` - set working directory, like `cd`. If the directory doesn't exist, it will be created.
- `VOLUME` - create mount-point for a volume
- `CMD` - set executable for container
- `COPY` - copy file from host to guest location `COPY source destination`
- `EXPOSE` - specify the container port

## Create a Dockerfile

At root of your project, create a file named `Dockerfile`. 

```
# syntax=docker/dockerfile:1

FROM eclipse-temurin:17-jdk-jammy

WORKDIR /app

COPY .mvn/ .mvn
COPY mvnw pom.xml ./
RUN ./mvnw dependency:resolve

COPY src ./src

CMD ["./mvnw", "spring-boot:run"]
```

## Build the image

```bash
docker build . --tag <imageName>
```

### Dockerfile Commands

#### `ENTRYPOINT`

> It specifies a command that will always be executed when the container starts. It works like Linux *alias*. Always prefix to your input.

For example, when `docker run -it ubuntu`, it starts the ubuntu and **gives you the shell prompt.** Why it gives you the shell prompt? Because it specifies the `ENTRYPOINT ["/bin/bash"]`.

---
The exec form, passing json array, is the preferred form:
```
ENTRYPOINT ["executable", "param1", "param2"]
```

##### Example

For example, this image always `ls` directory or file. Let's name it `listing_container`
```
FROM alpine
ENTRYPOINT ["/bin/ls", "-la"]
```

Let's initialize it, and pass argument to it. Remember the `docker run`:

`Usage:  docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`

```
$ docker run -i -t listing_container /srv

# this will be combined to command: "/bin/ls -la /srv"
# like you run the following command in the container

$ ls -la /srv
total 16
drwxr-xr-x  4 root  root  4096 11月 25  2021 .
drwxr-xr-x 17 root  root  4096 11月  7 10:14 ..
dr-xr-xr-x  2 root  ftp   4096 10月 16  2021 ftp
drwxr-xr-x 12 derry derry 4096  7月 16 14:55 http
```

So it's shorthand! You can build some containers for specific uses. 

If you want to change the default behaviour:
```
# this will run "cat /etc/passwd"
$ docker run --it --entrypoint="/bin/cat" ubuntu /etc/passwd
```

#### `CMD`

There are 3 forms but this form, pass json array, is preferred:
`CMD ["exectable", "param1", "param2"]`

```
CMD ["java", "-jar", "app.jar"]
```

##### Work as default behaviour

A Dockerfile:
```
FROM alpine
CMD ["/bin/ping", "www.google.com"]
```
```bash
docker build . -t ping_machine
docker run -it ping_machine

# you get:
PING www.google.com(...omit...) 56 data bytes
64 bytes from (...omit...) icmp_seq=1 ttl=119 time=22.3 ms
64 bytes from (...omit...) icmp_seq=1 ttl=119 time=20.3 ms
```

When run this container, it will start to ping the *www.google.com*.

##### Work as arguments for ENTRYPOINT

> It specifies arguments that will be fed to the `ENTRYPOINT`

```
FROM alpine
ENTRYPOINT ["/bin/ping"]
CMD ["localhost"]
```

Running the image, you will ping the *localhost*
```bash
docker build . --tag ping_container
docker run -it ping_container

# you get
PING localhost (127.0.0.1): 48 data bytes
56 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.096 ms
56 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.088 ms
56 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.088 ms
```

You can change the this default image behaviour by:
```bash
                             It is the argument passed to replace ["localhost"]
                                  vvvvvv
docker run -it ping_container www.google.com
```
