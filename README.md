# note-space
A space where I put all my developer notes

## Docker
[Docker Site](https://www.docker.com/)

The first thing you need to do is create a docker file, something like this:

```dockerfile
FROM node:latest
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY package.json /usr/src/app/
RUN npm install
ADD src /usr/src/app/src
ADD public /usr/src/app/public
RUN npm build
CMD [ "npm", "start" ]
EXPOSE 3000
```

Useful commands:
* Build image: 
  * `docker image build -t <id>:<version> .`
* Log into the docker image shell: 
  * `docker run image -it <id> bash`
* Run image: 
  
```shell
docker container run -d --name <id> -p <EXPOSED_PORT>:<LOCAL_PORT> <id>:<version>
```

* List running images: `docker ps -a`
* List images: `docker images`
* Remove a running image `docker rm <image name>`
  