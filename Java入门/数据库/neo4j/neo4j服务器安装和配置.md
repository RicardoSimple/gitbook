## neo4j服务器上的安装和配置

服务器配置是centos7.6

### 安装jdk

安装neo4j之前需要安装jdk，neo4j v4对应jdk11，v3对应jdk8

#### 1. 下载jdk11压缩包

国内镜像站下载 Java JDK。比如：[Oracle JDK 下载 - 编程宝库](http://www.codebaoku.com/jdk/jdk-oracle.html)

#### 2. 上传压缩包到服务器

也可以直接下载到服务器

`wget https://mirrors.huaweicloud.com/java/jdk/8u202-b08/jdk-8u202-linux-x64.tar.gz`

#### 3. 解压压缩包

也可以解压后上传到服务器上

进入下载的位置，执行命令

`tar zxvf jdk-8u202-linux-x64.tar.gz`

#### 4. 放入安装目录

一般都是`/usr/java/jdkxx`

#### 5. 修改环境变量

在`/etc/profile`文件中使用vi或者其他文件查看器来编辑文件

```xml
export JAVA_HOME=/usr/java/jdk-8u202-linux-x64
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
```

#### 6. 验证按照是否成功

`javac`和`java -version`命令

如果出现权限不够比如

```shell
-bash: /usr/java: Permission denied
```

执行`sudo chmod 777 /usr/java/`

或者`chmod +x /usr/java/jdk1.8.0_161/bin/javac`注意对应的路径一定要正确

### 安装neo4j

#### 1. 下载压缩包，安装

在[官网](https://go.neo4j.com)下载于jdk对应的压缩包社区版，比如

`neo4j-community-4.4.9-unix.tar.gz`

之后传递到服务器，解压，与安装jdk类似

#### 2. 修改配置文件

安装目录下进入conf找到neo4j.conf配置文件进行配置

```xml
一、dbms配置
dbms.default_database=neo4j

目录路径
dbms.directories.data=data
dbms.directories.plugins=plugins
dbms.directories.certificates=certificates
dbms.directories.logs=logs
dbms.directories.lib=lib
dbms.directories.run=run
dbms.directories.metrics=metrics

导入文件的目录，配置后只能从import目录导入，注释后可从任意文件目录导入
dbms.directories.import=import

认证
dbms.security.auth_enabled=false

允许更新
dbms.allow_upgrade=true

初始Java堆大小
dbms.memory.heap.initial_size=512m

最大Java堆大小
dbms.memory.heap.max_size=512m

jvm额外启动参数，多个配置多个
dbms.jvm.additional=-XX:MaxDirectMemorySize=512m

页缓存大小，默认为RAM大小减去最大堆内存后的50%(假如机器上只运行了neo4j)
dbms.memory.pagecache.size=10g

数据库总数
dbms.max_databases=100

是否允许在线备份
dbms.backup.enabled=true

默认只能localhost备份
dbms.backup.listen_address=0.0.0.0:6362
#The maximum time interval of a transaction within which it should be completed.
dbms.transaction.timeout

Defines whether memory for transaction state should be allocated on- or offheap.ON_HEAP, OFF_HEAP。默认OFF_HEAP
dbms.tx_state.memory_allocation=ON_HEAP

The number of Cypher query execution plans that are cached.
dbms.query_cache_size=1000

neo4j运行模式：SINGLE, CORE, READ_REPLICA
dbms.mode=SINGLE

二、JVM配置
初始Java堆大小
dbms.memory.heap.initial_size=512m

最大Java堆大小
dbms.memory.heap.max_size=512m

三、网络连接配置
默认只允许本地连接
dbms.connectors.default_listen_address=0.0.0.0

配置成当前机器IP或hostname
dbms.connectors.default_advertised_address=localhost

Bolt 连接
dbms.connector.bolt.enabled=true
dbms.connector.bolt.tls_level=DISABLED
dbms.connector.bolt.listen_address=:7687

Bolt连接保持时间
dbms.connector.bolt.thread_pool_keep_alive=5m

处理Bolt连接线程池最大线程数，默认400
dbms.connector.bolt.thread_pool_max_size

处理Bolt连接线程池最小线程数，默认5
dbms.connector.bolt.thread_pool_min_size

HTTP 连接
dbms.connector.http.enabled=true
dbms.connector.http.listen_address=:7474

HTTPS 连接
dbms.connector.https.enabled=false
dbms.connector.https.listen_address=:7473

neo4j工作线程数，只对REST连接生效
dbms.threads.worker_count=20

四、metris监控
default true
metrics.enabled=true

监控导出到CSV文件
metrics.csv.enabled=true

允许Prometheus，默认false
metrics.prometheus.enabled=true

The hostname and port to use as Prometheus endpoint
metrics.prometheus.endpoint=localhost:2004
```

#### 3. 启动

进入安装目录的bin文件夹执行

`./neo4j start`如果出现报错，检查jdk版本是否对应

也可自行添加环境变量来执行

#### 4. 其他指令

停止：`./neo4j stop`

查看图数据库状态：`./neo4j status`

#### 5. 客户端访问

http://服务器ip地址:7474/browser/

在浏览器访问图数据库所在的机器上的7474端口

（第一次访问账号neo4j，密码neo4j，会提示修改初始密码）

注意：端口需与自己配置文件设置的一样，服务器防火墙也需要检查是否开启对应端口

### 配置远程连接neo4j

#### 远程bolt连接：(端口不一定一样)

#dbms.connector.bolt.listen_address=:7687

改为：

dbms.connector.bolt.listen_address=0.0.0.0:7687
都是修改ip

#### 选择连接数据库：

dbms.active_database=importData

此时需要把：

#dbms.allow_format_migration=true

改为：

dbms.allow_format_migration=true
