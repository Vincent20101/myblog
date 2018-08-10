awk学习之cookbook技巧.md


马哥的淘宝店:https://shop592330910.taobao.com/

# Part 1: File Spacing, Numbering and Calculations

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



# Part 2: Text Conversion and Substitution


##3. Text Conversion and Substitution

21.将Windows/dos格式的换行(CRLF)转成Unix格式(LF)
```bash

awk '{ sub(/\r$/,""); print }'      #马哥的淘宝店:https://shop592330910.taobao.com/

```
这条语句使用了sub(regex,repl,[string])函数。此函数将匹配regex的string替换成repl，  
如果没有提供string参数，  则$0将会被默认使用。$0的含义在上一篇已经介绍过，代表整行。
这句话其实是将行结尾的‘\r’删除，然后打印，print语句会在行后自动添加一个ORS，也就是\n。


22.将Unix格式的换行(LF)换成Windows/dos格式(CRLF)
```bash
awk '{ sub(/$/,"\r"); print }'   #马哥的淘宝店:https://shop592330910.taobao.com/
```
这个技巧也是使用了sub(regex, repl, [string]) 函数，这个和上面那个刚好是相反的。
这次是替换行结尾$替换成\r，也就是CR。然后打印，print语句会在行后自动添加一个ORS，也就是\n。
这样就使得每行都以\r\n结尾了。

23.在Windows下，将Unix格式的换行换成Windows/dos格式的换行符
```bash

awk 1           #马哥的淘宝店:https://shop592330910.taobao.com/

```
这条语句不是在所有情况下都可以用，要视使用的awk版本是不是能识别Unix格式的换行而定。  
如果能，那么它会读出每个句子，然后输出并用CRLF结束。1其实就是{ print }的简写形式。

24.在Windows下，将Windows/dos格式的换行换成unix格式的换行符
```bash
gawk -v BINMODE="w" '1'      #马哥的淘宝店:https://shop592330910.taobao.com/

```
理论上来说,在DOS系统下面这一行程序应该转换CRLFs 为 LFs。
标准的GUN文档中提到：在DOS下，awk(和许多其他文本程序)默认将行尾"\r\n“f翻译为"\n"在input时候，将"\n"翻译为"\r\n"在output时候。
一个特别的变量"BINMODE”可以控制这个输出。

我的测试结果显示这个转换并不能完成，所以我认为BINMODE模式并不一定是正确的。所以使用tr更保险一些。
```bash
tr -d \r       #马哥的淘宝店:https://shop592330910.taobao.com/

```
tr程序可以转换一个设定的字符为另外一个。 使用-d选项是删除所有字符并且不做任何转换。  
在上面的用法用， '\r' (CR)字符将被删除。 CRLFs 成将会转换为LFs。

25.删除行首的空白字符（空格和制表符）
```bash
awk '{ sub(/^[ \t]+/, ""); print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里也使用了sub函数，他是把’ ^[ \t]+ ‘ 都替换为空，这个正则表达式匹配开头的空格/制表符一次或多次。

26.删除行首和行末的空格
```bash
awk '{ gsub(/^[ \t]+|[ \t]+$/, ""); print }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里使用了一个新的函数gsub（），这个是全局替换，就是会替换多次，只要有匹配上就会替换。

如果仅仅是要删除字段间的空格，你可以这样 
```bash

awk '{ $1=$1; print }'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这是条很取巧的语句，看起来是什么也没作，其实不是这样的。  
awk会在给字段重新赋值的时候对$0重新进行构建，用OFS也就是单个空格分隔所有字段，这样以来所有的多余的空格就消失了。 

28.在每行首加5个空格
```bash
awk '{ sub(/^/, "     "); print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个很简单，就是使用sub函数把每行的开头^替换为5个空格。这样就达到了行首插入5个空格的效果。

29.让内容在79个字符宽的页面上右对齐
```bash

awk '{ printf "%79s\n", $0 }'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里使用了printf函数，格式化输出，这里打印$0代表整行，长度不够79的会在左边补齐空格的。左边补齐就达到了右对齐的效果。


30.让内容在79个字符宽的页面上居中对齐
```bash

