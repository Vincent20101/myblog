Android下的配置管理之道之高通拆仓

马哥的淘宝店:https://shop592330910.taobao.com/



高通芯片平台的代码，一般可以分为android部分和amss部分。  
android部分很简单就谷歌那一整套的代码的。  
amss部分的代码属于高通自己的代码，算是私有代码。  不过这一部分代码仓库组织的很烂很烂。

amss这一部分 高通是把好多模块的代码分不同目录存放到一个git仓库里面了。导致这个git仓库十几GB的大小。
这个仓库的垃圾之处还在于有些不该加到git仓库里面的文件都加入了，例如一些编译生成的中间文件，
一些python执行的中间文件，每次编译这些文件都会被重新修改，真的很垃圾！！！shit～～～  

再来说这个amss的仓库，如下列出了某个amss仓库的目录结构，基本上一个目录就是一个模块啦。  
其中还有2个单独的文件about.html 和 contents.xml，这两个文件还是很重要的，about.html记录  
高通每个模块的版本信息的，非常重要的。contents.xml和编译各个模块有关系，也是比较重要的一个文件。    
其中还有一个比较特殊的目录LINUX/，这个是高通android侧的私有代码。应该放到android代码目录顶层   
下面的vendor/qcom/proprietary目录下的。   

鉴于以上种种原因，我们就需要对amss这个大仓库进行拆库操作了。

拆库有利用我们代码结构的整理，提交历史的整理。
每个模块单独一个仓库，研发提交可以不受影响各行其是。

马哥的淘宝店:https://shop592330910.taobao.com/



```bash

$ tree -L 1 -p  -F                                                                                         
.
├── [-rw-rw-r--]  about.html
├── [drwxrwxr-x]  adsp_proc/
├── [drwxrwxr-x]  aop_proc/
├── [drwxrwxr-x]  boot_images/
├── [drwxrwxr-x]  btfm_proc/
├── [drwxrwxr-x]  cdsp_proc/
├── [drwxrwxr-x]  common/
├── [-rwxrwxr-x]  contents.xml*
├── [drwxrwxr-x]  LINUX/
├── [drwxrwxr-x]  modem_proc/
├── [drwxrwxr-x]  slpi_proc/
├── [drwxrwxr-x]  spss_proc/
├── [drwxrwxr-x]  trustzone_images/
├── [drwxrwxr-x]  venus_proc/
├── [drwxrwxr-x]  wdsp_proc/
├── [drwxrwxr-x]  wigig_proc/
└── [drwxrwxr-x]  wlan_proc/

15 directories, 2 files
马哥的淘宝店:https://shop592330910.taobao.com/


```

通过上面我们基本上知道各个目录是上面了，也就知道该怎么拆库了。
基本上就是上面列出的每个目录拆库为一个小仓库，这些是amss侧各个模块的目录，   
LINUX/目录拆库为vendor/qcom/proprietary仓库。

下面给出命令
```bash
马哥的淘宝店:https://shop592330910.taobao.com/


# 首先我们要下载（下载使用git clone，第一次需要）或更新我们的amss代码，并且检出到对应的分支或者标签上（这里是r00455.2标签）。
cd amss && git fetch --all --tags && git checkout r00455.2
```


git subtree 命令拆库，在amss仓库顶层目录执行

通过git命令帮助我们能够指定subtree有个split的子命令，split英文意思就是拆分的意思。
```

       git-subtree - Merge subtrees together and split repository into subtrees


       git subtree add   -P <prefix> <commit>
       git subtree add   -P <prefix> <repository> <ref>
       git subtree pull  -P <prefix> <repository> <ref>
       git subtree push  -P <prefix> <repository> <ref>
       git subtree merge -P <prefix> <commit>
       git subtree split -P <prefix> [OPTIONS] [<commit>]
```

这里 -P 后面的需要拆库的目录，  -b 后面是拆库后把这些代码分支提交存放到哪个分支上。（这个分支要是一个不存在的分支）   
这里我们每个模块就取模块名称加上master组成我们的分支名称。

```bash
马哥的淘宝店:https://shop592330910.taobao.com/


git subtree split -P trustzone_images -b trustzone_images_master

```
执行完上面的命令后，amss仓库下面就会多一个本地分支 trustzone_images_master 这个分支就包含了   
目录 trustzone_images里面的所有代码，已经所有提交历史。



