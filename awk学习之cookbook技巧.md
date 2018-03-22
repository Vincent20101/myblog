awk学习之cookbook技巧.md


马哥的淘宝店:https://shop592330910.taobao.com/

#Awk One-Liners Explained, Part I: File Spacing, Numbering and Calculations

##1.Line Spacing

1.每行后面都加上一个空行
```bash
awk '1; { print "" }'       #马哥的淘宝店:https://shop592330910.taobao.com/
```

awk 程序包含一系列的 parttern-action（模式-动作）的语句。类似这样‘pattern{action}’的结构。
这个例子中包含了2个语句，一个是 “1”，另一个是“{print “” }”，模式和动作都可以省略的其中一个的。
如果模式省略了，默认是对所有的输入内容按行来出来的。如果是动作省略的默认是“{print }”。

注意awk程序是面向行的。马哥的淘宝店:https://shop592330910.taobao.com/

上面的等同于下面这个程序
```bash
awk '1 { print } { print "" }'     #马哥的淘宝店:https://shop592330910.taobao.com/
```

动作只有在模式匹配的情况下才执行。这里例子中的模式是“1”，这个模式总是true，也就是每一行都会匹配，
每一行也都会执行动作的。所以也等价于下面的代码：
```bash

awk '{ print } { print "" }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
awk的print语句总是会在最后打印一个ORS（output record separator）变量的，这个值模式是换行，
也就是\n  #马哥的淘宝店:https://shop592330910.taobao.com/
这里例子中的第一个print语句后面没有接任何参数，所有默认等于```print $0```，也就是打印整行内容，
$0代表的是整行内容。    #马哥的淘宝店:https://shop592330910.taobao.com/
例子中的第二个print 打印一个空字符串，也是上面都不输出，打上print会在最后加上一个换行，所以呢
这里就打印出来了一个空行了。#马哥的淘宝店:https://shop592330910.taobao.com/

2.另外一个给每行后面加上空行的技巧
```bash
awk 'BEGIN { ORS="\n\n" }; 1' #马哥的淘宝店:https://shop592330910.taobao.com/
```
BEGIN是一个特殊的模式，表示在读取内容之前无条件执行后面的动作。
这个技巧中是在动作语句中对ORS变量重新赋值了，将一个换行变成两个换行了。
后面 的1 等价于 {print} ，这样就达到了每行后面都加上新的空行的目的。
```
$ awk 'BEGIN { ORS="\n\n" }; 1' /etc/passwd
root:x:0:0:root:/root:/bin/bash

daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

bin:x:2:2:bin:/bin:/usr/sbin/nologin

```

3.在每个非空的行后面添加空行
```bash
awk 'NF { print $0 "\n" }' #马哥的淘宝店:https://shop592330910.taobao.com/

```  
这个技巧用到了awk中的另外一个特殊变量NF（number of fields）。这个表示每行被awk分割为多少个字段，
也就是多少列，默认是安装空格分割的。例如“this is a test”这行会被分割为4个字段。
如果是空行，是不会分割的，也就是NF的值是0。#马哥的淘宝店:https://shop592330910.taobao.com/
这里使用NF作为模式，只有NF值大于0条件才是真，后面的动作才执行。 
后面的动作是print $0，也就是打印整行内容，print第二个参数是个换行，也就是后面加个新的空行。

4、在每行后添加两个空行
```bash
awk '1; { print "\n" }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
1是等于{print}的，所以上面的可以等价于下面这个  #马哥的淘宝店:https://shop592330910.taobao.com/
```bash
awk '{ print; print "\n" }'  #马哥的淘宝店:https://shop592330910.taobao.com/

``` 
首先是打印一整行内容，然后再打印一个换行。 #马哥的淘宝店:https://shop592330910.taobao.com/

##2.Numbering and Calculations

