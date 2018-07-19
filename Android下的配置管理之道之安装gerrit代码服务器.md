
gerrit代码服务器的安装，

可以参考这个https://blog.csdn.net/mmh19891113/article/details/77196399

下面我们介绍另外一种安装方式。

就是native installers tools的安装方式。也就是 使用.deb 或者.rpm 包来安装。

```text
We are pleased to announce the general availability of RPM and Debian native distributions
 for Gerrit Ver. 2.10. They have been packaged from the original Gerrit WAR using the new 
 native installers tools developed and contributed back to the Gerrit community.

```
```text
gerrit在2.10版本的时候发布了另外格式的安装包，也就是 rpm/deb 格式的安装包。

``` 
为什么需要另外格式的安装包？
```text
Why yet another packaging format?

Gerrit Code Review has always been released as pure Java executable WAR that has then to be
 invoked with a target installation directory. This style of packaging has been working fine
  right now, however many companies use the Linux native software packaging tool to standardise
   the way they install and update software.

The reasons behind that choice are:

    automatic compatibility matrix and dependencies tracing and resolution: Gerrit dependencies 
    are automatically checked and download upon installation
    automatic update of the latest patch-set released on Gerrit using “yum update” or 
    “apt-get install –only-upgrade”
    out-of-the-box support for Puppet / Chef for unattended installations
    compliance with company standards: system administrators do not have to manage a “special case” for Gerrit

总之也是为了系统管理人员方便安装gerrit。不在像之前那种每一步都需要手动操作了。

```


提供了哪些格式的安装包？？
```text
What type of packages are provided?

The following two packaging formats are currently supported:
– RPM packages for RedHat / CentOS Linux distributions
– Deb packages for Ubuntu or other Debian distributions

```

如何添加这些安装包的源？？？
```text
How to enable Gerrit native packages on my Linux system?

对于centos系统使用下面命令：
rpm -i https://gerritforge.com/gerritforge-repo-1-2.noarch.rpm


对于ubuntu系统使用下面的命令：
echo "deb mirror://mirrorlist.gerritforge.com/deb gerrit contrib" > /etc/apt/sources.list.d/gerritforge.list

apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 1871F775
apt-get update

```

如何安装？？
```text
对于centos系统使用下面命令：
yum install -y gerrit


对于ubuntu系统使用下面的命令：
apt-get install gerrit

```

默认的安装配置是什么？？
```text
采用这种方式安装 一些默认的配置如下

    H2 Database
    DEVELOPMENT_BECOME_ANY_ACCOUNT authentication
    HTTP (no SSL) and SSH protocols

如果需要修改，可以修改gerrit.config 配置文件。


```

下一步
```text

    Docker (coming very soon)
    MacOS Pkg
    Windows MSI


```