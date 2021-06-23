# Creating dockerfile

```
FROM node <--- use the node image from docker hub

WORKDIR /app <--- change working directory

COPY . /app <-- copy all project files to /app directory

RUN npm install <--- run npm install when creating and image

EXPOSE 80 <--- expose port 80 from the isolated, docker environment

CMD ["node", "server.js"] <--- start server when running the container(CMD executes when running the container, RUN when building the image). The CMD command should be the last command in the Dockerfile

```


# Docker commands
docker build .  <-- builds an image in the current directory
docker run IMAGE_NAME <-- runs an container based on image
docker run -p LOCAL_PORT:DOCKER_PORT IMAGE_NAME
docker ps <-- list running containers
docker ps -a <-- list all containers
docker stop CONTAINER_NAME