awk '{ l=length(); s=int((79-l)/2); printf "%"(s+l)"s\n", $0 }'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里使用length函数计算行的长度，然后算出中间   #马哥的淘宝店:https://shop592330910.taobao.com/


31.替换每行的 "foo" 为 "bar"
```bash

awk '{ sub(/foo/,"bar"); print }'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个用到了sub()函数，需要注意的是，这个函数只会替换第一次出现匹配的。需要全部都替换用gsub()函数
```bash
awk '{ gsub(/foo/,"bar"); print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
其实还有一个函数gensub()也有替换的功能的
```bash
gawk '{ $0 = gensub(/foo/,"bar",4); print }'    #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个只会替换第四次出现foo的地方。这个函数原型是gensub(regex, s, h[, t])，
他将字符串t中的第h个regex匹配的替换成s。如果没有提供t参数，那么默认t就是$0，就是整行内容。
这个函数和sub()和gsub()不一样，这个函数是返回修改t之后的值。
那2个是直接在原来的基础上修改。所以这里它最后有重新赋值给变量$0了。

这个技巧里面regex = "/foo/", s = "bar", h = 4,  t = $0，它替换了第四次出现foo的地方为bar，最后把替换过的字符串重新赋值给$0，也就相当于赋值给整行了，相当于修改了整行。

32.替换包含baz的行里面的foo为bar
```bash
awk '/baz/ { gsub(/foo/, "bar") }; { print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
每个awk的都包含一个pattern-action这样的组合语句的。写法类似"pattern { action statements }"。
每个action都作用到匹配pattern的行上。
这个用法用pattern是/baz/，就是哪个行里面包含了字符串baz就会成功匹配上。
匹配上就会执行action，这里的动作是一个替换操作，执行gsub()函数，替换foo为bar。

32.替换不包含baz的行里面的foo为bar
```bash
awk '!/baz/ { gsub(/foo/, "bar") }; { print }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
这用法正好和上个是相反的。!/baz/表示不包含baz的行。!让搜到baz的返回为假


34.把"scarlet" 或者 "ruby" 或者 "puce" 替换为 "red".
```bash
awk '{ gsub(/scarlet|ruby|puce/, "red"); print}'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个用法没上面特别的，还是用到了gsub()函数，这里用sub()就不对了，这里主要的就是
正则里面的的写法/scarlet|ruby|puce/。|竖线用来分割多个匹配，类似or。

35.反转打印文件的行(类似命令"tac").
```bash

awk '{ a[i++] = $0 } END { for (j=i-1; j>=0;) print a[j--] }' #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个就是倒着打印文件的每一行，类似命令tac。
这里首先在 { a[i++] = $0 }里面把每一行的内容保存到数组a中。
最后在END动作里面倒序循环打印a数组。这样就做到了倒着打印文件每行了。

36.把以反斜线 \ 结束的行和下面一行链接到一行。
```bash
awk '/\\$/ { sub(/\\$/,""); getline t; print $0 t; next }; 1'  #马哥的淘宝店:https://shop592330910.taobao.com/

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
awk -F ":" '{ print $1 | "sort" }' /etc/passwd      #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里我们用到了awk的-F选项，这个是设置内部字段分隔符，也就每行按照那个字符来分割，
这里是按照冒号分割。应为/etc/passwd这个文件每行都是以冒号隔开的一个一个字段的。
awk -F ":"等价于awk 'BEGIN { FS=":" }'

38.打印第二个字段，然后是第一个字段
```bash
awk '{ print $2, $1 }' file     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个没什么讲的，很简单的。
 
39.交换第一个字段和第二个字段，然后是打印每行
```bash
awk '{ temp = $1; $1 = $2; $2 = temp; print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```

40.删除第二个字段，然后打印每行
```bash
awk '{ $2 = ""; print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

``` 
41.逆序一行的所有字段，然后打印整行
```bash
awk '{ for (i=NF; i>0; i--) printf("%s ", $i); printf ("\n") }'   #马哥的淘宝店:https://shop592330910.taobao.com/

``` 
注意这个这倒着打印文件不一样，这个是倒序每一行的所有字段的。
变量NF是整行的字段总数。我们只有倒着变量这个数字就像。先打印$NF,然后是$(NF-1),$(NF-2),最后是$1.

