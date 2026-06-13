# Docker Notes

### What is an Image ?

A docker image is a template which contains set of instructions which will be used to create a container. Think of an image like a blueprint or snapshot of what will be in a container when it runs.

An image is composed of multiple stacked layers, like layers in a photo editor, each changing something in the environment. Images contain the code or binary, runtimes, dependencies, and other filesystem objects to run an application.

### What is a Container ?

A container is a running instance of an image. It is a lightweight, standalone, executable package that includes everything needed to run a piece of software, including the code, runtime, system tools, system libraries, and settings.

### What is a Docker Registry ?

It is a storage like github where we can pusha nd pull the docker images.
Dockerhub: Official Default Docker Registry https://hub.docker.com
AWS ECR: AWS Registry for images
Artifact Registry: Googles Registry for images

### Docker Engine

It consists of following components:

- Docker Cli - this is where the docker commands are executed
- Docker API - acts as a messanger between docker cli and docker daemon
- Docker Daemon - manages images , containers and resources

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

3. Display all images

   `docker images`

4. Create Container from image
   This creates a new container from this image.
   `docker create image_name`

5. Start a container from an image  
   Run command pulls the image if it does not exist locally, and then created a new container from it and start it.
   `docker run image_name`

6. Only pulls the image and not start the container
   `docker pull image_name`

7. Start an existing container

   `docker start container_name/id`

8. Port mapping: forward the request coming on a particular port on your machine to a port exposed in the container

   `docker run -p 3000:3000 image_name`

   here, the request that will come on port 3000 on your windows will be forward to port 3000 open in your container

9. Stop a container

   `docker stop container_id`

10. We can also give names to conatainer

    `docker run -p 3000:3000 --name container_name image_name`

11. Run the cotainer in detached mode

    `docker run -d container_name`

12. Add Container Name

    `docker run --name my_nginx nginx`

13. Delete a container

    `docker rm container_name/id`

14. See Container Logs

    `docker logs container_name/id`
    This displays log that was present at this moment of command execution. It's not live.

    To get live logs:
    `docker logs -f container_name/id`

15. Execute commands inside the container/go inside the container

    `docker exec -it container_name/id bash`
    This commands executes `bash` command inside this container in the interactive mode.

16. Remove all the stopped containers and images
    `docker container prune`
    `docker image prune`

### Docker Image Tags

Tags are used to identify different versions of an image
Representation: image_name:tag

e.g., `nginx:latest`, `redis:7`

By default images are tagged as `latest`, and the docker `pull` command also pulls the `latest` tag by default

---

### Layers in Dockerfile

Exch step in the dockerfile is basically termed as layer.
If any layer change, then all the layer from that to the end will not use cached version of the previous build, instead will start fresh.

Dockerfile

```
FROM node:20

WORKDIR usr/src/app

COPY . .

RUN npm install

EXPOSE 3000

CMD ["node", "index.js"]
```

#### Why layers ?

- Caching
- Re-using layers
- Faster Build time

When we do docker build to create an image, then some files get downloaded like base image from whcih we are creating and further the steps that we have written in the dockerfile.

Now, if we build from the same file again then the process will take comparativly less time, as docker caches the steps. So if at a particular step it finds that it has executed it before and has the required files then it uses from cache.

But, for a particular step in dockerfile, if it finds that something has changes then from that step to the end of dockerfile, it doesn't use the cache instead freashly downloads the required dependencies.

But this might cause performance issue, like lets say we changed index.js file then from this step i.e, `COPY . .` it will reject the cache and will start fresh , which means in the further step `npm install` will also run, which logically should not happen as package.json didn't change. So we need some way to avoid this issue.

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

> Note:\*<br>
> **RUN:** This is executed during image build.
> <br>**CMD:** This represents the default command that will be executed when the container starts.

### Build the image

`docker build -t image_name .`

#### Tag name (-t):

It is used to add name to the image
like image_name:tag. If we don't specify the tag then it takes `latest` be default

#### Build Context (.):

It means the current directory which docker uses as the **build context.**
When building, Docker needs access to files like:

- package.json
- source code
- images
- configs

The build context is the folder Docker can access during build.

Internally Docker:

- Sends current directory to Docker daemon
- Reads the Dockerfile
- Executes instructions one by one
- Creates image layers
- Produces final image

