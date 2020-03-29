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
sudo chown -R 1000 /srv/jenkins
docker-compose up
```
When you run docker-compose up, the following happens:

1. A [network](https://docs.docker.com/compose/networking/) called docker-gitlab-jenkins_default is created.
2. A container is created using gitlab-ci’s configuration. It joins the network docker-gitlab-jenkins_default under the name gitlab-ci.
3. A container is created using jenkins-ci’s configuration. It joins the network docker-gitlab-jenkins_default under the name jenkins-ci.


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

In order to lists containers and networks
```
docker container ls  #Equivalent to `docker ps` lists all running containers in Docker Engine.
docker-compose ps    #list containers related to images declared in docker-compose file.
docker network ls    #Lists all the networks the Engine daemon knows about.
```

## Configuration (Jenkins, Gitlab CE)

### Access both Jenkins and Gitlab CE:

Type the following URLs in the web browser or your host:

* Jenkins: http://localhost:8080/
* Gitlab CE: http://localhost:8081/

### Configure GitLab

#### Configure GitLab users

Create a user or choose an existing user that Jenkins will use to interact through the GitLab API. This user will need to be a global Admin.

Go to http://localhost:8081/profile/personal_access_tokens and create a private API token and copy it somewhere. You will need this when configuring the Jenkins server later.

#### Get Gitlab server URL

In order to find the Repository URL needed for Jenkins, you must find the IPV4 of your  gitlab-ci container, for this, please run the following

```
docker network ls
docker network inspect docker-gitlab-jenkins_default
```
and then you should have something like:


```
       "Containers": {
            "0edd1fed075a54cdc52f40e484597d98735a2d370299465523a7bd3c4e7e5b1f": {
                "Name": "gitlab-ci",
                "EndpointID":  "e262af52786750ffa951a223a2126533b02ea3b892968a3e0781b300ce2156bd",
                "MacAddress": "02:42:ac:14:00:02",
                "IPv4Address": "172.20.0.2/16",
                "IPv6Address": ""
            },
            "1d725f99f31daae46ec9dadc23befdf4a8a9a09f254c10c906f40c43d2f68556": {
                "Name": "jenkins-ci",
                "EndpointID": "70489de4fcec5605beff01c6aeab955d0c837b235a281d20dd1fcc5049e2c1b1",
                "MacAddress": "02:42:ac:14:00:03",
                "IPv4Address": "172.20.0.3/16",
                "IPv6Address": ""
            }
        },

```
Once you created your project under your Gitlab container, the Repository URL should now be

```
IPV4 Address + /USER + /REPOSITORY_NAME
```
So in our example it is:
```
http://172.20.0.2/root/simple-java-maven-app
```

### Configure the Jenkins server 

#### Jenkins initial Admin Password 

You can find the Jenkins initial admin password in the following path of your host:
```
/srv/jenkins/home/secrets/initialAdminPassword

```

#### Jenkins Plugin Installation

In Jenkins, once you installed the recommanded Plugins and created your user, go to 
```
http://localhost:8080/pluginManager/available
```

and install the following plugins:
* Blue Ocean
* [Jenkins GitLab Plugin](https://plugins.jenkins.io/gitlab-plugin/)
* [Jenkins Git Plugin](https://plugins.jenkins.io/git/) 

When done, restart Jenkins.

#### Jenkins System Configuration

Go to Manage Jenkins -> Configure System or use the following url:
```
http://localhost:8080/configure
```
and scroll down to the ‘GitLab’ section. Enter the GitLab server URL in the ‘GitLab host URL’ field and click the 'Add' button to add a credential, choose 'GitLab API token' as the kind of credential, and paste your GitLab user's API key into the 'API token' field

*note: if you don't know the Gitlab server URL, please refer to the "Get Gitlab server URL" section above.*

This section should now look like the following:



For more information, see GitLab Plugin documentation about [Jenkins-to-GitLab authentication](https://github.com/jenkinsci/gitlab-plugin#jenkins-to-gitlab-authentication)


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
