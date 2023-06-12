# shiny-jenkins

## DOCKER-FILES
> The folder contains dockerfiles to create all nececarry docker image to work with jenkins.
- Dockerfile-jenkins-with-docker-access: let you create a jenkins image with docker-cli included. The image let you create a 
container that could execute docker-pipeline on your local machine, some extra step required that done in the docker-compose file.
- Dockerfile-tb-builder: Image includes all required tools to build thingsboard project. Maven, Nodejs, Yarn and some Mavens's cached dependencies. The existing tools may not really required but let them exist for a concealed porpuse.
- Dockerfile-dockerizer: Image contains docker CLI, it will connect to local docker. Build TB image and push it to gitlab is the main responsibility here. (Host docker.sock must mount to the image to be operational)
## JENKINS-FILES
> A place to keep track Jenkinsfile history.
- It's required to setup a secret credential text in jenkin's admin panel to connect to the git repo.
- Never run the mvn build in paralel, it cause exception for Gradle dowloading.

## DOCKER-COMPOSE
- `Jenkins` service need a docker.sock to comunicate with local docker, so the sock valoume should be mounted.
- For docker container registry we need to launch new service that called `registry` service, it gonna be the gitlab containers registry alternative for a while.
- We need to create a specific network to share it with `tb-builder`, so `shiny-jenkins` network exists in compose