# Creating dockerfile

```
FROM node <--- use the node image from docker hub

WORKDIR /app <--- change working directory

COPY . /app <-- copy all project files to /app directory

RUN npm install <--- run npm install when creating and image

EXPOSE 80 <--- expose port 80 from the isolated, docker environment

CMD ["node", "server.js"] <--- start server when running the container(CMD executes when running the container, RUN when building the image). The CMD command should be the last command in the Dockerfile

```

Each docker instruction is a separate layer. When we want rebuild an image, docker uses cached layers if they didn't change unless it finds a change. Once a layer is changed, all subsequent layers are executed again. It gives a room for optimisations - we'd like to executed some commands before the others to not execute it again. For example. we may want to replace
```
COPY . /app
RUN npm install
```
with:
``` 
COPY package.json /app
RUN npm install
COPY . /app
```
to not execute npm install every time we made a code change.

# Docker commands
___

| Command       | Description | 
| ------------- |-------------| 
| `docker build .` | builds an image in the current directory | 
| `docker run IMAGE_NAME`    | runs a container based on given image  |  
| `docker run -p LOCAL_PORT:DOCKER_PORT IMAGE_NAME` | runs a container based on given image and exposing DOCKER_PORT | 
| `docker ps` | lists running containers |
|`docker ps -a` | lists all containers |
| `docker stop CONTAINER_NAME`| stops container with given name |


## Attached/detached mode

`docker run` starts container in Attached mode by default, when `docker start` starts it in Detached mode by default
In detached mode we are not following container logs, and we can execute commands in the same terminal session. It's opposite to the attached mode.

We can change default behaviour by adding `-d` to `docker run` command, or `-a` to ` docker start`.

The other way is to attach to the running container. It's possible by executing `docker attach CONTAINER`. Also, if we are interested in container logs only, it's possible to run `docker logs -f CONTAINER` to fetch container logs. `-f` flag is to follow incoming logs

## ENTERING INTERACTIVE MODE

Attached mode means we are listening to the output of the container but we can not interact with the container(add input)

flag `-i` (interactive), `-t` (tty) can help here

When we're restaring container like that we need to add `-a` flag to attach the container and `-i` to run it in interactive mode. We don't need to put `-t` flag, bacause it's already memorized after executing it previously

## DELETING IMAGES AND CONTAINERS

`docker rm CONTAINER1 CONTAINER2 ...`

`docker rmi IMAGE1 IMAGE2` - container needs to be removed first. Even the stopped one

`docker prune `- remove all stopped containers at once

`docker image prune` - remove all unused images

We can also run container based on docker image with `--rm` flag, to automatically remove the container when it stops
