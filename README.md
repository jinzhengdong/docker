# Docker

## Ubuntu Commands

### Managing Packages

* apt update
* apt list
* apt install nano
* apt remove nano

### Navigating the file System

* pwd, to show the working directory
* ls, to show the files and directories
* ls -l, to show a long list
* cd /, go to the root directory
* cd bin, go to the bin directory
* cd .., go to one level up
* cd ~, got to the home directory

### Manipulating files and directories

* mkdir test
* mv test docker
* touch file.txt
* mv file.txt hello.txt
* rm hello.txt
* rm -r docker, remove docker directory recursively
* nano file.txt
* cat file.txt, to view file.txt
* less file.txt, to view with scrolling capabilities
* head file.txt, to view first 10 lines
* head -n 5 file.txt, to view first 5 lines
* tail file.txt, to view last 10 lines
* tail -n 5 file.txt, to view last 5 lines

### Searching for text

* ```grep hello file.txt```
* ```grep -i hello file.txt```, case-insensitive search
* ```grep -i hello file*.txt```, search files with pattern
* ```grep -ir hello .```, search current dir recursively

### Finding files and directories

* find, to list all files and dirs
* find -type d, to list dirs only
* find -type f, to list files only
* find -name "f*", to filter by name using a pattern

### Managing environment variables

* printenv, list all variables and the values
* printenv PATH, to show the value of PATH
* echo $PATH, to show the value of PATH
* export name=bob, to set a variable in the current session

### Managing Processes

* ps, to list thr running processes
* kill 37, to kill the process with id 37

### Managing users and groups

* useradd -m john, create user john with home directory
* adduser john, add a user with interactively
* usermod, to modify a user
* userdel, to del a user
* groupadd devs, create a group devs
* groups john, to view the groups for john
* groupmod, modify a group
* groupdel, del a group

### File Permissions

* chmod u+x deploy.sh, give owner user execute permission
* chmod g+x deploy.sh, give owner group execute permission
* chmod o+x deploy.sh, give everyone execute permission
* chmod ug+x deploy.sh, give owner user and group with execute permission
* chmod ug-x deploy.sh, remove execute permission from owner user and group

## Getting Started

After docker installed run below command:

```
docker run -dp 8080:80 docker/getting-started
```

* Above ```-d``` option, run container in detached mode (in the backgrournd)
* ```-p 8080:80``` option, map localhost port 8080 to container port 80
* ```docker/getting-started```, specify the image from docker

After started we can access the tutorial from http://localhost:8080 or we can back to Docker Desktop as below:

<img src="/images/img001.png">

Above figure toolbar we can:

* Open with browser, http://localhost:8080
* Open in terminal
* Pause
* Restart
* Stop
* Delete

## Our Application

Within ```/res``` there is ```app.zip``` extract it to ```app```, create ```Dockerfile``` under ```/app``` with following contents:

```
FROM node:12-alpine
# Adding build tools to make yarn install work on Apple silicon / arm64 machines
RUN apk add --no-cache python2 g++ make
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

Back to terminal run below command:

```
docker build -t tutorial .
```

* Above command use Dockerfile to build new image
* ```-t tutorial``` is the name
* After image created we can run it in container with below command:

```
docker run -dp 3000:3000 tutorial
```

* After above we can access tutorial from http://localhost:3000

## Updating our App

Update ```/app/src/app.js``` line 56 with following contents:

```html
<p className="text-center">You have no todo items yet! Add one above!</p>
```

Stop the running container ```tutorial``` and remove it, back to images and remove ```tutorial``` image also, then run below command:

```bash
docker build -t tutorial .
```

After image created we can run it with below command again:

```
docker run -dp 3000:3000 tutorial
```

Then check the updating from http://localhost:3000

## Sharing our App

### Create Repository

* Access docker hub from https://hub.docker.com
* Click ```Create Repository``` button
* Type ```tutorial``` as the name and make it public then click ```Create``` button
* After repository created we can see ```jinzd/tutorial``` created in the hub.

### Pushing Image

Back to the localhost client side terminal and execute below commands:

```bash
docker loin -u jinzd

docker tag tutorial jinzd/tutorial

