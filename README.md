# Docker

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

## Persisting DB

We can always run below command, but after it, a new container will be created:

```bash
docker run -dp 3000:3000 tutorial
``` 

Access localhost 3000 we find there is no data, and if we want to remove docker container, below command can be executed:

```base
docker container rm -f 0f9c6821616915989e97314d17b73590f5408d6a4bd55d8a407a7e65ab786c40
```