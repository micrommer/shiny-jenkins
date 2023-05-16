# shiny-jenkins

## DOCKER-FILES
> The folder contains dockerfiles to create all nececarry docker image to work with jenkins.
- Dockerfile-jenkins-with-docker-access: let you create a jenkins image with docker-cli included. The image let you create a 
container that could execute docker-pipeline on your local machine, some extra step required that done in the docker-compose file.
- Dockerfile-tb-builder: Image includes all required tools to build thingsboard project. Maven, Nodejs, Yarn and some Mavens's cached dependencies.

## JENKINS-FILES
> A place to keep track Jenkinsfile history.