doocker push jinzd/tutorial
```

### Running image on a new Instance

* Open browser and access [Play with Docker](https://labs.play-with-docker.com/).
* Login with your docker account then click ```Start``` button
* From right side panel click ```Add New Instance```
* After VM ready, in the terminal type below command and enter:

```base
docker run -dp 3000:3000 jinzd/tutorial
```

* After above container ok we can click top side port 3000 hyperlink button to access the application.

## Building Images

### Introduction

Having a solid understanding of Docker Images is crucial for us. In this section:

* Creating Docker files
* Versioning images
* Sharing images
* Saving and loading images
* Reducing the image size
* Speeding up build

### Images and Containers

An image includes everything and appliction needs to run, it contains:

* A cut-down OS
* Third party libraries
* Application files
* Environment variables

Once we have an image, we can start a container from it. Container is kind of like a VM, it provides an isolated environment to exec an appliction, it includes:

* Provides an isolated environment
* It can be stopped and restarted
* It is just a process, it is special kind of process, because it has its own file system which is provided by the image.

If there is running container, we can check it with ```docker ps``` command, see below result:

```
D:\xgit\docker>docker ps
CONTAINER ID   IMAGE      COMMAND                  CREATED       STATUS          PORTS                    NAMES
42ab14b53f1e   tutorial   "docker-entrypoint.s…"   6 hours ago   Up 10 seconds   0.0.0.0:3000->3000/tcp   boring_hugle
```

At this time we can start another container with below command:

```
D:\xgit\docker>docker run -it ubuntu
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
405f018f9d1d: Pull complete
Digest: sha256:b6b83d3c331794420340093eb706a6f152d9c1fa51b262d9bf34594887c2c7ac
Status: Downloaded newer image for ubuntu:latest
root@70de45f844de:/#
```

each container is an isolated environment for executing application. It's an isolated universe.

### Sample Web Application

Download ```react-app.zip``` and extract it to somewhre, in general we run the application with following steps:

* Install Node
* npm install
* npm start

Within the dir try the steps then http://localhost:3000

### Dockerfile Instructions

Dockerfile includes following parts:

* FROM
* WORKDIR
* COPY
* ADD
* RUN
* ENV
* EXPOSE
* USER
* CMD
* ENTRYPOINT

### Choosing the Right Base Image

For the Dockerfile we can access https://docs.docker.com/samples/

For image we can access https://hub.docker.com/ and find like "node"

* Open the project directory ```react-app```, and create ```Dockerfile``` under the dir with following contents:

```
FROM node:14.16.0-alpine3.13
```

### Copying Files and Directories

```
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
```

After above updating run below command:

```
docker build -t react-app .
```

then:

```bash
docker run -it react-app sh
```

### Excluding Files and Directories

When we build the image, for eaxmple node_modules should not be transfered, for this case, we should create a file named ```.dockerignore``` and add following line to it:

```
node_modules/
```

After above updating we can run below command again to build the image:

```
docker build -t react-app .
```

After image built we can run the image with below command

```bash
docker run -it react-app sh
```

### Running Commands

Continue with Dockerfile:

```
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
```

After above updating we can run below command again to build the image:

```
docker build -t react-app .
```

After image built we can run the image with below command

```bash
docker run -it react-app sh
```

### Setting Environment Variables

Continue with Dockerfile:

```
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.ereach.me/
```

then run:

```
docker build -t react-app .
```

After image built we can run the image with below command

```bash
docker run -it react-app sh
```

After above command we can check the env from terminal:

```
C:\Users\Jinzd\Documents\docker\react-app>docker run -it react-app sh
/app # printenv
NODE_VERSION=14.16.0
HOSTNAME=8ffdc77f2c0e
YARN_VERSION=1.22.5
SHLVL=2
HOME=/root
TERM=xterm
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
PWD=/app
API_URL=http://api.ereach.m
/app #
```

### Exposing Ports

Back to ```Dockerfile```:

```
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.ereach.me/
EXPOSE 3000
```

### Setting the User

* Start image react-app with sh exec:

```
docker run -it react-app sh

addgroup app
adduser -S -G app app
groups app
```

Above command we exec like below:

```
addgroup app && adduser -S -G app app
groups app
```

Back to Dockerfile:

```
FROM node:14.16.0-alpine3.13
WORKDIR /app
COPY . .
RUN npm install
ENV API_URL=http://api.ereach.me/
EXPOSE 3000
RUN addgroup app && adduser -S -G app app
USER app
```

After above we can run the command:

```
docker build -t react-app .
```

then:

```
docker run -it react-app sh
```

After above use ```whoami``` to check if current user is app.

