这个主要用来 比较 主gerit 和从 gerrit上 对应的仓库，对应的分支 是否存在差异


最近发现 gerrit的replication插件 有点问题，有些仓库的分支不能正常同步到从gerrit上。


```bash
function list_refs_diff(){
    basepath="/git/git/android" #需要比较的仓库的基准路径

    projects=$(find "$basepath"  -name "*.git" -type d )    #查找目录下面的所有git仓库路径

    for pro in $projects       # 遍历这些仓库
    do
        #echo "=$pro="
        master=$(git -C "$pro" for-each-ref "refs/heads/")  # 取出所有分支列表和分支对应的commit id

        sh=$(ssh -p 22 gerrit2@gerrit-sh.mage.com "git -C $pro for-each-ref refs/heads/") # 取出从gerrit上的所有分支列表和分支对应的commit id

        wx=$(ssh -p 22 gerrit2@gerrit-wx.mage.com "git -C $pro for-each-ref refs/heads/")

        #echo "=diff master sh="
        diff <(echo "$master") <(echo "$sh")  ||  echo "==$pro is diff master sh"   # 如果有差异就会执行后面的echo命令的，diff命令是有差异返回1，没有差异返回0.

        #echo "=diff master wx="
        diff <(echo "$master") <(echo "$wx")  ||  echo "==$pro is diff master wx" 
        #echo "=end="
    done
}

list_refs_diff

```
对于之前的代码是每次比较一个仓库都是取执行一下ｓｓｈ命令。这样比较慢。这里我们采用把所有的结果一次返回。只会执行一次ｓｓｈ连接。

最后来一个改进版本的。
```bash
#!/bin/bash

function printcolor() {
    printf "\033[1;33m[info]\033[0m   \033[1;31m$*\n\033[0m"
}

function fast_refs_diff(){
    basepath="/home/gerrit2/review_site/git/git/android"
    projectroot="/home/gerrit2/review_site/git/"

    gerrit_master="gerrit2@gerrit.mage.com"
    gerrit_sh="gerrit2@gerrit-sh.mage.com"
    gerrit_wx="gerrit2@gerrit-wx.mage.com"


    ma=$(for p in $(find $basepath  -name *.git -type d | sort ); do  echo $p ;git -C $p for-each-ref   --format="%(objectname) SPC %(objecttype) TAB %(refname) DIR ${p}"  refs/heads/; done; )

    sh=$(ssh -p 22 ${gerrit_sh}  "for p in \$(find $basepath  -name \*.git -type d | sort ); do  echo \$p ;git -C \$p for-each-ref  --format=\"%(objectname) SPC %(objecttype) TAB %(refname) DIR \${p}\"    refs/heads/;  done; ")

    wx=$(ssh -p 22 ${gerrit_wx} "for p in \$(find $basepath  -name \*.git -type d | sort ); do  echo \$p ;git -C \$p for-each-ref --format=\"%(objectname) SPC %(objecttype) TAB %(refname) DIR \${p}\"   refs/heads/;  done; ")

    d1=$(diff <(echo "$ma") <(echo "$sh") )  # master 和  sh 服务器差异
    printcolor  "---master--sh--diff---------------------------------------------------------------------------------"
    printcolor "\n$d1"
    projects1=$(echo "$d1" |  grep "DIR.*.git" -o | uniq | awk -F "$projectroot" '{print $2}' | awk -F "\\\\.git" '{print $1}' )

    if [ -n "$projects1" ]; then 
        printcolor "diff  master to sh  is not empty" 
        for p in  $projects1
        do
            printcolor "= replication start =$p=="
            ssh  -p 29418  ${gerrit_master}   replication start ${p}  --url  "gerrit-sh"
            sleep 10
        done
    fi


    d2=$(diff <(echo "$ma") <(echo "$wx") )  # master 和  wx 服务器差异
    printcolor  "---master--wx--diff---------------------------------------------------------------------------------"
    printcolor "\n$d2"
    projects2=$(echo "$d2" |  grep "DIR.*.git" -o | uniq | awk -F "$projectroot" '{print $2}' | awk -F "\\\\.git" '{print $1}' )

    if [ -n "$projects2" ]; then 
        printcolor "diff  master to wx  is not empty" 
        for p in  $projects2
        do
            printcolor "= replication start =$p=="
            ssh  -p 29418  ${gerrit_master}   replication start ${p}  --url  "gerrit-wx"
            sleep 10
        done
    fi
}


ssh gerrit.mage.com -p 29418

fast_refs_diff



```


