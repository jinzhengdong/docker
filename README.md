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

