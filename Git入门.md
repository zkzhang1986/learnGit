Git使用说明：

Git :分布式版本控制系统

![image-20200917164749060](C:\Users\zzk\AppData\Roaming\Typora\typora-user-images\image-20200917164749060.png)

![image-20200917164814287](C:\Users\zzk\AppData\Roaming\Typora\typora-user-images\image-20200917164814287.png)

![image-20200917164846172](C:\Users\zzk\AppData\Roaming\Typora\typora-user-images\image-20200917164846172.png)

![image-20200917164918514](C:\Users\zzk\AppData\Roaming\Typora\typora-user-images\image-20200917164918514.png)

![image-20200917164944790](C:\Users\zzk\AppData\Roaming\Typora\typora-user-images\image-20200917164944790.png)

## 1.安装

在Windows上使用Git，可以从Git官网直接[下载安装程序](https://git-scm.com/downloads)，然后按默认选项安装即可。

安装完成后，在开始菜单里找到“Git”->“Git Bash”，蹦出一个类似命令行窗口的东西，就说明Git安装成功！

### 1.1配置：

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```

因为Git是分布式版本控制系统，所以，每个机器都必须自报家门：你的名字和Email地址。你也许会担心，如果有人故意冒充别人怎么办？这个不必担心，首先我们相信大家都是善良无知的群众，其次，真的有冒充的也是有办法可查的。

注意`git config`命令的`--global`参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址



> git config  参数
>
> 读[--system],[--global], [--local],[--worktree],[--file<filename>]
>
> 写[--system],[--global],[--worktree],[--file<filename>]
>
> --global 
>
> 表示全局的，即当前用户都有效，该配置会出现在 ~/.gitconfig 文件中，~表示当前用户的目录，比如我的是：C:\Users\username\.gitconfig 
>
> 例：
>
> ```bash
> git config --global user.name "Your Name"
> git config --global user.email "email@example.com"
> ```
>
> 不加global 就是局部变量。局部是只对当前仓库起效的，它的配置信息会在当前仓库根目录/.git/config文件下。
>
> ```bash
> git config user.name "Your Name"
> git config user.email "email@example.com"
> ```
>
> ### **注意：**局部变量覆盖全局变量！！！和编程语言里面的变量关系是一样的。



### 1.2查看配置

```bash
git config --l
```

### 1.3修改配置

```bash
git config --replace-all user.name "name"
git config --replace-all user.email "123@qq.com"
```

### 1.4帮助

直接输入 git config ，就可以看到简单的命令列表。



## 2.创建版本库

什么是版本库呢？版本库又名仓库，英文名**repository**，你可以简单理解成一个目录，这个目录里面的所有文件都可以被Git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追踪历史，或者在将来某个时刻可以“还原”。所以，创建一个版本库非常简单。

#### 首先，选择一个合适的地方，创建一个空目录：

```bash
# 创建目录
$ mkdir learngit
$ cd clearngit
# 查看目录路径
$ pwd
/d/learngit

```

#### 第二步，通过`git init`命令把这个目录变成Git可以管理的仓库：

```bash
# 把文件夹变成Git可以管理的仓库
$ git init
# 把文件添加到仓库
$ git add readme.txt
# 把文件提交到仓库 -m 是本次提交的说明
$ git commit -m "wrote a readme file"
# 如果你没有看到.git目录，那是因为这个目录默认是隐藏的，用ls -ah命令就可以看见。
```

## 3.时光穿梭

### 3.1 版本回退

#### 3.1.1查看版本

```bash
$ git log
# 如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：
# 如果查看某个文件 git log <文件名>或 git log --pretty=oneline <文件名> 
commit 1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master)
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:06:15 2018 +0800

    append GPL

commit e475afc93c209a690c39c13a46716e8fa000c366
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 21:03:36 2018 +0800

    add distributed

commit eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0
Author: Michael Liao <askxuefeng@gmail.com>
Date:   Fri May 18 20:59:18 2018 +0800

    wrote a readme file