5、为每个文件的内容添加行号
```bash
awk '{ print FNR "\t" $0 }' #马哥的淘宝店:https://shop592330910.taobao.com/

``` 
这则技巧是在没一行的开头加上FNR（file line number行号）然后是一个\t（ tab键），最后是整行内容。
FNR变量记录了当前的行号。FNR会每次重置为0的。
```bash
$ awk '{ print FNR "\t" $0 }' ffmpeg.sh  
1	#!/bin/bash #马哥的淘宝店:https://shop592330910.taobao.com/
2	
3	INPUT_FILE="$1"
4	ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"
$ 

```   

6、为所有文件的所有行统一添加行号
```bash

awk '{ print NR "\t" $0 }'      #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个和上面的那个例子类似，只是这里使用NR（line number）变量了，这个不会重置为0的。
```bash

$ awk '{ print FNR "\t" $0 }' ffmpeg.sh ffmpeg.sh                                                                                                           
1	#!/bin/bash  #马哥的淘宝店:https://shop592330910.taobao.com/
2	
3	INPUT_FILE="$1"
4	ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"
1	#!/bin/bash   #马哥的淘宝店:https://shop592330910.taobao.com/
2	
3	INPUT_FILE="$1"
4	ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"






                                                                                                                                                            
$ awk '{ print NR "\t" $0 }' ffmpeg.sh ffmpeg.sh                                                                                                             
1	#!/bin/bash  #马哥的淘宝店:https://shop592330910.taobao.com/
2	
3	INPUT_FILE="$1"
4	ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"
5	#!/bin/bash  #马哥的淘宝店:https://shop592330910.taobao.com/
6	
7	INPUT_FILE="$1"
8	ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"
$

```
我们可以看到当作用到2个文件来显示行号的时候 FNR是会在第二个文件重置为0 的。 
NR变量则不会重置为0 的。


7、 格式化行号
```bash

awk '{ printf("%5d : %s\n", NR, $0) }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个例子使用到了printf 函数，这个类似bash/c语言中的printf（）函数。这个函数是不会在每行结尾追加打印ORS，也就是不会打印换行的。
```bash

$ awk '{ printf("%5d : %s\n", NR, $0) }' ffmpeg.sh ffmpeg.sh      #马哥的淘宝店:https://shop592330910.taobao.com/
    1 : #!/bin/bash #马哥的淘宝店:https://shop592330910.taobao.com/
    2 : 
    3 : INPUT_FILE="$1"
    4 : ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"
    5 : #!/bin/bash #马哥的淘宝店:https://shop592330910.taobao.com/
    6 : 
    7 : INPUT_FILE="$1"
    8 : ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"


```


8.非空行前面添加行号
```bash
awk 'NF { $0=++a " :" $0 }; { print }' #马哥的淘宝店:https://shop592330910.taobao.com/
```
awk中是可以使用变量的，当然也可以使用自定义的变量，和bash是类似的，不需要先定义在使用，可以直接使用的。
之前我们用到的那些个变量都是awk自己定义的。#马哥的淘宝店:https://shop592330910.taobao.com/

这个例子怎么理解呢？ 
首先 这里是2个模式动作语句。第一个是 ```NF { $0=++a " :" $0 }``` ，第二个是``` { print } ```
第一个里面 ++a是定义了一个变量a，这里初始就是0（数字零），而不是什么空或者空字符串。
为啥是数字也是因为这个++操作符是作用到数字上面的。#马哥的淘宝店:https://shop592330910.taobao.com/
然后执行++操作（类似c语言里面的前置++）a第一次执行就会变为1了。
什么时候第一次执行呢也就是第一个不是空行的时候才是首次执行。因为NF模式匹配非空行。上面有个例子讲过。

然后是```  ++a  “ ：” $0 ``` 这3个值按照字符串拼接那样拼接起来，然后一起重新复制给变量$0了，$0表示整行内容，
这里就实现了给非空行重新设置行号的目的了。注意拼接字符串不能使用加号，加号只能作用到数字上面。

第一个模式动作语句是不打印内容的。#马哥的淘宝店:https://shop592330910.taobao.com/