But if you wnat to push the image to dockerhub then the imgae name should be in this format: `dockerhub_username/image_name`

example: `docker build -t ritik/sample_app .`

---

### Pass Environment variables to the the container

- manually passing:

  Flag: `-e`

  `docker run -e port=4000 backend`

- using .env file:

  When you have a lot of environment variables then its not possible to pass them via command line. Instead we can have `.env` file and then use the following command to pass it to the container.
  <br>`docker run --env-file .env backend`

**Debug env:**
How to check if your envs are crrectly set in the container.

- `docker exec -it container_name printenv`
  <br>This executes `printenv` command inside the container and all the envs are displayed.
  To display a particular env use `printenv env_name`

> _Note:_ We can use both `-e` and `--env-file` together. And `-e` will overrde the variables if they are also present in the env file

---

## Volumes and Networks

### Volumes

- Used to persists data across starts
- specifically usefull for things like database

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

- To display available networks

  `docker network ls`

- First, create a network

  `docker network create my_network`

- Attach network to a container (express server)

  `docker run -p 3000:3000 --network network_name image_name`

- Attach mongo container also to the same network, we will also give the mongo container a name so that we can use it to communicate with this container from express server running on the same network

  `docker run -p 27017:27017 --name mongo_container --network network_name mongo`

- Now, in express server we will use the following url to connect to the mongodb server running in the container

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

A multi-stage build in Docker means using multiple FROM statements in one Dockerfile to make the final image:

- smaller
- cleaner
- more secure

#### The Problem Without Multi-Stage Builds

Suppose you have a Node.js app.

To build it, you may need:

- npm
- TypeScript
- dev dependencies
- build tools

But in production, you only need:

- compiled app
- production dependencies

Without multi-stage builds, all build tools remain inside the final image → image becomes large.

Example: **Normal Build (Single Stage)**

```
FROM node:22

WORKDIR /app

COPY . .

RUN npm install
RUN npm run build

CMD ["node", "dist/index.js"]
```

Problem:

- contains dev dependencies
- build cache
- TypeScript
- unnecessary files

**Multi Stage Version**

```
# Stage 1 - Build Stage
FROM node:22 AS builder

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build


# Stage 2 - Production Stage
FROM node:22-alpine

WORKDIR /app

COPY package*.json ./

RUN npm install --omit=dev

COPY --from=builder /app/dist ./dist

CMD ["node", "dist/index.js"]
```

When we do: `Docker build` then, Docker builds all stages and the last stage becomes the final image automatically.

- Only the final stage becomes the produced image.
- Intermediate stages are temporary build stages.

**Another Example use case:**

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

- `FROM node:20 AS base: `

  This line specifies the base image for the Docker container. In this case, it's a Node.js image version 20. It sets up the base environment for the subsequent instructions.

- `WORKDIR /user/src/app: `

  This sets the working directory inside the Docker container to /user/src/app. This is where the application code and files will be copied and where subsequent commands will be executed.

- `*COPY package.json .**:`

  This instruction copies the package.json and package-lock.json (if it exists) from the local file system into the /user/src/app directory in the Docker container. These files are needed for npm to install dependencies.

- `RUN npm install: `

  This command installs the dependencies listed in the package.json file into the Docker container. It's executed within the /user/src/app directory.

- `FROM base AS development:`

  This line creates a new stage in the Dockerfile based on the base stage. This is called a multi-stage build. It allows you to separate build-time dependencies from runtime dependencies, reducing the size of the final Docker image.

- `COPY . .: `

  This instruction copies all files from the current directory (presumably the application code) into the Docker container's /user/src/app directory.

- `CMD ["nodemon", "index.js"]: `

  This sets the default command to run when the container starts in development mode. It uses nodemon to watch for changes in the files and automatically restart the Node.js application when changes are detected.

- `FROM base AS production: `

  This creates another stage in the Dockerfile based on the base stage.

- `COPY . .: `

  Similar to the development stage, this instruction copies all files from the current directory into the Docker container's /user/src/app directory.

- `RUN npm prune --development:`

  This command removes development dependencies from the node_modules directory. It's done to reduce the size of the production image since development dependencies are not needed in the production environment.

- `CMD ["node", "index.js"]:`

  This sets the default command to run when the container starts in production mode. It runs the Node.js application directly without any watching mechanism like nodemon, which is suitable for production environments.