$ git log --pretty=oneline
1094adb7b9b3807259d8cb349e7df1d4d6477073 (HEAD -> master) append GPL
e475afc93c209a690c39c13a46716e8fa000c366 add distributed
eaadf4e385e865d25c48e7ca9c8395c3f7dfaef0 wrote a readme file
```

**一大串类似`1094adb...`的是`commit id`（版本号）是一个SHA1计算出来的一个非常大的数字，用十六进制表示,每提交一个新版本，实际上Git就会把它们自动串成一条时间线.**

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

#### 3.1.2 回退 

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

现在，我们要把当前版本`append GPL`回退到上一个版本`add distributed`，就可以使用`git reset`命令：

```bash
# 版本回退
$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
# cat 打开文件查看文件内容命令
$ cat readme.txt
Git is a distributed version control system.
Git is free software.
```

Git的版本回退速度非常快，因为Git在内部有个指向当前版本的`HEAD`指针，当你回退版本的时候，Git仅仅是把HEAD从指向`append GPL`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   └──> ○ append GPL
        │
        ○ add distributed
        │
        ○ wrote a readme file
```

改为指向`add distributed`：

```ascii
┌────┐
│HEAD│
└────┘
   │
   │    ○ append GPL
   │    │
   └──> ○ add distributed
        │
        ○ wrote a readme file
```

然后顺便把工作区的文件更新了。所以你让`HEAD`指向哪个版本号，你就把当前版本定位在哪

#### 3.1.3 回的未来版本

只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个`append GPL`的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```bash
# 回到未来版本
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

#### 3.1.4 查看历史版本

在Git中，总是有后悔药可以吃的。当你用`$ git reset --hard HEAD^`回退到`add distributed`版本时，再想恢复到`append GPL`，就必须找到`append GPL`的commit id。Git提供了一个命令`git reflog`用来记录你的每一次命令：

```bash
$ git reflog
d4668e8 (HEAD -> master) HEAD@{0}: reset: moving to d4668e8df066
4770e84 HEAD@{1}: reset: moving to HEAD^
d4668e8 (HEAD -> master) HEAD@{2}: commit: append GPL
4770e84 HEAD@{3}: commit: add distributed
31a1a8a HEAD@{4}: commit: wrote a readme file
3e2fd9f HEAD@{5}: commit: append GPL
73a342b HEAD@{6}: commit (initial): wrote a readme file
```

接着就可以用

```bash
$ git reset -hard + commit id
用来重置
```



### 3.2 工作区和暂存区

工作区：就是在电脑上能看到的目录，比如：learngit文件夹。

版本区：工作区有一个隐藏目录.git,这个不算工作区，而是git的版本库。

Git的版本库有很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，换有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

我们把文件往git版本库添加时，是分两步：

第一步：git add 把文件添加进去，实际就是把文件修改添加到暂存区；

第二步：git commit 提交更改，实际就是把暂存区的所有内容提交到当前分支；

**测试**

修改 readme.txt，同时在工作区建一目录名为：LICENSE文件

先用git status 查看状态

```bash
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme1.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        LICENSE

no changes added to commit (use "git add" and/or "git commit -a")
```

Git非常清楚地告诉我们，`readme.txt`被修改了，而`LICENSE`还从来没有被添加过，所以它的状态是`Untracked`

现在用git add 把readme.txt 和LICENSE,都添加，然后用 git status看下状态

```bash
cnsn@111-PC MINGW64 /d/learngit (master)
$ git add LICENSE

cnsn@111-PC MINGW64 /d/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   LICENSE

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme1.txt


cnsn@111-PC MINGW64 /d/learngit (master)
$ git add readme1.txt

cnsn@111-PC MINGW64 /d/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   LICENSE
        modified:   readme1.txt

```

git commit 可以一次性把暂存区所有修改提交到分支

```bash
$ git commit -m "understand how stage works"
[master e43a48b] understand how stage works
 2 files changed, 2 insertions(+)
 create mode 100644 LICENSE
 
$ git status
On branch master
nothing to commit, working tree clean
```



### 3.3 管理修改

**git跟踪并管理的是修改，而非文件。**

**测试：修改readme1.txt--添加 （git add）--查看（git status）修改readme.txt--提交（git commit）--查看（git status）**

```bash
# 查看
cnsn@111-PC MINGW64 /d/learngit (master)
$ cat readme1.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
test!
Git has a mutable index called stage

