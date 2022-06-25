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

