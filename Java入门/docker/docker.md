### docker

#### docker简介

虚拟机技术的进化，容器技术

基于lxc的容器引擎

镜像文件，容器虚拟化技术，docker hub--安装docker

#### docker官网

docker官网：

[docker](http://www.docker.com)

dockerHub:

[dockerHub](https://hub.docker.com/)

#### docker 安装

##### 前言

docker并不是一个通用的容器，依赖于已存在并运行的Linux内核环境

docker实质上是已经运行的Linux下制造了一个隔离的文件环境，因此执行的效率几乎等同于所部署的Linux主机

因此，docker必须部署在linux内核的系统上

##### 条件

CentOS仅发行版本中的内核支持docker。docker运行在CentOS 7 64bit上

要求系统为64位，Linux系统内核版本为3.8以上

##### 查看自己的内核

`cat /etc/redhat-release`

`uname -r`

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-24-22-25-33-image.png)

#### docker的基本组成

镜像image，容器container，仓库repository

镜像类似java类模板，容器类似镜像，仓库存放镜像

##### 镜像

就是一个只读的模板，可以用来创建Docker容器，一个镜像可以创建很多容器，也相当于一份root文件系统，

##### 容器

应用程序或服务运行在容器里，类似一个虚拟的运行环境，可以被启动，停止，删除，开始；每个容器都是相互独立的，保证安全的

可以把容器看作一个简易的linux环境和运行中其中的运行程序

##### 仓库

是集中存放镜像文件的地方

分公开库和私有库，国内有阿里云，网易云

#### docker平台架构

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-15-04-20-image.png)

docker是client-server结构，守护进程运行在主机上，通过socket连接从客户端访问，守护进程从客户端接收命令管理运行在主机的容器

后端是一个松耦合架构，众多模块各司其职

docker运行基本流程：

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-15-10-31-image.png)

#### CentOS7安装docker

[安装文档](https://docs.docker.com/engine/install/centos/)

##### 安装步骤

+ 卸载旧版本

+ yum安装gcc相关
  
  ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-15-43-54-image.png)

+ 按官网安装要求的软件包
  
  ```shell
   sudo yum install -y yum-utils
  ```

+ 设置stable镜像仓库
  
  官网命令：容易超时
  
  ```
   sudo yum-config-manager \
      --add-repo \
      https://download.docker.com/linux/centos/docker-ce.repo
  ```
  
  切换云：
  
  ```shell
  yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
  ```

+ 更新yum软件包索引
  
  ```shell
  yum makecache fast
  ```

+ 按官网安装docker引擎
  
  ```shell
   sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
  ```

+ 启动docker
  
  ```shell
  sudo systemctl start docker
  ```

#### 阿里云镜像加速

阿里云官网中的容器镜像服务中的镜像加速工具

按照文档安装即可

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://getka3tx.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

#### 运行hello-world

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-16-12-09-image.png)

运行完打印出结果后，hello-world就会停止运行，容器自动终止

run 完成的动作

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-16-14-06-image.png)

#### docker和虚拟机

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-16-15-28-image.png)

#### docker常用命令

三类：帮助启动类、镜像命令类、容器命令类

##### 帮助启动类

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-16-20-21-image.png)

##### 镜像命令

+ 列出主机上的镜像
  
  `docker images`
  
  ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-17-46-52-image.png)
  
  选项说明：
  
  ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-17-47-27-image.png)
  
  + -a 选项，列出本地所有的镜像(含历史映像层)
  
  + -q 只显示镜像id

+ 搜索镜像
  
  `docker search 镜像名`
  
  比如搜索admin
  
  ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-17-52-06-image.png)
  
  表头说明
  
  ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-17-53-32-image.png)
  
  + --limit 限制显示多少个结果
    
    ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-17-56-38-image.png)

+ 拉取镜像
  
  `docker pull 镜像名[:tag]`
  
  没有tag就是最新的(写的时候没有中括号)，且是和镜像名连着写

+ 查看镜像/容器/数据卷所占的空间内存
  
  `docker system df`
  
  ![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-18-07-03-image.png)

+ 删除镜像
  
  `docker rmi 镜像id`
  
  + -f 强制删除
  
  也可以删除多个，也可以写成镜像名加版本号
  
  支持参数续传，比如
  
  `docker rmi -f $(docker images -aq)`

面试题：docker虚悬镜像是什么

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-10-25-18-12-43-image.png)

#### 容器命令

+ 新建并启动容器
  
  `docker run [options] image [command] [arg]`
  
  + options说明
    
    --name:为容器指定新名称
    
    -d：后台运行容器并返回容器id，也即启动守护式线程
    
    -i：以交互模式运行容器，通常与-t同时使用
    
    -t：为容器重新分配一个伪输入终端，与-i同时使用，也即启动交互式容器
    
    -P大写P，随机端口映射
    
    -p小写p，指定端口映射