42.删除重复行 (类似 "uniq")
```bash
awk 'a != $0; { a = $0 }'   #马哥的淘宝店:https://shop592330910.taobao.com/
```
注意这个a != $0;是省略了action的，其实是a != $0 { print };
开始第一行肯定不满足条件 a != $0的，所以执行后面的 把第一行内容先赋值给变量a。
然后执行下面的行判断下面的每一行和先前的变量a是否相等。相等就会重新赋值变量a。
不想等才会打印当前行。

43.删除重复不连续的行
```bash
awk '!a[$0]++'      #马哥的淘宝店:https://shop592330910.taobao.com/
```
把每一行保存到索引数组a中，第一次a[]++整个会是0的。但是a[]此时已经是1了。
下次再有同样的行a[]++就会非0值啦。

```bash

awk '!($0 in a) { a[$0]; print }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```


44.把每5行用逗号相连.
```bash

awk 'ORS=NR%5?",":"\n"'    #马哥的淘宝店:https://shop592330910.taobao.com/

```
如果行号NR能被5整除，重新赋值ORS的值


# Part 3: Selective Printing and Deleting of Certain Lines

##4. Selective Printing of Certain Lines

45.打印文件前10行(模拟命令"head -10").
```bash
awk 'NR < 11'      #马哥的淘宝店:https://shop592330910.taobao.com/
```
NR是行号，这里pattern是 'NR < 11' 表示行号小于11，也就是小于等于10行的行都会打印。
这里省略了action，省略了就是{print}

上面这个有一点不好的就是超过10行的行还会继续循环下去，只是什么都不做而已。下面给个更好的解决方法。
```bash
awk '1; NR == 10 { exit }'  #马哥的淘宝店:https://shop592330910.taobao.com/
```
这就是当行号等于10行就exit，退出了。


46.打印文件第一行(模拟命令 "head -1").
```bash
awk 'NR > 1 { exit }; 1'   #马哥的淘宝店:https://shop592330910.taobao.com/

```

47.打印文件最后2行 (模拟命令 "tail -2").
```
awk '{ y=x "\n" $0; x=$0 }; END { print y }'
```
这一句看起来确实有些别扭。第一句总是把一个在当前行前面再加上变量x的内容赋值给y，然后用x记录当前行内容。
这样的效果是y的内容始终是上一行加上当前行的内容。
在最后END模式中输出y的内容。
如果仔细看的话，不难发现这个写法是很不高效的，因为它不停的进行赋值和字符串连接，只为了找到最后一行！所以，如果你想要输出文件的最后两行，tail -n 2是最好的选择。


48.打印文件最后1行 (模拟命令 "tail -1")
```bash
awk 'END { print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个可能工作，也可能不工作，这在有些版本的awk上不是我们想用的结果。这个取决于变量$0是否在输入结束后被重置。

兼容的写法
```bash
awk '{ rec=$0 } END{ print rec }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个也有效率的问题，使用tail -1是最好的选择


49.打印匹配正则表达式的某行 "/regex/" (模拟命令 "grep").
```bash
awk '/regex/'      #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里使用'/regex/'作为pattern，省略了action，如果当前行匹配这个正则表达式，结果是true，就会执行后面的
action，就是{print}，相当于打印整行内容。

50. 打印不匹配正则表达式的某行 "/regex/" (模拟命令 "grep -v").
```bash
awk '!/regex/'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
！这个感叹号会把结果置为相反的。这里就是表示不匹配。

51.打印匹配模式的行的上一行，而非当前行
```bash
awk '/regex/ { print x }; { x=$0 }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个类似grep -B 1

这里有2个 pattern-action， 
{ x=$0 } 这一组是总是会执行的，总是把当前行内容保存到变量x中
/regex/ { print x } 这一组是只有在匹配到regex时候才会执行后面的action的，
这里是打印x变量内容。x变量此时保存的还是上次的结果，也就是当前匹配行的上一行的内容。

如果是第一行就匹配了，直接执行{ print x }，但是此时x还是空值的。

如果是第一行不匹配，然后接着执行 { x=$0 } ，这个时候x保存第一行内容。
然后是第二行，如果匹配，就执行{ print x }，也就是打印了第一行的内容。其他的依次类推。


```bash

