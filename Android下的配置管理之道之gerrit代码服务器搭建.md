****

#Android下的配置管理之道之gerrit代码服务器搭建

gerrit 代码服务器搭建  Version v2.11.5



一般参考gerrit的文档就可以了。下面大部分都是文档的，列出一些注意点，一些工具的选择取舍等。
#所需环境

jdk，git 等相关的工具
gerrit是一个java web 应用，所以java运行时环境是不能少的，安装jdk或jre都可以的。
gerrit是基于git来管理代码的，所以git也是必不可少的。
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/


```
安装方法：
sudo apt-get install openjdk-7-jdk  git

# 或者  sudo apt-get install openjdk-7-*  git，
# 这里星号会匹配所有的安装包，这样会安装的比较多，这是懒人的做法。
# tab 键补全出来，可以看到如下的包。
	#openjdk-7-dbg           
	#openjdk-7-doc           
	#openjdk-7-jre           
	#openjdk-7-jre-lib       
	#openjdk-7-source                                                    
	#openjdk-7-demo          
	#openjdk-7-jdk           
	#openjdk-7-jre-headless  
	#openjdk-7-jre-zero 
```
或使用jdk8.  ubuntu 16 就可以使用jdk8了。
```
sudo apt-get install openjdk-8-jdk  git

```

最新的gerrit基本上都需要jdk1.7 或者 jdk1.8了。jdk或者jre的java环境都可以的。
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
下载gerrit的安装包gerrit.war包。
Gerrit Releases site.https://gerrit-releases.storage.googleapis.com/index.html

最新的版本有2.14了（可能也有更新的了，文章可能更新不及时）。

操作系统一般用ubuntu, 对于目前2017年来说用ubuntu16.04 server版本的最好。

有的系统安装不了jdk8，怎么办？像ubuntu 14.04就不能安装jdk8，如下：
```
sudo apt-get update
sudo apt-get install -y openjdk-7-jdk

#添加 openjdk：
sudo add-apt-repository ppa:openjdk-r/ppa
sudo apt-get update
sudo apt-get install openjdk-7-jdk
sudo apt-get install openjdk-8-*
sudo apt-get install openjdk-7-*

#添加 oracle jdk：
#To install the oracle JDK, use the following command – 
sudo add-apt-repository ppa:webupd8team/java

#配置默认的jdk：
sudo update-alternatives --config java
sudo update-alternatives --config javac

#apt的缓存目录： /var/cache/apt/archives/
```

#数据库的设置

gerrit支持 h2，mysql，oracle，postgresql等这几种数据库

 1. H2

如果你选择h2类型的数据库，不需要额外安装什么，也不需要配置什么，这个数据库就是java实现的一个本地的数据库。一般公司中不推荐使用这个，直接测试学习可以试一下的。尤其是大型公司不推荐使用这个类型的数据库的。
不推荐是应为没有一个简单的方法来和数据库交互，除非gerrit先停止运行。也就是说gerrit在运行的状态下是没有方法直接访问这个h2类型的数据库的。当然用 ssh gerrit.com gerrit gsql 这个命令是另外一回事了。马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
不推荐是还是个原因是不容易备份数据。
最后一个不推荐的原因是不支持负载平衡，热备份之类的。

 2. PostgreSQL

这个是非常推荐使用的数据库。
这个需要单独安装的，
sudo apt-get install postgresql 就可以了了。或者马哥一般习惯安装 phppgadmin，这样apache有了，管理数据库的，带界面的软件也有了，非常的方便的。
需要先创建一个给gerrit web应用使用的一个数据库的角色或账号。
下面命令一般需要切换到叫postgres的一个linux账户下面去执行sudo su postgres。
  
```
$ createuser --username=postgres -RDIElPS gerrit2   
# 这个是创建一个数据库的角色，上面的$　美元符不是命令的一部分，是linux命令提示符而已
#　有的可能是类似这样的命令提示符：gerrit2@gerrit-master:~$ 
  
$ createdb --username=postgres -E UTF-8 -O gerrit2 reviewdb     
#这个是创建一个名字叫reviewdb的数据库，属于gerrit2这个角色。
```
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
更多详细的关于postgresql数据库的内容可以访问官网：http://www.postgresql.org/docs/9.1/interactive/index.html

补充：
安装完成phppgadmin之后直接访问会报错，需要改个配置文件
/etc/apache2/conf-available/phppgadmin.conf文件中 `Require local` 改为 `allow from all`
```
buildfarm@gerrit-tags:~$ cat /etc/apache2/conf-available/phppgadmin.conf 
Alias /phppgadmin /usr/share/phppgadmin

<Directory /usr/share/phppgadmin>

<IfModule mod_dir.c>
DirectoryIndex index.php
</IfModule>
AllowOverride None

# Only allow connections from localhost:
#Require local
allow from all

<IfModule mod_php.c>
  php_flag magic_quotes_gpc Off
  php_flag track_vars On
  #php_value include_path .
</IfModule>
<IfModule !mod_php.c>
  <IfModule mod_actions.c>
    <IfModule mod_cgi.c>
      AddType application/x-httpd-php .php
      Action application/x-httpd-php /cgi-bin/php
    </IfModule>
    <IfModule mod_cgid.c>
      AddType application/x-httpd-php .php
      Action application/x-httpd-php /cgi-bin/php
    </IfModule>
  </IfModule>
</IfModule>

</Directory>
```


