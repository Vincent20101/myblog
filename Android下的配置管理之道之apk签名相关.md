


#如何生成apk的签名key文件
```
# 建key方法：
sub='/C=CN/ST=ShangHai/L=ShangHai/O=example/OU=CM/CN=example/emailAddress=buildfarm@example.com'
for key in platform shared media testkey;do
    ./development/tools/make_key $key "$sub"
done


```


#怎么给某个apk重签名

命令如下：

```
java -Xmx2048m -jar signapk.jar xxx.x509.pem  xxx.pk8 unsigned.apk signed.apk
```

pem和pk8文件 ，对于是userdebug、eng 这些使用的 是代码里面的key文件。可以在build/target/product/security这个路径下面得到。










#如何获取签名key的sha1值

解压apk文件得到RSA文件

APK以zip文件方式打开，在\META-INF\目录中存在一个.RSA后缀的文件，一般名为CERT.RSA


使用keytool命令获取证书信息（包括MD5）
运行如下keytool命令即可： keytool -printcert -file CERT.RSA

```
keytool -printcert -file CERT.RSA
```

```
Owner: EMAILADDRESS=buildfarm@example.com, CN=example, OU=CM, O=example, L=ShangHai, ST=ShangHai, C=CN
Issuer: EMAILADDRESS=buildfarm@example.com, CN=example, OU=CM, O=example, L=ShangHai, ST=ShangHai, C=CN
Serial number: f03839c4296d10e2
Valid from: Tue Nov 22 14:21:36 CST 2016 until: Sat Apr 09 14:21:36 CST 2044
Certificate fingerprints:
MD5: B5:A2:01:A5:8D:54:CF:BE:ED:85:20:B6:5D:81:37:B0
SHA1: 44:1C:6D:43:17:06:FC:95:17:6E:B8:39:CF:A7:B7:E7:5E:C4:C1:AC
SHA256: 97:12:E0:92:B0:23:CE:F9:82:E4:92:EA:08:E8:7E:42:D0:90:75:D4:B1:34:FD:41:A3:63:03:DA:AE:EB:74:DA
Signature algorithm name: SHA1withRSA
Version: 3

Extensions:

#1: ObjectId: 2.5.29.35 Criticality=false
AuthorityKeyIdentifier [
KeyIdentifier [
0000: 08 79 56 A2 9D B2 49 28 36 01 47 E5 1C 6C 3F 9E .yV...I(6.G..l?.
0010: 12 40 9B 38 .@.8
]
]

#2: ObjectId: 2.5.29.19 Criticality=false
BasicConstraints:[
CA:true
PathLen:2147483647
]

#3: ObjectId: 2.5.29.14 Criticality=false
SubjectKeyIdentifier [
KeyIdentifier [
0000: 08 79 56 A2 9D B2 49 28 36 01 47 E5 1C 6C 3F 9E .yV...I(6.G..l?.
0010: 12 40 9B 38 .@.8
]
]
```


#怎么制作ota差分包

ota全包升级方法：  
把全包ota的zip文件 adb push 到手机的/sdcard/ 下面 然后改名为update.zip  
adb reboot recovery，手机会进入到一个界面，点击升级按钮就可以了。  



怎么做差分包

基本命令如下：
```
python Bins/releasetools/ota_from_target_files -v -p Bins/linux-x86 -k  releasekey -i old.zip new.zip  update.zip
```

-p Bins/linux-x86 这里的-p参数指定了一下imgdiff，bsdiff 工具的路径的，这个没有的话在做差分包时候会报错的。


制作差分包需要很多工具和so文件，这些都可以在android源码编译环境下面获取到，一般在out 下 host 下面有。linux系统就选择 linux-x
86那个目录。

```

# 这个是和制作ota差分包相关的一些文件，没有这些文件是无法成功做差分包的
# copy bins for ota
src:o:build/tools/releasetools/blockimgdiff.py:                             debug/Bins/releasetools/
src:o:build/tools/releasetools/build_image.py:                              debug/Bins/releasetools/
src:o:build/tools/releasetools/rangelib.py:                                 debug/Bins/releasetools/
src:o:build/tools/releasetools/sparse_img.py:                               debug/Bins/releasetools/
src:o:build/tools/releasetools/ota_from_target_files:                       debug/Bins/releasetools/
src:o:build/tools/releasetools/add_img_to_target_files.py:                  debug/Bins/releasetools/
src:o:build/tools/releasetools/edify_generator.py:                          debug/Bins/releasetools/
src:o:build/tools/releasetools/common.py:                                   debug/Bins/releasetools/
out:o:../../../host/linux-x86/bin/bsdiff:                                   debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/imgdiff:                                  debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/simg2img:                                 debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/make_ext4fs:                              debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/mkuserimg.sh:                             debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/e2fsck:                                   debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/brillo_update_payload:                    debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/delta_generator:                          debug/Bins/linux-x86/bin/
out:o:../../../host/linux-x86/bin/lib/shflags/shflags:                      debug/Bins/linux-x86/bin/lib/shflags/
out:o:../../../host/linux-x86/framework/signapk.jar:                        debug/Bins/linux-x86/framework/
out:o:../../../host/linux-x86/lib64/libc++.so:                              debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libconscrypt_openjdk_jni.so:            debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libdivsufsort64.so:                     debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libdivsufsort.so:                       debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libcutils.so:                           debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libselinux.so:                          debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/liblog.so:                              debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2fs-host.so:                      debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2_blkid-host.so:                  debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2_uuid-host.so:                   debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2_profile-host.so:                debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2_quota-host.so:                  debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2_com_err-host.so:                debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libext2_e2p-host.so:                    debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libbase.so:                             debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libbrillo.so:                           debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libbrillo-stream.so:                    debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libchrome.so:                           debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libcrypto-host.so:                      debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libevent-host.so:                       debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libprotobuf-cpp-lite.so:                debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libselinux.so:                          debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libsparse-host.so:                      debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libssl-host.so:                         debug/Bins/linux-x86/lib64/
out:o:../../../host/linux-x86/lib64/libz-host.so:                           debug/Bins/linux-x86/lib64/

```