awk '/regex/ { print (x=="" ? "match on line 1" : x) }; { x=$0 }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
这个无非就是加了个判断x是不是空值，因为第一行就匹配的时候x是空的。

52.打印匹配模式的下一行.
```bash
awk '/regex/ { getline; print }'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里使用了getline函数取得下一行的内容并输出。getline的作用是将$0的内容置为下一行的内容，
并同时更新NR，NF，FNR变量。如果匹配的是最后一行，getline会出错，$0不会被更新，最后一行会被打印。

这个类似grep -A 1


53.打印匹配AAA或者BBB或者CCC的行.
```bash
awk '/AAA|BBB|CCC/'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里用到竖线 | 符号，类似or的意思

54.打印包含"AAA"  "BBB",  "CCC" 先后顺序要对.
```bash

awk '/AAA.*BBB.*CCC/' #马哥的淘宝店:https://shop592330910.taobao.com/

```

55.打印行长度大于64的行.
```bash
awk 'length > 64'   #马哥的淘宝店:https://shop592330910.taobao.com/

```
56.打印行长度小于64的行.
```bash
awk 'length < 64'   #马哥的淘宝店:https://shop592330910.taobao.com/
```
57.打印匹配regex的行开始到文件结尾的之间的行.
```bash
awk '/regex/,0'    #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里用到了'pattern1, pattern2'  这样的模式匹配，表示一个区间的。熟悉vim的都知道。
这个两个模式区间都是闭区间的。也就是包括pattern1的行，结束也是包括pattern2的行的。

58.打印 第8到第12行(包括).

```bash
awk 'NR==8,NR==12'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
NR表示行号，使用==来判断相等，

59.打印第52行.
```bash
awk 'NR==52'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这样会影响效率，52行之后的照样会循环一下，但是上面都不做。
好的做法是52行后之间退出循环。
```bash

awk 'NR==52 { print; exit }'  #马哥的淘宝店:https://shop592330910.taobao.com/

```
60.打印连个正则表达式匹配的行之间的行(包括).
```bash
awk '/Iowa/,/Montana/'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里再次用到了区间匹配模式"pattern1,pattern2"


##5. Selective Deletion of Certain Lines

这里只有一个技巧

61.删除所有空白行
```bash

awk NF         #马哥的淘宝店:https://shop592330910.taobao.com/

```
这里用到了特殊变量NF，表示当前行的字段数
字段数如果是0表明是空行呗，0表示false，所有后面省略的action就不会执行啦。
字段数如果不是0就表明整行有内容。就打印。

还有另外一个实现
```bash
awk '/./'     #马哥的淘宝店:https://shop592330910.taobao.com/

```
模式/./会匹配一个字符，空行当然是不会被匹配上的啦。   #马哥的淘宝店:https://shop592330910.taobao.com/

# Part 4: String and Array Creation

## String Creation

1.创建一个固定长度的字符串.
```bash
awk 'BEGIN { while (a++<513) s=s "x"; print s }'

```
这个段程序用BEGIN这个特殊的匹配模式让后面的代码在awk试图读入任何东西前就执行。在里面是一个被执行了513次的循环，每次循环中“x”都被添加到变量s的最后。循环结束后，s的内容被输出。因为这段代码只有这一句，所以awk在执行完BEGIN模式语句后就退出了。
这段循环代码不仅仅可用在BEGIN中，你可以在awk的任何代码段里面使用，包括END。

很不幸这段代码不是最有效率的，这是一个线性的解决方案
```

function rep(str, num,     remain, result) {
    if (num < 2) {
        remain = (num == 1)
    } else {
        remain = (num % 2 == 1)
        result = rep(str, (num - remain) / 2)
    }
    return result result (remain ? str  : "")
}
```

