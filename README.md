## Index
- [Container](#container)
- [Volume](#volume)
- [Entrypoint](#entrypoint)
- [Build Images](#build-images)
- [Networks](#networks)
---
### Container
##### Create image as a container
```.dockerfile
docker run -d -p 80:80 --name nginx nginx
docker exec -it nginx bash
docker stop nginx
```
##### Kill
```.dockerfile
docker rm nginx -f
docker rm $(docker ps -a -q) -f
```
##### directory method:bind
```.dockerfile
docker run -d --name nginx -p 8081:80 --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html/ nginx
```
##### directory method:-v
```.dockerfile
docker run -d -v "$(pwd)"/html/x:/usr/share/nginx/html/ nginx
```
---
### Volume
##### Creating Volumes
```.dockerfile
docker volume create myvol
docker volume inspect myvol
docker volume prune
```
##### Sharing volumes
```.dockerfile
docker run --name nginx -d --mount type=volume,source=myvolume,target=/app nginx
docker run --name nginx2 -d --mount type=volume,source=myvolume,target=/app nginx
```
---
### Entrypoint
File:Dockerfile
```.dockerfile
FROM ubuntu:latest

ENTRYPOINT ["echo","Hello"]

CMD["World"]
```
---
### Build Images
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
docker rmi ubuntu
```
---
### Networks
##### Create network - bridge
```.dockerfile
docker network create --driver bridge mynetwork
```
##### Create container with network
```.dockerfile
docker run -dit --name ubuntu1 --network mynetwork bash
```
##### Connecting container to a network
```.dockerfile
docker run -dit --name ubuntu2 bash
docker network connect mynetwork ubuntu2
```
##### Connection test 
```.dockerfile
ping ubuntu1
```

