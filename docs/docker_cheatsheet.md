# DOCKER CHEAT SHEET 

## Help 

```
docker --help | more # scroll one page at a time
```

NB: Also see [Docker Cheat Sheet](https://www.docker.com/sites/default/files/d8/2019-09/docker-cheat-sheet.pdf) 

## Build image in multi-stage Dockerfile:

`export DOCKER_BUILDKIT=1` # add as system environment variable


```
# Notice the dot at the end (this specifies the directory in which Dockerfile is contained). 
# Also, of note: target tells which block of code to use in the multi-stage build Dockerfile.

docker build -t <image name> --target <target name> .

# valid target names; ml, cnn and vote
# image names: repository/name, e.g., nlpieumn/ml

```

## List all docker images on compute node

```
docker images
```


## Pull image from Docker Hub

```
docker pull docker/whalesay
```

## Pull image from other repository

```
docker pull nlpieumn/ml
```

## Run image:

### Hello World

```
docker run docker/whalesay cowsay "Hello W22"
```

### ssh into docker container

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/ml /bin/bash
```

#### Explore container

```
pwd
ls -la
ls -la /data
cat /etc/os-release
ls /bin
exit
```

### Example to run help to see available ml classifiers

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/ml python /home/tutorial/ml.py --help
```

### Example to run default - svm - classifier

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/ml python /home/tutorial/ml.py -c svm
# note, svm is the default ml classifier and will be run if no arrgumet is given
```

### Example to run mlp classifier (or log/rf/bag/boost)

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/ml python /home/tutorial/ml.py -c mlp
```

### Example to run voting ML ensemble classifier

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/ml python /home/tutorial/ensemble.py
```

### Example to run cnn/keras classifier

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/cnn PYTHONHASHSEED=0 python /home/tutorial/cnn.py
```

### Example to run majority voting ensemble:

```
docker run -it -v /home/amia/tutorial/:/data nlpieumn/vote python /home/tutorial/vote.py
```

## Other useful commands:

### Run docker container using mlp as background daemon process

```
docker run -d -it -v /home/amia/tutorial/:/data nlpieumn/ml python /home/tutorial/ml.py -c mlp
```

### Stop daemon (background) process container 
(NB: use container id returned from previous step, or get from running “docker ps”)

```
docker stop <conatiner id>
```

### Push to nlpieumn org repo 
(NB: need to have nlpieumn organization privileges to do this), specify version number `n`

```
docker push nlpieumn/ml
```

### Remove docker image
```
docker rmi <image id> # get <image id> from running “docker images”
```

### Monitor docker processes/resource utilization

```
docker ps
```

```
docker stats
```
