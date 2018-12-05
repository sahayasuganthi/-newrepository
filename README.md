# Careplanning

Careplanning SOA service

## Requirements

* [Docker](https://www.docker.com)
* [NodeJS](https://www.nodejs.org) >= v6
* [Yarn](https://yarnpkg.com/lang/en/)

## Set-up

### Docker

We use a private docker registry with [Artifactory](https://advancedcsg.jfrog.io/advancedcsg/). You will need to configure this before development to ensure you have all the dependencies. Setup instructions for docker are explained on the [Artifactory](https://advancedcsg.jfrog.io/advancedcsg/) website. You will need to login to docker using your Artifactory username and API key as the password:

    $ docker login advancedcsg-docker-release-virtual.jfrog.io
    $ docker login advancedcsg-docker-snapshot-virtual.jfrog.io

You also need to put docker in swarm mode. This is needed for docker networking and only needs to be run once. Simply run:

    $ docker swarm init

### Curl

This project uses curl tool to transfer data from or to a server.

First install curl.exe

You'll want to make curl.exe available anywhere from the command line. To do this, pick ahc-careplanning folder on local repo and add it to the system path as outlined below:

Click the Windows start menu, start typing 'environment'

You'll see the menu item Edit the System Environment Variables, choose it

A System Properties window will pop up. Click the Environment Variables button

Select the path variable, click the Edit button

Click the Add button, paste in the folder path where curl.exe lives

Click OK as needed. Close open command prompt windows and reopen, so they get the new path location

### NPM

This project uses npm for package management. We use a private npm registry with [Artifactory](https://advancedcsg.jfrog.io/advancedcsg/). You will need to configure this before development to ensure you have all the dependencies. Setup instructions for npm are explained on the [Artifactory](https://advancedcsg.jfrog.io/advancedcsg/) website. You will need to add `client/.npmrc` and `server/.npmrc` files containing your Artifactory authentication tokens:

```bash
$ curl -u"YOUR-ARTIFACTORY-USERNAME:YOUR-ARTIFACTORY-AUTH-TOKEN" "https://advancedcsg.jfrog.io/advancedcsg/api/npm/auth" >> client\.npmrc
$ curl -u"YOUR-ARTIFACTORY-USERNAME:YOUR-ARTIFACTORY-AUTH-TOKEN" "https://advancedcsg.jfrog.io/advancedcsg/api/npm/npm-snapshot-virtual/auth/advanced" >> client\.npmrc

$ curl -u"YOUR-ARTIFACTORY-USERNAME:YOUR-ARTIFACTORY-AUTH-TOKEN" "https://advancedcsg.jfrog.io/advancedcsg/api/npm/auth" >> server\.npmrc
$ curl -u"YOUR-ARTIFACTORY-USERNAME:YOUR-ARTIFACTORY-AUTH-TOKEN" "https://advancedcsg.jfrog.io/advancedcsg/api/npm/npm-snapshot-virtual/auth/advanced" >> server\.npmrc
```
Edit the `client/.npmrc` and `server/.npmrc` files with visual studio and save as encoding utf8.


## Development Typescript dependencies

To install all required Typescript dependencies for local server development run:

	$ cd server
    $ yarn

To install all required Typescript dependencies for local client development run:

	$ cd client
    $ yarn

## Importing the PowerShell tasks module

A PowerShell module with common tasks is provided to assist with local development.

In PowerShell, from this diectory, run `Import-Module ./Tasks.psm1`.

To see the entire list of available commands, type `Get-Command -Module Tasks`.

## Running in development mode

To run the project in develop mode run:

	$ docker-compose up

To end docker instances

	$ docker-compose down

## Running in QA mode

With QA mode you are able to run production server built by the CI pipeline. You run the production image on your machine with seeded data. You need to know build version (= docker image tag) assigned during CI process. You can find this version in title of the build on Jenkins or in Artifactory. There are 3 Powershell commands to manage QA instances:

Starting QA instances:

	$ Docker-Qa-Up TheBuildTagGoesHere

Example: `Docker-Qa-Up 0.1.1-dev-aa1b5d-102`

The application will run on port 8889 but, as this is a production build, it won't have user session established. All requests will fail until you fake the *host* (being Roster or CareSys)
and its ability to authenticate users and provide session security token. You inject the fake (seeded) session id with this JavaScript command in JavaScript console:

	window.roster_javascript_post = () => "aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa";

or by openning *Roster Host* dev tool extension tab, whichever you find more convenient (see https://github.com/advancedcsg/Ahc-care-ChromeDevTool for more information on *Roster Host* dev tool extension).

Stopping QA instances (this will preserve data in the database):

	$ Docker-Qa-Down

If you want to also purge QA database:

	$ Docker-Qa-Down -RemoveData

Note: you may run `Docker-Qa-Down -RemoveData` at any time, not only when services are up. It will always purge MongoDB data.

Removing QA images downloaded by `Docker-Qa-Up`:

	$ Docker-Qa-Remove TheBuildTagGoesHere

Example: `Docker-Qa-Remove 0.1.1-dev-aa1b5d-102`

## Seed data

Data is automatically seeded when care planning is run in a development environment
or when the containers are started using `Docker-Qa-Up` powershell command.

### In development environment

Data is only seeded once. To clear the mongo database in order for the data to be reseeded, run `docker-compose down -v`. This will remove all volumes for the care planning containers.

### In QA environment

Data is only seeded once. Subsequent calls to `Docker-Qa-Up` and `Docker-Qa-Down` will keep the data. `Docker-Qa-Remove` will keep the data too. To clear the mongo database in order for the data to be reseeded, run `Docker-Qa-Down -RemoveData`. This will remove all volumes for the care planning containers.

## E2E tests

[See running E2E tests](e2e/README.md)