该函数同样用到三元运算a?b:c，而定义的该函数的内部又不停的调用函数本身，达到循环的目的。而前面的if……else语句主要用业说明remain是一个奇数。而通过下面的语句调用该函数：
```bash
awk 'BEGIN { s = rep("x", 513) }'

```


```bash



#Another way to make n copies of a string s:

function repeat(n, s      , str)
{
   str = sprintf("%*s", n, " "); # make n spaces
   gsub(/ /, s, str); # replace space with s
   return str;
}

#Another idiom I sometimes use is this to make a string of "-" to underline another string:

ul = str; # copy the string
gsub(/./, "-", ul); # replace each char with "-"
print str; # print the string
print ul; # underline it



```

2.在某个位置插入指定长度的字符串
```bash

gawk --re-interval 'BEGIN{ while(a++<49) s=s "x" }; { sub(/^.{6}/,"&" s) }; 1'

```
这段代码只能在gawk下使用，因为它用到了interval expression，即这里的{6}，作用是让前一个字符.匹配多次。.{6}便可以匹配6个任意字符。gawk使用interval expression需要用到参数-re-interval。

同前一例一样，首先在BEGIN段里面，建立了一个49个字符长的字符串放在变量s里。接下来是对每一行，进行替换，&这里代表的是匹配的字符串部分，所以sub的结果是将每一行第7个字符开始的内容替换成了s。然后是逐行输出。

如果不是gawk，需要这样写
```bash

awk 'BEGIN{ while(a++<49) s=s "x" }; { sub(/^....../,"&" s) }; 1

```

其输出结果为从每行的第六个字符开始，连续输出49个x，然后再接着输出原来的行第七个字符及其以后的内容。如果每行不足七个字符时，该行输出的值不变。

注意：上面语句中的点并不是点本身的意思，而是代码任意一个字符，有点类似于bash脚本中用到字段?所代表的意思


## Array Creation

3.利用一个字符串创建一个数组.
```bash
split("Jan Feb Mar Apr May Jun Jul Aug Sep Oct Nov Dec", month, " ")

```


4.建立一个索引数组.
```bash

for (i=1; i<=12; i++) mdigit[month[i]] = i

```

5.打印第5个字段是 "abc123" 的那一行.
```bash

awk '$5 == "abc123"'

```
```bash
awk '{ if ($5 == "abc123") { print $0 } }'

```

6.打印第5个字段不等于 "abc123"的行.
```bash

awk '$5 != "abc123"'

```
```bash
awk '{ if ($5 != "abc123") { print $0 } }'

```
```bash
awk '!($5 == "abc123")'

```

7.打印第7个字段匹配某个正则表达式的行.
```bash

awk '$7  ~ /^[a-f]/'

```
这里用到了～波浪符号，这里表示$7 是否匹配 后面的正则表达式/^[a-f]/'

下面这表示不匹配
```bash
awk '$7 !~ /^[a-f]/'


```
```bash
awk '$7 ~ /^[^a-f]/'

```

# 10 Awk Tips, Tricks and Pitfalls


## Be idiomatic!
假设想打印某些匹配的行，一般会这样写awk代码：
```bash
awk '{if ($0 ~ /pattern/) print $0}'

```
不过这里有几点需要注意的，这个结构和我们平常使用awk结构是不一样的，我通常的结构是这样的：
```bash
condition { actions }
```
所以上面的可以很清楚的改写为下面这样：
```bash
awk '$0 ~ /pattern/ {print $0}'
```
/pattern/ 是和这个等价的 $0 ~ /pattern/
print是和print $0语句一个意思的。默认省略了$0.

```bash
awk '/pattern/'
``` 
如果是这样的没有action的，默认是print $0的。

如果你只是想按照某些条件来打印文件的某行可以像下面这样
```bash

awk '(NR%2 && /pattern/) || (!(NR%2) && /anotherpattern/)'
```
这里省略了action部分，除非你想要的action不是print $0，你就要明确的给出action了。


```bash
awk 1
awk '"a"'   # single quotes are important!
```
上面这2个都是打印源文件的每一行，每一行都没有做任何改变的。
  
如果你想操作修改输出的某些行，但是你还想打印出所有的行，你可以像这样 
```bash
awk '{sub(/pattern/,"foobar")}1'
```

