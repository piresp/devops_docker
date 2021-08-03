## # Index
- [Container](#container)
- [Volume](#volume)
- [Entrypoint](#entrypoint)
- [Build Images](#build-images)
- [Networks](#networks)
- [Docker Compose](#docker-compose)
---
### Container:
##### Create image as a container
```.dockerfile
docker run -d -p 80:80 --name {name} nginx
docker exec -it {name} bash
docker stop {name}
```
##### Kill
```.dockerfile
docker rm {name} -f
docker rm $(docker ps -a -q) -f
```
##### directory method:bind
```.dockerfile
docker run -d --name {name} -p 80:80 --mount type=bind,source=$(pwd),target=/usr/share/nginx/html/ nginx
```
##### directory method:-v
```.dockerfile
docker run -d -v $(pwd)/html/x:/usr/share/nginx/html/ nginx
```
---
### Volume:
##### Creating Volumes
```.dockerfile
docker volume create {name}
docker volume inspect {name}
docker volume prune
```
##### Sharing volumes
```.dockerfile
docker run --name {name1} -d --mount type=volume,source=myvolume,target=/app nginx
docker run --name {name2} -d --mount type=volume,source=myvolume,target=/app nginx
```
---
### Entrypoint:
File:Dockerfile
```.dockerfile
FROM ubuntu:latest

ENTRYPOINT ["echo","Hello"]

CMD["World"]
```
---
### Build Images:
##### PHP with Lavarel
file:Dockerfile
```.dockerfile
FROM php:7.4-cli

WORKDIR /var/www

RUN apt-get update && \
    apt-get install libzip-dev -y && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');"

RUN php composer.phar create-project --prefer-dist laravel/laravel laravel

ENTRYPOINT [ "php","laravel/artisan","serve" ]

CMD [ "--host=0.0.0.0" ]
```
##### List and purge images
```.dockerfile
docker images
docker rmi {name}
```
---
### Networks:
##### Create network - bridge
```.dockerfile
docker network create --driver bridge {namenet}
```
##### Create container with network
```.dockerfile
docker run -dit --name {namecont1} --network {namenet} bash
```
##### Connecting container to a network
```.dockerfile
docker run -dit --name {namecont2} bash
docker network connect {namenet} {namecont2}
```
##### Connection test 
```.dockerfile
ping {namecont1}
```
---
### Docker Compose:
```.dockerfile
docker-compose up
docker-compose up -d --build
docker-compose ps
```