3.MySQL

这个也是非常推荐使用的数据库
这个也需要单独安装，
```
sudo apt-get install mysql-client mysql-server
```
马哥比较推荐的一个安装是 
```
sudo apt-get install mysql-client mysql-server phpmyadmin
```
这里多安装一个mysql数据库的管理软件。很方便的，带界面的，通过浏览器来管理mysql数据库的。创建数据库，创建角色，设置密码，查询，修改都可以通过浏览器来进行，非常的方便的。
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
```
  mysql创建数据库账户如下（这些命令需要链接到mysql后执行的）：

  CREATE USER 'gerrit2'@'localhost' IDENTIFIED BY 'secret';   #这个是创建个mysql的账号
  CREATE DATABASE reviewdb;
  GRANT ALL ON reviewdb.* TO 'gerrit2'@'localhost';    #创建数据库，名字叫reviewdb
  FLUSH PRIVILEGES;                      #记得最后刷新，这样才生效。
```

当然上面的命令，你就可以直接通过phpmyadmin 来在浏览器上面操作了。

4.Oracle
 这个数据库也是非常好的一个，不过马哥本人没有用过，不好多说什么。公司中最常用的还是mysql和postgresql这2个数据库。我在几个公司带过，基本上用的都是mysql和postgresql。oracle是要钱的吧？！
```
SQL> create user gerrit2 identified by secret_password default tablespace users;               同样的也是要先建个gerrit2的数据库的账户。
SQL> grant connect, resources to gerrit2;
```
数据怎么创建好像官方文档中没有介绍。
一般Oracle数据库（Oracle Database）可以分为两部分，即实例（Instance）和数据库（Database）。
```
SQL> create database reviewdb   #可能是这个，不一定对，大家可以下去仔细查下资料。马哥本人没有用过，不好多说。
```

这里还需要配置个Instance
在文件 $site_path/etc/gerrit.config:

```
[database]
        type = oracle
        instance = xe
        hostname = localhost
        username = gerrit2
        port = 1521
```

数据库我们不都创建了一个账户吗？账户是有密码的，这个密码怎么配置到gerrit web　中呢？
密码写到这个文件中　$site_path/etc/secure.config:
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
```
[database]
        password = secret_pasword
```

5.SAP MaxDB

这个数据库马哥本人没有用过，不好多说，大家只能自行研究了
运行Gerrit 在MaxDB数据库, 你可能需要 MaxDB JDBC driver.相关的数据库驱动的jar包，可能的安装位置是：
在windows的路径一般会是 "C:\Program Files\sdb\MaxDB\runtime\jar\sapdbc.jar"
在linux上路径一般是 "/opt/sdb/MaxDB/runtime/jar/sapdbc.jar"

配置文件 $site_path/etc/gerrit.config:
```
[database]
        type = maxdb
        database = reviewdb
        hostname = localhost
        username = gerrit2
```
     
密码配置文件 $site_path/etc/secure.config:

```
[database]
        password = <secret password>
```

   
上面说到的gerrit需要的数据库。

一般数据库相关的配置会在文件 $site_path/etc/gerrit.config和文件 $site_path/etc/secure.config。
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/



#**初始化gerrit**
１．先添加个linux的系统账户，让gerrit　web运行在这个非root账户下面，这样更安全。
这个账户也不要设置密码。执行adduser添加系统账户，不需要给这个账户设置密码。
一般的马哥安装完系统会创建一个sudo权限的账户，名：mage。

```
  sudo adduser gerrit2
```
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
  
 ２．切换到gerrit2这个linux系统账户下。不需要密码我一般是使用sudo su来切换到该账户下的。

```
  sudo su gerrit2
```

  
３．执行init命令

```
  java -jar gerrit.war init -d /path/to/your/gerrit_application_directory
```

一般安装到 /home/gerrit2/review_site这个路径下面，这个是官方文档中给出的一个路径。
当然也可以安装到其他路径下。例如如/var/lib/gerrit2 ，注意不要太随便的找个路径。推荐这个路径：/home/gerrit2/review_site。
一般的这个路径就是gerrit2 这个系统账户的home目录的。
```
  java -jar gerrit.war init -d　/home/gerrit2/review_site
```

  
它会一步一步的提示你输入相关的内容的。初始化之后可以再来修改这些配置的。
关于gerrit的主要的配置是在文件 '$site_path/etc/gerrit.config'中，

----------
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/

