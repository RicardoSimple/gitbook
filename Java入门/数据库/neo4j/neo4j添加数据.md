### 配置导入文件的路径

使用`load csv WITH HEADERS from 'file:///HouseType.csv' as line create (:风格{name:line.name,sketch:line.sketch,detail:line.detail})`报错`Couldn't load the external resource at: file:/HouseType.csv ()`

原因：

Windows版Neo4j的配置文件conf/neo4j.conf中默认配置了`dbms.directories.import=import`，所以可以将文件放入improt后使用相对路径导入

而docker版Neo4j的配置文件中没有配置`dbms.directories.import`参数，所以需要使用全路径导入

或者在`conf/neo4j.conf`中配置`dbms.directories.import=import`后重启

### neo4j环境变量配置

```xml
export NEO4J_HOME=/home/kg/neo4j/neo4j-community-3.4.5
export PATH=$PATH:$NEO4J_HOME/bin
```

路径可变

### load csv指令

用于导入csv文件到数据库

```shell
load csv WITH HEADERS
```

是表明文件的第一行是属性名

```shell
load csv WITH HEADERS from 'file:///HouseType.csv' as line
 create (:风格{name:line.name,sketch:line.sketch,detail:line.detail})
```

指令里的create也可以使用merge指令，create直接添加数据，不会管重复，而merge如果有重复就不会继续添加

导入有关系的csv文件

```sql
load csv WITH HEADERS from 'file:///流程关系.csv' as line 
match(from:`流程`{title:line.fromtitle}),(to:`流程`{title:line.totitle}) 
merge(from)-[r:`接着是`{relationship:line.relationship}]->(to)
```

要注意csv文件记事本打开**不能有引号**