记录一下为什么会有这个脚本。

```bash
最近发现 gerrit的replication插件 有点问题，有些仓库的分支不能正常同步到从gerrit上。

如果执行 replication start --all 然后看  gerrit-show-queue 会发现 有很多 push的任务，这个看着是很正常的。

但是过了一会 在执行 gerrit-show-queue 会发现 这些push的任务都没有了。

通过后台replication 日志文件发现 有一句这个 "Cancelled 1846 replication events during shutdown" ， 看着挺像是这些任务被取消了。

通过查看后台error 日志 还发现 “Reloading plugin replication”  “Unloading plugin replication, version v2.12”  
Reloaded plugin replication, version v2.12 ，看样子插件总是被重新加载，
这个插件重新加载 默认是1分钟就会执行一下。

目前还不确定是否和这个插件重新被加载有关系。 

```

```bash
my gerrit version is 2.12 and replication plugin is 2.12

my config:

mage@gerrit-master:/home/gerrit2/review_site$ cat etc/replication.config
[gerrit]
    defaultForceUpdate = true
    autoReload = true

[remote "gerrit-sh"]
    url  = gerrit2@gerrit-sh-mage-com:/home/gerrit2/review_site/git/${name}.git
    projects = "^git/android/.*"
 
    projects = "^Permission_parent/.*"
    projects = "All-Projects"
    projects = "All-Users"

    push = +refs/*:refs/*
    mirror = true
    replicatePermissions=true
    threads = 16
    replicationDelay = 5

[remote "gerrit-wx"]
    url  = gerrit2@gerrit-wx-mage-com:/home/gerrit2/review_site/git/${name}.git
    projects = "^git/android/.*"
    projects = "^Permission_parent/.*"
    projects = "All-Projects"
    projects = "All-Users"

    push = +refs/heads/*:refs/heads/*
    push = +refs/users/*:refs/users/*
    push = +refs/meta/*:refs/meta/*
    push = +refs/tags/*:refs/tags/*

    mirror = true
    replicatePermissions=true
    threads = 6
    replicationDelay = 5

```


