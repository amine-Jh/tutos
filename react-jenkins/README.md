# Goal 
Those are the steps to create a jenkins pipeline for a react application 
for this we run jenkins as a docker container

# 1 - Run Jenkins in Docker

first we create a #bridge network 
`docker network create jenkins`

In order to execute Docker commands inside Jenkins nodes, download and run the docker:dind Docker image using the following docker run command:

`
docker run \
  --name jenkins-docker \
  --rm \
  --detach \
  --privileged \
  --network jenkins \
  --network-alias docker \
  --env DOCKER_TLS_CERTDIR=/certs \
  --volume jenkins-docker-certs:/certs/client \
  --volume jenkins-data:/var/jenkins_home \
  --publish 2376:2376 \
  --publish 3000:3000 --publish 5000:5000 \
  docker:dind \
  --storage-driver overlay2 
`

Create dockerFile for JenkinsBlue Ocean tool

`
FROM jenkins/jenkins:2.375.1-jdk11
USER root
RUN apt-get update && apt-get install -y lsb-release
RUN curl -fsSLo /usr/share/keyrings/docker-archive-keyring.asc \
  https://download.docker.com/linux/debian/gpg
RUN echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/usr/share/keyrings/docker-archive-keyring.asc] \
  https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" > /etc/apt/sources.list.d/docker.list
RUN apt-get update && apt-get install -y docker-ce-cli
USER jenkins
RUN jenkins-plugin-cli --plugins "blueocean:1.25.8 docker-workflow:521.v1a_a_dd2073b_2e"
`

Build the image of jenkinsBlueOcean run the command

## docker build -t myjenkins-blueocean:2.3751-1 .

and then we run the image above as container :

‘
docker run \
  --name jenkins-blueocean \
  --detach \
  --network jenkins \
  --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client \
  --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 \
  --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  --volume "$HOME":/home \
  --restart=on-failure \
  --env JAVA_OPTS="-Dhudson.plugins.git.GitSCM.ALLOW_LOCAL_CHECKOUT=true" \
  myjenkins-blueocean:2.375.1-1
‘


# 2 - Run Jenkins Wizard

browse http://localhost:8080 

run the command below to show the generated password

‘docker logs jenkins-blueocean‘

install suggested plugins

# 3 - Create your Pipeline project in Jenkins
go to jenkins and create a new job by clicking on New Item