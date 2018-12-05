# Clinincal-Content-Discovery

Clinincal Content-Discovery

## Requirements

* [Docker](https://www.docker.com)
* [NodeJS](https://www.nodejs.org) latest(LTS)
* [Yarn](https://yarnpkg.com/lang/en/)

## Set-up

### Docker

We use a private docker registry with [Artifactory](https://advancedcsg.jfrog.io/advancedcsg/). You will need to configure this before development to ensure you have all the dependencies. Setup instructions for docker are explained on the [Artifactory](https://advancedcsg.jfrog.io/advancedcsg/) website. You will need to login to docker using your Artifactory username and API key as the password:

    $ docker login advancedcsg-docker-release-virtual.jfrog.io
    $ docker login advancedcsg-docker-snapshot-virtual.jfrog.io

You also need to put docker in swarm mode. This is needed for docker networking and only needs to be run once. Simply run:

    $ docker swarm init