来一个马哥自己的配置：
```
gerrit2@gerrit-master:~/review_site$ cat etc/gerrit.config 
[gerrit]
    basePath = git  # git 仓库的一个路径，这里个相对路径，等于/home/gerrit2/review_site/git。
    canonicalWebUrl = http://gerrit.xxx.com:8080/ # 登陆gerrit web的url，默认使用8080端口。
[database]
    type = postgresql
    hostname = localhost
    database = reviewdb
    username = gerrit2
    poolMinIdle = 4
    poolMaxIdle = 10
    poolLimit = 128
    poolMaxWait = 10s
[index]
    type = LUCENE
[auth]
    type = LDAP
[ldap]
    server = ldap://AD01.xxx.com
    username = ldapuser
    accountBase = DC=xxx,DC=com
    groupBase = DC=xxx,DC=com
    localUsernameToLowerCase = true  #这个比较关键，之后忽略大小写，一定要配置，马哥之前吃过亏的。
[sendemail]
    enable = true
    smtpServer = ip
    smtpUser = gerrit2@xxx.com
    from = gerrit2@xxx.com
[container]
    user = gerrit2
    javaHome = /usr/lib/jvm/java-8-openjdk-amd64/jre
    heapLimit=100g
[sshd]
    listenAddress = *:29418　＃ssh监听的端口，最好不要随便修改
    threads = 112
    batchThreads = 16
    streamThreads = 20
    commandStartThreads = 6
    maxConnectionsPerUser = 32
[httpd]
    listenUrl = http://*:8080/
[cache]
    directory = cache
[theme]
    backgroundColor = FCFEEF
    textColor = 000000
    trimColor = D4E9A9
    selectionColor = FFFFCC
    topMenuColor = D4E9A9
    changeTableOutdatedColor = F08080
[theme "signed-in"]
    backgroundColor = FFFFFF

[commentlink "jira"]
    match = \\[([A-Z]+-[0-9]+)\\]
    link = http://jira.xxx.com/browse/$1　　＃这个配置和jira单号关联

[cache "web_sessions"]
    maxAge = 1 week

[gitweb]
    type = custom
    linkname = gitiles
    url = /plugins/gitiles/
    revision = ${project}/+/${commit}
    project = ${project}
    roottree = ${project}
    branch = ${project}/+/${branch}
    filehistory = ${project}/+log/${branch}/${file}
    file = ${project}+${commit}/${file}


```
下面这个是密码相关的配置文件：

```
gerrit2@gerrit-master:~/review_site$ cat etc/secure.config 
[database]
    password = xxxxx
[auth]
    registerEmailPrivateKey = xxx
    restTokenPrivateKey = xx
[ldap]
    password = xxxx
[sendemail]
    smtpPass = xxxx

```


初始化好之后我们就可以启动我们的gerrit web应用了

执行下面的命令就可以了，很简单的。一个sh脚本
```
review_site/bin/gerrit.sh start
review_site/bin/gerrit.sh stop
review_site/bin/gerrit.sh restart
```
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/

在启动gerrit 服务的时候很有可能会起不起来。这个时候我们就需要
去看review_site/logs/下面的那个错误日志文件了。
```
cat 　review_site/logs/error_log

或者

tail -f review_site/logs/error_log
```

启动起来的日志看起来像这样
```
[2017-08-15 09:19:33,250] [plugin-manager-preloader] INFO  com.googlesource.gerrit.plugins.manager.OnStartStop : 57 plugins successfully pre-loaded
[2017-08-15 09:22:56,169] [ShutdownCallback] INFO  com.google.gerrit.pgm.Daemon : caught shutdown, cleaning up
[2017-08-15 09:22:56,205] [ShutdownCallback] INFO  org.eclipse.jetty.server.AbstractConnector : Stopped ServerConnector@29174dfe{HTTP/1.1,[http/1.1]}{0.0.0.0:8081}
[2017-08-15 09:22:56,207] [ShutdownCallback] INFO  org.eclipse.jetty.server.handler.ContextHandler : Stopped o.e.j.s.ServletContextHandler@49f41c2e{/,null,UNAVAILABLE}
[2017-08-15 09:22:56,209] [ShutdownCallback] INFO  com.google.gerrit.sshd.SshDaemon : Stopped Gerrit SSHD
[2017-08-15 09:23:11,656] [main] INFO  org.eclipse.jetty.util.log : Logging initialized @6446ms
[2017-08-15 09:23:11,710] [main] INFO  com.google.gerrit.server.git.LocalDiskRepositoryManager : Defaulting core.streamFileThreshold to 439m
[2017-08-15 09:23:12,573] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loading plugins from /var/gerrit/plugins
[2017-08-15 09:23:12,617] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin avatars-gravatar, version ab2ff52
[2017-08-15 09:23:12,639] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin commit-message-length-validator, version v2.14.1
[2017-08-15 09:23:12,686] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin delete-project, version v2.13-13-gd9f1d5f
[2017-08-15 09:23:12,731] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin download-commands, version v2.14.1
[2017-08-15 09:23:12,766] [main] INFO  com.google.gerrit.server.config.PluginConfigFactory : No /var/gerrit/etc/gitiles.config; assuming defaults
[2017-08-15 09:23:12,790] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin gitiles, version d983e33
[2017-08-15 09:23:12,845] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin hooks, version v2.14.1
[2017-08-15 09:23:12,905] [plugin-manager-preloader] INFO  com.googlesource.gerrit.plugins.manager.OnStartStop : Start-up: pre-loading list of plugins from registry
[2017-08-15 09:23:12,906] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin plugin-manager, version v2.14-2-g1a2fc03
[2017-08-15 09:23:12,973] [main] WARN  com.googlesource.gerrit.plugins.replication.ReplicationFileBasedConfig : Config file /var/gerrit/etc/replication.config does not exist; not replicating
[2017-08-15 09:23:12,974] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin replication, version v2.14.1
[2017-08-15 09:23:13,001] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin reviewnotes, version v2.14.1
[2017-08-15 09:23:13,018] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin singleusergroup, version v2.14.1
[2017-08-15 09:23:13,074] [main] INFO  com.google.gerrit.server.plugins.PluginLoader : Loaded plugin uploadvalidator, version 5a674b7
[2017-08-15 09:23:13,076] [main] INFO  com.google.gerrit.server.change.ChangeCleanupRunner : Ignoring missing changeCleanup schedule configuration
[2017-08-15 09:23:13,092] [main] INFO  com.google.gerrit.sshd.SshDaemon : Started Gerrit SSHD-CORE-1.4.0 on *:29418
[2017-08-15 09:23:13,094] [main] INFO  org.eclipse.jetty.server.Server : jetty-9.3.17.v20170317
[2017-08-15 09:23:13,641] [main] INFO  com.googlesource.gerrit.plugins.gitiles.HttpModule : No /var/gerrit/etc/gitiles.config; assuming defaults
[2017-08-15 09:23:13,896] [main] INFO  org.eclipse.jetty.server.handler.ContextHandler : Started o.e.j.s.ServletContextHandler@1774c4e2{/,null,AVAILABLE}
[2017-08-15 09:23:13,904] [main] INFO  org.eclipse.jetty.server.AbstractConnector : Started ServerConnector@32fd5bc{HTTP/1.1,[http/1.1]}{0.0.0.0:8081}
[2017-08-15 09:23:13,904] [main] INFO  org.eclipse.jetty.server.Server : Started @8695ms
[2017-08-15 09:23:13,905] [main] INFO  com.google.gerrit.pgm.Daemon : Gerrit Code Review 2.14.1 ready
```

