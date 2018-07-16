---
layout: post
title:  "Remote MySQL Server with Docker"
date:  2018-07-08 09:09
description: "MySQL is a freely available open-source relational database management system that uses SQL. Docker is an virtualization software that performs operating-system-level virtualization."
img: docker-mysql.jpg
tags: [docker, system, db]
---

### Take a quick glance at MySQL and Docker
MySQL is a freely available open-source relational database management system that uses SQL.
Docker is an virtualization software that performs operating-system-level virtualization.
Unlike traditional VM, docker doesn't take up a lot of space, memory and processing power.
Docker provides a light-weight container environment which can be used to host any application of your choice. Because it's light-weight, it can be easily shipped to other machines and be executed on those machines, then you can get the exact same environment in dev and product system. Or you can clone your environment for any new team members easily and quickly, without painful setups.
With Docker, you can easily launch containers based on images that contain different versions of the software.
You could create as many container as you want on the host machine, and each of the container are isolated from one another.
Also, each docker images are version controlled.

### Step 1: Get the docker image of MySQL
You can search what you want from the https://hub.docker.com/ .
In this tutorial, I will use the first one: mysql/mysql-server.
Here is the command to down the image from the server to your local machine:

```shell
docker pull mysql/mysql-server:latest
```

After download finished, you can see it through the docker image command:
```shell
docker images
```


### Step 2: Start running a docker container from MySQL image
Now, you can start a mysql-server instance with the docker run command:
```shell
docker run - name=mysql1 -d mysql/mysql-server
# print the logs of mysql1
docker logs mysql1
```


If you only want to access it locally, it's enough, but if you want to access your mysql server remotely, you need mapping the port 3306 of the container to port 3306 of the host machine. Also, you need to set the environment variable MYSQL_ROOT_HOST with wildcards to allow root connections from other hosts:
```shell
# stop and remove the container and run again:
docker stop mysql4 && docker rm mysql4
docker run - name=mysql1 -e MYSQL_ROOT_HOST=% -p 3306:3306 -d mysql/mysql-server
```


### Step 3: Connecting to the MySQL Server instance
Since it's already running, you can connect to it with the docker exec command:
```shell
# login into mysql
docker logs mysql1 2>&1 | grep GENERATED # check the automatically generated password of root user, copy it
docker exec -it mysql1 mysql -u root -p # parse and press the Enter key
```


The system will ask you to change to root password, use the following command to achieve it:
```shell
ALTER USER 'root'@'localhost' IDENTIFIED BY '<password>';
```


Now your mysql server instance is running inside the docker, and you can access it from other hosts through 3306 port.
You can check your running containers through docker ps command:
```shell
docker ps
# to check all the containers, include not-running containers
docker ps -a
```


Problem solving for remotely access
If you got the same problem like this while connect to MySQL server from another host (It depends on which version of MySQL you are using):
java.sql.SQLNonTransientConnectionException: Public Key Retrieval is not allowed
You should change your password of root user by using the native password hashing method to fix it:
```shell
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '<password>';
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '<password>';
```
