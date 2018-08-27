
>本文主要介绍了python and or使用的一些小技巧


在python中 and 和 or 执行布尔逻辑运算，但是他们并不返回布尔值，而是返回他们实际进行比较的值之一。

```
'a' and 'b'  这个返回 ‘b’
使用 and 时， 从左到右计算表达式的值。 0、''、[]、()、{}、None 在布尔环境中为假；其它任何东西都真。
在这个例子中  and  计算 ‘a’ 的值为真，然后 是计算 ‘b’的值也为真，最终返回 ‘b’ 这个值。

'' and 'b'
如果布尔环境中的某个值为假，则 and 返回第一个假值。在这例子返回第一个假值。在这例子中， '' 是第一个假值。 

'a' and 'b' and 'c' 
所有值都为真，返回最后一个真值，返回最后一个真值，'c'。
```


```
'a' or 'b'
使用 or 时，计算从左往右，如果有一个值为真，or立刻返回该值，本例中‘a’是第一个真值。

'' or 'b'


'' or [] or {}



def sidefx():
	print "in sidefx()"
	return 1
'a' or sidefx()
注意 注意 or 在布尔环境中会一直进行表达式计算直到找第个真值，然后就会忽略剩余的比较值，如果某些值有副作用，例如这里的函数sidefx就不会被调用了。


```



```
>>> a = "first"
>>> b = "second"
>>> 1 and a or b (1)
'first'
这个语法看起来类似于 C 语言中的 语言中的 语言中的 bool ? a : b 表达式。 整个从左到 右计算， 所以先and 表达式 。
1 and 'first'  值为  'first'，   然后 'first' or 'second' 的值为 'first'。

>>> 0 and a or b (2)
'second'
0 and 'first' 值为 False，然后 0 or 'second' 值为 'second'


>>> a = ""
>>> b = "second"
>>> 1 and a or b 
'second'
像这样的 and 后面的 a  是一个假值，最后返回的是 b 值，并不是我们期望的效果。（我们期望是 and前面的值的真假 来控制
最后返回是 or 左边 还是 or 右边的值，真的情况返回左边的值，假的情况返回右边的值。）

processFunc = collapse and (lambda s: " ".join(s.split())) or (lambda s: s)
像这样的，and后面哪个表达式是一个lambda，不会是假的，这样就可以通过 collapse变量的真假来 决定最后返回的是
or哪边的lambda表达式。

这种方式就是把一个判断从一个函数中摘取出来，通过这个判断来决定使用哪个函数，这样更为高效。
```

```
>>> a = ""
>>> b = "second"
>>> (1 and [a] or [b])[0] (1)
''
这里我们可以采用列表来强制的把or两边的值都设置成真值。这样我们可以根据 and 前面的值 来决定 最后表达式的值拉。

这里其实我们就是想 模仿 一个 bool ？  a ： b  这样的一个效果。

这里在python中我们可以使用 if else 来做的。

```