然后是第二个模式动作语句，里面只有一个print，默认就是打印$0，默认就是打印整行内容。
这里不管是空行，还是非空行都统统的打印出来，非空行因为是之前在第一个模式动作中重新被
赋值了，这里也就实现了打印非空行行号的目的了。#马哥的淘宝店:https://shop592330910.taobao.com/

同时我们可以看到这个awk程序作用到2个文件的时候，变量a是不会被重置的。#马哥的淘宝店:https://shop592330910.taobao.com/


```bash

$ awk 'NF { $0=++a " :" $0 }; { print }' ffmpeg.sh ffmpeg.sh     #马哥的淘宝店:https://shop592330910.taobao.com/
1 :#!/bin/bash #马哥的淘宝店:https://shop592330910.taobao.com/

2 :INPUT_FILE="$1"
3 :ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"
4 :#!/bin/bash #马哥的淘宝店:https://shop592330910.taobao.com/

5 :INPUT_FILE="$1"
6 :ffmpeg -y -i "$INPUT_FILE" -vf drawtext="fontfile=fonts/simsun.ttc: text='欢迎光临 马哥的淘宝店<马哥私房菜> 地址是：shop592330910.taobao.com':fontcolor=red:fontsize=40:box=1:boxcolor=black@0:x=if(eq(mod(t\,3)\,0)\,rand(0\,(w-text_w))\,x):y=h-text_h" -codec:a copy "new.${INPUT_FILE}"


```



9.计算文件行数（模拟 wc -l 命令）
```bash

awk 'END { print NR }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里又用到了一个特殊的模式END，这个表示所有行处理完，最后在执行这个模式后面的动作。
这个例子就是打印NR的值，也就是最终的行数。 #马哥的淘宝店:https://shop592330910.taobao.com/
```bash
$ awk 'END { print NR }' ffmpeg.sh  #马哥的淘宝店:https://shop592330910.taobao.com/
11

$ awk 'END { print NR }' ffmpeg.sh ffmpeg.sh ffmpeg.sh     #马哥的淘宝店:https://shop592330910.taobao.com/
33

```

10、对每行的所有的列求和

```bash
awk '{ s = 0; for (i = 1; i <= NF; i++) s = s+$i; print s }'  #马哥的淘宝店:https://shop592330910.taobao.com/

#或分开多行书写：

awk '{  #马哥的淘宝店:https://shop592330910.taobao.com/
s = 0; 
for (i = 1; i <= NF; i++) 
    s = s+$i; 
print s 
}'

```
这个例子使用到了awk编程中的for循环，这个和c语言的for循环类似。
这个例子是把每行的所有列都加到一起 复制给变量s，然后打印总和。
每次计算一行的总和，然后就输出。而不是把所有行加起来。下面例子11才是把所有行加起来的。
特别注意这里是``` s + $i ``` ，要特别注意那个美元符，这里是需要美元符号的，有
美元符号表示的列的值，没有的表示的循环变量 i的值，i的值会是1,2,3,4,5,6等。但是列的值可不一定是这样的。

下面我们看执行结果，直接复制代码执行后面不跟文件，会停留等待用户输入，这个时候你可以输入一行数字，
每个数字可以空格分割，每输入一行（按完回车键）就会执行一次计算，然后打印。
结束输入可以按ctrl+D键。
```bash

$ awk '{  #马哥的淘宝店:https://shop592330910.taobao.com/
s = 0;
for (i = 1; i <= NF; i++)
    s = s+$i;
print s
}'          
1 2 3 4 5              # 这行是输入内容
15
1 2 3 4 5 6 7 8 9 10   # 这行是输入内容
55


```

11.对所有的行所有的列求和
```bash

awk '{ for (i = 1; i <= NF; i++) s = s+$i }; END { print s+0 }'  #马哥的淘宝店:https://shop592330910.taobao.com/

#或分开多行书写： #马哥的淘宝店:https://shop592330910.taobao.com/
awk '{ 
for (i = 1; i <= NF; i++) 
    s = s+$i 
}; 

END { print s+0 }' #马哥的淘宝店:https://shop592330910.taobao.com/