cnsn@111-PC MINGW64 /d/learngit (master)
# 修改后查看
$ cat readme1.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
test!
Git has a mutable index called stage.
Git tracks changes.

cnsn@111-PC MINGW64 /d/learngit (master)
# 添加
$ git add readme1.txt

cnsn@111-PC MINGW64 /d/learngit (master)
# 查看状态
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme1.txt

cnsn@111-PC MINGW64 /d/learngit (master)
# 修改后查看
$ cat readme1.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
test!
Git has a mutable index called stage.
Git tracks changes of files.

cnsn@111-PC MINGW64 /d/learngit (master)
# 提交
$ git commit -m "git tracks changes"
[master b551a18] git tracks changes
 1 file changed, 2 insertions(+), 1 deletion(-)

cnsn@111-PC MINGW64 /d/learngit (master)
# 查看状态
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme1.txt

no changes added to commit (use "git add" and/or "git commit -a")


```

为什么第二次的修改没有被提交？

答：没有添加到暂存区。

用`git diff HEAD -- readme.txt`命令可以查看工作区和版本库里面最新版本的区别：

```bash
$ git diff HEAD -- readme1.txt
diff --git a/readme1.txt b/readme1.txt
index db5bbb0..4412a00 100644
--- a/readme1.txt
+++ b/readme1.txt
@@ -2,4 +2,4 @@ Git is a distributed version control system.
 Git is free software distributed under the GPL.
 test!
 Git has a mutable index called stage.
-Git tracks changes.
\ No newline at end of file
+Git tracks changes of files.

```

### 3.4 撤销修改

#### 3.4.1 提交前发现错误：手工修改或者恢复到上一个版本的状态。

git restore <file>放弃工作目录中的修改

```bash
$ git status
On branch master                       
```

#### 3.4.2 提交后即已经到暂存区了。

git restore --staged <file> 可以把暂存区的修改撤销掉，重新返回工作区。

```bash
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   readme1.txt
# git restore --staged <file> 可以把暂存区的修改撤销掉，重新返回工作区。
$ git restore --staged readme1.txt

```



### 3.5 删除文件

在Git中，删除也是一个修改操作，我们实战一下，先添加一个新文件`test.txt`到Git并且提交：

```bash
cnsn@111-PC MINGW64 /d/learngit (master)
$ git add test.txt

