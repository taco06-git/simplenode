# Sample Node.js Project

A Node.js project written using Express. EJS was used as the view engine.

# Installation

You need to write the following commands on the terminal screen so that you can run the project locally.

```sh
1. git clone git@github.com:acemilyalcin/sample-node-project.git
2. cd sample-node-project
3. npm install
4. npm start
```

The application is running on [localhost](http://localhost:3000).

#------------------------------------------------------------------------------------------------------------------------------------------------

# Create a jenkins CI BUILD on docker container 
# Objective Build a docker image on build container and push to docker hub , Jenkins file is added in the repo

# First we need to build container image with docker cli 
# Go to Docker node which is a slave to jenkins and login to docker hub 
$ docker login -u <username> # give password after you hit enter 

# Dockerfile with node and cli , this image is for jenkin build container , inside which we wil run our CI JOB
$ vi Dockerfile
FROM node:18
RUN apt-get update && \
apt-get install -y docker.io && \
rm -rf /var/lib/apt/lists/*
RUN docker --version

# Build and push this image (just once)
$ docker build -t yourdockerhubuser/node-docker .
$ docker push yourdockerhubuser/node-docker

# Then update your Jenkinsfile to use it:

agent {
    docker {
        image 'yourdockerhubuser/nodejs-with-docker-cli' # node js with docker client 
        args '-v /var/run/docker.sock:/var/run/docker.sock'
    }
}
# NOTE: You need Docker Daemon access inside the container to build and push images.
# We can solve that by mounting the host Docker socket into the container. That way, even though the Jenkins steps run inside a node:18 container, 
# they can talk to the host Docker engine.
# Security Note : Mounting /var/run/docker.sock gives full access to Docker — only use this on trusted nodes.


# Add Docker Hub Credentials to Jenkins
Go to Jenkins > Manage Jenkins > Credentials
Add a new credential:
Kind: Username and Password
Scope: Global
ID: docker-hub-creds (or any ID you prefer)
Username: your Docker Hub username
Password: your Docker Hub password or token