```
注意这个例子11和例子10的区别，例子10 有个 s = 0的语句，使得每次计算新的一行都会重新初始化为0.
这个例子与上一个基本一致，除了输出的是所有行所有字段的和。由于变量会被自动定义，s只需要定义一次，
故而不需要把s定义成0。另外需要注意的是，最后在END模式里面它输出{print s+0}而非{print s}，
这是因为如果文件为空，s不会被定义就不会有任何输出了，输出s+0可以保证在这种情况下也会输出更有意义的0。
或者我们可以写一个BEGIN{s = 0 }，来一个s初始值的设置。这样最后可以不用打印s + 0啦。

下面我们看执行结果，直接复制代码执行后面不跟文件，会停留等待用户输入，这个时候你可以输入一行数字，
每个数字可以空格分割，每输入一行就会执行一次计算/或打印，这个例子是最后才打印结果。
结束输入可以按ctrl+D键。
```bash

$ awk '{    #马哥的淘宝店:https://shop592330910.taobao.com/
for (i = 1; i <= NF; i++)
    s = s+$i
};

END { print s }' #马哥的淘宝店:https://shop592330910.taobao.com/ 
1 2 3 4 5 6  # 这行是输入内容
21

```
```bash

$ awk '{   #马哥的淘宝店:https://shop592330910.taobao.com/
for (i = 1; i <= NF; i++)
    s = s+$i
};

END { print s }' #马哥的淘宝店:https://shop592330910.taobao.com/
10 9 8 7 6      # 这行是输入内容
5 4 3 2 1 0     # 这行是输入内容
-1 -2 -3 -4 -5  # 这行是输入内容   
40
 

```

12、将所有字段替换为其绝对值
```bash

awk '{ for (i = 1; i <= NF; i++) if ($i < 0) $i = -$i; print }'   #马哥的淘宝店:https://shop592330910.taobao.com/


awk '{     #马哥的淘宝店:https://shop592330910.taobao.com/
  for (i = 1; i <= NF; i++) {
    if ($i < 0) {
      $i = -$i;
    }
  }
  print
}'    #马哥的淘宝店:https://shop592330910.taobao.com/

```
这条语句用了if语句，和C语言类似。
它对每一行检查，检查每个字段的值是否小于0，如果值小于0，则将其改为正数，然后重新赋值给这个字段。
字段名可以间接地用变量的形式引用，如i=5;$i='hello'会将第5个字段的内容置为hello。

13、计算文件中的总字段（单词）数
```bash

awk '{ total = total + NF }; END { print total+0 }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个例子是对每行的NF（NF就是字段数，列数，或者说是单词个数）值进行累加，最后打印输出
这里最后为什么是``` print total+0 ``` 可以参考例子11的说明。

14、输出含有单词“马哥私房菜”的行的数目
```bash

awk '/马哥私房菜/ { n++ }; END { print n+0 }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个例子含有两个语句。第一句找出匹配/马哥私房菜/的行，并对变量n进行累加。在/…/之间的内容为正则表达式，
/马哥私房菜/匹配所有含有“马哥私房菜”的单词。
第二句在文件处理完成后输出n的数值。这里用n+0是为了让n为空的情况下输出0而不是一个空行。

15、寻找第一个字段为数字且最大的行
```bash

awk '$1 > max { max=$1; maxline=$0 }; END { print max, maxline }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个例子用变量max记录第一个字段的最大值，并把第一个字段最大的行的内容存在变量maxline中。
在循环终止后，输出max和maxline的内容。
马哥的淘宝店:https://shop592330910.taobao.com/

注意：如果在数字都为负数的情况下，这个例子就不能用了，下面的是修改过的版本
```bash

awk 'NR == 1 { max = $1; maxline = $0; next; } $1 > max { max=$1; maxline=$0 }; END { print max, maxline }'

```

16、在每一行前添加该行的字段数，并打印
```bash

awk '{ print NF ":" $0 } ' #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个例子仅仅是在逐行输出字段数NF，一个冒号，以及该行的内容。


17、输出每行的最后一个字段
```bash

