

mvn命令跳过单元测试
```bash

mvn 命令加上   -DskipTests  #，不执行测试用例，但编译测试用例类生成相应的class文件至target/test-classes下。

mvn 命令加上  -Dmaven.test.skip=true  #，不执行测试用例，也不编译测试用例类。 


其中-D的意思是  -D,--define <arg>              Define a system property
```

执行特定的测试
```bash
mvn test -Dtest=[ClassName]


mvn -Dtest=com.mamh.aais.aais.TriggerAndroidBuildTest test

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.mamh.aais.aais.TriggerAndroidBuildTest
[   info]lastbuild file: [/dailybuild/android/sdm660/LAST_BUILD.sdm660_nougat_20170308]
last manifest file path: /dailybuild/android/sdm660/2017-08-03_sdm660_nougat_20170308/manifest.xml
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.121 sec - in com.mamh.aais.aais.TriggerAndroidBuildTest

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

```


使用逗号分割要测试的类
```bash

mvn -Dtest=com.mamh.aais.aais.TriggerAndroidBuildTest,com.mamh.aais.aais.AaisJenkinsTest test

# 也可以支持通配符的形式
mvn -Dtest=com.mamh.aais.aais.*Test test

```

使用#指定测试方法，使用*通配测试方法
```bash
mvn test -Dtest=[ClassName]#[MethodName] 

mvn -Dtest=com.mamh.aais.aais.TriggerAndroidBuildTest#testGetLastBuildManifestFile test


```
使用+号指定一个类中的多个测试方法
```bash
mvn -Dtest=com.mamh.aais.aais.AaisGitTest#testLog+testRevParse test


mvn -Dtest=com.mamh.aais.aais.AaisGitTest#testLog+testRevParse,com.mamh.aais.aais.TriggerAndroidBuildTest#testGetLastBuildManifestFile test


```