最后我们只需要把这个本地分支推送到gerrit代码服务器上对应的仓库就可以了
```bash
马哥的淘宝店:https://shop592330910.taobao.com/


git push ssh://gerrit.com:29418/git/android/AMSS/trustzone_images  trustzone_images_master:r00455.2 -f

```
这里我们amss侧的仓库都放到这样的路径下面 git/android/AMSS/。
推送到上面起个上面分支可以自己决定。一般     高通平台_r00455.2_rebase_日期  这样来命名这个分支。

其他的目录以此类推。


然后在给个  LINUX/ 拆库的命令
```bash
马哥的淘宝店:https://shop592330910.taobao.com/


git subtree split -P LINUX/android/vendor/qcom/proprietary -b proprietary_master

git push ssh://gerrit.com:29418/git/android/platform/vendor/qcom/proprietary proprietary_master:r00455.2 -f

```
这里LINUX/ 拆库的目录要到最里面的，不要值到LINUX/这一级别。要到LINUX/android/vendor/qcom/proprietary。    
存放到gerrit上的路径也建议是 git/android/platform/vendor/qcom/proprietary。   

git/android/ 下面都是我们代码仓库。当然不同公司可能组织代码仓库目录结构不太一样，有的可能开头叫个qcom/  ,或者有些就没有个开头的目录。

还是建议把所有的git仓库分分类，整理干净。



最后这个about.html 和 contents.xml  文件在单独提交一个patch到common仓库下面。


关于proprietary仓库，也可以再进行一次拆库，这个和amss的类似，就是每个子目录分别拆库放到不同的仓库下面。

最后能有个脚本自动，然后最后把拆库后的给个仓库是revision 信息打印一下，然后 组织的manifest也打印一下。
```bash
马哥的淘宝店:https://shop592330910.taobao.com/


         trustzone_images ['9dc0fd4591ceffce056f38c71d702d80c6c184ab', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                 aop_proc ['06aaa952c6aa549dcc78874e3a791dc051229181', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                spss_proc ['fed2fd8a54f97453195032f124835e0a9957b50f', 'Commit label r00446.1 - Post-CS6 0.0.446.1']
               modem_proc ['331caebb7ab6b68f203fcd1633f272c92a436b78', 'Commit label r00455.2.139522.1.139771.3 - Post-CS7 0.0.455.2.139522.1.139771.3']
              boot_images ['ea97b9aaa34c959eaf83e4c74f2bbc231a4e4b6c', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
               venus_proc ['c64930e9346a4de402ec05beb44bef89c0e34b65', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                   common ['028afc65f556c681cc7b597374065517a3b9632d', 'Commit label r00455.2.139522.1.139771.3 - Post-CS7 0.0.455.2.139522.1.139771.3']
                slpi_proc ['0b8c16d09c40b5593ac7643282f3392ad2d12629', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                    LINUX ['9ed75e0dc096e7c7935134b9c444233005102e8f', 'Commit label r00455.2.139522.1.139771.3 - Post-CS7 0.0.455.2.139522.1.139771.3']
                adsp_proc ['b2c789928e38fe434f8d9df87806d99d5effe3aa', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                btfm_proc ['1280aa122fcd766ccf1bbfa1ea7eb1de25a38585', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                wlan_proc ['674a8a55269bef51abb4e99f5ed3793cc0ca836c', 'Commit label r00455.2.139522.1.139771.3 - Post-CS7 0.0.455.2.139522.1.139771.3']
               wigig_proc ['67dd08d91caad71284c475a53787bb9372e149e5', 'Commit label r004361.1 - Post-CS5 0.0.4361.1']
                wdsp_proc ['775c398d1590cf18605f90e7fba2582743d1d73f', 'Commit label r004361.1 - Post-CS5 0.0.4361.1']
                cdsp_proc ['47870d27ab8d7dff3e5e414cfef45138de2c5021', 'Commit label r00455.2 - Post-CS7 0.0.455.2']
                马哥的淘宝店:https://shop592330910.taobao.com/

  <project name="AMSS/trustzone_images" path="AMSS/trustzone_images" revision="9dc0fd4591ceffce056f38c71d702d80c6c184ab" />
  <project name="AMSS/aop_proc" path="AMSS/aop_proc" revision="06aaa952c6aa549dcc78874e3a791dc051229181" />
  <project name="AMSS/spss_proc" path="AMSS/spss_proc" revision="fed2fd8a54f97453195032f124835e0a9957b50f" />
  <project name="AMSS/modem_proc" path="AMSS/modem_proc" revision="331caebb7ab6b68f203fcd1633f272c92a436b78" />
  <project name="AMSS/boot_images" path="AMSS/boot_images" revision="ea97b9aaa34c959eaf83e4c74f2bbc231a4e4b6c" />
  <project name="AMSS/venus_proc" path="AMSS/venus_proc" revision="c64930e9346a4de402ec05beb44bef89c0e34b65" />
  <project name="AMSS/common" path="AMSS/common" revision="028afc65f556c681cc7b597374065517a3b9632d" />
  <project name="AMSS/slpi_proc" path="AMSS/slpi_proc" revision="0b8c16d09c40b5593ac7643282f3392ad2d12629" />
  <project name="platform/vendor/qcom/proprietary" path="vendor/qcom/proprietary" revision="9ed75e0dc096e7c7935134b9c444233005102e8f" />
  <project name="AMSS/adsp_proc" path="AMSS/adsp_proc" revision="b2c789928e38fe434f8d9df87806d99d5effe3aa" />
  <project name="AMSS/btfm_proc" path="AMSS/btfm_proc" revision="1280aa122fcd766ccf1bbfa1ea7eb1de25a38585" />
  <project name="AMSS/wlan_proc" path="AMSS/wlan_proc" revision="674a8a55269bef51abb4e99f5ed3793cc0ca836c" />
  <project name="AMSS/wigig_proc" path="AMSS/wigig_proc" revision="67dd08d91caad71284c475a53787bb9372e149e5" />
  <project name="AMSS/wdsp_proc" path="AMSS/wdsp_proc" revision="775c398d1590cf18605f90e7fba2582743d1d73f" />
  <project name="AMSS/cdsp_proc" path="AMSS/cdsp_proc" revision="47870d27ab8d7dff3e5e414cfef45138de2c5021" />
  马哥的淘宝店:https://shop592330910.taobao.com/



```


