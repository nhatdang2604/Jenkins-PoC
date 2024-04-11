# How to setup Jenkins via Docker

## Prerequisite 

### Create network
`docker network create jenkins-poc`

### Pull and create docker:dind
`docker image pull docker:dind`

```
docker run \
    --name jenkins-poc-docker \
    --rm \
    --detach \
    --privileged \
    --network jenkins-poc \
    --network-alias docker \
    --env DOCKER_TLS_CERTDIR=/certs \
    --volume jenkins-docker-certs:/certs/client \
    --volume jenkins-data:/var/jenkins_home \
    --publish 2376:2376 \
    docker:dind \
    --storage-driver overlay2
```

### Create Docker file

Follow /Dockerfile

### Build the image

`docker build -t jenkins-poc-blueocean:2.440.2-1 .`

### Run the image

```
docker run \
    --name jenkins-poc-blueocean \
    --restart=on-failure \
    --detach \
    --network jenkins-poc \
    --env DOCKER_HOST=tcp://docker:2376 \
    --env DOCKER_CERT_PATH=/certs/client \
    --env DOCKER_TLS_VERIFY=1 \
    --publish 8080:8080 --publish 50000:50000 \
    --volume jenkins-data:/var/jenkins_home \
    --volume jenkins-docker-certs:/certs/client:ro \
    jenkins-poc-blueocean:2.440.2-1
```