```bash
[2018-07-18 10:23:09,884] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:23:09,918] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1021_3860692822570197640.jar
[2018-07-18 10:23:09,918] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1022_4762025572797757776.jar
[2018-07-18 10:24:10,579] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:24:10,613] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:24:10,613] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:25:12,058] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:25:12,097] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:25:12,097] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:25:12,134] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1023_5011203833786628342.jar
[2018-07-18 10:25:12,154] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1024_245704929299728553.jar
[2018-07-18 10:26:12,730] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:26:12,776] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:26:12,777] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:27:14,460] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:27:14,500] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:27:14,500] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:27:14,552] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1025_2452805446666057698.jar
[2018-07-18 10:27:14,552] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1026_6488649232714175918.jar
[2018-07-18 10:28:15,031] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:28:15,061] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:28:15,061] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:29:16,510] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:29:16,545] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:29:16,545] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:29:16,611] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1027_6972588421229996675.jar
[2018-07-18 10:29:16,635] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1028_8705235427221148549.jar
[2018-07-18 10:30:17,188] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:30:17,226] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:30:17,226] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:31:19,808] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:31:19,841] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:31:19,841] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:31:19,885] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1029_7220565993763881433.jar
[2018-07-18 10:31:19,885] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1030_3211410806584102420.jar
[2018-07-18 10:32:20,368] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:32:20,399] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:32:20,399] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:33:23,171] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:33:23,202] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:33:23,202] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:33:23,270] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1031_3532051622204535818.jar
[2018-07-18 10:33:23,293] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1032_3518576603210188688.jar
[2018-07-18 10:34:23,756] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:34:23,787] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:34:23,787] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:35:26,374] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:35:26,435] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:35:26,436] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:35:26,490] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1033_3173290789620809060.jar
[2018-07-18 10:35:26,490] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1034_3243258291234611954.jar
[2018-07-18 10:36:27,073] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:36:27,117] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:36:27,118] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:37:28,565] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:37:28,599] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:37:28,599] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:37:28,653] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1035_7690035941061570015.jar
[2018-07-18 10:37:28,683] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1036_1818915235514519933.jar
[2018-07-18 10:38:29,210] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:38:29,243] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:38:29,243] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:39:32,099] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:39:32,131] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:39:32,132] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:39:32,166] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1037_7816220041072392883.jar
[2018-07-18 10:39:32,166] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1038_9091179206012022809.jar
[2018-07-18 10:40:32,717] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:40:32,752] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:40:32,753] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:41:34,278] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:41:34,310] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:41:34,311] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:41:34,351] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1039_5955426085462605117.jar
[2018-07-18 10:41:34,367] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1040_3341707177051832324.jar
[2018-07-18 10:42:34,991] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:42:35,021] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:42:35,022] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:43:36,549] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:43:36,582] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:43:36,583] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:43:36,633] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1041_6120575548543048800.jar
[2018-07-18 10:43:36,633] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1042_2088089649518781718.jar
[2018-07-18 10:44:37,241] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:44:37,271] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:44:37,272] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:45:38,942] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:45:38,974] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:45:38,974] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:45:39,042] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1043_3852219101794057249.jar
[2018-07-18 10:45:39,067] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1044_3327491795138948697.jar
[2018-07-18 10:46:39,564] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:46:39,605] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:46:39,605] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:47:41,015] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:47:41,048] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:47:41,048] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:47:41,094] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1045_2811963957141055977.jar
[2018-07-18 10:47:41,094] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1046_273525048384380240.jar
[2018-07-18 10:48:41,641] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:48:41,700] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:48:41,700] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:49:43,206] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:49:43,238] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:49:43,239] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:49:43,294] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1047_272666651402220329.jar
[2018-07-18 10:49:43,319] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1048_1886131061602645452.jar
[2018-07-18 10:50:43,829] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:50:43,862] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:50:43,863] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:51:46,575] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:51:46,605] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:51:46,606] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:51:46,634] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1049_3625723034051752540.jar
[2018-07-18 10:51:46,634] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1050_4805579986654259071.jar
[2018-07-18 10:52:47,210] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:52:47,240] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:52:47,240] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:53:48,702] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:53:48,734] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:53:48,735] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:53:48,776] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1051_356271207300496626.jar
[2018-07-18 10:53:48,789] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1052_7203444530048515907.jar
[2018-07-18 10:54:49,301] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:54:49,345] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:54:49,345] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:55:50,868] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:55:50,900] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:55:50,901] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:55:50,957] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1053_7545488379288996127.jar
[2018-07-18 10:55:50,957] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1054_3479486679409250223.jar
[2018-07-18 10:56:51,494] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:56:51,530] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:56:51,530] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:57:54,337] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:57:54,370] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:57:54,370] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:57:54,413] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1055_5399760820856320355.jar
[2018-07-18 10:57:54,426] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1056_4845128901276667838.jar
[2018-07-18 10:58:55,091] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:58:55,127] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:58:55,127] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:59:56,587] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 10:59:56,618] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 10:59:56,618] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 10:59:56,666] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1057_7334641974075376717.jar
[2018-07-18 10:59:56,666] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1058_7795989231640053285.jar
[2018-07-18 11:00:57,265] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:00:57,301] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:00:57,301] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:01:59,656] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:01:59,692] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:01:59,692] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:01:59,735] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1059_9195424758840757280.jar
[2018-07-18 11:01:59,752] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1100_8240248555821628526.jar
[2018-07-18 11:03:00,205] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:03:00,237] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:03:00,237] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:04:02,836] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:04:02,888] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:04:02,888] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:04:02,931] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1101_3124561644333642277.jar
[2018-07-18 11:04:02,931] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1103_2841037657025152082.jar
[2018-07-18 11:05:03,434] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:05:03,466] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:05:03,467] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:06:04,854] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:06:04,885] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:06:04,886] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:06:04,942] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1104_4121034320161072034.jar
[2018-07-18 11:06:04,968] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1105_2364929244244494681.jar
[2018-07-18 11:07:05,404] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:07:05,434] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:07:05,434] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:08:06,837] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:08:06,875] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:08:06,875] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:08:06,928] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1106_2094512482721418530.jar
[2018-07-18 11:08:06,928] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1107_3795519196444990009.jar
[2018-07-18 11:09:07,427] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:09:07,458] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:09:07,458] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:10:08,990] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:10:09,038] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:10:09,038] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:10:09,093] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1108_4399043180016575749.jar
[2018-07-18 11:10:09,117] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1109_6422650863394829569.jar
[2018-07-18 11:11:09,603] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:11:09,634] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:11:09,634] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:12:12,284] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:12:12,314] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:12:12,315] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:12:12,360] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1110_1067719430772915137.jar
[2018-07-18 11:12:12,360] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1111_9004235982263492860.jar
[2018-07-18 11:13:13,073] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:13:13,106] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:13:13,107] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:14:14,549] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:14:14,584] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:14:14,585] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:14:14,637] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1112_2626749900708177879.jar
[2018-07-18 11:14:14,664] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1113_6772125476075781558.jar
[2018-07-18 11:15:15,113] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:15:15,144] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:15:15,145] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:16:17,719] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:16:17,751] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:16:17,752] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:16:17,803] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1114_252533825523289002.jar
[2018-07-18 11:16:17,803] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1115_8832595381680391936.jar
[2018-07-18 11:17:18,369] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:17:18,407] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:17:18,408] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:18:19,918] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:18:19,950] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:18:19,950] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:18:20,010] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1116_179748246267773948.jar
[2018-07-18 11:18:20,037] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1117_1689738563632955983.jar
[2018-07-18 11:19:20,480] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:19:20,511] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:19:20,511] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:20:22,221] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:20:22,255] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:20:22,255] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:20:22,277] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1118_4760025156322457987.jar
[2018-07-18 11:20:22,277] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1119_6829261146704435143.jar
[2018-07-18 11:21:22,820] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:21:22,851] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:21:22,851] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:22:24,415] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:22:24,455] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:22:24,455] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:22:24,506] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1120_1744063963651337061.jar
[2018-07-18 11:22:24,531] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1121_8162224298569579146.jar
[2018-07-18 11:23:24,984] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:23:25,016] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:23:25,016] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:24:26,568] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloading plugin replication
[2018-07-18 11:24:26,600] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Unloading plugin replication, version v2.12
[2018-07-18 11:24:26,600] [PluginScanner] INFO  com.google.gerrit.server.plugins.PluginLoader : Reloaded plugin replication, version v2.12
[2018-07-18 11:24:26,647] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1122_1421263321538922666.jar
[2018-07-18 11:24:26,647] [WorkQueue-1] INFO  com.google.gerrit.server.plugins.PluginLoader : Cleaned plugin plugin_replication_180718_1123_5099118356426659978.jar

```