启动成功关键是看最后一句：
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
```
[2017-08-15 09:23:13,905] [main] INFO  com.google.gerrit.pgm.Daemon : Gerrit Code Review 2.14.1 ready
```

错误的日志看起来像这样的：
```
[2017-08-15 08:53:20,516] [HTTP-75] ERROR com.google.gerrit.server.auth.ldap.LdapRealm : Cannot query LDAP to authenticate user
javax.naming.CommunicationException: 10.0.64.234:389 [Root exception is java.net.ConnectException: 连接超时 (Connection timed out)]
	at com.sun.jndi.ldap.Connection.<init>(Connection.java:226)
	at com.sun.jndi.ldap.LdapClient.<init>(LdapClient.java:137)
	at com.sun.jndi.ldap.LdapClient.getInstance(LdapClient.java:1615)
	at com.sun.jndi.ldap.LdapCtx.connect(LdapCtx.java:2749)
	at com.sun.jndi.ldap.LdapCtx.<init>(LdapCtx.java:319)
	at com.sun.jndi.ldap.LdapCtxFactory.getUsingURL(LdapCtxFactory.java:192)
	at com.sun.jndi.ldap.LdapCtxFactory.getUsingURLs(LdapCtxFactory.java:210)
	at com.sun.jndi.ldap.LdapCtxFactory.getLdapCtxInstance(LdapCtxFactory.java:153)
	at com.sun.jndi.ldap.LdapCtxFactory.getInitialContext(LdapCtxFactory.java:83)
	at javax.naming.spi.NamingManager.getInitialContext(NamingManager.java:684)
	at javax.naming.InitialContext.getDefaultInitCtx(InitialContext.java:313)
	at javax.naming.InitialContext.init(InitialContext.java:244)
	at javax.naming.InitialContext.<init>(InitialContext.java:216)
	at javax.naming.directory.InitialDirContext.<init>(InitialDirContext.java:101)
	at com.google.gerrit.server.auth.ldap.Helper.open(Helper.java:135)
	at com.google.gerrit.server.auth.ldap.LdapRealm.authenticate(LdapRealm.java:234)
	at com.google.gerrit.server.account.AccountManager.authenticate(AccountManager.java:111)
	at com.google.gerrit.httpd.auth.ldap.LdapLoginServlet.doPost(LdapLoginServlet.java:122)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:648)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:729)
	at com.google.inject.servlet.ServletDefinition.doServiceImpl(ServletDefinition.java:286)
	at com.google.inject.servlet.ServletDefinition.doService(ServletDefinition.java:276)
	at com.google.inject.servlet.ServletDefinition.service(ServletDefinition.java:181)
	at com.google.inject.servlet.ManagedServletPipeline.service(ManagedServletPipeline.java:91)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:85)
	at com.google.gerrit.httpd.raw.StaticModule$PolyGerritFilter.doFilter(StaticModule.java:483)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gerrit.httpd.GetUserFilter.doFilter(GetUserFilter.java:75)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gerrit.httpd.UniversalWebLoginFilter.doFilter(UniversalWebLoginFilter.java:74)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gerrit.httpd.RunAsFilter.doFilter(RunAsFilter.java:111)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gwtexpui.server.CacheControlFilter.doFilter(CacheControlFilter.java:70)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gerrit.httpd.RequestMetricsFilter.doFilter(RequestMetricsFilter.java:57)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gerrit.httpd.AllRequestFilter$FilterProxy$1.doFilter(AllRequestFilter.java:133)
	at com.google.gerrit.httpd.AllRequestFilter$FilterProxy.doFilter(AllRequestFilter.java:135)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.gerrit.httpd.RequestContextFilter.doFilter(RequestContextFilter.java:72)
	at com.google.inject.servlet.FilterChainInvocation.doFilter(FilterChainInvocation.java:82)
	at com.google.inject.servlet.ManagedFilterPipeline.dispatch(ManagedFilterPipeline.java:120)
	at com.google.inject.servlet.GuiceFilter.doFilter(GuiceFilter.java:135)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1759)
	at org.eclipse.jetty.servlet.ServletHandler.doHandle(ServletHandler.java:582)
	at org.eclipse.jetty.server.session.SessionHandler.doHandle(SessionHandler.java:224)
	at org.eclipse.jetty.server.handler.ContextHandler.doHandle(ContextHandler.java:1180)
	at org.eclipse.jetty.servlet.ServletHandler.doScope(ServletHandler.java:512)
	at org.eclipse.jetty.server.session.SessionHandler.doScope(SessionHandler.java:185)
	at org.eclipse.jetty.server.handler.ContextHandler.doScope(ContextHandler.java:1112)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:141)
	at org.eclipse.jetty.server.handler.RequestLogHandler.handle(RequestLogHandler.java:56)
	at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:134)
	at org.eclipse.jetty.server.Server.handle(Server.java:534)
	at org.eclipse.jetty.server.HttpChannel.handle(HttpChannel.java:320)
	at org.eclipse.jetty.server.HttpConnection.onFillable(HttpConnection.java:251)
	at org.eclipse.jetty.io.AbstractConnection$ReadCallback.succeeded(AbstractConnection.java:283)
	at org.eclipse.jetty.io.FillInterest.fillable(FillInterest.java:108)
	at org.eclipse.jetty.io.SelectChannelEndPoint$2.run(SelectChannelEndPoint.java:93)
	at org.eclipse.jetty.util.thread.strategy.ExecuteProduceConsume.executeProduceConsume(ExecuteProduceConsume.java:303)
	at org.eclipse.jetty.util.thread.strategy.ExecuteProduceConsume.produceConsume(ExecuteProduceConsume.java:148)
	at org.eclipse.jetty.util.thread.strategy.ExecuteProduceConsume.run(ExecuteProduceConsume.java:136)
	at org.eclipse.jetty.util.thread.QueuedThreadPool.runJob(QueuedThreadPool.java:671)
	at org.eclipse.jetty.util.thread.QueuedThreadPool$2.run(QueuedThreadPool.java:589)
	at java.lang.Thread.run(Thread.java:748)
Caused by: java.net.ConnectException: 连接超时 (Connection timed out)
	at java.net.PlainSocketImpl.socketConnect(Native Method)
	at java.net.AbstractPlainSocketImpl.doConnect(AbstractPlainSocketImpl.java:350)
	at java.net.AbstractPlainSocketImpl.connectToAddress(AbstractPlainSocketImpl.java:206)
	at java.net.AbstractPlainSocketImpl.connect(AbstractPlainSocketImpl.java:188)
	at java.net.SocksSocketImpl.connect(SocksSocketImpl.java:392)
	at java.net.Socket.connect(Socket.java:589)
	at java.net.Socket.connect(Socket.java:538)
	at java.net.Socket.<init>(Socket.java:434)
	at java.net.Socket.<init>(Socket.java:211)
	at com.sun.jndi.ldap.Connection.createSocket(Connection.java:363)
	at com.sun.jndi.ldap.Connection.<init>(Connection.java:203)
	... 65 more
```
错误的种类比较多，一般都要具体分析啦。之后有遇到再记录一下吧。
马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/