awk '{ print $NF }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
awk里面的字段可以用变量的形式引用。这一句输出第NF个字段的内容，而NF就是该行的字段数。


18、打印最后一行的最后一个字段
```bash
awk '{ field = $NF }; END { print field }' #马哥的淘宝店:https://shop592330910.taobao.com/
```

这个例子用field记录最后一个字段的内容，并在循环后输出field的内容。

这里是一个更好的版本。它更常用、更简洁也更高效：
```bash

awk 'END {print $NF}' #马哥的淘宝店:https://shop592330910.taobao.com/

```
19、输出所有字段数大于4的行
```bash

awk 'NF > 4'         #马哥的淘宝店:https://shop592330910.taobao.com/

awk 'NF > 4{print}'  #马哥的淘宝店:https://shop592330910.taobao.com/

```

这个例子省略了要执行的动作。如前所述，省略动作等价于{print}。


20、输出所有最后一个字段大于4的行
```bash

awk '$NF > 4'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个例子用$NF引用最后一个字段，如果它的数值大于4，那么就输出。



#Awk One-Liners Explained, Part II: Text Conversion and Substitution


##3. Text Conversion and Substitution

21.将Windows/dos格式的换行(CRLF)转成Unix格式(LF)
```bash

awk '{ sub(/\r$/,""); print }'

```
这条语句使用了sub(regex,repl,[string])函数。此函数将匹配regex的string替换成repl，  
如果没有提供string参数，  则$0将会被默认使用。$0的含义在上一篇已经介绍过，代表整行。
这句话其实是将行结尾的‘\r’删除，然后打印，print语句会在行后自动添加一个ORS，也就是\n。


22.将Unix格式的换行(LF)换成Windows/dos格式(CRLF)
```bash
awk '{ sub(/$/,"\r"); print }'
```
这个技巧也是使用了sub(regex, repl, [string]) 函数，这个和上面那个刚好是相反的。
这次是替换行结尾$替换成\r，也就是CR。然后打印，print语句会在行后自动添加一个ORS，也就是\n。
这样就使得每行都以\r\n结尾了。

23.在Windows下，将Unix格式的换行换成Windows/dos格式的换行符
```bash

awk 1

```
这条语句不是在所有情况下都可以用，要视使用的awk版本是不是能识别Unix格式的换行而定。  
如果能，那么它会读出每个句子，然后输出并用CRLF结束。1其实就是{ print }的简写形式。

24.在Windows下，将Windows/dos格式的换行换成unix格式的换行符
```bash
gawk -v BINMODE="w" '1'

```
理论上来说,在DOS系统下面这一行程序应该转换CRLFs 为 LFs。
标准的GUN文档中提到：在DOS下，awk(和许多其他文本程序)默认将行尾"\r\n“f翻译为"\n"在input时候，将"\n"翻译为"\r\n"在output时候。
一个特别的变量"BINMODE”可以控制这个输出。

我的测试结果显示这个转换并不能完成，所以我认为BINMODE模式并不一定是正确的。所以使用tr更保险一些。
```bash
tr -d \r

```
tr程序可以转换一个设定的字符为另外一个。 使用-d选项是删除所有字符并且不做任何转换。  
在上面的用法用， '\r' (CR)字符将被删除。 CRLFs 成将会转换为LFs。

25.删除行首的空白字符（空格和制表符）
```bash
awk '{ sub(/^[ \t]+/, ""); print }'

```
这里也使用了sub函数，他是把’ ^[ \t]+ ‘ 都替换为空，这个正则表达式匹配开头的空格/制表符一次或多次。

26.删除行首和行末的空格
```bash
awk '{ gsub(/^[ \t]+|[ \t]+$/, ""); print }'

```
这里使用了一个新的函数gsub（），这个是全局替换，就是会替换多次，只要有匹配上就会替换。

如果仅仅是要删除字段间的空格，你可以这样 
```bash

