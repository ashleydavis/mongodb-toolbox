# Mongodb Toolbox

Quickly boot a Mongodb container so that you have tools for working with Mongodb.

Includes a shared directory so you can easily copy files (e.g. a database dump) between the container and the host OS.

If you like this project, please star this repo and [support my work](https://www.codecapers.com.au/about#support-my-work)

## Pre requisite

[Have Docker installed](https://www.docker.com/products/docker-desktop).

## Bring the container up

Use Docker-Compose to boot the container:

```bash
cd mongodb-toolbox
docker compose up
```

## Port mapping

The standard Mongodb port (27017) is mapped to your host OS. So you can connect to the database using tools on your dev PC (e.g. Robo 3T).

If you **already have** MongoDB installed on your host OS then the port will conflict. Please update `docker-compose.yaml` to choose a new port on your host OS.

## Directory mapping

The current directory on the host OS is mapped through to the `/shared` directory in the container. Anything you copy to this directory is shared between the host OS and the container automatically.

## Connect to the container

You can also connect to the container to run tools like `mongodump` and `mongorestore`.

Open a new terminal and open a shell into the MongoDB container:

```bash
docker exec -it mongo-toolbox bash
```

## Do a backup

Once connected to the container you can run Mongodb tools, for example use `mongodump` to do a database backup:

```bash
mongodump --uri mongodb://<src-host>:27017
```
## Do a restore

Later you can use `mongorestore` to restore your database:

```bash
mongorestore --uri mongodb://<dest-host>:27017 dump
```

**NOTE TO SELF** If encoding a password in the Mongodb connection string on the command line you need to escape exclamation marks (!) with a backslash (e.g. \!).

## Accessing the host from the container

Inside the container you can use `host.docker.internal` to access the host OS.

For example if you have a MongoDB database mapped to port 27020 on the host OS you can dump it like this:

```bash
mongodump --uri=mongodb://host.docker.internal:27020
```

See [Windows](https://docs.docker.com/docker-for-windows/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host) and [Mac](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host) networking docs for details. 

## Install other tools in the container

You can install other Linux tools in the container, as example let's install `curl`:

```bash
apt-get update
apt-get install curl
```