出现这种错误需要reindex一下
执行命令`java -jar /home/gerrit2/review_site/bin/gerrit.war reindex`
```
[2017-12-26 10:25:49,111] [main] ERROR com.google.gerrit.pgm.Daemon : Unable to start daemon
com.google.inject.ProvisionException: Unable to provision, see the following errors:

1) No index versions for index 'accounts' ready; run java -jar /home/gerrit2/review_site/bin/gerrit.war reindex --index accounts

1 error
	at com.google.gerrit.server.index.VersionManager.initIndex(VersionManager.java:161)
	at com.google.gerrit.server.index.VersionManager.start(VersionManager.java:92)
	at com.google.gerrit.lifecycle.LifecycleManager.start(LifecycleManager.java:92)
	at com.google.gerrit.pgm.Daemon.start(Daemon.java:349)
	at com.google.gerrit.pgm.Daemon.run(Daemon.java:256)
	at com.google.gerrit.pgm.util.AbstractProgram.main(AbstractProgram.java:61)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at com.google.gerrit.launcher.GerritLauncher.invokeProgram(GerritLauncher.java:203)
	at com.google.gerrit.launcher.GerritLauncher.mainImpl(GerritLauncher.java:108)
	at com.google.gerrit.launcher.GerritLauncher.main(GerritLauncher.java:63)
	at Main.main(Main.java:24)
```


