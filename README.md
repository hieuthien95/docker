# PULL 1 image
```
$ docker -v
Docker version 18.09.2, build 6247962

$ docker-compose -version
docker-compose version 1.23.2, build 1110ad01

$ docker-machine -version
docker-machine version 0.16.1, build cce350d7

$ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
1b930d010525: Already exists
Digest: sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Status: Downloaded newer image for hello-world:latest
```
-----------------------------------------------
# DELETE 1 image
```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
hello-world         latest              fce289e99eb9        8 weeks ago         1.84kB

$ docker rmi -f hello-world:latest
Untagged: hello-world:latest
Untagged: hello-world@sha256:2557e3c07ed1e38f26e389462d03ed943586f744621577a99efb77324b0fe535
Deleted: sha256:fce289e99eb9bca977dae136fbe2a82b6b7d4c372474c9235adc1741675f587e

$ docker images
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
```


# CREATE container mysql/phpMyAdmin/wordpress
```
$ docker network create mysql_network
a558bbc6e4b41965faf3945e83d671b2f68f9c81285ddaa98ca8a2965acf4869

$ docker run --name mysql_container --network mysql_network -v /home/moe/mysql_data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456789 -d mysql:5.7
dc3b6aa28d452ec385e1b9b562215d5f4dc3438e93a1dc14f8a53e477109cda8

$ docker run --name phpmyadmin_container -d --network mysql_network -p 8080:80 -e PMA_HOST=mysql_container phpmyadmin/phpmyadmin
fa7bb9c16a94da5aed15b41fdb7fa61348470fa77a6db5f6bacc681b3ce8ae27

$ docker run --name wordpress_container -d --network mysql_network -p 8800:80 --link mysql_container:mysql wordpress
9301f10a3c69fedba9cd50789ec64f3ce89d7fddc8d8b3be8e222ac008155467
```
```
$ docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
3918a04c98be        bridge              bridge              local
efe43ad1aaaa        host                host                local
9de39be13875        none                null                local
a558bbc6e4b4        mysql_network       bridge              local

$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED              STATUS              PORTS                            NAMES
9301f10a3c69        wordpress               "docker-entrypoint.s…"   About a minute ago   Up About a minute   0.0.0.0:8800->80/tcp             wordpress_container
a5160b172f11        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   16 minutes ago       Up 16 minutes       9000/tcp, 0.0.0.0:8080->80/tcp   phpmyadmin_container
b5beda06ce42        mysql:5.7               "docker-entrypoint.s…"   16 minutes ago       Up 16 minutes       3306/tcp, 33060/tcp              mysql_container
```

# LIST cac container
```
$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
fa7bb9c16a94        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   5 minutes ago       Up 5 minutes        9000/tcp, 0.0.0.0:8081->80/tcp   myadmin
dc3b6aa28d45        mysql:5.7               "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        3306/tcp, 33060/tcp              learn_mysql

$ docker ps -a
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                    PORTS                            NAMES
fa7bb9c16a94        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   57 seconds ago      Up 55 seconds             9000/tcp, 0.0.0.0:8081->80/tcp   myadmin
dc3b6aa28d45        mysql:5.7               "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes              3306/tcp, 33060/tcp              learn_mysql
d3d049bd7c6c        hello-world             "/hello"                 22 hours ago        Exited (0) 22 hours ago                                    relaxed_wing
```

# START 1 container
```
$ docker start fa7bb9c16a94
fa7bb9c16a94

$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
fa7bb9c16a94        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   5 minutes ago       Up 5 minutes        9000/tcp, 0.0.0.0:8081->80/tcp   myadmin
```

# STOP 1 container
```
$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
dc3b6aa28d45        mysql:5.7               "docker-entrypoint.s…"   7 minutes ago       Up 7 minutes        3306/tcp, 33060/tcp              learn_mysql

$ docker stop dc3b6aa28d45
dc3b6aa28d45

$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
```

# DELETE 1 container
```
$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
fa7bb9c16a94        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   5 minutes ago       Up 5 minutes        9000/tcp, 0.0.0.0:8081->80/tcp   myadmin

$ docker rm fa7bb9c16a94
fa7bb9c16a94

$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
```

# STOP tat ca cac container dang chay
```
$ docker ps
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS              PORTS                            NAMES
4eb65ca28ddb        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   5 days ago          Up 2 seconds        9000/tcp, 0.0.0.0:8080->80/tcp   phpmyadmin_container
6945cd8bf720        mysql:5.7               "docker-entrypoint.s…"   5 days ago          Up 8 seconds        3306/tcp, 33060/tcp              mysql_container

$ docker stop $(docker ps -qa)
4eb65ca28ddb
6945cd8bf720
d3d049bd7c6c

$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

# DELETE tat ca cac container
```
$ docker ps -a
CONTAINER ID        IMAGE                   COMMAND                  CREATED             STATUS                    PORTS                            NAMES
4eb65ca28ddb        phpmyadmin/phpmyadmin   "/run.sh supervisord…"   5 days ago          Exited (255) 4 days ago   9000/tcp, 0.0.0.0:8080->80/tcp   phpmyadmin_container
6945cd8bf720        mysql:5.7               "docker-entrypoint.s…"   5 days ago          Exited (255) 4 days ago   3306/tcp, 33060/tcp              mysql_container
d3d049bd7c6c        hello-world             "/hello"                 6 days ago          Exited (0) 5 days ago                                      relaxed_wing

$ docker rm $(docker ps -qa)

$ docker ps -a
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

# Xem log container
```
Docker logs 4eb65ca28ddb
```

# Create image
_Chuẩn bị *Dockerfile*_
```
FROM golang:1.11.4-alpine  as builder
RUN pwd
RUN ls
WORKDIR /go/src/gitlab.com/lak8s/lession1
RUN pwd
RUN ls
COPY . /go/src/gitlab.com/lak8s/lession1/
RUN go build -o ./dist/app
RUN pwd
RUN ls
# p2
WORKDIR /demo
RUN pwd
RUN ls
COPY --from=builder /go/src/gitlab.com/lak8s/lession1/dist/app .
EXPOSE 9090
ENTRYPOINT ["./app"]
```
_build docker image_
```
docker build -t my_image .
```
