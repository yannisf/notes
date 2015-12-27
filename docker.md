# docker on Linux Mint 17

## Install
```
sudo apt-get install apparmor lxc cgroup-lite
sudo apt-get install docker.io
```

## Permissions
Docker requires root (or sudo) to run. In order to run docker as a simple user:
1. Add docker group
2. Add user to docker group
3. Restart docker
4. Logout/login 

## Verify
```
sudo service docker.io start
docker run hello-world
```

## Images
``docker images``

### Pull image
``docker pull mysql``

## Dockerfile
Dockerfiles contain instructions for inheriting from an existing image, where you can then add software or customize configurations.

```
FROM mysql:5.5.45
RUN echo America/New_York | tee /etc/timezone && dpkg-reconfigure --frontend noninteractive tzdata
```

## Run a docker image
``docker run --name container_name -p 3306:3306 -d image_name``

## Remove a docker image
``docker rmi image_name``

## Show docker containers
Active containers: ``docker ps``

All containers: ``docker ps -a``

## Start/Stop/Remove a container
```
docker start container_name
docker stop container_name
docker rm container_name
```

## Obtain shell
```
docker exec -it container_name bash
```

## Oracle XE 11g

### Installation
```
docker pull wnameless/oracle-xe-11g
```

Run with 22 and 1521 ports opened:
```
docker run -d -p 1522:22 -p 1521:1521 wnameless/oracle-xe-11g
```

Connect database with following setting:
```
hostname: localhost
port: 1521
sid: xe
username: system
password: oracle
```

Login by SSH
```
ssh root@localhost -p 1522
password: admin
```