awk '{ $1=$1; print }'

```
这是条很取巧的语句，看起来是什么也没作，其实不是这样的。  
awk会在给字段重新赋值的时候对$0重新进行构建，用OFS也就是单个空格分隔所有字段，这样以来所有的多余的空格就消失了。 

28.在每行首加5个空格
```bash
awk '{ sub(/^/, "     "); print }'

```
这个很简单，就是使用sub函数把每行的开头^替换为5个空格。这样就达到了行首插入5个空格的效果。

29.让内容在79个字符宽的页面上右对齐
```bash

awk '{ printf "%79s\n", $0 }' 

```
这里使用了printf函数，格式化输出，这里打印$0代表整行，长度不够79的会在左边补齐空格的。左边补齐就达到了右对齐的效果。


30.让内容在79个字符宽的页面上居中对齐
```bash

awk '{ l=length(); s=int((79-l)/2); printf "%"(s+l)"s\n", $0 }'

```
这里使用length函数计算行的长度，然后算出中间


31.替换每行的 "foo" 为 "bar"
```bash

awk '{ sub(/foo/,"bar"); print }'

```
这个用到了sub()函数，需要注意的是，这个函数只会替换第一次出现匹配的。需要全部都替换用gsub()函数
```bash
awk '{ gsub(/foo/,"bar"); print }'

```
其实还有一个函数gensub()也有替换的功能的
```bash
gawk '{ $0 = gensub(/foo/,"bar",4); print }'

```
这个只会替换第四次出现foo的地方。这个函数原型是gensub(regex, s, h[, t])，
他将字符串t中的第h个regex匹配的替换成s。如果没有提供t参数，那么默认t就是$0，就是整行内容。
这个函数和sub()和gsub()不一样，这个函数是返回修改t之后的值。
那2个是直接在原来的基础上修改。所以这里它最后有重新赋值给变量$0了。

这个技巧里面regex = "/foo/", s = "bar", h = 4,  t = $0，它替换了第四次出现foo的地方为bar，最后把替换过的字符串重新赋值给$0，也就相当于赋值给整行了，相当于修改了整行。

32.替换包含baz的行里面的foo为bar
```bash
awk '/baz/ { gsub(/foo/, "bar") }; { print }'

```
每个awk的都包含一个pattern-action这样的组合语句的。写法类似"pattern { action statements }"。
每个action都作用到匹配pattern的行上。
这个用法用pattern是/baz/，就是哪个行里面包含了字符串baz就会成功匹配上。
匹配上就会执行action，这里的动作是一个替换操作，执行gsub()函数，替换foo为bar。

32.替换不包含baz的行里面的foo为bar
```bash
awk '!/baz/ { gsub(/foo/, "bar") }; { print }'

```
这用法正好和上个是相反的。!/baz/表示不包含baz的行。!让搜到baz的返回为假


34.把"scarlet" 或者 "ruby" 或者 "puce" 替换为 "red".
```bash
awk '{ gsub(/scarlet|ruby|puce/, "red"); print}'

```
这个用法没上面特别的，还是用到了gsub()函数，这里用sub()就不对了，这里主要的就是
正则里面的的写法/scarlet|ruby|puce/。|竖线用来分割多个匹配，类似or。

35.反转打印文件的行(类似命令"tac").
```bash

awk '{ a[i++] = $0 } END { for (j=i-1; j>=0;) print a[j--] }'

```
这个就是倒着打印文件的每一行，类似命令tac。
这里首先在 { a[i++] = $0 }里面把每一行的内容保存到数组a中。
最后在END动作里面倒序循环打印a数组。这样就做到了倒着打印文件每行了。

36.把以反斜线 \ 结束的行和下面一行链接到一行。
```bash
awk '/\\$/ { sub(/\\$/,""); getline t; print $0 t; next }; 1'

