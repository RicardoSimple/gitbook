### 删除所有容器和镜像

删除包括其卷在内的所有容器

`docker rm -vf $(docker ps -aq)`

删除所有镜像

`docker rmi -f $(docker images -aq)`
