Android下的配置管理之道之git的使用

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/


    【马哥私房菜】亲情推出《linux shell脚本攻略》视频教程

    【马哥私房菜】亲情推出 git 视频教程



# git help
git help 命令用来显示任何命令的 Git 自带文档。     
对于每一个命令的完整的可选项及标志列表，你可以随时运行 git help <command> 命令来了解。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git config
/etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。
``` 
git config --system
``` 
~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。
``` 
git config --global 
``` 
当前项目的 git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。
```bash
git config --local

```
每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖~/.gitconfig, /etc/gitconfig 中的同名变量。 

```bash
$ git config --global user.name "马哥私房菜"
$ git config --global user.email bright.ma@mage.com
$ git config --global core.editor vim
$ git config --global color.ui true

```
最后在~/.gitconfgi 文件下内容如下了
```
[color]
    ui = true
[user]
    email = bright.ma@mage.com
    name = 马哥私房菜
[core]
    editor = vim

    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
这种git的配置文件我们也可以直接使用编辑器取编辑修改。  
配置文件中的空行被忽略。井号，分号开头的行是注释行，被忽略的。

配置文件中的数据类型
```
Type
    --bool                value is "true" or "false"
    --int                 value is decimal number
    --bool-or-int         value is --bool or --int
    --path                value is a path (file or directory name)