cnsn@111-PC MINGW64 /d/learngit (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   test.txt

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   readme1.txt


cnsn@111-PC MINGW64 /d/learngit (master)
$ git commit -m "add test.txt"
[master 356bfd5] add test.txt
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 test.txt

```

已提交到git 。一般情况在直接在文件管理中把没用的文件删掉。用rm 删除后

Git知道你删除了文件，因此，工作区和版本库就不一致了，`git status`命令会立刻告诉你哪些文件被删除了：

```bash
$ rm test.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

有两个选择：

1. 一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

   ```bash
   $ git rm test.txt
   rm 'test.txt'
   $ git commit -m "remove test.txt"
   [master 0f604e1] remove test.txt
    1 file changed, 0 insertions(+), 0 deletions(-)
    delete mode 100644 test.txt
   ```

2. 删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

   ```bash
   # 删除文件
   $ rm test.txt
   # 查看状态
   $ git status
   On branch master
   Changes not staged for commit:
     (use "git add/rm <file>..." to update what will be committed)
     (use "git checkout -- <file>..." to discard changes in working directory)
   
           deleted:    test.txt
   
   no changes added to commit (use "git add" and/or "git commit -a")
   # 误删的文件恢复到最新
   $ git checkout -- test.txt
   ```

3. 如果真的把文件删了（即在git版本库中也删除了），可以通过日志还原回来。

## 4.远程仓库

### 4.1 添加远程仓库

现在的情景是，你已经在本地创建了一个Git仓库后，又想在GitHub创建一个Git仓库，并且让这两个仓库进行远程同步，这样，GitHub上的仓库既可以作为备份，又可以让其他人通过该仓库来协作，真是一举多得。

GitHub 创建仓库名叫 learnGit 仓库，过程（略）。

在GitHub上的这个`learnGit`仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

现在，我们根据GitHub的提示，在本地的`learngit`仓库下运行命令：

```bash
$ git remote add origin https://github.com/zaaaaa/learnGit.git
```

请千万注意，把上面的https 后面的替换成你自己的GitHub，否则，你在本地关联的就是我的远程库，关联没有问题，但是你以后推送是推不上去的，因为你的SSH Key公钥不在我的账户列表中。添加后，远程库的名字就是`origin`，这是Git默认的叫法，也可以改成别的，但是`origin`这个名字一看就知道是远程库。

下一步，就可以把本地库的所有内容推送到远程库上：

```bash
$ git push -u origin master
Username for 'https://github.com': zk_ang198@sina.com
Counting objects: 3, done.
Writing objects: 100% (3/3), 208 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/zkzhang1986/learnGit.git
 * [new branch]      master -> master
Branch master set up to track remote branch master from origin.

```

把本地库的内容推送到远程，用`git push`命令，实际上是把当前分支`master`推送到远程。

由于远程库是空的，我们第一次推送`master`分支时，加上了`-u`参数，Git不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令。

从现在起，只要本地作了提交，就可以通过命令：

```bash
$ git push origin master
```

把本地`master`分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

### 4.2 从远程库克隆

上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

首先，登陆GitHub，创建一个新的仓库，名字叫`gitskills`：

下一步是用命令`git clone`克隆一个本地库：

```bash
$ git clone https://github.com/zkzhang1986/gitskills.git
Cloning into 'gitskills'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
Checking connectivity... done.
$ cd gitskills
$ ls
README.md
```

GitHub给出的地址不止一个，还可以用`https://github.com/michaelliao/gitskills.git`这样的地址。实际上，Git支持多种协议，默认的`git://`使用ssh，但也可以使用`https`等其他协议。

使用`https`除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用`ssh`协议而只能用`https`。(我只会用`https`55555555555)

## 5.分支管理

### 5.1 创建与合并分支

在[版本回退](https://www.liaoxuefeng.com/wiki/896043488029600/897013573512192)里，你已经知道，每次提交，Git都把它们串成一条时间线，这条时间线就是一个分支。截止到目前，只有一条时间线，在Git里，这个分支叫主分支，即`master`分支。`HEAD`严格来说不是指向提交，而是指向`master`，`master`才是指向提交的，所以，`HEAD`指向的就是当前分支。

一开始的时候，`master`分支是一条线，Git用`master`指向最新的提交，再用`HEAD`指向`master`，就能确定当前分支，以及当前分支的提交点：

![git-br-initial](https://www.liaoxuefeng.com/files/attachments/919022325462368/0)

每次提交，`master`分支都会向前移动一步，这样，随着你不断提交，`master`分支的线也越来越长。

当我们创建新的分支，例如`dev`时，Git新建了一个指针叫`dev`，指向`master`相同的提交，再把`HEAD`指向`dev`，就表示当前分支在`dev`上：

![git-br-create](https://www.liaoxuefeng.com/files/attachments/919022363210080/l)

你看，Git创建一个分支很快，因为除了增加一个`dev`指针，改改`HEAD`的指向，工作区的文件都没有任何变化！

不过，从现在开始，对工作区的修改和提交就是针对`dev`分支了，比如新提交一次后，`dev`指针往前移动一步，而`master`指针不变：

![git-br-dev-fd](https://www.liaoxuefeng.com/files/attachments/919022387118368/l)

假如我们在`dev`上的工作完成了，就可以把`dev`合并到`master`上。Git怎么合并呢？最简单的方法，就是直接把`master`指向`dev`的当前提交，就完成了合并：

![git-br-ff-merge](https://www.liaoxuefeng.com/files/attachments/919022412005504/0)

所以Git合并分支也很快！就改改指针，工作区内容也不变！

合并完分支后，甚至可以删除`dev`分支。删除`dev`分支就是把`dev`指针给删掉，删掉后，我们就剩下了一条`master`分支：

![git-br-rm](https://www.liaoxuefeng.com/files/attachments/919022479428512/0)

真是太神奇了，你看得出来有些提交是通过分支完成的吗？

下面开始实战。

首先，我们创建`dev`分支，然后切换到`dev`分支：

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout`命令加上`-b`参数表示创建并切换，相当于以下两条命令：

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

然后，用`git branch`命令查看当前分支：

```
$ git branch
* dev
  master
```

`git branch`命令会列出所有分支，当前分支前面会标一个`*`号。

然后，我们就可以在`dev`分支上正常提交，比如对`readme.txt`做个修改，加上一行：

```bash
Creating a new branch is quick.
```

然后提交：

```bash
# 修改文件添加
$ git add readme.txt
# 查看状态
$ git status
On branch dev # 在分支 dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
# 修改文件提交
$ git commit -a -m"branch test"
[dev 3dfb87a] branch test
 1 file changed, 2 insertions(+), 1 deletion(-)
 # 查看dev日志
$ git log --pretty=oneline
3dfb87a1034828261349347f22aa1a638d0e256e branch test # 提交后的
a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
# 切换HEAD指向 master
$ git checkout master #可以用 git switch master
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
# 查看master日志
$ git log --pretty=oneline
a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT

```

现在，我们把`dev`分支的工作成果合并到`master`分支上：

```bash
# 把分支 dev 分支合并到 master
$ git merge dev
Updating a497770..3dfb87a
Fast-forward # 快进模式 
 readme.txt | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)
 # 查看 master 日志
 $ git log --pretty=oneline
3dfb87a1034828261349347f22aa1a638d0e256e branch test # 已经把分支dev 合并到master 分支
a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
```

注意到上面的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

当然，也不是每次合并都能`Fast-forward`，我们后面会讲其他方式的合并。

合并完成后，就可以放心地删除`dev`分支了：

```bash
$ git branch -d dev
Deleted branch dev (was 3dfb87a).
```

因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在`master`分支上工作效果是一样的，但过程更安全。

**switch**

我们注意到切换分支使用`git checkout <branch>`，而前面讲过的撤销修改则是`git checkout -- <file>`，同一个命令，有两种作用，确实有点令人迷惑。

实际上，切换分支这个动作，用`switch`更科学。因此，最新版本的Git提供了新的`git switch`命令来切换分支：

创建并切换到新的`dev`分支，可以使用：

```bash
$ git switch -c dev
```

直接切换到已有的`master`分支，可以使用：

```bash
$ git switch master
```

使用新的`git switch`命令，比`git checkout`要更容易理解。

### 5.2解决冲突

准备新的`feature1`分支，继续我们的新分支开发：

```bash
# 由于我版本比较旧。不支持switch命令。所有用branch、checkout 命令
$ git branch feature1
$ git branch
  feature1
* master
$ git checkout feature1
Switched to branch 'feature1'
$ git branch
* feature1
  master
```

修改`readme.txt`最后一行，改为：Creating a new branch is quick AND simple.

```bash
$ git add readme.txt
$ git commit readme.txt -m"AND simple"
[feature1 cba86da] AND simple
 1 file changed, 2 insertions(+), 1 deletion(-)
$ git branch
* feature1
  master
$ git log --pretty=oneline
cba86dafadf87ff0c0156f222cbff2698cfb59bf AND simple
3dfb87a1034828261349347f22aa1a638d0e256e branch test
a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
```

切换到master

```bash
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
$ git log --pretty=oneline
3dfb87a1034828261349347f22aa1a638d0e256e branch test
a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
```

Git还会自动提示我们当前`master`分支比远程的`master`分支要超前1个提交。

在`master`分支上把`readme.txt`文件的最后一行改为：Creating a new branch is quick & simple.

提交：

```bash
$ git add readme.txt
$ git commit readme.txt -m "Git (master)"
[master 3fa164a] Git (master)
 1 file changed, 1 insertion(+), 1 deletion(-)
$ git log --pretty=oneline
3fa164a3a65547f7a80528a253fe952c90d579fd Git (master)
3dfb87a1034828261349347f22aa1a638d0e256e branch test
a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
```

现在，`master`分支和`feature1`分支各自都分别有新的提交，变成了这样：

![git-br-feature1](https://www.liaoxuefeng.com/files/attachments/919023000423040/0)

这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突，我们试试看：

```bash
$ git merge feature1
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt
Automatic merge failed; fix conflicts and then commit the result.
```



果然冲突了！Git告诉我们，`readme.txt`文件存在冲突，必须手动解决冲突后再提交。`git status`也可以告诉我们冲突的文件：

```bash
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")

$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick.
Creating a new branch is quick AND simple.
>>>>>>> feature1
Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存：Creating a new branch is quick and simple.

$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick AND simple.

$ git add readme.txt

$ git commit -m "conflict fixed"
[master d989193] conflict fixed
```

现在，`master`分支和`feature1`分支变成了下图所示：

![git-br-conflict-merged](https://www.liaoxuefeng.com/files/attachments/919023031831104/0)

用带参数的`git log`也可以看到分支的合并情况：

```bash

$ git log --graph --pretty=oneline
*   d98919319c7d30b58733dd43db2adc15239e9c30 conflict fixed
|\
| * ff1274a884d7700d1308d408fb3a7b8593533dd2 AND simple
* | b67312d91575eb701ec70c8bb9c96d5e88015f38 & simple
|/
* 3dfb87a1034828261349347f22aa1a638d0e256e branch test
* a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
* 6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
* 206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
* 6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
* e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
```

最后，删除`feature1`分支：

```bash
$ git branch -D feature1
Deleted branch feature1 (was cba86da).
```



### 5.3 分支管理策略

通常，合并分支时，如果可能，Git会用`Fast forward`模式，但这种模式下，删除分支后，会丢掉分支信息。

如果要强制禁用`Fast forward`模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

下面我们实战一下`--no-ff`方式的`git merge`：

首先，仍然创建并切换`dev`分支：

```bash
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

修改readme.txt文件，并提交一个新的commit：

```bash
$ git add readme.txt 
$ git commit readme.txt -m "add merge"
[dev ff73b46] add merge
 1 file changed, 1 insertion(+)
```

现在，我们切换回`master`：

```bash
$ git checkout master
Switched to branch 'master'
```

准备合并`dev`分支，请注意`--no-ff`参数，表示禁用`Fast forward`：

```bash
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme.txt | 1 +
 1 file changed, 1 insertion(+)
```

因为本次合并要创建一个新的commit，所以加上`-m`参数，把commit描述写进去。

合并后，我们用`git log`看看分支历史：

```bash
$ git log --graph --pretty=oneline
*   d6ffa5e603b0e61b51e6673bfdf9e4de056110f3 merge with no-ff
|\
| * ff73b4698061087c834dd1c0b30d42611b0ff9e9 add merge
|/
*   1da8dfc5b4a1a6b31948053f4e6f0c9a040cd938 mergegit add readme.txt
|\
| * 2f1452e162786801cadc32bf627f488e3e41acd5 fetch1
* | 865064d181516c44c62fd705e5bd28c4d453e017 master
|/
*   d98919319c7d30b58733dd43db2adc15239e9c30 conflict fixed
|\
| * ff1274a884d7700d1308d408fb3a7b8593533dd2 AND simple
* | b67312d91575eb701ec70c8bb9c96d5e88015f38 & simple
|/
* 3dfb87a1034828261349347f22aa1a638d0e256e branch test
* a49777050286abb3a33dabcd446780522ca79951 crate readme1.txt
* 6e8f3ae286353db7a9a031a1bf46622c509ddb44 add this a test
* 206596d4252a7838db6a4199bd39ee37b5d875c3 crate a readme.txt
* 6379bd5ba45974a3c707645e7265efa139a0efda write testgit add test.txt!
* e70b703e51d3d49f9ebc19a06fcbe1aa86221071 Create a TXT
...
```

首先，`master`分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在`dev`分支上，也就是说，`dev`分支是不稳定的，到某个时候，比如1.0版本发布时，再把`dev`分支合并到`master`上，在`master`分支发布1.0版本；

你和你的小伙伴们每个人都在`dev`分支上干活，每个人都有自己的分支，时不时地往`dev`分支上合并就可以了。

所以，团队合作的分支看起来就像这样：

![git-br-policy](https://www.liaoxuefeng.com/files/attachments/919023260793600/0)

### 5.4 Bug分支

软件开发中，bug就像家常便饭一样。有了bug就需要修复，在Git中，由于分支是如此的强大，所以，每个bug都可以通过一个新的临时分支来修复，修复后，合并分支，然后将临时分支删除。

当你接到一个修复一个代号101的bug的任务时，很自然地，你想创建一个分支`issue-101`来修复它，但是，等等，当前正在`dev`上进行的工作还没有提交

并不是你不想提交，而是工作只进行到一半，还没法提交，预计完成还需1天时间。但是，必须在两个小时内修复该bug，怎么办？

幸好，Git还提供了一个`stash`功能，可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作：

```bash
$ git stash
Saved working directory and index state WIP on dev: 8e5b335 issue-101-102
HEAD is now at 8e5b335 issue-101-102
```

现在，用`git status`查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在`master`分支上修复，就从`master`创建临时分支：

略

现在，是时候接着回到`dev`分支干活了！切换到dev分支

```bash
$ git checkout dev
Switched to branch 'dev'
```

工作区是干净的，刚才的工作现场存到哪去了？用`git stash list`命令看看：

```bash
$ git stash list
stash@{0}: WIP on dev: 8e5b335 issue-101-102
```

工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除；

另一种方式是用`git stash pop`，恢复的同时把stash内容也删了：

```bash
$ git stash pop
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   hello.py

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

Dropped refs/stash@{0} (5d677e2ee266f39ea296182fb2354265b91b3b2a)
```

再用`git stash list`查看，就看不到任何stash内容了：

```bash
$ git stash list
```

你可以多次stash，恢复的时候，先用`git stash list`查看，然后恢复指定的stash，用命令：

```bash
$ git stash apply stash@{0}
```

**复制修改了内容。前提：需要`git stash`命令保存现场，再 git cherry-pick 4c805e2 再恢复现场**

在master分支上修复了bug后，我们要想一想，dev分支是早期从master分支分出来的，所以，这个bug其实在当前dev分支上也存在。

那怎么在dev分支上修复同样的bug？重复操作一次，提交不就行了？

有木有更简单的方法？

有！

同样的bug，要在dev上修复，我们只需要把`4c805e2 fix bug 101`这个提交所做的修改“复制”到dev分支。注意：我们只想复制`4c805e2 fix bug 101`这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个`cherry-pick`命令，让我们能复制一个特定的提交到当前分支：

```
$ git branch
* dev
  master
$ git cherry-pick 4c805e2
[master 1d4b803] fix bug 101
 1 file changed, 1 insertion(+), 1 deletion(-)
```

Git自动给dev分支做了一次提交，注意这次提交的commit是`1d4b803`，它并不同于master的`4c805e2`，因为这两个commit只是改动相同，但确实是两个不同的commit。用`git cherry-pick`，我们就不需要在dev分支上手动再把修bug的过程重复一遍。

有些聪明的童鞋会想了，既然可以在master分支上修复bug后，在dev分支上可以“重放”这个修复过程，那么直接在dev分支上修复bug，然后在master分支上“重放”行不行？当然可以，不过你仍然需要`git stash`命令保存现场，才能从dev分支切换到master分支。

例：

```bash
$ git stash
Saved working directory and index state WIP on dev: 8e5b335 issue-101-102
HEAD is now at 8e5b335 issue-101-102

$ git cherry-pick b924e9b90359e86b1e7fa85bf92ff0e7cc97ef5
[dev d6ddf26] issue-103
 Date: Thu Sep 17 14:32:40 2020 +0800
 1 file changed, 1 insertion(+), 1 deletion(-)

$ git stash list
stash@{0}: WIP on dev: 8e5b335 issue-101-102
stash@{1}: WIP on dev: 8e5b335 issue-101-102
stash@{2}: WIP on dev: 8e5b335 issue-101-102

$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick AND simple.
master and fetch1
add merge
add fix bug(issue-102).
Fix bugs(issue-101).
Fix bugs(issue-103).
zzk@zzk-PC MINGW64 /d/learngit (dev)
$ git stash apply stash@{2}
Auto-merging readme.txt
CONFLICT (content): Merge conflict in readme.txt

$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files.
Creating a new branch is quick AND simple.
master and fetch1
add merge
add fix bug(issue-102).
Fix bugs(issue-101).
<<<<<<< Updated upstream
Fix bugs(issue-103).
=======
dev abc...
>>>>>>> Stashed changes

```

### 5.5 Feature分支

添加一个新功能时，你肯定不希望因为一些实验性质的代码，把主分支搞乱了，所以，每添加一个新功能，最好新建一个feature分支，在上面开发，完成后，合并，最后，删除该feature分支。

### 5.6 多人协作

当你从远程仓库克隆时，实际上Git自动把本地的`master`分支和远程的`master`分支对应起来了，并且，远程仓库的默认名称是`origin`。

要查看远程库的信息，用`git remote`：

```bash
$ git remote
origin
```

或者，用`git remote -v`显示更详细的信息：

```bash
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
```

上面显示了可以抓取和推送的`origin`的地址。如果没有推送权限，就看不到push的地址。

#### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上：

```bash
$ git push origin master
```

如果要推送其他分支，比如`dev`，就改成：

```bash
$ git push origin dev
```

但是，并不是一定要把本地分支往远程推送，那么，哪些分支需要推送，哪些不需要呢？

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

总之，就是在Git中，分支完全可以在本地自己藏着玩，是否推送，视你的心情而定！

#### 抓取分支

多人协作时，大家都会往`master`和`dev`分支上推送各自的修改。

现在，模拟一个你的小伙伴，可以在另一台电脑（注意要把SSH Key添加到GitHub）或者同一台电脑的另一个目录下克隆：

```bash
$ git clone git@github.com:michaelliao/learngit.git
Cloning into 'learngit'...
remote: Counting objects: 40, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 40 (delta 14), reused 40 (delta 14), pack-reused 0
Receiving objects: 100% (40/40), done.
Resolving deltas: 100% (14/14), done.
```

当你的小伙伴从远程库clone时，默认情况下，你的小伙伴只能看到本地的`master`分支。不信可以用`git branch`命令看看：

```bash
$ git branch
* master
```

现在，你的小伙伴要在`dev`分支上开发，就必须创建远程`origin`的`dev`分支到本地，于是他用这个命令创建本地`dev`分支：

```bash
$ git checkout -b dev origin/dev
```

现在，他就可以在`dev`上继续修改，然后，时不时地把`dev`分支`push`到远程：

```bash
$ git add env.txt

$ git commit -m "add env"
[dev 7a5e5dd] add env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
Counting objects: 3, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 308 bytes | 308.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   f52c633..7a5e5dd  dev -> dev
```



你的小伙伴已经向`origin/dev`分支推送了他的提交，而碰巧你也对同样的文件作了修改，并试图推送：

```bash
$ cat env.txt
env

$ git add env.txt

$ git commit -m "add new env"
[dev 7bd91f1] add new env
 1 file changed, 1 insertion(+)
 create mode 100644 env.txt

$ git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```bash
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

`git pull`也失败了，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接：

```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

再pull：

```bash
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

这回`git pull`成功，但是合并有冲突，需要手动解决，解决的方法和分支管理中的[解决冲突](http://www.liaoxuefeng.com/wiki/896043488029600/900004111093344)完全一样。解决后，提交，再push：

```bash
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```



因此，多人协作的工作模式通常是这样：

1. 首先，可以试图用`git push origin <branch-name>`推送自己的修改；
2. 如果推送失败，则因为远程分支比你的本地更新，需要先用`git pull`试图合并；
3. 如果合并有冲突，则解决冲突，并在本地提交；
4. 没有冲突或者解决掉冲突后，再用`git push origin <branch-name>`推送就能成功！

如果`git pull`提示`no tracking information`，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream-to <branch-name> origin/<branch-name>`。

这就是多人协作的工作模式，一旦熟悉了，就非常简单。

#### 小结

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

### 5.7 Rebase

略

## 6.标签管理

略

## 7.使用GitHub

**见5.6多人协作**