马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
可以设置开机自启动

```
# 做个软连接到　/etc/init.d/下面
sudo ln -snf `pwd`/review_site/bin/gerrit.sh /etc/init.d/gerrit

＃ 再做个软连接到/etc/rc3.d/下面，也可以使用update-rc.d这个命令来弄。
sudo ln -snf /etc/init.d/gerrit /etc/rc3.d/S90gerrit`
```

#**gerrit　其他个性化设置**

1.Reverse Proxy　反向代理设置，80端口到8080端口
设置apache的反向代理，可以把8080端口重定向到80端口

在上面安装数据的时候　马哥推荐了　phpmyadmin 和 phppgadmin　这个２个管理工具，
安装这个工具就会把 apache2  给安装了。所以特别推荐这样安装， apache2  也安装了，数据库也安装了。

```
sudo apt-get install apache2   安装apache2 服务
sudo a2enmod proxy        #使能这个反向代码模块，需要重启apache服务的        
sudo a2enmod proxy_http
```

在文件/etc/apache2/sites-available中添加几行

```
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	ServerName ci.company.com
	ServerAlias ci
	ProxyRequests Off
	<Proxy *>
		Order deny,allow
		Allow from all
	</Proxy>
	
	＃添加下面的3行，具体配置啥意思，可以查看手册，一般这样配置是不会有问题的。
	ProxyPreserveHost on
	ProxyPass / http://your.gerrit.url.com:8080/ nocanon
	AllowEncodedSlashes NoDecode

</VirtualHost>
```

保存这个文件，重启apache2服务.

2. 可以安装个gitweb

```
sudo apt-get install gitweb
```

或者安装个 gitiles ，这个比较好用一些，谷歌自己用的也是这个插件。

gerrit插件可以在这个网站下载到	https://gerrit-ci.gerritforge.com/

3. 删除系统中不需要的软件包
https://peteris.rocks/blog/can-you-kill-it/
```
sudo apt remove lvm2 -y --purge
sudo apt remove at -y --purge
sudo apt remove snapd -y --purge
sudo apt remove open-iscsi -y --purge
sudo apt remove dbus -y --purge
sudo apt remove accountsservice -y --purge
sudo apt remove mdadm -y --purge
sudo apt remove policykit-1 -y --purge
sudo apt remove lxcfs -y --purge






```
# gerrit升级

```
gerrit目前使用的是2.11.5版本。
```
```
2.11.5 ->  2.11.11 这个升级没有涉及到数据库的更新。可以正常直接升级成功。没有遇到什么问题。
```

```
2.11.11  -> 2.12 这个有数据库表的更新，需要特别注意。做好备份。

gerrit2@gerrit-master:~/review_site$ java -jar gerrit-2.12.war init -d /home/gerrit2/review_site
Using secure store: com.google.gerrit.server.securestore.DefaultSecureStore

*** Gerrit Code Review 2.12
*** 


Installing plugins.
Install plugin singleusergroup version v2.12 [y/N]? y
Install plugin commit-message-length-validator version v2.12 [y/N]? y
Install plugin reviewnotes version v2.12 [y/N]? y
Install plugin replication version v2.12 [y/N]? y
Install plugin download-commands version v2.12 [y/N]? y
Initializing plugins.
No plugins found with init steps.

Upgrading schema to 108 ...
Upgrading schema to 109 ...
Upgrading schema to 110 ...
Upgrading schema to 111 ...
Upgrading schema to 112 ...
Upgrading schema to 113 ...
Upgrading schema to 114 ...
Upgrading schema to 115 ...
Migrating data to schema 108 ...
Listing all changes ...
Detected open changes referring to the following non-existent projects:

It is highly recommended to remove
the obsolete open changes, comments and patch-sets from your DB.

done
Updating groups for open changes ...
done
Migrating data to schema 109 ...
Migrating data to schema 110 ...
Migrating data to schema 111 ...
Migrating data to schema 112 ...
Migrating data to schema 113 ...
Migrating data to schema 114 ...
Migrating data to schema 115 ...
Execute the following SQL to drop unused objects:

  DROP TABLE account_diff_preferences;
  DROP TABLE patch_set_ancestors;
  ALTER TABLE accounts DROP COLUMN contact_filed_on;

Execute now [Y/n]? Y
Initialized /home/gerrit2/review_site

gerrit2@gerrit-master:~/review_site$ bin/gerrit.sh start
Starting Gerrit Code Review: OK

```

```
2.11.11  -> 2.12.8




*** Gerrit Code Review 2.12.8
*** 


Initializing plugins.
No plugins found with init steps.

Upgrading schema to 108 ...
Upgrading schema to 109 ...
Upgrading schema to 110 ...
Upgrading schema to 111 ...
Upgrading schema to 112 ...
Upgrading schema to 113 ...
Upgrading schema to 114 ...
Upgrading schema to 115 ...
Migrating data to schema 108 ...
Listing all changes ...
Detected open changes referring to the following non-existent projects:

It is highly recommended to remove
the obsolete open changes, comments and patch-sets from your DB.

