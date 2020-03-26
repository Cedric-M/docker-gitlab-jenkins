# Docker Continuous Integration

Using Docker Engine, Docker Compose, Gitlab CE, Jenkins

## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

You need to install [Docker Engine](https://docs.docker.com/engine/release-notes/) and [Docker Compose](https://docs.docker.com/compose/)
If you already have them, check the version using the following command:

```
docker --version
# should return:
# Docker version 19.03.6

docker-compose --version
# should return:
# docker-compose version 1.25.4

```
### Installing 

A step by step series of examples to get a development env running

#### Ubuntu

Please follow the following Docker documentation to set up Docker for Ubuntu

* [Get Docker Engine (Ubuntu)](https://docs.docker.com/install/linux/docker-ce/ubuntu/)
* [Get Docker Compose (Ubuntu)](https://docs.docker.com/compose/install/#install-compose-on-linux-systems)

Once again, check if both of them are now successfully installed by running:

```
docker --version
docker-compose --version
```

#### CentOS

Please follow the following Docker documentation to set up Docker for CentOS

* [Get Docker Engine (CentOS)](https://docs.docker.com/install/linux/docker-ce/centos/)
* [Get Docker Compose (CentOS)](https://docs.docker.com/compose/install/#install-compose-on-linux-systems)

Once again, check if both of them are now successfully installed by running:

```
docker --version
docker-compose --version
```

## Deployment

Once you cloned the git repository, all you have to do is to use the following commands:



```


docker-compose start      # Starts existing containers for a service.
docker-compose stop       # Stops running containers without removing them.

docker-compose pause      # Pauses running containers of a service.
docker-compose unpause    # Unpauses paused containers of a service.

docker-compose ps         # Lists containers.
docker-compose up         # Builds, (re)creates, starts, and attaches to containers for a service.
docker-compose down       # Stops containers and removes containers, networks, volumes, and images created by up.


```

### Launching procedure

So in our case, in order to deploy it, we'll do:


```
sudo chown $USER /var/run/docker.sock
docker-compose up
```

### Shutdown procedure

In order to stops running containers
```
docker-compose stop
```


In order to stops containers and removes containers, networks, volumes, and images created by up.
```
docker-compose down
```
### Supervision procedure

In order to lists containers.
```
docker-compose ps
```

## Built With

* [Docker Engine](https://docs.docker.com/engine) - Open source containerization technology
* [Docker Compose](https://docs.docker.com/compose/) - Tool for defining and running multi-container Docker applications
* [Gitlab CE](https://docs.gitlab.com/ee/install/docker.html)
* [Jenkins](https://hub.docker.com/r/jenkins/jenkins/)



## Versioning

We use [GitHub](https://github.com/Cedric-M/docker-gitlab-jenkins) for versioning.

## Authors

* **Cedric-M** - *DevOps Engineer*


## Acknowledgments

* Docker Engine
* Docker Compose
