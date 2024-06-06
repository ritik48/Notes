# Docker Notes

Dockerfile to create an image of our express app

```
FROM node:20 -> base image from which we want to create an image for
                our application

WORKDIR usr/src/app -> this is the path where our application/code will be
                       stores in the image

COPY . . -> means copy everthing from current folder to that WORKDIR

RUN npm install -> runs npm install to get all the required package while   
                   creting image
EXPOSE 3000

CMD ["node", "index.js"] -> this runs when we start the container
```

### Docker commands
1. Create an image from this docker file

    `docker build . -t test_app`
2. Display all running containers

    `docker ps`
3. Display all images

    `docker images`
4. Start a container from an image

    `docker run image_name`
5. Port mapping: forward the request coming on a particular port on your machine to a port exposed in the container

    `docker run -p 3000:3000 image_name`

    here, the request that will come on port 3000 on your windows will be forward to port 3000 open in your container
6. Stop a container

    `docker stop container_id`

7. We can also give names to conatainer

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

---

### ENV Variables

Let's say we are using Environment variable inside out js file, like for port number, then to pass the port number from command line itself to the nodejs process we can do:

`PORT=8000 node index.js`

now, we can access it inside js file like:

`process.env.PORT`

To pass the environment variables inside container, we can use `-e` tag followed by environment variable.

`Docker run -p 3000:3000 -e MONGO_URI=SOME_URI mongo_image`


---

### Multi-stage builds

Let's say we we want to run different command when container starts, like 

`on development: node index.js`

`on production: nodemon index.js`

So, for this we can create a docker file where we can specify multiple stages. And while creating an image, we can only use a specific stage

With multi-stage builds, you use multiple `FROM` statements in your Dockerfile. Each `FROM` instruction can use a different base, and each of them begins a new stage of the build.

Example of docker file with three stages, one will be common i.e, from which all other stages will start
```
FROM node:20 AS base
WORKDIR user/src/app
COPY package*.json .
RUN npm install

FROM base as development
COPY . .
CMD ["nodemon", "index.js"]

FROM base as production
COPY . .
RUN npm prune --development
CMD ["node", "index.js"]
```
Here, we are creating 3 images. Only, image 2 and 3 use image 1 as their base image.

**Explaination of above dockerfile.**

-   `FROM node:20 AS base: `

    This line specifies the base image for the Docker container. In this case, it's a Node.js image version 20. It sets up the base environment for the subsequent instructions.

-   `WORKDIR /user/src/app: `

    This sets the working directory inside the Docker container to /user/src/app. This is where the application code and files will be copied and where subsequent commands will be executed.

-   `*COPY package.json .**:` 

    This instruction copies the package.json and package-lock.json (if it exists) from the local file system into the /user/src/app directory in the Docker container. These files are needed for npm to install dependencies.

-   `RUN npm install: `

    This command installs the dependencies listed in the package.json file into the Docker container. It's executed within the /user/src/app directory.

-   `FROM base AS development:` 

    This line creates a new stage in the Dockerfile based on the base stage. This is called a multi-stage build. It allows you to separate build-time dependencies from runtime dependencies, reducing the size of the final Docker image.

-   `COPY . .: `

    This instruction copies all files from the current directory (presumably the application code) into the Docker container's /user/src/app directory.

-   `CMD ["nodemon", "index.js"]: `

    This sets the default command to run when the container starts in development mode. It uses nodemon to watch for changes in the files and automatically restart the Node.js application when changes are detected.

-   `FROM base AS production: `

    This creates another stage in the Dockerfile based on the base stage.

-   `COPY . .: `

    Similar to the development stage, this instruction copies all files from the current directory into the Docker container's /user/src/app directory.

-   `RUN npm prune --development:` 

    This command removes development dependencies from the node_modules directory. It's done to reduce the size of the production image since development dependencies are not needed in the production environment.

-   `CMD ["node", "index.js"]:` 

    This sets the default command to run when the container starts in production mode. It runs the Node.js application directly without any watching mechanism like nodemon, which is suitable for production environments.


Now, to create an image from a aprticular stage, we use  `--target stage_name` with the build command.

To create an image for development:

`docker build . --target development -t dev_image`

To create an image for production:

`docker build . --target production -t prod_image`


---


