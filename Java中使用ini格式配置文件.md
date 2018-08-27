#ini简介


>ini格式配置文件,这种配置文件在win系统上很常见.  
>还有git的配置文件也很类似ini的,不过还是不太一样的.


```
[repo]
    GIT_ANDROID_ROOT=git/android/
    REPO_MANIFEST_ADDR=ssh://gerrit.mage.com:29418/git/android/platform/manifest
    REPO_MANIFEST_BRANCH=mage_master
    REPO_MANIFEST_FILE=mage/default.xml
    REPO_MIRROR=/home/mirror
    REPO_GROUP=
    REPO_GROUP_AMSS=mage_amss,mage_common
    REPO_GROUP_ANDROID=all,-mage_amss

[build]
    SOC_NAME=
    ANDROID_TARGET_PRODUCT_LIST=mage
    ANDROID_BUILD_VARIANT_LIST=userdebug,user,eng
    ANDROID_TARGET_CARRIER_LIST=eu
    ANDROID_BUILD_TYPE=release
    ANDROID_EXTRA_BUILD_STEPS=
    ANDROID_EXTRA_BUILD_STEPS=
    ANDROID_EXTRA_BUILD_COMBINATION=
    ANNOUNCE_LIST=

```

在java中我们可以使用 init4j 这个模块来解析ini文件

```
maven的依赖坐标
<dependency>
    <groupId>org.ini4j</groupId>
    <artifactId>ini4j</artifactId>
    <version>0.5.2</version>
</dependency>
```

这里主要介绍一下ConfigParser这个,这个和python中的ConfigParser很类似的.

读取ini文件,直接调用ConfigParser 中的read 方法,读取之后 就可以使用 ConfigParser 中的get方法来获取对应的值了.
```
try {
    ConfigParser config = new ConfigParser();
    config.read(cf.toFile());
    if (!config.hasSection(secn))
        return;

    List<Map.Entry<String, String>> items = config.items(secn);
    for (Map.Entry<String, String> item : items) {
        System.out.println(item.getKey() + " = " + item.getValue());
    }//end for
} catch (Exception e) {
    e.printStackTrace();
}

```

写入也是先调用ConfigParser 的read,然后调用 ConfigParser 的 set 方法这是相应的section的相应的key对应的值.
```

try {
    ConfigParser config = new ConfigParser();
    config.read(cf.toFile());

    if (!config.hasSection(buildSectionName)) {
        config.addSection(buildSectionName);
    }
    for (Map.Entry<String, String> entry : map.entrySet()) {
        String value = entry.getValue();
        String key = entry.getKey();
        // 这里保存的时候要把key都变为小写字母
        config.set(buildSectionName, key.toLowerCase(), value);
    }
    config.write(cf.toFile());
} catch (Exception e) {
    e.printStackTrace();
}

```


```
//这个方法就是设置相应的section下的相应的key对应的value值
public void set(String sectionName, String optionName, Object value) throws NoSectionException

//这个方法就是获取相应的section下的相应的key对应的值.
public String get(String section, String option) throws NoSectionException, NoOptionException, InterpolationException

例如上面例子中ini配置文件,获取repo下面的REPO_MANIFEST_ADDR的值可以这样:
get("repo", "REPO_MANIFEST_ADDR")

```

除了直接用ConfigParser 类,还可以直接使用Ini这个类.用法都是比较简单的.