```bash

awk 'NR % 6'            # prints all lines except those divisible by 6
awk 'NR > 5'            # prints from line 6 onwards (like tail -n +6, or sed '1,5d')
awk '$2 == "foo"'       # prints lines where the second field is "foo"
awk 'NF >= 6'           # prints lines with 6 or more fields
awk '/foo/ && /bar/'    # prints lines that match /foo/ and /bar/, in any order
awk '/foo/ && !/bar/'   # prints lines that match /foo/ but not /bar/
awk '/foo/ || /bar/'    # prints lines that match /foo/ or /bar/ (like grep -e 'foo' -e 'bar')
awk '/foo/,/bar/'       # prints from line matching /foo/ to line matching /bar/, inclusive
awk 'NF'                # prints only nonempty lines (or: removes empty lines, where NF==0)
awk 'NF--'              # removes last field and prints the line
awk '$0 = NR" "$0'      # prepends line numbers (assignments are valid in conditions)

```


```bash

awk 'NR==FNR { # some actions; next} # other condition {# other actions}' file1 file2

```

```bash
# prints lines that are both in file1 and file2 (intersection)
awk 'NR==FNR{a[$0];next} $0 in a' file1 file2

```

```bash
# use information from a map file to modify a data file
awk 'NR==FNR{a[$1]=$2;next} {$3=a[$3]}1' mapfile datafile

```

```bash
# replace each number with its difference from the maximum
awk 'NR==FNR{if($0>max) max=$0;next} {$0=max-$0}1' file file

```
## Pitfall: shorten pipelines


```bash

somecommand | head -n +1 | grep foo | sed 's/foo/bar/' | tr '[a-z]' '[A-Z]' | cut -d ' ' -f 2

somecommand | awk 'NR>1 && /foo/{sub(/foo/,"bar"); print toupper($2)}'

```

## Print lines using ranges


```bash
# prints lines from /beginpat/ to /endpat/, inclusive
awk '/beginpat/,/endpat/'

```

有时候我们像打印匹配某2个条件之间的某些行，不包括匹配的那个行
```bash
# prints lines from /beginpat/ to /endpat/, not inclusive
awk '/beginpat/,/endpat/{if (!/beginpat/&&!/endpat/)print}'

# prints lines from /beginpat/ to /endpat/, not including /beginpat/
awk '/beginpat/,/endpat/{if (!/beginpat/)print}'

```

```bash
# prints lines from /beginpat/ to /endpat/, not inclusive
awk '/endpat/{p=0};p;/beginpat/{p=1}'

# prints lines from /beginpat/ to /endpat/, excluding /endpat/
awk '/endpat/{p=0} /beginpat/{p=1} p'

# prints lines from /beginpat/ to /endpat/, excluding /beginpat/
awk 'p; /endpat/{p=0} /beginpat/{p=1}'


# prints lines from /beginpat/ to /endpat/, inclusive
awk '/beginpat/{p=1};p;/endpat/{p=0}'
```
上面这个例子都是在遇到/beginpat/的行的时候把p设置为1.遇到/endpat/的行的时候把p设置为0.

## Split file on patterns

有个文件内容如下，我们想把匹配/^FOO/的行找出来，然后创建一些文件，out1，out2.
out1里面保存前4行。out2里面保存line5，line6这2行。
```text
line1
line2
line3
line4
FOO1
line5
line6
FOO2
line7
line8
FOO3
line9
line10
line11
FOO4
line12
FOO5
line13

```

```bash
# first way, works with all versions of awk
awk -v n=1 '/^FOO[0-9]*/{close("out"n);n++;next} {print > "out"n}' file

```
"-v n=1" 是告诉awk把变量n初始值为1.
```bash
# another way, needs GNU awk
LC_ALL=C gawk -v RS='FOO[0-9]*\n' -v ORS= '{print > "out"NR}' file

```

