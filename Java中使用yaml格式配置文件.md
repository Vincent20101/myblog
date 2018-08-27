#yaml简介

```
YAML是“YAML不是一种标记语言”的外语缩写
“YAML Ain't Markup Language”

反正就是一种标记语言呗,她不像xml那样繁琐,有那么多标签,他的格式比较简单,以数据为中心,侧重点是数据.
```

先来一个yaml格式的配置
```
repo:
  GIT_ANDROID_ROOT: git/android/
  REPO_MANIFEST_ADDR: ssh://gerrit.example.com:29418/git/android/platform/manifest
  REPO_MANIFEST_BRANCH: master
  REPO_MANIFEST_FILE: default.xml
  REPO_MIRROR: /home/mirror
  REPO_GROUP: ''
  REPO_GROUP_AMSS: amss,common
  REPO_GROUP_ANDROID: all,-amss
build:
  ANDROID_TARGET_PRODUCT_LIST: civic
  ANDROID_BUILD_VARIANT_LIST: user
  ANDROID_TARGET_CARRIER_LIST: whole_netcom
  ANDROID_BUILD_TYPE: release
  ANDROID_EXTRA_BUILD_STEPS: null
  ANDROID_EXTRA_BUILD_COMBINATION: civic,eng,fn,android:civic,userdebug,fn,android
  ANNOUNCE_LIST: null

```

#如何使用java来读取写入yaml文件

```
这里使用snakeyaml这个模块来解析写入
maven的依赖坐标是:

<!-- https://mvnrepository.com/artifact/org.yaml/snakeyaml -->
<dependency>
    <groupId>org.yaml</groupId>
    <artifactId>snakeyaml</artifactId>
    <version>1.21</version>
</dependency>
```

读取是直接使用 yaml 对象中的load方法,会返回一个map对象的.然后遍历这个map得到自己想要的数据就可以了.
```
    private Map parseYaml(Path cf) {
        Yaml yaml = new Yaml();
        try {
            return yaml.load(new FileInputStream(cf.toFile()));
        } catch (FileNotFoundException e) {
            //e.printStackTrace();
        }
        return null;
    }
```

当然,也可以定义一个实体类

然后是写入,直接调用yaml对象的一个dump方法,他是可以把一个map写入为yaml格式文件的

```java
	//我这个方法就是修改上面那个yaml文件的 build下面的某些字段的.
    public void updateBuild(Path cf, Map<String, String> map) {
        String secn = "build";
        try {
            DumperOptions dumperOptions = new DumperOptions();
            dumperOptions.setDefaultFlowStyle(DumperOptions.FlowStyle.BLOCK);
            dumperOptions.setDefaultScalarStyle(DumperOptions.ScalarStyle.PLAIN);
            dumperOptions.setPrettyFlow(false);
            Yaml yaml = new Yaml(dumperOptions);
            Map m = yaml.load(new FileInputStream(cf.toFile()));
            if (m == null) {
                m = new LinkedHashMap();
            }
            if (m.containsKey(secn)) {
                Map<String, String> mm = (Map<String, String>) m.get(secn);
                mm.putAll(map);
            } else {
                m.put(secn, map);
            }
            yaml.dump(m, new OutputStreamWriter((new FileOutputStream(cf.toFile()))));
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
写入需要注意的地方是,要先把文件中的内容读取出来,然后添加 或 修改 map中的内容,然后把修改后的map在写入到文件.
如果不读取会把之前的配置给覆盖掉的.


#DumperOptions介绍
这里介绍一下DumperOptions的作用.
通过代码我们可以知道 yaml 文件有几种不同的格式,像上面那个就是平常我们比较喜欢用的格式.
在写入的时候我们要设置为 DumperOptions.FlowStyle.BLOCK 格式才行.
```
    public enum FlowStyle {
        FLOW(Boolean.TRUE), BLOCK(Boolean.FALSE), AUTO(null);

        private Boolean styleBoolean;

        private FlowStyle(Boolean flowStyle) {
            styleBoolean = flowStyle;
        }

        public Boolean getStyleBoolean() {
            return styleBoolean;
        }

        @Override
        public String toString() {
            return "Flow style: '" + styleBoolean + "'";
        }
    }

