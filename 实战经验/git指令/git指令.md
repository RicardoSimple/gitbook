### git commit

提交记录，相当于快照

### git branch <分支名>

创建分支，创建分支后git commit会推进master

### git checkout <分支名>

切换到分支上然后进行git commit会推进分支

### git checkout -b <分支名>

创建分支的同时切换到新创建的分支

### git merge

将两个分支合并到一起。合并两个分支时会产生两个父节点

`git merge bugfix`是将bugfix合并到master里

`git checkout bugfix;git merge master`是把master合并到bugfix里

### git rebase

第二种合并分支的方法。rebase实际上是取出一系列的提交记录，复制他们然后在另外一个地方逐个放下(不会有两个父节点)

`git rebase master`是将(此时在bugfix分支下)bugfix分支里的工作直接移到master里

### HEAD

是一个对当前检出记录的符号引用，也就是指向你正在其基础上进行工作的提交记录

HEAD总是指向当前分支上最近一次的提交记录。大多数修改提交树的git命令都是从改变HEAD的指向开始的

HAED通常情况下是指向分支名的

`cat .git/HEAD`查看HEAD指向

`git symbolic-ref HEAD`查看head指向引用的指向

### 分离的HEAD

就是让其指向某个具体的提交记录而不是分支名

### 相对引用

`git log`查看提交记录的哈希值，但是实际的哈希值很长，但是git是提供前面几个字符就可以，比如

`fed2da64c0efc5293610bdd892f82a58e8cbc5d8`哈希值提供`fed2`

但是相对引用的话，可以从一个易于记忆的地方开始计算

使用`^`向上移动一个提交记录

使用`~<num>`向上移动多个提交记录，比如`~3`

比如`git checkout master^`切换到master的父节点

也可以将HEAD作为相对引用的参照

`git checkout HEAD^`

### 强制修改分支位置

可以直接使用`-f`选项让分支指向另一个提交，比如

`git branch -f master HEAD~3`会将master分支强制指向HEAD的第三级父提交

### 撤销变更

撤销变更由底层部分(暂存区的独立文件或者片段)和上层部分(变更到底是通过哪种方式被撤销的)组成

主要有两种方法`git reset`和`git revert`

#### git reset

通过把分支记录回退几个提交记录来实现撤销改动，向上移动分支

比如`git reset HEAD~1`

#### git revert

git reset这种改写历史的方法对于大家一起使用的远程分支是无效的。未来撤销更改并分享给其他人需要用`git revert`

`git revert HEAD`撤销当前的提交，实际上生成了一个新的提交，和之前的内容一样。

### 整理提交记录

比如：我想把这个提交放在这里，刚才的提交放在那个提交的后面

#### git cherry-pick

语法：`git cherry-pick <提交号>`

如果想将一些提交复制到当前所在的位置(HEAD)下，cherry-pick是最直接的方式

`git cherry-pick c2 c4`是将c2 c4的提交记录复制到当前的分支下，但是不会合并分支

### 交互式的rebase

当知道需要的提交记录的哈希值，用cherry-pick是最简单的方式

但是如果不清楚想要的提交记录的哈希值，可以利用交互式的rebase

交互式的rebase指的是使用带参数`-    -interactive`的rebase命令，简写为`-i`

加上这个命令之后会打开一个ui界面并列出将要被复制到目 标分支的备选提交记录，还会显示每个提交记录的哈希值和提交说明

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-07-18-49-13-image.png)

比如指令

`git rebase -i HEAD~4`会出现ui界面

### 本地栈式提交

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-07-18-52-51-image.png)

通过复制来解决

### 提交的技巧

比如：之前在newImage分支上进行了一次提交。然后又基于它创建了一个caption分支，然后又进行了一次提交

此时想对之前的提交做一些小小的修改，但是这个提交记录不是最新的

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-07-18-57-56-image.png)

### git tags

tag可以永久的为某个特定的提交命名为里程碑，然后可以像分支一样被引用

不会随着新的提交而移动，也不能检出到某个标签上面进行修改提交，就像是提交树上的一个锚点，表示了一个特定的位置

建立一个指向提交记录c1的标签：

`git tag v1 c1`标签名为v1,如果不指定提交记录，git会用head所指向的位置

### git describe

标签在代码库中起锚点的作用，git还设计了一个命令来描述离最近的锚点

git describe能帮你在提交记录中移动了多次之后找到方向；用git bisect(查找产生bug的提交记录的指令)找到某个提交记录的时候

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-08-20-image.png)

### 多分支rebase

一般涉及提交记录的合并和排序，可以多次使用git rebase最后使用交互式的rebase

### 选择父提交记录

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-25-39-image.png)

操作符还支持链式操作

`git checkout HEAD~^2~2`

### 远程仓库

git clone 命令

### 远程分支

git clone后本地会多一个分支，叫远程分支

远程分支在检出时会自动处于HEAD分离状态，所以必须更新了远程分支后才能更新工作成果

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-46-56-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-48-24-image.png)

### git fetch

从远程仓库获取信息

从远程仓库获取数据时, 远程分支也会更新以反映最新的远程仓库

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-51-03-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-51-42-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-52-04-image.png)

### git pull

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-53-30-image.png)

先抓取更新再合并到本地仓库的操作常用就提供了专门的命令

git pull

`git pull` 就是` git fetch` 和` git merge <just-fetched-branch>` 的缩写！

### 模拟团队合作

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-17-56-30-image.png)

### git push

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-18-17-35-image.png)

### 偏离的工作

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-18-21-12-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-18-21-40-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-18-22-35-image.png)

rebase可以换成merge

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-18-25-21-image.png)

`git pull`是fetch和merge的简写，`git pull --rebase`是fetch和rebase的简写

### 合并特性分支

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-18-28-56-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-19-14-05-image.png)

### 远程跟踪分支

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-19-17-52-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-19-18-34-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-19-18-52-image.png)

![](C:\Users\ricardo\AppData\Roaming\marktext\images\2022-07-08-19-20-01-image.png)