## Locale-based pitfalls
```text
-rw-r--r-- 1 waldner users 46592 2003-09-12 09:41 file1
-rw-r--r-- 1 waldner users 11509 2008-10-07 17:42 file2
-rw-r--r-- 1 waldner users 11193 2008-10-07 17:41 file3
-rw-r--r-- 1 waldner users 19073 2008-10-07 17:45 file4
-rw-r--r-- 1 waldner users 36332 2008-10-07 17:03 file5
-rw-r--r-- 1 waldner users 33395 2008-10-07 16:53 file6
-rw-r--r-- 1 waldner users 54272 2008-09-18 16:20 file7
-rw-r--r-- 1 waldner users 20573 2008-10-07 17:50 file8

```

```bash
$ LC_ALL=en_US.utf8 awk --re-interval '{sub(/^([^[:space:]]+[[:space:]]+){3}/,"")}1' file
-rw-r--r-- 1 waldner users 46592 2003-09-12 09:41 file1
-rw-r--r-- 1 waldner users 11509 2008-10-07 17:42 file2
-rw-r--r-- 1 waldner users 11193 2008-10-07 17:41 file3
-rw-r--r-- 1 waldner users 19073 2008-10-07 17:45 file4
-rw-r--r-- 1 waldner users 36332 2008-10-07 17:03 file5
-rw-r--r-- 1 waldner users 33395 2008-10-07 16:53 file6
-rw-r--r-- 1 waldner users 54272 2008-09-18 16:20 file7
-rw-r--r-- 1 waldner users 20573 2008-10-07 17:50 file8

```

```bash
$ LC_ALL=C awk --re-interval '{sub(/^([^[:space:]]+[[:space:]]+){3}/,"")}1' file
users 46592 2003-09-12 09:41 file1
users 11509 2008-10-07 17:42 file2
users 11193 2008-10-07 17:41 file3
users 19073 2008-10-07 17:45 file4
users 36332 2008-10-07 17:03 file5
users 33395 2008-10-07 16:53 file6
users 54272 2008-09-18 16:20 file7
users 20573 2008-10-07 17:50 file8

```

```bash
$ echo 'èòàù' | LC_ALL=en_US.utf8 awk '/[a-z]/'
èòàù

```


## Parse CSV

解析csv格式文件，这种格式文件都是逗号分割的一个一个字段，我们可以设置FS=','，字段的前后可能会有空格
，这些空格是我们不需要的，最后需要去掉这些空格。
```text
    field1  ,   field2   , field3   , field4

```

变量 FS 可以是一个正则表达式,例如 FS='^ *| *, *| *$'. 但是呢这个有可能有问题的:

* 实际的分割后的数据可能对应 字段1 ... 字段NF 或者 字段2 ... 字段NF, 这取决于这一行开头是否有空格;
* 出于某种原因把FS赋值为正则的，如果字段中包含了空格可能导致不确定的结果。

变量FS是field-separator，字段分隔符的意思。

对于这个例子，我们最好的做法是设置FS=","，然后移除字段前后的空格。
```bash
# FS=','
for(i=1;i<=NF;i++){
  gsub(/^ *| *$/,"",$i);
  print "Field " i " is " $i;
}

```

另外一种格式的csv文件：
```text
"field1","field2","field3","field4"
```
这个是每个字段都包含了双引号。这个我们可以简单的设置FS='^"|","|"$' (或者 FS='","|"')
这个需要注意的是我们最后取的字段的位置是2, 3 ... NF-1.

我们还可以扩展FS，让他可以处理到空格的：
```text
   "field1"  , "field2",   "field3" , "field4"
```
FS可以设置为：FS='^ *"|" *, *"|" *$'，注意字段的位置我们这里是2 ... NF-1 是我们需要的。
当然你也可以把FS设置为：FS=',',然后手动处理多余的空格和双引号等字符。
```bash

# FS=','
for(i=1;i<=NF;i++){
  gsub(/^ *"|" *$/,"",$i);
  print "Field " i " is " $i;
}

```

另外一种特别的格式的csv文件：
```bash

 field1, "field2,with,commas"  ,  field3  ,  "field4,foo"

```
这个是有的字段有双引号，有的字段没有，而且中间可能有空格。
```bash
$0=$0",";                                  # yes, cheating
while($0) {
  match($0,/[^,]*,| *"[^"]*" *,/);            
  sf=f=substr($0,RSTART,RLENGTH);          # save what matched in sf
  gsub(/^ *"?|"? *,$/,"",f);               # remove extra stuff
  print "Field " ++c " is " f;
  sub(sf,"");                              # "consume" what matched
}

```