```

#DumperOptions的FlowStyle属性
DumperOptions.FlowStyle.BLOCK格式的样子
```
repo:
  GIT_ANDROID_ROOT: git/android/
  REPO_MANIFEST_ADDR: ssh://gerrit.example.com:29418/git/android/platform/manifest
  REPO_MANIFEST_BRANCH: master
  REPO_MANIFEST_FILE: default.xml
  REPO_MIRROR: /home/mirror
  REPO_GROUP: ''
  REPO_GROUP_AMSS: amss,common
  REPO_GROUP_ANDROID: all,-amss
build:
  ANDROID_TARGET_PRODUCT_LIST: zl1,civic
  ANDROID_BUILD_VARIANT_LIST: user
  ANDROID_TARGET_CARRIER_LIST: whole_netcom
  ANDROID_BUILD_TYPE: release
  ANDROID_EXTRA_BUILD_STEPS: null
  ANDROID_EXTRA_BUILD_COMBINATION: civic,eng,fn,android:civic,userdebug,fn,android
  ANNOUNCE_LIST: null
```

下面这个就是 DumperOptions.FlowStyle.FLOW 格式的.这种格式对我们来说看起来不太方便的.
```
{repo: {GIT_ANDROID_ROOT: git/android/, REPO_MANIFEST_ADDR: 'ssh://gerrit.example.com:29418/git/android/platform/manifest',
    REPO_MANIFEST_BRANCH: master, REPO_MANIFEST_FILE: default.xml, REPO_MIRROR: /home/mirror,
    REPO_GROUP: '', REPO_GROUP_AMSS: 'amss,common', REPO_GROUP_ANDROID: 'all,-amss'},
  build: {ANDROID_TARGET_PRODUCT_LIST: 'zl1,civic', ANDROID_BUILD_VARIANT_LIST: user,
    ANDROID_TARGET_CARRIER_LIST: whole_netcom, ANDROID_BUILD_TYPE: release, ANDROID_EXTRA_BUILD_STEPS: null,
    ANDROID_EXTRA_BUILD_COMBINATION: 'civic,eng,fn,android:civic,userdebug,fn,android',
    ANNOUNCE_LIST: null}}
    
```
DumperOptions.FlowStyle.AUTO 格式和 上面的FLOW估计是一样的,因为设置了也没见他变化.

#DumperOptions的ScalarStyle属性
DumperOptions中还有个ScalarStyle属性,可以设置数据外面是否加上引号等.其中有如下四种,默认是PLAIN的效果
```
DOUBLE_QUOTED('"'), 
SINGLE_QUOTED('\''), 
LITERAL('|'), 
PLAIN(null);

```


DumperOptions.ScalarStyle.DOUBLE_QUOTED 双引号
```
"repo":
  "GIT_ANDROID_ROOT": "git/android/"
  "REPO_MANIFEST_ADDR": "ssh://gerrit.example.com:29418/git/android/platform/manifest"
  "REPO_MANIFEST_BRANCH": "master"
  "REPO_MANIFEST_FILE": "default.xml"
  "REPO_MIRROR": "/home/mirror"
  "REPO_GROUP": ""
  "REPO_GROUP_AMSS": "amss,common"
  "REPO_GROUP_ANDROID": "all,-amss"
"build":
  "ANDROID_TARGET_PRODUCT_LIST": "zl1,civic"
  "ANDROID_BUILD_VARIANT_LIST": "user"
  "ANDROID_TARGET_CARRIER_LIST": "whole_netcom"
  "ANDROID_BUILD_TYPE": "release"
  "ANDROID_EXTRA_BUILD_STEPS": !!null "null"
  "ANDROID_EXTRA_BUILD_COMBINATION": "civic,eng,fn,android:civic,userdebug,fn,android"
  "ANNOUNCE_LIST": !!null "null"