done
Updating groups for open changes ...
done
Migrating data to schema 109 ...
Migrating data to schema 110 ...
Migrating data to schema 111 ...
Migrating data to schema 112 ...
Migrating data to schema 113 ...
Migrating data to schema 114 ...
Migrating data to schema 115 ...
Execute the following SQL to drop unused objects:

  DROP TABLE account_diff_preferences;
  DROP TABLE patch_set_ancestors;
  ALTER TABLE accounts DROP COLUMN contact_filed_on;

Execute now [Y/n]? Y
Initialized /home/gerrit2/review_site
```

```
2.11.5  ->  2.13  升级。这个会把sshkey 放到All-users.git 仓库中


*** Gerrit Code Review 2.13
*** 



Upgrading schema to 120 ...
Upgrading schema to 121 ...
Upgrading schema to 122 ...
Upgrading schema to 123 ...
Upgrading schema to 124 ...
Upgrading schema to 125 ...
Upgrading schema to 126 ...
Upgrading schema to 128 ...
Upgrading schema to 129 ...
Migrating data to schema 120 ...
Generating Superproject subscriptions table to submodule ACLs
Migrating data to schema 121 ...
Migrating data to schema 122 ...
Migrating data to schema 123 ...
Updating references:    100% (520/520)
Migrating data to schema 124 ...
Migrating data to schema 125 ...
Migrating data to schema 126 ...
Migrating data to schema 128 ...
Migrating data to schema 129 ...
Execute the following SQL to drop unused objects:

  DROP TABLE account_patch_reviews;
  DROP TABLE account_diff_preferences;
  DROP TABLE account_ssh_keys;
  DROP TABLE patch_set_ancestors;
  DROP TABLE starred_changes;
  DROP TABLE submodule_subscriptions;
  ALTER TABLE accounts DROP COLUMN contact_filed_on;
  ALTER TABLE accounts DROP COLUMN show_site_header;
  ALTER TABLE accounts DROP COLUMN time_format;
  ALTER TABLE accounts DROP COLUMN maximum_page_size;
  ALTER TABLE accounts DROP COLUMN use_flash_clipboard;
  ALTER TABLE accounts DROP COLUMN diff_view;
  ALTER TABLE accounts DROP COLUMN copy_self_on_email;
  ALTER TABLE accounts DROP COLUMN download_command;
  ALTER TABLE accounts DROP COLUMN download_url;
  ALTER TABLE accounts DROP COLUMN date_format;
  ALTER TABLE accounts DROP COLUMN legacycid_in_change_table;
  ALTER TABLE accounts DROP COLUMN review_category_strategy;
  ALTER TABLE accounts DROP COLUMN relative_date_in_change_table;
  ALTER TABLE accounts DROP COLUMN size_bar_in_change_table;
  ALTER TABLE accounts DROP COLUMN mute_common_path_prefixes;

Execute now [Y/n]? Y
Initialized /home/gerrit2/review_site
```

#升级问题记录

```text
在从之前的2.11.5升级到 2.12的时候碰到了一个问题。
详情可以看 https://blog.csdn.net/mmh19891113/article/details/81094626

```



```text
在从之前的2.14 升级到 2.15的时候碰到了一个问题。

