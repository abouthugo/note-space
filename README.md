# The Motivation

I am always learning new stuff and keeping track of my knowledge base and more importanttly my notes has become a major pain point to me. For this reason, I'm starting this repo to put all my notes, for now it will be a little messy but as I keep adding more stuff I will organize it gradually.

## Docker
[Official Docker Site](https://www.docker.com/)

In order to get started with docker you first need to create a `Dockerfile`. Here is an example of one for a node application:

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

### Some useful commands
* `docker version` shows useful information about the  docker installation on the current machine
* `docker info` displays a bunch of information about your docker images, containers and more.
* Build image: 
  * `docker image build -t <id>:<version> .`
* Log into the docker image shell: 
  * `docker run image -it <id> bash`
* Run image: 
  
```shell
docker container run -d --name <id> -p <LOCAL_PORT>:<EXPOSED_PORT> <id>:<version>
```

* List all running(or that were running) images: `docker ps -a`
* List images: `docker images`
* Stop a running image: `docker stop <container id>`
* Stop all running images: `docker stop $(docker ps -aq)`
* Remove all running/previously ran images `docker rm $(docker ps -aq)`
* Remove an image `docker rmi <image name>`
* Remove all images `docker rmi $(docker images -q)`
* To download images from Docker Hub: `docker pull <name of image>`

Under the hood, whenever you run any of these commands the docker client is making API calls to the docker daemon aka docker engine.

![Docker client and daemon](/illustrations/docker1.png)
  
### Some lingo

**Images**: blueprint of containers

**Containers**: running images

### Proper Native Clustering with Swarm

**Note:** this is totally a opt feature

A collection of dockers engines joined into a cluster is called a **Swarm**. Then, the docker engines running on each node inside the swarm are said to be running in **Swarm Mode**.

**Manager nodes** maintain the swarm. It is recommended to have an odd number of them, 3 or 5.

In order to run **services** you need to be running your containers in swarm mode. A service is a declarative way of running and scaling tasks.

For example, if I was to deploy 2 microservices one for backend storage and another one for frontend. If I wanted to tell docker that I want 3 instances of the container running the frontend always running I would execute a command like this:

#### `docker service create --name web-frontend --replicas 5`
To start a swarm:

#### `docker swarm init`

but sometimes you are better off specifying an advertised(exposed) ip address as well as a listen:

#### `docker swarm init --advertise-addr 127.0.0.0:2377 --listen-addr 127.0.0.0:2377`

**Important ports**
* Engine port: 2375
* Secure engine port: 2376
* Swarm port: 2377 (not officially yet)