下面给出一个python版本的拆库自动脚本，放到jenkins上面，填上参数，自动执行。
```python
马哥的淘宝店:https://shop592330910.taobao.com/


#!/usr/bin/env python
# coding:utf-8

import os
import commands
import sys
import shutil
import optparse

# 这里import aais 脚本里面的一些方法
from aais.log import Log
from aais.utils import Utils


class SubtreeGitError(Exception):
    pass


class SubtreeGit(object):马哥的淘宝店:https://shop592330910.taobao.com/


    def __init__(self, basepath, srcbranch, targetpath, targetbranch):
        dry_run = os.environ.get("DRY_RUN", "true").strip()
        dry_run = True if dry_run == "true" else False
        self.dryrun = dry_run

        self.basepath = basepath
        self.srcbranch = srcbranch

        self.targetpath = targetpath
        self.targetbranch = targetbranch

        self.qcom_base = os.path.basename(basepath)

        self.proprietary_project = "git/android/platform/vendor/qcom/proprietary"
        self.gerrit_host = "gerrit.example.com"
        self.gerrit_port = "29418"

    def CloneGit(self):马哥的淘宝店:https://shop592330910.taobao.com/


        if os.path.exists(self.qcom_base) and os.path.isdir(self.qcom_base):
            cmd = "cd %s && git fetch --all --tags && git checkout %s" % (self.qcom_base, self.srcbranch)
        else:
            cmd = "git clone ssh://%s:%s/%s %s && cd %s && git checkout %s" % (self.gerrit_host, self.gerrit_port,
                                                                               self.basepath, self.qcom_base,
                                                                               self.qcom_base, self.srcbranch)

        Log.Info("will clone/fetch base git and checkout to branch: %s, cmd: %s" % (self.srcbranch, cmd))
        ret = os.system(cmd)
        if ret != 0:
            raise SubtreeGitError("git clone/fetch/checkout fail")

    # 列出分仓列表
    def GetRepertory(self, config=""):
        if config == "":
            repertory_L = os.listdir(self.qcom_base)  # 列出当前目录下面的所有内容
            repertory_L = [basedir.strip() for basedir in repertory_L if
                           os.path.isdir(os.path.join(self.qcom_base, basedir))
                           and not basedir.startswith(".")]
            repertory_D = dict(zip(repertory_L, repertory_L))
            return repertory_D
        else:
            repertory_D = Utils.StringToDict(config, firstreg=",|;", sedondreg=":")  # 字符串转换为字典
            return repertory_D

    def CreatePropject(self, targetpath, targetbranch, repertory_L):
        # 获取gerrit上相关仓库
        cmd = "ssh -p %s %s gerrit ls-projects | grep -E \"%s|%s\"  " % (self.gerrit_port, self.gerrit_host,
                                                                         targetpath, self.proprietary_project)
        Log.Info("list project on gerrit: %s" % cmd)

        status, output = commands.getstatusoutput(cmd)
        if status == 0:
            project_L = output.splitlines()

            create_L = []
            for repertory in repertory_L:
                project = os.path.join(targetpath, repertory)
                if repertory == "LINUX" or repertory == "proprietary":
                    project = self.proprietary_project
                if project not in project_L:
                    create_L.append(project)

            for basedir in create_L:
                cmd = "ssh -p %s %s gerrit create-project %s --empty-commit --parent Permission_parent/All-bsp " \
                      "--submit-type REBASE_IF_NECESSARY --branch %s" % (
                      self.gerrit_port, self.gerrit_host, basedir, targetbranch)
                Log.Info("will create project: %s" % cmd)
                if not self.dryrun and os.system(cmd) != 0:
                    Log.Error("crate %s failed" % (basedir))
                    raise SubtreeGitError("create project fail")

    # print each repertory log for manifest
    def PrintGitLog(self, repertory_L):马哥的淘宝店:https://shop592330910.taobao.com/



        Log.Info("The repertory info list is:")
        manifest_str = ""
        for repertory in repertory_L:
            if repertory == "LINUX" or repertory == "proprietary":
                log_cmd = "git log -1 -b proprietary_master --pretty=\"%H<gitlog>%s\""
            else:
                local_branch = repertory.replace("/", "__")
                log_cmd = "git log -1 -b %s_master --pretty=\"%%H<gitlog>%%s\"" % (local_branch)
            status, output = commands.getstatusoutput(log_cmd)

            if status == 0:
                temp_L = output.strip().split("<gitlog>")
                Log.Red("%25s %s\n" % (repertory, temp_L))
                git_commit_id = temp_L[0]

                if self.qcom_base == "proprietary": # 表明是proprietary需要分仓
                    manifest_str += "  <project name=\"platform/vendor/qcom/proprietary/%s\" path=\"vendor/qcom/proprietary/%s\" revision=\"%s\" />\n" % (
                        repertory, repertory, git_commit_id)
                else:
                    #　其他的表示 高通的大仓库需要分仓
                    if repertory == "LINUX" or repertory == "proprietary":
                        manifest_str += "  <project name=\"platform/vendor/qcom/proprietary\" path=\"vendor/qcom/proprietary\" revision=\"%s\" />\n" % (
                        git_commit_id)
                    else:
                        manifest_str +="  <project name=\"AMSS/%s\" path=\"AMSS/%s\" revision=\"%s\" />\n" % (
                        repertory, repertory, git_commit_id)

        Log.Blue("\n%s\n" % manifest_str)

    # 分仓
    def SubtreeRepertory(self, config=""):马哥的淘宝店:https://shop592330910.taobao.com/


        self.CloneGit()

        repertory_D = self.GetRepertory(config)
        self.CreatePropject(self.targetpath, self.targetbranch, repertory_D.keys())

        os.chdir(self.qcom_base)

        cmd = 'git branch -D $(git for-each-ref --format="%(refname:short)" refs/heads/\*_master)'
        ret = os.system(cmd)
        if ret != 0:
            Log.Error("delete branch fail")

        for (repertory, basedir) in repertory_D.items():
            # sepcial for proprietary and build 对于特殊的LINUX目录需要特殊处理
            if repertory == "LINUX" or repertory == "proprietary":
                if repertory == "LINUX":  # 这里等于LINUX的时候表明是没有填写配置文件的
                    basedir = "LINUX/android/vendor/qcom/proprietary"
                cmd = "git subtree split -P %s -b proprietary_master" % (basedir)
                Log.Info("subtree proprietary cmd: %s" % cmd)
                ret = os.system(cmd)
                if not self.dryrun and os.system(cmd) != 0:
                    raise SubtreeGitError("subtree split git fail")

                cmd = "git push ssh://%s:%s/%s proprietary_master:%s -f" % (
                self.gerrit_host, self.gerrit_port, self.proprietary_project, self.targetbranch)
                Log.Info("will proprietary push to gerrit: %s" % cmd)
                if not self.dryrun and os.system(cmd) != 0:
                    raise SubtreeGitError("push proprietary to gerrit fail")
            else:
                local_branch = repertory.replace("/", "__")
                cmd = "git subtree split -P %s -b %s_master" % (basedir, local_branch)
                Log.Info("subtree cmd: %s" % cmd)
                ret = os.system(cmd)
                if not self.dryrun and os.system(cmd) != 0:
                    raise SubtreeGitError("subtree split git fail")

                cmd = "git push ssh://%s:%s/%s/%s  %s_master:%s -f" % (
                self.gerrit_host, self.gerrit_port, self.targetpath, repertory, local_branch, self.targetbranch)
                Log.Info("will push to gerrit: %s" % cmd)
                if not self.dryrun and os.system(cmd) != 0:
                    raise SubtreeGitError("push to gerrit fail")

        self.PrintGitLog(repertory_D.keys())


def parseargs():马哥的淘宝店:https://shop592330910.taobao.com/


    usage = "usage: [options] arg1 arg2"
    parser = optparse.OptionParser(usage=usage)

    optiongroup = optparse.OptionGroup(parser, "common options")
    optiongroup.add_option("", "--base-path", dest="basepath", help="git base path", default="")
    optiongroup.add_option("", "--target-path", dest="targetpath", help="git push target path", default="")

    optiongroup.add_option("", "--src-branch", dest="srcbranch", help="srcbranch", default="")
    optiongroup.add_option("", "--target-branch", dest="targetbranch", help="targetbranch", default="")

    optiongroup.add_option("", "--config", dest="config", help="config", default="")

    parser.add_option_group(optiongroup)

    (options, args) = parser.parse_args()

    return (options, args)


def main():马哥的淘宝店:https://shop592330910.taobao.com/


    (options, args) = parseargs()
    basepath = options.basepath.strip()
    srcbranch = options.srcbranch.strip()  # 源仓库 git分支

    targetpath = options.targetpath.strip()  # 最后需要push到哪个仓库下面
    targetbranch = options.targetbranch.strip()  # 最后需要push到哪个分支上面

    config = options.config.strip()

    sub = SubtreeGit(basepath, srcbranch, targetpath, targetbranch)

    sub.SubtreeRepertory(config)


if __name__ == "__main__":马哥的淘宝店:https://shop592330910.taobao.com/


    # need flush print
    sys.stdout = sys.stderr
    main()


```