```

DumperOptions.ScalarStyle.SINGLE_QUOTED 单引号

```

'repo':
  'GIT_ANDROID_ROOT': 'git/android/'
  'REPO_MANIFEST_ADDR': 'ssh://gerrit.example.com:29418/git/android/platform/manifest'
  'REPO_MANIFEST_BRANCH': 'master'
  'REPO_MANIFEST_FILE': 'default.xml'
  'REPO_MIRROR': '/home/mirror'
  'REPO_GROUP': ''
  'REPO_GROUP_AMSS': 'amss,common'
  'REPO_GROUP_ANDROID': 'all,-amss'
'build':
  'ANDROID_TARGET_PRODUCT_LIST': 'zl1,civic'
  'ANDROID_BUILD_VARIANT_LIST': 'user'
  'ANDROID_TARGET_CARRIER_LIST': 'whole_netcom'
  'ANDROID_BUILD_TYPE': 'release'
  'ANDROID_EXTRA_BUILD_STEPS': !!null 'null'
  'ANDROID_EXTRA_BUILD_COMBINATION': 'civic,eng,fn,android:civic,userdebug,fn,android'
  'ANNOUNCE_LIST': !!null 'null'

```
DumperOptions.ScalarStyle.FOLDED

```

"repo":
  "GIT_ANDROID_ROOT": >-
    git/android/
  "REPO_MANIFEST_ADDR": >-
    ssh://gerrit.example.com:29418/git/android/platform/manifest
  "REPO_MANIFEST_BRANCH": >-
    master
  "REPO_MANIFEST_FILE": >-
    default.xml
  "REPO_MIRROR": >-
    /home/mirror
  "REPO_GROUP": ""
  "REPO_GROUP_AMSS": >-
    amss,common
  "REPO_GROUP_ANDROID": >-
    all,-amss
"build":
  "ANDROID_TARGET_PRODUCT_LIST": >-
    zl1,civic
  "ANDROID_BUILD_VARIANT_LIST": >-
    user
  "ANDROID_TARGET_CARRIER_LIST": >-
    whole_netcom
  "ANDROID_BUILD_TYPE": >-
    release
  "ANDROID_EXTRA_BUILD_STEPS": !!null >-
    null
  "ANDROID_EXTRA_BUILD_COMBINATION": >-
    civic,eng,fn,android:civic,userdebug,fn,android
  "ANNOUNCE_LIST": !!null >-
    null

```

DumperOptions.ScalarStyle.LITERAL
```

"repo":
  "GIT_ANDROID_ROOT": |-
    git/android/
  "REPO_MANIFEST_ADDR": |-
    ssh://gerrit.example.com:29418/git/android/platform/manifest
  "REPO_MANIFEST_BRANCH": |-
    master
  "REPO_MANIFEST_FILE": |-
    default.xml
  "REPO_MIRROR": |-
    /home/mirror
  "REPO_GROUP": ""
  "REPO_GROUP_AMSS": |-
    amss,common
  "REPO_GROUP_ANDROID": |-
    all,-amss
"build":
  "ANDROID_TARGET_PRODUCT_LIST": |-
    zl1,civic
  "ANDROID_BUILD_VARIANT_LIST": |-
    user
  "ANDROID_TARGET_CARRIER_LIST": |-
    whole_netcom
  "ANDROID_BUILD_TYPE": |-
    release
  "ANDROID_EXTRA_BUILD_STEPS": !!null |-
    null
  "ANDROID_EXTRA_BUILD_COMBINATION": |-
    civic,eng,fn,android:civic,userdebug,fn,android
  "ANNOUNCE_LIST": !!null |-
    null

```

