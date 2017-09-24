# Deploying BDI Stack

As an example BDI Stack we will use [Docker Hadoop Spark Workbench](https://github.com/big-data-europe/docker-hadoop-spark-workbench).

## Local deployment
Clone the Docker Hadoop Spark Workbench:
```
$ git clone git@github.com:big-data-europe/docker-hadoop-spark-workbench.git && cd docker-hadoop-spark-workbench
```
Pull all the necessary docker images:
```
$ docker-compose pull
```
Deploy the stack:
```
$ docker-compose up
```

## Deploying in Swarm
There is a `swarm` folder in the Docker Hadoop Spark Workbench repository. You can deploy docker-compose files from that folder on docker swarm cluster. First create an overlay network on your Docker Swarm cluster:
```
$ docker network create --driver=overlay --attachable core
```
Then you can deploy any of the docker-compose definitions using:
```
$ docker stack deploy -c docker-compose.yml servicename
```