```

列出所有配置信息 可以使用 git config --list 命令
```bash
$ git config --list --local
user.email=<https://shop592330910.taobao.com/>
user.name=马哥私房菜
core.repositoryformatversion=0
core.filemode=true
core.bare=false
core.logallrefupdates=true
remote.origin.url=https://github.com/mageSFC/myblog.git
remote.origin.fetch=+refs/heads/*:refs/remotes/origin/*
branch.master.remote=origin
branch.master.merge=refs/heads/master


```

指定使用某个配置文件，可以使用-f，--file选项,该选项和--system,--local,--global不能同时使用的
```bash
git config -f some_to_path/config --list
```

下面再举个例子
```text
#Given a .git/config like this:

#
# This is the config file, and
# a '#' or ';' character indicates
# a comment
#

; core variables
[core]
    ; Don't trust file modes
    filemode = false

; Our diff algorithm
[diff]
    external = /usr/local/bin/diff-wrapper
    renames = true

; Proxy settings
[core]
    gitproxy=proxy-command for kernel.org
    gitproxy=default-proxy ; for all the rest

; HTTP
[http]
    sslVerify
[http "https://weak.example.com"]
    sslVerify = false
    cookieFile = /tmp/cookie.txt
```

设置filemode为true：
```
git config core.filemode true
```
删除diff下面的renames
```
git config --unset diff.renames
```
查询某个给定的key的value
```
git config --get core.filemode

git config       core.filemode
```



# git init

```
git init        # 在一个空目录下面初始化一个空的仓库。
                # 在存在的仓库下会reinit一个仓库的。作用不大。
```
```
git init --bare # 建一个裸仓库
git init --bare my.git
一般的裸仓库名称我们习惯在目录名上带上.git结尾。
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
```
git init --shared[=(false|true|umask|group|all|world|everybody|0xxx)]

git init --shared=everybody

git init --shared=777 # 这样个出来的.git目录权限就会是777.
```

```
git init --template=<template_directory> #这个是指定一个模板路径。默认的路径是/usr/share/git-core/templates.

```

```bash
git init --separate-git-dir=<git dir> #这个选项用来分开存放.git目录的。

git init my  --separate-git-dir my.git # 这样初始化一个空的仓库，名字是my，my这个目录也就是我们的工作区间。
但是真正的仓库是在my.git这个目录下面的。 同时在my工作区间目录下面不会再有.git目录了，代替的一个.git文件。

$ git init my --separate-git-dir=my.git
Initialized empty Git repository in /home/mamh/tmp/my.git/

$ ls 
 my/  my.git/  test/

$ ll my                                                                                                     
total 0
$ ls -lha my
total 12K
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 ./
drwxrwxr-x 7 mamh mamh 4.0K 6月   1 08:42 ../
-rw-rw-r-- 1 mamh mamh   30 6月   1 08:42 .git
$ cat my/.git
gitdir: /home/mamh/tmp/my.git

$ ll my.git                                                                                                 
total 32K
-rw-rw-r-- 1 mamh mamh   23 6月   1 08:42 HEAD
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 branches/
-rw-rw-r-- 1 mamh mamh   92 6月   1 08:42 config
-rw-rw-r-- 1 mamh mamh   73 6月   1 08:42 description
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 hooks/
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:42 info/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:42 objects/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:42 refs/
$                                                 
```
```bash
git init --separate-git-dir=../my.git  在一个已经存在的git仓库下面执行这样的命令，可以把当前仓库的.git目录移到外面某个目录下面。

如果是对一个裸仓库执行这样的命令，他也会把仓库移到指定的目录下面，但是原来的仓库好像没什么变化的。
```

环境变量 GIT_DIR   和   GIT_WORK_TREE
```bash
#  如果设置了这个$GIT_DIR环境变量，默认初始化时候是带当前目录下面创建的.git目录，有了这个环境变量， .git目录下面的内容就会放到这个环境变量路径下面了。

export GIT_DIR=/home/mamh/tmp/aaa.git
$ git init bbb 
Initialized empty Git repository in /home/mamh/tmp/aaa.git/ 
$ ls -lh 
total 575M
drwxrwxr-x  7 mamh mamh 4.0K 6月   1 08:54 aaa.git/
drwxrwxr-x  2 mamh mamh 4.0K 6月   1 08:54 bbb/

$ ls -lha aaa.git bbb
aaa.git:
total 40K
drwxrwxr-x 7 mamh mamh 4.0K 6月   1 08:54 ./
drwxrwxr-x 6 mamh mamh 4.0K 6月   1 08:54 ../
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 branches/
-rw-rw-r-- 1 mamh mamh   66 6月   1 08:54 config
-rw-rw-r-- 1 mamh mamh   73 6月   1 08:54 description
-rw-rw-r-- 1 mamh mamh   23 6月   1 08:54 HEAD
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 hooks/
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 info/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:54 objects/
drwxrwxr-x 4 mamh mamh 4.0K 6月   1 08:54 refs/

bbb:
total 8.0K
drwxrwxr-x 2 mamh mamh 4.0K 6月   1 08:54 ./
drwxrwxr-x 6 mamh mamh 4.0K 6月   1 08:54 ../
通过上面的初始化方法，我初始化了一个bbb的仓库，通过环境变量GIT_DIR指定仓库的路径，这里是aaa.git，然后我们发现bbb目录是个空目录。下面什么文件都没的。这里我们的bbb目录应该是一个工作区间目录的。
$ git status 
fatal: This operation must be run in a work tree
$ git diff 
fatal: This operation must be run in a work tree
这个时候你执行一些命令会报不是个工作目录的。这个时候我们可以使用另外一个环境变量来解决GIT_WORK_TREE

$ export GIT_WORK_TREE=/home/mamh/tmp/bbb 
$ git add .                   
$ git st                       
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   t.txt
	
这个时候git就能正常的识别到哪个是仓库目录，哪个是工作目录了。

一般情况很少这样使用的。


```
选项 --work-tree 和   --git-dir 和上面的环境变量起到了同样的作用。
```
git --work-tree=<some path> --git-dir=<some path> 

git --work-tree=/home/mamh/tmp/bbb --git-dir=/home/mamh/tmp/aaa.git   status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

	new file:   t.txt
```



# git clone

```
git clone user@git.example.com:/opt/git/my_project.git

git clone --bare https://github.com/mageSFC/my_project my_project.git

git clone https://github.com/mageSFC/myblog.git

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

--local选项，--no-hardlinks选项
```bash
git clone --local /home/mirror/repo.git r1
--local，-l 选项，这个会在克隆一个本地路径的仓库时候 它会把  .git/objects/ 下面的文件做成硬链接的形式。（尽可能的，如果不能做成硬链接，他就会是复制的形式了）

这个只有在路径是个本地路径的情况下面才有用的。如果是ssh://, http://这样的远端仓库路径就不会使用硬链接了。

--no-local 和上面的意思相反，他不会使用硬链接的。

$ git clone --local /home/mirror/repo.git 
Cloning into 'repo'...
done.

$ ll repo/.git/objects/pack 
total 924K
-r--r--r-- 2 mamh mamh 102K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.idx
-r--r--r-- 2 mamh mamh 818K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.pack

$ ll repo/.git/objects/pack -i    
total 924K
57278473 -r--r--r-- 2 mamh mamh 102K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.idx
57278469 -r--r--r-- 2 mamh mamh 818K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.pack

$ ll /home/mirror/repo.git/objects/pack -i  
total 924K
57278473 -r--r--r-- 2 mamh mamh 102K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.idx
57278469 -r--r--r-- 2 mamh mamh 818K 11月 28  2017 pack-b33f3eb5a3197675040d0e89cf2842ce644a973f.pack

通过上面的--local我们看到文件的i节点编号是一样的，说明我们克隆的时候采用了硬链接的方式。

默认不加上--local，git自行判断，也会优化使用硬链接的方式的。

git clone --no-hardlinks      -- copy files instead of hardlinking when doing a local clone 也是不采用硬链接的方式。 

git clone --no-hardlinks   /home/mirror/repo.git repo_bak #一般的这个使用情况是，你想备份一个仓库，就可以使用这种不带硬链接的克隆了。

```

--bare和--mirror的区别
```bash
git clone --mirror https://github.com/mageSFC/myblog.git  myblog.git   #服务器上面会这么来克隆一个裸仓库，服务器上引用第三方仓库。

git clone --bare https://github.com/mageSFC/my_project my_project.git   # 这个选项仅仅是复制克隆一个裸仓库。

--bare和--mirror的区别是

$ git clone --mirror /home/mirror/repo.git r1
$ git clone --bare /home/mirror/repo.git r2

$ ls r1 r2                                                                                                                                     
r1:
branches/  config  description  HEAD  hooks/  info/  objects/  packed-refs  refs/

r2:
branches/  config  description  HEAD  hooks/  info/  objects/  packed-refs  refs/
从表面上看，这两种方式克隆下面的都是裸仓库，没上面区别。但是我们看看他们的config文件就知道了
$ cat r1/config    
[core]
	repositoryformatversion = 0
	filemode = true
	bare = true
[remote "origin"]
	url = /home/mirror/repo.git
	fetch = +refs/*:refs/*
	mirror = true
 
$ cat r2/config   
[core]
	repositoryformatversion = 0
	filemode = true
	bare = true
[remote "origin"]
	url = /home/mirror/repo.git

我们可以看到--mirror的配置文件上是多了这个“fetch = +refs/*:refs/*”的。
这个说明我们可以执行git fetch来继续获取上游仓库的更新。没有这个是不能继续更新的。所以--mirror更常用一些。



```

--reference 使用参考仓库来克隆仓库
```bash
git clone https://github.com/mageSFC/myblog.git --reference <本地仓库镜像库路径>

git clone https://github.com/mageSFC/myblog.git --reference         /home/mirror/myblog.git
git clone https://github.com/mageSFC/myblog.git --reference-if-able /home/mirror/myblog.git

如果参考的本地镜像仓库不存在--reference  选项会报错的。    --reference-if-able不会报错，会继续克隆的。

这个参考的仓库要是一个本地的裸仓库的。加上这个选项会节省克隆时间，节省磁盘空间的。


$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 --reference /home/mirror/kernel/msm-4.9.git                             
Cloning into 'msm-4.9'...
remote: Counting objects: 3064590, done
remote: Finding sources: 100% (3012366/3012366)
remote: Total 3012366 (delta 2393732), reused 2939537 (delta 2393732)
Receiving objects: 100% (3012366/3012366), 1.36 GiB | 11.24 MiB/s, done.
Resolving deltas: 100% (2393732/2393732), completed with 9850 local objects.
Checking connectivity: 8230985, done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.

$ du -sh msm-4.9/.git  
1.5G	msm-4.9/.git
$ cat msm-4.9/.git/objects/info/alternates  
/home/mirror/kernel/msm-4.9.git/objects
使用了 --reference 选项克隆后.git目录只有1.5G大小。
使用了 --reference 选项的克隆，.git/objects/info/alternates文件里面会指向镜像仓库的路径。


$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 
Cloning into 'msm-4.9'...
remote: Counting objects: 8230985, done
remote: Finding sources: 100% (8230985/8230985)
remote: Total 8230985 (delta 6720180), reused 8140778 (delta 6720180)
Receiving objects: 100% (8230985/8230985), 2.70 GiB | 18.83 MiB/s, done.
Resolving deltas: 100% (6720180/6720180), done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.
$ du -sh msm-4.9/.git  
3.0G	msm-4.9/.git
$ cat msm-4.9/.git/objects/info/alternates 
cat: msm-4.9/.git/objects/info/alternates: No such file or directory

通过比较加不加 --reference /home/mirror/kernel/msm-4.9.git 我们看到.git目录大小不一样，加上要小很多的。
然后就是加上会有msm-4.9/.git/objects/info/alternates这个文件的。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 --reference /home/mirror/msm-4.9.git                                 
Cloning into 'msm-4.9'...
fatal: reference repository '/home/mirror/msm-4.9.git' is not a local repository.
如果这个本地镜像仓库路径不存在，会报错的。

如果使用了--reference-if-able不会报错的。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/kernel/msm-4.9 --reference /home/mirror/kernel/msm-4.9.git --dissociate  
Cloning into 'msm-4.9'...
remote: Counting objects: 3064590, done
remote: Finding sources: 100% (3012366/3012366)
remote: Total 3012366 (delta 2393732), reused 2939537 (delta 2393732)
Receiving objects: 100% (3012366/3012366), 1.36 GiB | 16.33 MiB/s, done.
Resolving deltas: 100% (2393732/2393732), completed with 9850 local objects.
Checking connectivity: 8230985, done.
Counting objects: 8230985, done.
Compressing objects: 100% (2540121/2540121), done.
Writing objects: 100% (8230985/8230985), done.
Total 8230985 (delta 5970312), reused 6375489 (delta 4914488)
warning: remote HEAD refers to nonexistent ref, unable to checkout.

$ du -sh msm-4.9/.git  
3.7G	msm-4.9/.git

$ cat msm-4.9/.git/objects/info/alternates
cat: msm-4.9/.git/objects/info/alternates: No such file or directory

--dissociate选项：
如果同时使用这个选项和--reference 选项，克隆的时候也会参考镜像仓库的，也是会提高克隆速度的。
但是不会创建 .git/objects/info/alternates 文件的。 “.git”目录的大小会变大的。

这个--reference选项对于特别大的仓库很有用。对于多人在一台服务器上克隆然后开发的情况也是很有用的。
这个镜像仓库一般可以使用git clone --mirror来创建。

```


--branch <name>, -b <name>选项  和 --single-branch选项
```bash
在不使用--branch选项的时候git clone的时候检出的分支是HEAD指向的那个分支。
使用这个选项我们就可以在克隆时候检出某个指定的分支了。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo 
Cloning into 'repo'...
remote: Counting objects: 3990, done
remote: Finding sources: 100% (3990/3990)
remote: Total 3990 (delta 2671), reused 3990 (delta 2671)
Receiving objects: 100% (3990/3990), 1.23 MiB | 18.55 MiB/s, done.
Resolving deltas: 100% (2671/2671), done.
$ cd repo                                
$ git branch -a                                                                                      
* stable
  remotes/origin/HEAD -> origin/stable
  remotes/origin/maint
  remotes/origin/master
  remotes/origin/stable

下面我们使用了-b选项，指定了检出maint分支。
$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo -b maint 
Cloning into 'repo'...
remote: Counting objects: 3990, done
remote: Finding sources: 100% (3990/3990)
remote: Total 3990 (delta 2671), reused 3990 (delta 2671)
Receiving objects: 100% (3990/3990), 1.23 MiB | 26.84 MiB/s, done.
Resolving deltas: 100% (2671/2671), done.
$ cd repo                                                                                                                                
$ git branch -a 
* maint          这里说明了检出的分支是maint分支。
  remotes/origin/HEAD -> origin/stable
  remotes/origin/maint
  remotes/origin/master
  remotes/origin/stable

--single-branch选项什么意思呢？ 它和-b选项配合使用，让克隆的时候只下载-b指定的这一个分支。
在上面的例子我们看到了克隆的时候git会把所有分支都下载到本地的，例如有maint，master，stable，这3个分支都下载了。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo -b maint --single-branch 
Cloning into 'repo'...
remote: Counting objects: 1668, done
remote: Finding sources: 100% (1764/1764)
remote: Total 1764 (delta 1117), reused 1700 (delta 1117)
Receiving objects: 100% (1764/1764), 716.75 KiB | 7.47 MiB/s, done.
Resolving deltas: 100% (1117/1117), done.
$ cd repo                    
$ git branch -a
* maint
  remotes/origin/maint
$  
这个时候我们看到只有maint分支了。其他分支都没有下载。
$ cat .git/config 
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo
	fetch = +refs/heads/maint:refs/remotes/origin/maint
[branch "maint"]
	remote = origin
	merge = refs/heads/maint
通过.git/config 配置文件我们可以看到 fetch = +refs/heads/maint:refs/remotes/origin/maint 这么一行，它就是指定了只下载服务器上的maint分支到本地。
之前的这一行样子是这个的： fetch = +refs/heads/*:refs/remotes/origin/*，星号代表所有，refs/heads/*也是所有分支的意思。
你要是想再下载其他分支，可以在那个fetch行下面加上一行  fetch = +refs/heads/stable:refs/remotes/origin/stable 。


这个-b和--single-branch的一个使用场景是：
在一个仓库下面，我们有个分支源码分支bsui，
另一个是二进制分支bsui_apk.二进制分支都是放的源码分支编译生成的apk文件的，这个分支很大，很占磁盘空间的。
对于研发人员来说，他只会关系bsui源码分支，下载的时候也是只想下载这个bsui分支。另外一个分支是没什么用的。
这个时候就可以使用 --branch bsui --single-branch 这2个选项了。


$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService                                      [mamh@10.0.63.43 ] 18-06-01 11:25  /home/mamh/tmp
Cloning into 'I19tService'...
remote: Counting objects: 6884, done
remote: Finding sources: 100% (6884/6884)
remote: Total 6884 (delta 3550), reused 6698 (delta 3550)
Receiving objects: 100% (6884/6884), 6.68 GiB | 47.00 MiB/s, done.
Resolving deltas: 100% (3550/3550), done.
warning: remote HEAD refers to nonexistent ref, unable to checkout.
$ du -sh I19tService/.git                                                                                                                    [mamh@10.0.63.43 ] 18-06-01 11:28  /home/mamh/tmp
6.7G	I19tService/.git

下面是使用了--branch bsui --single-branch选项的效果，磁盘空间只有600MB。之前是6GB。这就是差异。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch        [mamh@10.0.63.43 ] 18-06-01 11:28  /home/mamh/tmp
Cloning into 'I19tService'...
remote: Counting objects: 5995, done
remote: Finding sources: 100% (5995/5995)
remote: Total 5995 (delta 2897), reused 5604 (delta 2897)
Receiving objects: 100% (5995/5995), 663.97 MiB | 50.77 MiB/s, done.
Resolving deltas: 100% (2897/2897), done.
$ du -sh I19tService/.git                                                                                                                    [mamh@10.0.63.43 ] 18-06-01 11:29  /home/mamh/tmp
665M	I19tService/.git

如果使用了--reference .git目录大小会更小。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git             
Cloning into 'I19tService'...
$ du -sh I19tService/.git                                                                                                                    [mamh@10.0.63.43 ] 18-06-01 11:33  /home/mamh/tmp
164K	I19tService/.git

如果使用了--dissociate和 --reference 选项，目录大小恢复为600MB了。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git  --dissociate
Cloning into 'I19tService'...
Counting objects: 5995, done.
Compressing objects: 100% (3211/3211), done.
Writing objects: 100% (5995/5995), done.
Total 5995 (delta 2519), reused 4618 (delta 1722)
$ du -sh I19tService/.git                                                                                                                    [mamh@10.0.63.43 ] 18-06-01 11:32  /home/mamh/tmp
646M	I19tService/.git



```

--depth <depth>浅克隆
```bash
git clone --depth 3  /home/mirror/repo.git
使用这个选项是，浅克隆的意思，后面指定一个数字，下载的时候只下载指定的提交历史。

$ git clone ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo --depth 3 
Cloning into 'repo'...
remote: Counting objects: 87, done
remote: Finding sources: 100% (90/90)
remote: Total 90 (delta 7), reused 45 (delta 7)
Receiving objects: 100% (90/90), 164.53 KiB | 4.22 MiB/s, done.
Resolving deltas: 100% (7/7), done.
$ cd repo                                                                                                                                    
$ git ll                                                                                                                                
* eceeb1b - Support broken symlinks when cleaning obsolete paths                      — Dan Willemsen (HEAD -> stable, tag: v1.12.37, origin/stable, origin/HEAD) - (1 year, 8 months ago)
* 16889ba - Revert "Repo: fall back to http, if ssh connection fails for http repos"  — Dan Willemsen (tag: v1.12.36) - (1 year, 8 months ago)
*   40d3952 - Merge "On project cleanup, don't remove nested projects"                — David Pursehouse (tag: v1.12.35) - (1 year, 8 months ago)
|\  
| * 4350791 - On project cleanup, don't remove nested projects                       — Dan Willemsen (grafted) - (1 year, 9 months ago)
* d648045 - implement optional 'pushurl' in the manifest file                        — Steve Rae (grafted) - (1 year, 10 months ago)
$
加上了--depth选项，这个git log历史只有这么4个,或者说是5个了。

$ cat .git/config                                                                                                                       [mamh@10.0.63.43 ] 18-06-01 10:57  /home/mamh/tmp/repo
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://gerrit.blackshark.com:29418/git/aosp/tools/repo
	fetch = +refs/heads/stable:refs/remotes/origin/stable
[branch "stable"]
	remote = origin
	merge = refs/heads/stable
加上这个--depth，克隆的时候只会下载到一个分支，这里是stable，应为服务器上配置的HEAD执向的是stable分支。

$ ls .git                                                                                                                               [mamh@10.0.63.43 ] 18-06-01 10:57  /home/mamh/tmp/repo
branches/  config  description  HEAD  hooks/  index  info/  logs/  objects/  packed-refs  refs/  shallow
在浅克隆的时候.git目录下面会有个shallow文件



```


--origin <name>, -o <name>选项
```bash
默认克隆的时候，使用的名称是origin，我们可以通过.git/config 看到
$ cat I19tService/.git/config                                                                                                                [mamh@10.0.63.43 ] 18-06-01 11:36  /home/mamh/tmp
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "origin"]
	url = ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService
	fetch = +refs/heads/bsui:refs/remotes/origin/bsui
[branch "bsui"]
	remote = origin
	merge = refs/heads/bsui
	
这里[remote "origin"]标记的就是远端仓库名称是origin。

使用--origin选项在克隆的时候可以更改这个名称，一般不会取改它的。
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git   --origin o
fatal: destination path 'I19tService' already exists and is not an empty directory.
$ rm -rf I19tService                                                                                                                         [mamh@10.0.63.43 ] 18-06-01 11:38  /home/mamh/tmp
$ git clone ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService --branch bsui --single-branch --reference /home/mirror/platform/vendor/zeusis/zui/app/I19tService.git   --origin o
Cloning into 'I19tService'...
$ cat I19tService/.git/config                                                                                                                [mamh@10.0.63.43 ] 18-06-01 11:38  /home/mamh/tmp
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
[remote "o"]
	url = ssh://gerrit-sh.blackshark.com:29418/git/android/platform/vendor/zeusis/zui/app/I19tService
	fetch = +refs/heads/bsui:refs/remotes/o/bsui
[branch "bsui"]
	remote = o
	merge = refs/heads/bsui
这里就使用了  --origin o选项，把这个名称改为了“o”
```


--no-checkout选项
```bash
加上这个选项 clone的时候不会把源码检出到工作目录。默认是会检出的。有时候会提示个warning，就是检出源码失败。
 warning: remote HEAD refers to nonexistent ref, unable to checkout.
一般这种情况是服务器端HEAD引用的那个分支是个不存在的分支，下载到本地检出代码的时候就报错了。

$ git clone /home/mirror/kernel/msm-4.9.git --no-checkout                                                                                    
Cloning into 'msm-4.9'...
done.
$ ls msm-4.9                                                                                                                                 

$ rm -rf msm-4.9                                                                                                                             
$ git clone /home/mirror/kernel/msm-4.9.git                                                                                                  
Cloning into 'msm-4.9'...
done.
Checking out files: 100% (58866/58866), done.
$ ls msm-4.9                                                                                                                                 [mamh@10.0.63.43 ] 18-06-01 10:47  /home/mamh/tmp
AndroidKernel.mk  build.config.goldfish.arm    build.config.goldfish.mips64  certs/   crypto/         firmware/  init/   Kconfig  MAINTAINERS  net/            samples/   sound/     usr/
arch/             build.config.goldfish.arm64  build.config.goldfish.x86     COPYING  Documentation/  fs/        ipc/    kernel/  Makefile     README          scripts/   techpack/  virt/
block/            build.config.goldfish.mips   build.config.goldfish.x86_64  CREDITS  drivers/        include/   Kbuild  lib/     mm/          REPORTING-BUGS  security/  tools/
$  
```

--template=<template_directory>选项
```bash
git clone --template=<template_directory>
--template=<template_directory>选项指定一个模板路径，克隆的时候使用。我们在git init时候讲过这个选项。

$ git clone --template=/usr/share/git-core/templates /home/mirror/kernel/msm-4.9.git

```

--no-tags选项
```bash
git clone  --no-tags /home/mirror/repo.git
克隆的时候不下载tags标签，使用这个选项会在.git/config 配置文件中有个这个配置：remote.<remote>.tagOpt=--no-tags 。
不使用这个选项是会下载标签的。      

```

-v,-q,--progress选项
```bash
git clone -v, --verbose 克隆的时候显示更多的详细信息。
git clone -q, --quiet 克隆时候不显示任何提示信息。
git clone --progress 强制显示进度报告。

```












# git add
git add 命令将内容从工作目录添加到暂存区（或称为索引（index）区），以备下次提交。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git status

git status 命令将为你展示工作区及暂存区域中不同状态的文件。 这其中包含了已修改但未暂存，或已经暂存但没有提交的文件。  
一般在它显示形式中，会给你展示一些关于如何在这些暂存区域之间移动文件的提示。  

红色表示修改了。  

绿色的文件表示执行过git add之后的状态  

 -s, --short 选项，显示简短格式的状态  
 --long 选项，默认选项，就是长格式的状态。  

# git diff

此命令可以查看你工作环境与你的暂存区的差异（git diff 默认的做法）  

你暂存区域与你最后提交之间的差异（git diff --staged ）  
或者比较两个提交记录的差异（git diff master branchB）  

--staged 和 --cached 是同义词  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git difftool

当你不想使用内置的 git diff 命令时。git difftool 可以用来简单地启动一个外部工具来为你展示两棵树之间的差异。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git commit

git commit 命令将所有通过 git add 暂存的文件内容在数据库中创建一个持久的快照，然后将当前分支上的分支指针移到其之上。  

git commit --amend  追加提交到最后一个快照上。  
-m 选项，将提交信息与命令放在同一行  
-a 选项, 跳过使用git add命令，直接把修改过后的文件直接作为一个提交  
-s, --signoff 添加signoff行  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  


# git rm
git rm 是 Git 用来从工作区，或者暂存区移除文件的命令。 在为下一次提交暂存一个移除操作上，它与 git add 有一点类似  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git mv
git mv 命令是一个便利命令，用于移到一个文件并且在新文件上执行git add命令及在老文件上执行git rm命令。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git clean
git clean 是一个用来从工作区中移除不想要的文件的命令。 可以是编译的临时文件或者合并冲突的文件。  
git clean 命令只会移除没有忽略的未跟踪文件,-x 选项就会移除.gitignore里面忽略的文件。  
-d 移除未追踪的目录  
-n 选项来运行命令，这意味着 “做一次演习然后告诉你 将要 移除什么”。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
git clean -fdx  

  

# git branch
git branch testing 这会在当前所在的提交对象上创建一个指针。  

git branch 如果不加任何参数运行它，会得到当前所有分支的一个列表  
--merged 与 --no-merged 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。  

git branch -d testing 删除分支，-D 如果真的想要删除分支并丢掉那些工作，如同帮助信息里所指出的，可以使用 -D 选项强制删除它。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
git branch -u origin/serverfix  

# git checkout
git checkout 命令用来切换分支，或者检出内容到工作目录
git checkout <branch>  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
git checkout -b <branch> --track <remote>/<branch>  
git checkout -b [branch] [remotename]/[branch]  跟踪分支  
git checkout --track origin/serverfix  
git checkout -b|-B <new_branch> [<start point>]  

# git reset
git reset 命令主要用来执行撤销操作  
--soft 会将HEAD引用指向给定提交。索引和工作目录的内容保持不变。撤销上次git commit操作的。变成绿色状态。  
--mixed 会将HEAD引用指向给定提交。索引内容也跟着改变以符合给定提交的树结构。但是工作目录的内容不变。默认是这个。变成红色状态。  
--hard 将HEAD引用指向给定提交，索引内容发生改变，工作目录内容也发生改变。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
取消暂存的文件,就是取消你git add 后的那个文件，就是把绿色的变成红色的

# git merge
git merge 工具用来合并一个或者多个分支到你已经检出的分支中。 然后它将当前分支指针移动到合并结果上。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  
--ff  快进式的合并（fast-forward），  这时候只会更新分支的指向，不会创建一个"merge xxxx"这样的一个提交的。这个是默认行为。    
--no-ff 这个选项是 非快快进的，它总是会创建一个”merge xxx“这样的一个提交的。不管你是快进式，还是非快进式的。  
--ff-only  这个选项是只采用快进式的，如果不是快进式就会报错退出合并操作的。  
--squash, --no-squash              
git merge --abort #终止合并操作，恢复合并前的状态。  
git merge --continue # 继续合并操作  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git mergetool
当你在 Git 的合并中遇到问题时，可以使用 git mergetool 来启动一个外部的合并帮助工具。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git log
git log 命令用来展示一个项目的可达历史记录，从最近的提交快照起。   
默认情况下，它只显示你当前所在分支的历史记录，但是可以显示不同的甚至多个头记录或分支以供遍历。   
此命令通常也用来在提交记录级别显示两个或多个分支之间的差异。  
    last = log -1 HEAD  
git lo   = log --oneline  
git ll   = log --graph --format=format:'%C(bold blue)%h%C(reset) %C(red)%<(100,trunc)%s%C(reset) %C(bold black)— %an%C(reset)%C(bold green)%d%C(reset) - %C(bold green)(%ar)%C(reset)' --abbrev-commit --date=relative --show-signature   

比较2个分支的差异的  
git lc   = log --left-right --cherry-pick --date=short --pretty='%m || %h ||  %<(120,trunc)%s (%<(10,trunc)%an) (%cd)'    
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git stash
git stash 命令用来临时地保存一些还没有提交的工作，以便在分支上不需要提交未完成工作就可以清理工作目录。  
git stash list 查看现有的储藏   
git stash apply 重新应用你刚刚实施的储藏  
git stash apply stash@{2}。指定应用第二个。如果你不指明，Git 默认使用最近的储藏并尝试应用它.  
git stash drop，加上你希望移除的储藏的名字  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git tag
git tag 命令用来为代码历史记录中的某一个点指定一个永久的书签。  
git tag -l  
git tag -d  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git fetch
git fetch 命令与一个远程的仓库交互，并且将远程仓库中有但是在当前仓库的没有的所有信息拉取下来然后存储在你本地数据库中。
git fetch origin   

--all 下载所有的远程仓库  
-p, --prune 删除本地所有追踪分支，对应的服务器上的分支不存在的追踪分支。  
-n, --no-tags 不下载标签  
-t, --tags 下载所有标签  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/


引用规格的格式由一个可选的 + 号和紧随其后的 <src>:<dst> 组成，  
其中 <src> 是一个模式（pattern），代表远程版本库中的引用；  
<dst> 是那些远程引用在本地所对应的位置。  
+ 号告诉 Git 即使在不能快进的情况下也要（强制）更新引用。  

默认情况下，引用规格由 git remote add 命令自动生成， Git 获取服务器中 refs/heads/ 下面的所有引用，  
并将它写入到本地的 refs/remotes/origin/ 中。 所以，如果服务器上有一个 master 分支，我们可以在本地通过下面这种方式来访问该分支上的提交记录：  
```
$ git log origin/master
$ git log remotes/origin/master
$ git log refs/remotes/origin/master

# 上面的三个命令作用相同，因为 Git 会把它们都扩展成 refs/remotes/origin/master。

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

如果想让 Git 每次只拉取远程的 master 分支，而不是所有分支，可以把（引用规格的）获取那一行修改为：
```
fetch = +refs/heads/master:refs/remotes/origin/master


欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
当然你也可以直接在git fetch 后面跟上,你也可以指定多个引用规格。 在命令行中，你可以按照如下的方式拉取多个分支：

```
$ git fetch origin master:refs/remotes/origin/mymaster

$ git fetch origin master:refs/remotes/origin/mymaster topic:refs/remotes/origin/topic
From git@github.com:schacon/simplegit
! [rejected]        master     -> origin/mymaster  (non fast forward)
* [new branch]      topic      -> origin/topic
#在这个例子中，对 master 分支的拉取操作被拒绝，因为它不是一个可以快进的引用。 我们可以通过在引用规格之前指定 + 号来覆盖该规则。


欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```

你也可以在配置文件中指定多个用于获取操作的引用规格。 如果想在每次获取时都包括 master 和 experiment 分支，添加如下两行：
```
[remote "origin"]
    url = https://github.com/schacon/simplegit-progit
    fetch = +refs/heads/master:refs/remotes/origin/master
    fetch = +refs/heads/experiment:refs/remotes/origin/experiment

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

我们不能在模式中使用部分通配符，所以像下面这样的引用规格是不合法的：
```
fetch = +refs/heads/qa*:refs/remotes/origin/qa*  # 这个不对的，不能两边都是星号的.这个在高版本git中可以用
fetch = +refs/heads/*:refs/remotes/origin/qa*   # 不过可以这样，这样把服务器的分支都下载本地，本地都以qa开头。

fetch = +refs/heads/qa/*:refs/remotes/origin/qa/* # 这样的也是可以的,heads后面可以跟任意多层的，就像路径那样的层次结构一样

欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```

# git pull
git pull 命令基本上就是 git fetch 和 git merge 命令的组合体，Git 从你指定的远程仓库中抓取内容，然后马上尝试将其合并进你所在的分支中。  
git pull 等价于 git fetch  然后 git merge FETCH_HEAD  
git pull --rebase  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/



# git push
git push origin HEAD:master  
git push origin HEAD:refs/heads/master  
git push git@192.168.1.106:mage/my-git-project.git  HEAD:refs/heads/dev  
git push origin HEAD:refs/for/master  
git push origin HEAD:refs/tags/v1.0  
git push origin master:refs/heads/master  
git push origin refs/heads/master:refs/heads/master  
git push origin master:refs/heads/qa/master  
git push origin :topic 删除  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git remote
git remote 命令是一个是你远程仓库记录的管理工具。   
它允许你将一个长的 URL 保存成一个简写的句柄，例如 origin ，这样你就可以不用每次都输入他们了。   
你可以有多个这样的句柄，git remote 可以用来添加，修改，及删除它们。  
git remote add <name> <url>   
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git archive
git archive 命令用来创建项目一个指定快照的归档文件。  
我们在 准备一次发布 一节中，使用 git archive 命令来创建一个项目的归档文件用于分享。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git submodule
git submodule 命令用来管理一个仓库的其他外部仓库。 它可以被用在库或者其他类型的共享资源上。  
submodule 命令有几个子命令, 如（add、update、sync 等等）用来管理这些资源。  
git submodule update --remote 来从上游拉取新工作  
git submodule update --remote --rebase  
参考：https://git-scm.com/book/zh/v2/Git-工具-子模块   
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git show
git show 命令可以以一种简单的人类可读的方式来显示一个 Git 对象。 你一般使用此命令来显示一个标签或一个提交的信息。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git shortlog
git shortlog 是一个用来归纳 git log 的输出的命令。 它可以接受很多与 git log 相同的选项，
但是此命令并不会列出所有的提交，而是展示一个根据作者分组的提交记录的概括性信息。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git describe
git describe 命令用来接受任何可以解析成一个提交的东西，然后生成一个人类可读的字符串且不可变。 
这是一种获得一个提交的描述的方式，它跟一个提交的 SHA-1 值一样是无歧义，但是更具可读性。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git bisect
git bisect 工具是一个非常有用的调试工具，它通过自动进行一个二分查找来找到哪一个特定的提交是导致 bug 或者问题的第一个提交。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/


# git blame
git blame 命令标注任何文件的行，指出文件的每一行的最后的变更的提交及谁是那一个提交的作者。 当你要找那个人去询问关于这块特殊代码的信息时这会很有用。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git grep
git grep 命令可以帮助在源代码中，甚至是你项目的老版本中的任意文件中查找任何字符串或者正则表达式。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git cherry-pick
git cherry-pick 命令用来获得在单个提交中引入的变更，然后尝试将作为一个新的提交引入到你当前分支上。  
从一个分支单独一个或者两个提交而不是合并整个分支的所有变更是非常有用的。  
git cherry-pick --ff  
git cherry-pick --continue  
git cherry-pick --abort  
git cherry-pick --quit  
git cherry-pick c6ade72c576^..b299ad990db 一个连续的提交记录,基线升级使用。https://blog.csdn.net/mmh19891113/article/details/79258738  
git cherry-pick start^..end  表示包括 start，结束到end。都是一个闭区间。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git rebase
git rebase 命令基本是是一个自动化的 cherry-pick 命令。 它计算出一系列的提交，然后再以它们在其他地方以同样的顺序一个一个的 cherry-picks 出它们。  
git rebase -i 交互式的，可以合并多个提交为一个，可以调整提交的前后顺序等  
git rebase --onto  基线升级使用  
git rebase  --onto  new   start_point   end_point   
git rebase --continue | --skip | --abort | --quit  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  

# git revert
git revert 命令本质上就是一个逆向的 git cherry-pick 操作。  
它将你提交中的变更的以完全相反的方式的应用到一个新创建的提交中，本质上就是撤销或者倒转。  
git revert --continue  
git revert --quit  
git revert --abort                                       
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/



# git apply
git apply 命令应用一个通过 git diff 或者甚至使用 GNU diff 命令创建的补丁。 它跟补丁命令做了差不多的工作，但还是有一些小小的差别。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git am
git am 命令用来应用来自邮箱的补丁。特别是那些被 mbox 格式化过的。 这对于通过邮件接受补丁并将他们轻松地应用到你的项目中很有用。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git format-patch
git format-patch 命令用来以 mbox 的格式来生成一系列的补丁以便你可以发送到一个邮件列表中。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git imap-send
git imap-send 将一个由 git format-patch 生成的邮箱上传至 IMAP 草稿文件夹。 
我们在 通过邮件的公开项目 一节中见过一个通过使用 git imap-send 工具向一个项目发送补丁进行贡献的例子。

# git send-email
git send-mail 命令用来通过邮件发送那些使用 git format-patch 生成的补丁。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git request-pull
git request-pull 命令只是简单的用来生成一个可通过邮件发送给某个人的示例信息体。 如果你在公共服务器上有一个分支，
并且想让别人知道如何集成这些变更，而不用通过邮件发送补丁，你就可以执行此命令的输出发送给这个你想拉取变更的人。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git svn
git svn 可以使 Git 作为一个客户端来与 Subversion 版本控制系统通信。
这意味着你可以使用 Git 来检出内容，或者提交到 Subversion 服务器。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git fast-import
对于其他版本控制系统或者从其他任何的格式导入，你可以使用 git fast-import 快速地将其他格式映射到 Git 可以轻松记录的格式。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git gc
git gc 命令在你的仓库中执行 “garbage collection” ，删除数据库中不需要的文件和将其他文件打包成一种更有效的格式。
此命令一般在背后为你工作，虽然你可以手动执行它-如果你想的话。    
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git fsck
git fsck 命令用来检查内部数据库的问题或者不一致性。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git reflog
git reflog 命令分析你所有分支的头指针的日志来查找出你在重写历史上可能丢失的提交。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git filter-branch
git filter-branch 命令用来根据某些规则来重写大量的提交记录，
例如从任何地方删除文件，或者通过过滤一个仓库中的一个单独的子目录以提取出一个项目。  
欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

# git中2个点和3个点区别
What are the differences between double-dot “..” and triple-dot “…” in Git commit ranges?

https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection#Commit-Ranges

对于在git log中使用double dot和triple dot的区别：  

对于2个分支A和B来说：  
```bash
git log A..B    # 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
这个会显示B分支有的，而A分支没有的那些提交，下面这个图片可以清楚的显示出来

![Android下的配置管理之道之git的使用_1.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_1.png)




对于2个分支A和B来说： 
```bash
git log A...B   # 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```
这个将会显示A分支有的B分支没有的， B分支有的A分支没有的。用下面的图清楚的表示  

![Android下的配置管理之道之git的使用_2.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_2.png)


这个3个点的非常有用，经常会配合--lfet-right来一起用
```bash

$ git log --oneline --decorate --left-right --graph master...origin/master
< 1794bee (HEAD, master) Derp some more
> 6e6ce69 (origin/master, origin/HEAD) Add hello.txt

```
上面就是比较2个分支的差异，小于号开头的行是表示master分支有的提交，大于号表示origin/master分支的提交



```bash

$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-right --boundary                  
< 036a649 (shgit/dev_20180319) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
< 547f251 [SHAR-995][BSP] Disable ext4 barrier
< 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
| > 89f5290 (HEAD, dev_20171101) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
| > 29b877a [SHAR-995][BSP] Disable ext4 barrier
|/  
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
通过这个命令我们可以看到左右2个分支之间的差异，使用--left-right选项，小于号开始的表示是shgit/dev_20180319有的提交，  
大于号开始的表示shgit/dev_20171101分支的提交。最后一行表示他们的共同祖先提交。这个是加了--boundary的效果。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/  


```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-right --boundary   --cherry-pick 
< 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
同样的这个是加了  --cherry-pick选项，我们发现少了几行，这个选项会认为通过cherr-pick的提交是一样的提交，会过滤掉这些提交的。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 



```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-right   --cherry-pick 
< 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
```
这个是去掉 --boundary  选项，就是不显示他们的起始点那个提交。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
 

```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101 --left-only --boundary 
* 036a649 (shgit/dev_20180319) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
* 547f251 [SHAR-995][BSP] Disable ext4 barrier
* 2b55f87 [SHAR-7690][Drv][WLAN]Enable wifi host side recovery
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
--left-only 选项只会显示左边那个分支的提交就是只显示shgit/dev_20180319分支的提交。  
同样的他们的共同起始提交是最后一行那个。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 



```
$ git log --oneline --decorate --graph shgit/dev_20180319...shgit/dev_20171101  --right-only --boundary   
* 89f5290 (HEAD, shgit/dev_20171101) [SHARK-4246] device/qcom/sdm845: add startOffsetMs to 300
* 29b877a [SHAR-995][BSP] Disable ext4 barrier
o 9aebb83 [SR-886][app]performance optimization:disable iop from xiaomi
```
--right-only 选项只会显示右边那个分支的提交，就是只显示shgit/dev_20171101分支的提交。  
同样的他们的共同起始提交是最后一行那个。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/ 




![Android下的配置管理之道之git的使用_5.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_5.png)






对于在git diff中使用double dot和triple dot的区别：  

```
$ git diff topic master    (1)      #欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
$ git diff topic..master   (2)  
$ git diff topic...master  (3)  
```
1.Changes between the tips of the topic and the master branches.  
2.Same as above.  
3.Changes that occurred on the master branch since when the topic branch was started off it. 
第一个和第二个一样，都是比较topic 分支到 master分支在那个最后一个提交处的代码的差异。  
第三个就是比较两个分支共同起始点那个提交开始，到master结束的那个提交 的差异。  
我们给个图片就清楚可以看到了： 
![Android下的配置管理之道之git的使用_3.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_3.png)

git diff topic..master  等价 git diff topic master      #欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
git diff topic...master 等价 git diff $(git merge-base topic master) master     
![Android下的配置管理之道之git的使用_4.png](https://raw.githubusercontent.com/mageSFC/myblog/master/images/Android%E4%B8%8B%E7%9A%84%E9%85%8D%E7%BD%AE%E7%AE%A1%E7%90%86%E4%B9%8B%E9%81%93%E4%B9%8Bgit%E7%9A%84%E4%BD%BF%E7%94%A8_4.png)


# git中HEAD^ 和HEAD~ 区别

What's the difference between HEAD^ and HEAD~ in git

```bash
       <rev>^, e.g. HEAD^, v1.5.1^0   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           A suffix ^ to a revision parameter means the first parent of that commit object.  ^<n> means the <n>th parent (i.e.  <rev>^ is equivalent to <rev>^1). As a special rule,
           <rev>^0 means the commit itself and is used when <rev> is the object name of a tag object that refers to a commit object.

```
```bash

       <rev>~<n>, e.g. master~3   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           A suffix ~<n> to a revision parameter means the commit object that is the <n>th generation ancestor of the named commit object, following only the first parents. I.e.  <rev>~3
           is equivalent to <rev>^^^ which is equivalent to <rev>^1^1^1. See below for an illustration of the usage of this form.

```

类似这样的一个符号的 ～  和  ^  都是表示父提交，  
两个符号 ～～  和 ^^ 都是表示的爷爷提交。  

他们的不同是体现在后面跟上了数字。  
～2 表示的是上面2层。这个是在层级上的。~2 means up two levels in the hierarchy。  
^2 表示的第二个父提交。一个^ 表示第一个父提交，两个^表示第二个父提交。^2 means the second parent where a commit has more than one parent (i.e. because it's a merge)  
这2个符号也可以组合起来使用的。HEAD~2^3 means HEAD's grandparent commit's third parent commit  


下面我们看一个提交树状图。从下往上，从左往右。 欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
```bash

       Here is an illustration, by Jon Loeliger. Both commit nodes B and C are parents of commit node A. Parent commits are ordered left-to-right.

           G   H   I   J
            \ /     \ /
             D   E   F
              \  |  / \
               \ | /   |
                \|/    |
                 B     C
                  \   /
                   \ /
                    A

           A =      = A^0
           B = A^   = A^1     = A~1    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           C = A^2  = A^2              欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           D = A^^  = A^1^1   = A~2    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           E = B^2  = A^^2              欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           F = B^3  = A^^3              欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           G = A^^^ = A^1^1^1 = A~3     欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           H = D^2  = B^^2    = A^^^2  = A~2^2  欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           I = F^   = B^3^    = A^^3^    欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/
           J = F^2  = B^3^2   = A^^3^2   欢迎光临 马哥私房菜 淘宝https://shop592330910.taobao.com/

```




