## Pitfall: validate an IPv4 address


检查ipv4地址：
```bash

awk -F '[.]' 'function ok(n){return (n>=0 && n<=255)} {exit (ok($1) && ok($2) && ok($3) && ok($4))}'

```
上面的没有对字母进行验证。像传入这样的 '123b.44.22c.3' 也能验证通过。

```bash
awk -F '[.]' 'function ok(n) {
  return (n ~ /^([01]?[0-9]?[0-9]|2[0-4][0-9]|25[0-5])$/)
}
{exit (ok($1) && ok($2) && ok($3) && ok($4))}'

```

## Check whether two files contain the same data
```bash
awk '!($0 in a) {c++;a[$0]} END {exit(c==NR/2?0:1)}' file1 file2

```

## Pitfall: contexts and variable types in awk

```text
1,2,3,,5,foo
1,2,3,0,5,bar
1,2,3,4,5,baz

```

```bash

awk -F ',' -v OFS=',' '{if ($4) $6="X"}1'

```
这个我们是想把某行第四个字段不是空的情况的行里面的第6个字段复制为X。但是这3行只有最后一行被赋值了。
倒数第二行没有赋值。这是怎么回事呢？难道0是个空字符吗？

在awk中，只有数字和字符串2中类型的数据。awk会把看起来像数字的当成数字，其他的当成字符串。
这个"if ($4)"没有给出确定的上下文，它可以测试任何类型的数据。  
在第一行 $4是一个空字符串，所以这里if里面是false。   
在第二行 $4 是 ”0“，这个看起来是个数字，所以awk把它当成数字0 了。而不是字符串"0"，所以if也是false。  

幸运的是我们有方法来告诉awk数据是上面类型。
* 我们可以在$4后面跟上一个空的双引号来告诉awk我们想让这个当成字符串来使用。后面追加个空字符串原来是值是不变的。
* 我们还有可以在$4上加上0来告诉awk我们要把它作为数字来使用，加个0原来的数值是不变的。

```bash
awk -F ',' -v OFS=',' '{if ($4"") $6="X"}1'   # the "" forces awk to evaluate the variable as a string
```

另外一个典型的问题就是下面这个：
```bash

awk '/foo/{tot++} END{print tot}'

```
这个是计算包含/foo/行的个数的，但是如果没有一行匹配，最后是会打印个空字符串的。而不是打印个0.

```bash
awk '/foo/{tot++} END{print tot+0}'
```
和上面说的一样。我们可以给一个变量加上一个0，让awk把它作为数字来使用。


## Pulling out things

假设有这样一段文本。我们像提取出=something=之间的内容的
```bash
Yesterday I was walking in =the street=, when I saw =a
black dog=. There was also =a cat= hidden around there. =The sun= was shining, and =the sky= was blue.
I entered =the
music
shop= and I bought two CDs. Then I went to =the cinema= and watched =a very nice movie=.
End of the story.

```

```bash
awk -v RS='=' '!(NR%2)'
# awk -v RS='=' '!(NR%2){gsub(/\n/," ");print}'    # if you want to reformat embedded newlines

```
这里我们设置了RS为‘=’,这样我们可以打印偶数的。

对于这样的<tag>something</tag>.格式的数据。使用gnu awk可以这样做：
```bash

gawk -v RS='</?tag>' 'RT=="</tag>"'

```
或者
```bash

gawk -v RS='</?tag>' '!(NR%2)'

```

RS          The input record separator, by default a newline.

RT          The record terminator.  Gawk sets RT to the input text that matched the character or regular expression specified by RS.

```bash
gawk -v RS='[0-9]+' 'RT{print RT}'

```
改进版，检查RT是否是null。
```bash
gawk -v RS='--[0-9]+--' 'RT{gsub(/--/,"",RT);print RT}'

```





































```bash
#==

























#==
```