这个是在我本地测试环境升级的，采用的是sudo apt update && sudo apt install gerrit
的方式升级的，这个升级之后发现一个报错
[2018-07-19 10:44:53,268] [HTTP-77] ERROR com.google.gerrit.pgm.http.jetty.HiddenErrorHandler : Error in GET /changes/git%2Fshared%2Ftools%2Faais-java~142/detail?O=116414
java.lang.NoSuchMethodError: com.google.gerrit.server.change.RevisionResource.getControl()Lcom/google/gerrit/server/project/ChangeControl;
	at com.googlesource.gerrit.plugins.wip.WorkInProgress.getDescription(WorkInProgress.java:71)
	at com.googlesource.gerrit.plugins.wip.WorkInProgress.getDescription(WorkInProgress.java:36)
	at com.google.gerrit.server.extensions.webui.UiActions.describe(UiActions.java:116)
	at com.google.gerrit.server.extensions.webui.UiActions.lambda$from$0(UiActions.java:74)
	at java.util.stream.ReferencePipeline$3$1.accept(ReferencePipeline.java:193)
	at java.util.Iterator.forEachRemaining(Iterator.java:116)
	at java.util.Spliterators$IteratorSpliterator.forEachRemaining(Spliterators.java:1801)
	at java.util.stream.AbstractPipeline.copyInto(AbstractPipeline.java:481)
	at java.util.stream.AbstractPipeline.wrapAndCopyInto(AbstractPipeline.java:471)
	at java.util.stream.ReduceOps$ReduceOp.evaluateSequential(ReduceOps.java:708)
	at java.util.stream.AbstractPipeline.evaluate(AbstractPipeline.java:234)
	at java.util.stream.ReferencePipeline.collect(ReferencePipeline.java:499)
	at com.google.gerrit.server.extensions.webui.UiActions.from(UiActions.java:76)
	at com.google.gerrit.server.extensions.webui.UiActions.from(UiActions.java:67)
	at com.google.gerrit.server.change.ActionJson.toActionMap(ActionJson.java:213)
	at com.google.gerrit.server.change.ActionJson.addRevisionActions(ActionJson.java:110)
	at com.google.gerrit.server.change.ChangeJson.toRevisionInfo(ChangeJson.java:1348)
	at com.google.gerrit.server.change.ChangeJson.revisions(ChangeJson.java:1247)
	at com.google.gerrit.server.change.ChangeJson.toChangeInfo(ChangeJson.java:604)
	at com.google.gerrit.server.change.ChangeJson.format(ChangeJson.java:339)
	at com.google.gerrit.server.change.ChangeJson.format(ChangeJson.java:330)
	at com.google.gerrit.server.change.ChangeJson.format(ChangeJson.java:309)
	at com.google.gerrit.server.change.GetChange.apply(GetChange.java:47)
	at com.google.gerrit.server.change.GetDetail.apply(GetDetail.java:50)
	at com.google.gerrit.server.change.GetDetail.apply(GetDetail.java:25)
	at com.google.gerrit.httpd.restapi.RestApiServlet.service(RestApiServlet.java:397)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:729)
	at com.google.inject.servlet.ServletDefinition.doServiceImpl(ServletDefinition.java:286)
	at com.google.inject.servlet.ServletDefinition.doService(ServletDefinition.java:276)
	at com.google.inject.servlet.ServletDefinition.service(ServletDefinition.java:181)
	at com.google.inject.servlet.ManagedServletPipeline.service(ManagedServletPipeline.java:91)
	at com.google.gerrit.httpd.raw.StaticModule$PolyGerritFilter.doFilter(StaticModule.java:483)
	at com.google.gerrit.httpd.GetUserFilter.doFilter(GetUserFilter.java:100)
	at com.google.gerrit.httpd.RunAsFilter.doFilter(RunAsFilter.java:122)
	at com.google.gwtexpui.server.CacheControlFilter.doFilter(CacheControlFilter.java:69)
	at com.google.gerrit.httpd.RequestMetricsFilter.doFilter(RequestMetricsFilter.java:57)
	at com.google.gerrit.httpd.AllRequestFilter$FilterProxy$1.doFilter(AllRequestFilter.java:133)
	at com.google.gerrit.httpd.AllRequestFilter$FilterProxy.doFilter(AllRequestFilter.java:135)
	at com.google.gerrit.httpd.RequestContextFilter.doFilter(RequestContextFilter.java:69)
	at com.google.inject.servlet.ManagedFilterPipeline.dispatch(ManagedFilterPipeline.java:120)
	at com.google.inject.servlet.GuiceFilter.doFilter(GuiceFilter.java:135)
	at org.eclipse.jetty.servlet.ServletHandler$CachedChain.doFilter(ServletHandler.java:1759)
	at org.eclipse.jetty.servlet.ServletHandler.doHandle(ServletHandler.java:582)
	at org.eclipse.jetty.server.session.SessionHandler.doHandle(SessionHandler.java:224)
	at org.eclipse.jetty.server.handler.ContextHandler.doHandle(ContextHandler.java:1180)
	at org.eclipse.jetty.servlet.ServletHandler.doScope(ServletHandler.java:512)
	at org.eclipse.jetty.server.session.SessionHandler.doScope(SessionHandler.java:185)
	at org.eclipse.jetty.server.handler.ContextHandler.doScope(ContextHandler.java:1112)
	at org.eclipse.jetty.server.handler.ScopedHandler.handle(ScopedHandler.java:141)
	at org.eclipse.jetty.server.handler.RequestLogHandler.handle(RequestLogHandler.java:56)
	at org.eclipse.jetty.server.handler.HandlerWrapper.handle(HandlerWrapper.java:134)
	at org.eclipse.jetty.server.Server.handle(Server.java:534)
	at org.eclipse.jetty.server.HttpChannel.handle(HttpChannel.java:320)
	at org.eclipse.jetty.server.HttpConnection.onFillable(HttpConnection.java:251)
	at org.eclipse.jetty.io.AbstractConnection$ReadCallback.succeeded(AbstractConnection.java:283)
	at org.eclipse.jetty.io.FillInterest.fillable(FillInterest.java:108)
	at org.eclipse.jetty.io.SelectChannelEndPoint$2.run(SelectChannelEndPoint.java:93)
	at org.eclipse.jetty.util.thread.strategy.ExecuteProduceConsume.executeProduceConsume(ExecuteProduceConsume.java:303)
	at org.eclipse.jetty.util.thread.strategy.ExecuteProduceConsume.produceConsume(ExecuteProduceConsume.java:148)
	at org.eclipse.jetty.util.thread.strategy.ExecuteProduceConsume.run(ExecuteProduceConsume.java:136)
	at org.eclipse.jetty.util.thread.QueuedThreadPool.runJob(QueuedThreadPool.java:671)
	at org.eclipse.jetty.util.thread.QueuedThreadPool$2.run(QueuedThreadPool.java:589)
	at java.lang.Thread.run(Thread.java:748)
通过排查，发现是某个插件没有升级到对应的2.15版本导致的。


```

#其他
有问题可以联系马哥，马哥的淘宝店:马哥私房菜 https://shop592330910.taobao.com/
更新于2018/7/19