Now, to create an image from a aprticular stage, we use `--target stage_name` with the build command.

To create an image for development:

`docker build . --target development -t dev_image`

To create an image for production:

`docker build . --target production -t prod_image`

---

#### Assignment

Curently, If you start a container in development mode which uses nodemon, then when you edit the index.js file inside the container, then nodemon will restart the server. But if you edit it outside, basically the file which is present locally, it won't restart the nodemon.
So, what should you do, so that even while editing the file locally should make nodemon to re-run the server ?

#### Solution:

We will use volume here. As we know that volumes allows us to mount a folder inside container, such that they are linked, meaning anychage to the containers file will be affected locally and any change locally will affect the container too.

So, we will mount the local project folder to the containers project folder. Doing so, will make them connected and any change made in file locally will also reflect inside the containers file system as well.

**command:**

`docker run -p 3000:3000 -v .:/user/src/app image_name`

it mounts `. (current directory locally)` to `"/usr/src/app" (directory in container)`

---

### Docker Compose

`docker-compse.yml`

A project has a lot of auxillary dependency it needs to use like mongodb/kafka/redis ... , this requires us to run multiple docker commands. So, we have to start multiple terminals and execute different commands, but this approach is not efficient and prone to errors.

Instead, we can use `Docker Compose` to manage and run all the required services with a single command, simplifying the process and ensuring that all dependencies are correctly configured and started together.

Compose is a tool used for defining and **running muti-container** Docker Applications. With compose, you use a **YAML file** to **configure your application's services**. Then, with a single command, you create and start all the services from your configuration.

docker-compose.yaml

    services:
        mongodb_db:
            image: mongo:latest
            ports:
                - 27017:27017

        my-app:
            build: ./
            ports:
                - 3000:3000

**Command to run this:**

`docker-compose up`

When we run docker-compose, it `automatically creates a network`, and therefore all the services are connected to each other, means they can communicate with each other. And the containers can be referenced by their name, here `mongodb_db`, `my-app`

So, if I want my-app to cnnect with the mongodb, the url will be

`mongoose.connect(mongodb://mongodb_db:27017/my-db)`

How to create Volumes ?

Volumes can be added as a key to the YAML file and can be referenced by all containers.

e.g,

docker-compose.yaml

    services:
        mongodb_db:
            image: mongo:latest
            ports:
                - 27017:27017
            volumes:
                - mongo_volume:/data/db

        my-app:
            build: ./
            image: rtk_image
            ports:
                - 3000:3000
    volumes:
        mongo_volume:

#### Commands:

`docker compose up`

This command pulls the image or build the image if not already present and then start the container.

>***Note:*** it recreates the container only when there is a chnage in the doker-compose file else it will just use the existing container. Let's say when the container was running, you did ctrl+c, this just stops the container and not remove it. Therefore, when you do `docker compose up` after a code chnage that won't be reflected. For that you have to run `docker compose up --build`

`docker compose up -d`: to run in detached mode

`docker compose up --build`: Builds the images and then start the container

`docker compose down`: removes all the containers

`docker compose restart`: restarts the container

`docker compose ps`: displays the running compose containers

`docker compose ps -a`: displays all the compose containers

`docker compose logs`: check the logs at the moment this command runs when containers are running in detached mode

`docker compose logs -f`: checkt the live logs

`docker compose logs service_name`: check the logs of a particular service (name defined in the docker compose file)


**Adding environment variables**:

There are two ways to add env in compose:

1. environment

   ```
    services:
        mongodb_db:
            image: mongo:latest
            ports:
                - 27017:27017
            environment:
                - JWT_SECRET: sample
                - COOKIE_SECRET: sample
   ```

   Now, this is not the right way of adding env as its hardcoded in the compose file so anyone can see it. Therefore, we can use env variables. For this create an env file and then we can access it inside the compose file like below:

   ```
   services:
     mongodb_db:
         image: mongo:latest
         ports:
             - 27017:27017
         environment:
             - JWT_SECRET: ${JWT_SECRET}
             - COOKIE_SECRET: ${COOKIE_SECRET}
   ```

2. env_file

   ```
   services:
       mongodb_db:
           image: mongo:latest
           ports:
               - ${HOST_PORT}:${CONTAINER_PORT}
           env_file:
           - ./.env
   ```