```bash

[2018-07-18 08:31:59,369] [] Cancelled 1846 replication events during shutdown
[2018-07-18 08:38:08,417] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:04:35,912] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:06:37,898] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:08:39,583] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:10:41,440] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:12:43,431] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:14:45,433] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:16:47,558] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:18:49,557] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:20:52,536] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:22:55,703] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:24:57,796] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:27:00,929] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:29:04,101] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:31:06,238] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:33:08,280] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:35:10,223] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:37:11,913] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:39:13,830] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:41:15,833] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:43:17,994] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:45:21,303] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:47:23,274] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:49:25,355] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:51:27,510] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:53:30,610] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:54:32,082] [] Cancelled 2 replication events during shutdown
[2018-07-18 09:55:32,798] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:11:53,227] [] Cancelled 1 replication events during shutdown
[2018-07-18 10:13:56,721] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:15:58,860] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:18:02,562] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:36:27,118] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:38:29,243] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:40:32,753] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:42:35,022] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:43:36,582] [] Cancelled 2 replication events during shutdown
[2018-07-18 10:44:37,272] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:05:03,467] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:07:05,434] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:09:07,458] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:11:09,634] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:13:13,107] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:15:15,144] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:17:18,408] [] Cancelled 2 replication events during shutdown
[2018-07-18 11:19:20,511] [] Cancelled 2 replication events during shutdown

```