jenkins 上面job里面写入下面的调用方式

```bash
#!/bin/bash -x
马哥的淘宝店:https://shop592330910.taobao.com/

export PYTHONPATH=aais

python tools/python/subtree_qcom.py  \
--base-path "$BASE_PATH" --src-branch "$SRC_BRANCH" \
--target-path "$TARGET_PATH" --target-branch "$TARGET_BRANCH" \
--config "$CONFIG"

```

其中配置带参数的job，参数有

* BASE_PATH
需要分仓的仓库地址：

例如： AMSS地址为 git/shared/qcom/snapdragon-high-med-2016-spf-2-0_amss_standard_oem    
properity 为 git/android/platform/vendor/qcom/proprietary   

这个地址会拼接成一个完整的url给git clone命令使用的。   

* SRC_BRANCH

高通的release tag点或者proprietary 仓库的revision值，用于git checkout  


* TARGET_PATH

分仓之后的gerrit路径,就是最后要push到哪个project下面,主要是为AMSS提交地址做区分：

例如： AMSS 对应为 git/android/AMSS ，modem_proc 就会push到 git/android/AMSS/modem_proc下面。

proprietary 为 git/android/platform/vendor/qcom/proprietary。

填写的是project的 base path，不包括最后的 那个路径


* TARGET_BRANCH

