# shiny-jenkins

## DOCKER-FILES
> The folder contains dockerfiles to create all necessary docker image to work with jenkins.
- Dockerfile-jenkins-with-docker-access: let you create a jenkins image with docker-cli included. The image let you create a 
container that could execute docker-pipeline on your local machine, some extra step required that done in the docker-compose file.
- Dockerfile-tb-builder: Image includes all required tools to build thingsboard project. Maven, Nodejs, Yarn and some Mavens's cached dependencies. The existing tools may not really required but let them exist for a concealed porpuse.
- ~~Dockerfile-dockerizer~~: Image contains docker CLI, it will connect to local docker. Build TB image and push it to gitlab is the main responsibility here. (Host docker.sock must mount to the image to be operational)
- ~~Dockerfile-tb-included~~: Image contains pre-built thingsboard project source code.
- ~~Dockerfile-deployer~~: Image contains essential tools to deploy thingsboard.
## JENKINS-FILES
> A place to keep track Jenkinsfile history.
### Jenkinsfile-tb-builder
- It's required to setup a secret credential text in Jenkin's admin panel to connect to the git repo.
- Never run the mvn build in parallel, it causes exception for Gradle downloading.
- It needs to connect to host's docker daemon.
- It works with `build-on-jenkins` branch that commented `tb-cassandra` image generation.
- To customize the pipeline, you need to change variable inside the Jenkinsfile, there is nothing controlled by environment manager right now.
- Docker plugin is required to run the pipeline.

### Jenkinsfile-tb-deployer
- There is a bunch on environment variable that are customizable
  - remoteName * 
  - remoteHost *
  - remoteUser *
  - remotePassword *
  - remoteAllowAnyHosts *
  - postgresqlVersion **
  - dbName **
  - containerName **
  - dockerComposePath **
  - dockerRegistryHost **
  - dockerRegistryPort **

  ###### * are the variables that have not a default value, and you should define them in file or environment.
  ###### ** are the variables that have a default value, and you are allowed to not define them.
 - `Pipeline Utility Steps` and `Environment Inject` are essential for this pipeline.
 - The target host machine should have a dedicated postgresql and docker service.
 - The target host machine should set up using docker compose v2.

## JENKINS-ENVS
> A folder contain different environment for host machines
- It should mount to Jenkins container (It mentioned in docker-compose.yaml)
- The mounted dir inside the container is very important, the dir used inside the pipeline pre-start environment.

## DOCKER-COMPOSE
- `Jenkins` service need a docker.sock to communicate with local docker, so the sock volume should be mounted.
- For docker container registry we need to launch new service that called `registry` service, it's going to be the gitlab containers registry alternative for a while.
- We need to create a specific network to share it with `tb-builder`, so `shiny-jenkins` network exists in compose.
- `jenkins-env` should mount to the Jenkins container.