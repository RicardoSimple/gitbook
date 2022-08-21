购买服务器

[Y 栈](https://ham.youkeda.com/articles/detail/5f3753a15e205f30b2c2ab0a)

#### 安装docker

依次执行

```shell
sudo yum -y update
sudo yum -y install epel-release
sudo yum -y install docker-io
```

如果出现错误"未找到匹配的参数  docker-io"

执行`sudo yum -y install docker`

ubuntu安装教程[Ubuntu Docker 安装 | 菜鸟教程](https://www.runoob.com/docker/ubuntu-docker-install.html)

#### 安装MongoDB

##### 先启动Docker

```shell
sudo systemctl start docker
```

再执行命令查看docker版本号

```shell
sudo docker version
```

##### 再安装MongoDB并启动

下载镜像

```shell
sudo docker pull mongo:latest
```

再执行命令查看mongodb的镜像

```shell
sudo docker images
```

启动mongodb

```shell
sudo docker run -itd --name mongo -p 27017:27017 mongo --auth
```

##### 检查mongodb是否启动成功

执行命令

```shell
sudo docker ps
```

看到有mongo的信息就成功了

##### 创建admin账户

#### 先登录到mongodb软件控制台

```shell
sudo docker exec -it mongo mongo admin
```

##### 创建管理员账户

```shell
db.createUser({ user:'admin',pwd:'123456',roles:[ { role:'root', db: 'admin'}]});
```

user和pwd都是可以修改的

##### 认证管理员账户

```shell
db.auth('admin', '123456')
```

系统返回结果1则表示管理员账户创建并验证成功

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-05-17-17-27-05-image.png)

#### 创建数据库实例

mongodb登录状态下创建一个数据库，再创建一个可读写操作的用户

数据库对应的用户是用来读写本数据库的，不同于admin管理员账户，管理员账户不能用来读写数据库

##### 切换数据库

使用use命令创建并切换

系统输出`switched to db xxxx`就表示成功了

###### 创建读写用户

`ad.createUser`用于创建可读写操作的用户

```shell
db.createUser({ user:'xxxx',pwd:'xxxxxx',roles:[{ role:'root', db: 'admin'},{ role:'dbAdmin', db: 'practice'}]});
```

practice对应数据库的名称

###### 认证数据库

创建读写用户之后执行一次`db.auth`验证自己设置的用户和密码

```shell
db.auth('xxxx', 'xxxxxx')
```

###### 退出登录

`exit`

#### 云服务器需要开放端口

![](https://style.youkeda.com/img/ham/course/j4/j4-8-2-2.png?x-oss-process=image/resize,w_800/watermark,image_d2F0ZXJtYXNrLnBuZz94LW9zcy1wcm9jZXNzPWltYWdlL3Jlc2l6ZSx3XzEwMA==,t_60,g_se,x_10,y_10)

##### Spring 项目中的配置

```properties
## 购买的云服务器的公网 IP
spring.data.mongodb.host=
## MongoDB 服务的端口号
spring.data.mongodb.port=27017
## 创建的数据库及用户名和密码
spring.data.mongodb.database=
spring.data.mongodb.username=
spring.data.mongodb.password=
```