分仓之后生成的分支名，分仓之后需要push到哪个分支下面  ，就是push到gerrit服务器上面起的分支名称。


* CONFIG

默认为空。

马哥的淘宝店:https://shop592330910.taobao.com/


但是有些amss仓库里面的子目录不正常，是多个平台共用的，跟正常的差太多了，  
所以需要通过上传config来做特殊处理。后续遇到特殊情况，也可以用这个方法。

前面部分是仓库名，后面部分是地址。中间用冒号分隔，后面用分号，注意都是英文符号。

注意最后一行不要加分号,两行之间不要留空行

common:MSM8953.LA.2.0/Common;

proprietary:LA.UM.5.6/LINUX/android/vendor/qcom/proprietary;

adsp_proc:ADSP.8953.2.8.2/adsp_proc;

rpm_proc:RPM.BF.2.4/rpm_proc;

wcnss_proc:CNSS.PR.4.0/wcnss_proc;

cpe_proc:CPE.TSF.1.0/cpe_proc;

trustzone_images:TZ.BF.4.0.5/trustzone_images;

boot_images:BOOT.BF.3.3/boot_images;

modem_proc:MPSS.TA.2.3/modem_proc;

venus_proc:VIDEO.VE.4.2/venus_proc

马哥的淘宝店:https://shop592330910.taobao.com/


config 如果填写 这个仓库一定是proprietary:LA.UM.5.6/LINUX/android/vendor/qcom/proprietary; 关键字一定是proprietary
