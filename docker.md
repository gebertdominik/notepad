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


Each docker 