```
在某些编程语言中，行太长 可以拆成2行的，就是以反斜线来表示的。反斜线表示链接这一行和下一行内容。  
这用法中/\\$/ 就是匹配反斜线结尾的行，然后是用sub()替换删除这个反斜线。
然后getline函数获取下一行的内容并保存到变量t中。
然后是print 打印当前行$0,（这个行已经移除结尾的反斜线了）和变量t的内容。 
然后是next跳过下一行。
上面的一切都是在满足匹配/\\$/的情况下才会执行花括号里面的动作的。
如果不满足匹配，也就是不是反斜线结尾的行 就会直接执行1，也就是打印整行内容。

不过这不能作用到2个以上连续行都有反斜线的情况。

37.打印，排序登陆的所有用户
```bash
awk -F ":" '{ print $1 | "sort" }' /etc/passwd

```
这里我们用到了awk的-F选项，这个是设置内部字段分隔符，也就每行按照那个字符来分割，
这里是按照冒号分割。应为/etc/passwd这个文件每行都是以冒号隔开的一个一个字段的。
awk -F ":"等价于awk 'BEGIN { FS=":" }'

38.打印第二个字段，然后是第一个字段
```bash
awk '{ print $2, $1 }' file

```
这个没什么讲的，很简单的。
 
39.交换第一个字段和第二个字段，然后是打印每行
```bash
awk '{ temp = $1; $1 = $2; $2 = temp; print }'

```

40.删除第二个字段，然后打印每行
```bash
awk '{ $2 = ""; print }'

``` 
41.逆序一行的所有字段，然后打印整行
```bash
awk '{ for (i=NF; i>0; i--) printf("%s ", $i); printf ("\n") }'

``` 
注意这个这倒着打印文件不一样，这个是倒序每一行的所有字段的。
变量NF是整行的字段总数。我们只有倒着变量这个数字就像。先打印$NF,然后是$(NF-1),$(NF-2),最后是$1.

42.删除重复行 (类似 "uniq")
```bash
awk 'a != $0; { a = $0 }'
```
注意这个a != $0;是省略了action的，其实是a != $0 { print };
开始第一行肯定不满足条件 a != $0的，所以执行后面的 把第一行内容先赋值给变量a。
然后执行下面的行判断下面的每一行和先前的变量a是否相等。相等就会重新赋值变量a。
不想等才会打印当前行。

43.删除重复不连续的行
```bash
awk '!a[$0]++'
```
把每一行保存到索引数组a中，第一次a[]++整个会是0的。但是a[]此时已经是1了。
下次再有同样的行a[]++就会非0值啦。

```bash

awk '!($0 in a) { a[$0]; print }'

```


44.把每5行用逗号相连.
```bash

awk 'ORS=NR%5?",":"\n"'

```
如果行号NR能被5整除，重新赋值ORS的值


#Awk One-Liners Explained, Part III: Selective Printing and Deleting of Certain Lines

##4. Selective Printing of Certain Lines

45.打印文件前10行(模拟命令"head -10").
```bash
awk 'NR < 11'
```
NR是行号，这里pattern是 'NR < 11' 表示行号小于11，也就是小于等于10行的行都会打印。
这里省略了action，省略了就是{print}

上面这个有一点不好的就是超过10行的行还会继续循环下去，只是什么都不做而已。下面给个更好的解决方法。
```bash
awk '1; NR == 10 { exit }'
```
这就是当行号等于10行就exit，退出了。


46.打印文件第一行(模拟命令 "head -1").
```bash
awk 'NR > 1 { exit }; 1'

```

47.打印文件最后2行 (模拟命令 "tail -2").
```
awk '{ y=x "\n" $0; x=$0 }; END { print y }'
```
这一句看起来确实有些别扭。第一句总是把一个在当前行前面再加上变量x的内容赋值给y，然后用x记录当前行内容。
这样的效果是y的内容始终是上一行加上当前行的内容。
在最后END模式中输出y的内容。
如果仔细看的话，不难发现这个写法是很不高效的，因为它不停的进行赋值和字符串连接，只为了找到最后一行！所以，如果你想要输出文件的最后两行，tail -n 2是最好的选择。






















```bash
#==

























#==
```
