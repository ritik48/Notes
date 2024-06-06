# Docker Notes

### What is an Image ?

A docker image is a template which contains set of instructions which will be used to create a container. Think of an image like a blueprint or snapshot of what will be in a container when it runs.

An image is composed of multiple stacked layers, like layers in a photo editor, each changing something in the environment. Images contain the code or binary, runtimes, dependencies, and other filesystem objects to run an application.

### what is a Container ?

A container is a running instance of an image. It is a lightweight, standalone, executable package that includes everything needed to run a piece of software, including the code, runtime, system tools, system libraries, and settings.

---

Dockerfile to create an image of our express app

```
FROM node:20 -> base image from which we want to create an image for
                our application

WORKDIR usr/src/app -> this is the path where our application/code will be
                       stored in the image

COPY . . -> means copy everthing from current folder to that WORKDIR

RUN npm install -> runs npm install to get all the required package while   
                   creating image
EXPOSE 3000

CMD ["node", "index.js"] -> this runs when we start the container
```

### Docker commands
1. Create an image from this docker file

    `docker build . -t test_app`
2. Display all running containers

    `docker ps`
4. Display all images

    `docker images`
5. Start a container from an image

    `docker run image_name`
6. Port mapping: forward the request coming on a particular port on your machine to a port exposed in the container

    `docker run -p 3000:3000 image_name`

    here, the request that will come on port 3000 on your windows will be forward to port 3000 open in your container
7. Stop a container

    `docker stop container_id`

8. We can also give names to conatainer

    `docker run -p 3000:3000 --name container_name image_name`

Exch step in the dockerfile is basically termed as layer.
If any layer change, then all the layer from that to the end will not use cached version of the previous build, instead will start fresh.

#### Why layers ?
-   Caching
-   Re-using layers
-   Faster Build time

When we do docker build to create an image, then some files get downloaded like base image from whcih we are creating and further the steps that we have written in the dockerfile.

Now, if we build from the same file again then the process will take comparativly less time, as docker caches the steps. So if at a particular step it finds that it has executed it before and has the required files then it uses from cache. 

But, for a particular step in dockerfile, if it finds that something has changes then from that step to the end of dockerfile, it doesn't use the cache instead freashly downloads the required dependencies.

But this might cause performance issue, like lets say we changed index.js file then from this step i.e, `COPY . .` it will reject the cache and will start fresh , which means in the further step `npm install` will also run, which logically should not happen as  package.json didn't change. So we need some way to avoid this issue. 

Solution:
Introduce a new layer, where we first copy the package.json file, and then run npm install and then copy all the files, then it will solve the issue. Now even if the index.js file changeds, still as the first step is package.json, and sicne it didn't change so the npm install will not run again in the subsequent build.

Modified dockerfile

```
FROM node:20

WORKDIR usr/src/app

COPY package*.

RUN npm install

COPY . .

EXPOSE 3000

CMD ["node", "index.js"]
```

---

## Volumes and Networks

### Volumes
-   Used to persists data across starts
-   specifically usefull for things like database

we first create the volume

`docker volume create volume_name`

Now, when we start the container let's say of mongo, then we will mount this volume to the directory where mongo stores the data i.e, in `/data/db`

`docker run -v volume_name:/data/db -p 27017:27017 mongo`

In the context of Docker, when we mount a volume inside a container, it essentially links a directory on the host machine to a directory inside the container. This means that any data written to that directory from within the container is actually stored on the host machine, thanks to the volume mount.

Now, whenever we run the mongo container with this volume, then the data will persists, as volumes exists outside of the container lifecycle.

---
### Networks

It basically allows two containers to communicate with each other.
Let's say we have an express server running inside container A, and a mongo server running inside container B, then Networks allows these two to talk to each other.

#### Commands
-   To display available networks

    `docker network ls`

-   First, create a network

    `docker network create my_network`
-   Attach network to a container (express server)
    
    `docker run -p 3000:3000 --network network_name image_name`

- Attach mongo container also to the same network, we will also give the mongo container a name so that we can use it to communicate with this container from express server running on the same network

    `docker run -p 27017:27017 --name mongo_container --network network_name mongo` 

-   Now, in express server we will use the following url to connect to the mongodb server running in the container

    `mongoose.connect("mongodb://mongo_container:27017/my_db")`
