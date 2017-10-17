## Apache Docker

[![Run Status](https://api.shippable.com/projects/54cf015b5ab6cc13528a7b6a/badge?branch=envoyer)](https://app.shippable.com/projects/54cf015b5ab6cc13528a7b6a)
[![Circle CI](https://circleci.com/gh/htmlgraphic/Apache/tree/envoyer.svg?style=svg)](https://circleci.com/gh/htmlgraphic/Apache/tree/envoyer) 
[![](https://images.microbadger.com/badges/image/htmlgraphic/apache:envoyer.svg)](https://microbadger.com/images/htmlgraphic/apache:envoyer "Get your own image badge on microbadger.com")
[![Beerpay](https://beerpay.io/htmlgraphic/Apache/badge.svg?style=beer)](https://beerpay.io/htmlgraphic/Apache) [![Beerpay](https://beerpay.io/htmlgraphic/Apache/make-wish.svg?style=flat)](https://beerpay.io/htmlgraphic/Apache)


This repo will give you a turn key Docker container build for use in production OR local development. The setup includes an Apache web service, PHP 7.0 OR PHP 5.5.9, PHP Composer, linked MySQL instance and a persistent data volume.


If you found this repo you are probably looking into Docker or already have knowledge as to what Docker can help you with. In this repo you will find a number of complete Dockerfile builds used in **development** and **production** environments. Listed below is an explanation of each file. [Ask a question](https://github.com/htmlgraphic/Apache/issues/new)

#### Dependencies
- Docker
- Docker Compose
- Git
- Make ([Windows](https://stackoverflow.com/questions/32127524/how-to-install-and-use-make-in-windows-8-1))

---

## Build Breakdown

```shell
Apache                       # → Root of Docker Build
├── app/                     # → App conf to manage application on container
│   ├── apache-config.conf   # → Default Apache configuration
│   ├── index.php            # → Default web page, enter the IP address `docker-machine ls` to load this page.
│   ├── mac-permissions.sh   # → Run manually on container to match uid / gid permissions of local docker container to Mac OS X
│   ├── postfix.sh           # → Used by *supervisord.conf* to start Postfix
│   ├── run.sh               # → Setup apache, conf files, and start process on container
│   ├── sample.conf          # → located within `/data/apache2/sites-enabled` duplicate / modify to host others domains
│   └── supervisord          # → Supervisor is a client / server system which monitors and controls a number of processes on UNIX-like operating systems
├── .env.example             # → Rename file to `.env` for local environment variables used within build
├── .circleci/               # → CircleCI 2.0
│   └── config.yml           # → CircleCI Configuration
├── docker-cloud.yml         # → Used to lauch a container directly to Docker Cloud
├── docker-compose.local.yml # → Local build 
├── docker-compose.yml       # → Production build
├── Dockerfile               # → Uses a basefile build to help speed up the docker container build process
├── Makefile                 # → Build command shortcuts
├── shippable.yml            # → Configuration for Shippable.com testing
└── tests/
    └── build_tests.sh       # → Build test processes
```
Docker Compose YML configuration guide [more info](https://docs.docker.com/docker-cloud/apps/deploy-to-cloud-btn/) 

## Quick Start

Launch the **Apache** instance locally and setup a local MySQL database container for persistant database data, the goal is to create a easy to use development environment. Type `make` for more build options

```bash
	$ git clone https://github.com/htmlgraphic/Apache.git && cd Apache
	$ make run 
	OR (non Make Windows)
	$ docker-compose -f docker-compose.local.yml up -d
```

---
## Deploy to Docker Cloud
Create a [Docker Cloud](https://cloud.docker.com) account, add a   **Cloud provider**. Once complete, `one click` will create a functional Apache & MySQL Service.

[![Deploy to Docker Cloud](https://files.cloud.docker.com/images/deploy-to-dockercloud.svg)](https://cloud.docker.com/stack/deploy/)

---

## Test Driven Development
These continuous integration services will fully test the creation of your container and can push the complete image to your private Docker repo if you desire.


**[CircleCI 2.0](https://circleci.com/gh/htmlgraphic/Apache)** - Test **production** and **dev** Docker builds, can the container be built the without error? Verify each build process using docker-compose. Code can be tested using ```lxc-attach / docker inspect``` inside the running container


---

**[Shippable](https://shippable.com)** - Test **production** and **dev** Docker builds, can the container be built the without error? The ```/tests/build_tests.sh``` file ensures the can run with parameters defined. Shippable allows the use of [matrix environment variables](http://docs.shippable.com/ci_configure/#using-environment-variables) reducing build time and offer a more robust tests. If any test(s) fail the system should be reviewed closer.


## Interacting with containers:

  View all running containers:

  `docker ps`

  View all containers (including stopped containers):

  `docker ps -a`

  Read the log of a running container:

  `docker logs [container name or id]`

  Follow the log of a running container:

  `docker logs -f [container name or id]`

  Read the Apache log:

  `docker exec apache_web_1 cat ./data/apache2/logs/access_log`

  Follow the Apache log:

  `docker exec apache_web_1 tail -f ./data/apache2/logs/access_log`

  Follow the outgoing mail log:

  `docker exec apache_web_1 tail -f ./var/log/mail.log`

  Gain terminal access to a running container:

  `docker exec -it [container name or id] sh`

  Restart a running container:

  `docker restart [container name or id]`

  Stop and start a container in separate operations:

  `docker stop [container name or id]`

  `docker start [container name or id]`

## Teardown 
#### (Stop all running containers started by Docker Compose):

```
    $ make rm 
    OR (non Make Windows)
    $ docker rm -f apache_web_1 && docker rm -f apache_db_1
```
