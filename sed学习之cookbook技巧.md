
Sed One-Liners Explained, 

# Part 1: File Spacing, Numbering and Text Conversion and Substitution


## 1. File spacing.

1.Double-space a file.
```bash
sed G
```
这个G命令会给每行后面都添加一个新的换行。


2.每行后仅加上一个空行
```bash
sed '/^$/d;G'

```
这个是先删除文件中的空行，然后在每行后面都加上新的空行。  /^$/这个是一个匹配模式，匹配empty lines。

3.Triple-space a file.
```bash
sed 'G;G'

```

4.Undo double-spacing.
```bash
sed 'n;d'

```
5.Insert a blank line above every line that matches "regex".
```bash
sed '/regex/{x;p;x;}'

```


6 Insert a blank line below every line that matches "regex".
```bash
sed '/regex/G'

```

7.Insert a blank line above and below every line that matches "regex".
```bash
sed '/regex/{x;p;x;G;}'

```

## 2. Numbering.


8.Number each line of a file (named filename). Left align the number.
```bash
sed = filename | sed 'N;s/\n/\t/'

```
9.Number each line of a file (named filename). Right align the number.
```bash

sed = filename | sed 'N; s/^/     /; s/ *\(.\{6,\}\)\n/\1  /'
```

10.Number each non-empty line of a file (called filename).
```bash
sed '/./=' filename | sed '/./N; s/\n/ /'

```
11.Count the number of lines in a file (emulates "wc -l").
```bash
sed -n '$='

```

## 3. Text Conversion and Substitution.

12.Convert DOS/Windows newlines (CRLF) to Unix newlines (LF).
```bash
sed 's/.$//'

```

13.Another way to convert DOS/Windows newlines (CRLF) to Unix newlines (LF).
```bash
sed 's/^M$//'

```

14.Yet another way to convert DOS/Windows newlines to Unix newlines.
```bash

sed 's/\x0D$//'
```

15-17. Convert Unix newlines (LF) to DOS/Windows newlines (CRLF).
```bash

sed "s/$/`echo -e \\\r`/"

```
18.Another way to convert Unix newlines (LF) to DOS/Windows newlines (CRLF).
```bash
sed 's/$/\r/'

```

19.Convert Unix newlines (LF) to DOS/Windows newlines (CRLF) from DOS/Windows.
```bash
sed "s/$//"

```
20.Another way to convert Unix newlines (LF) to DOS/Windows newlines (CRLF) from DOS/Windows.
```bash
sed -n p

```
21.Convert DOS/Windows newlines (LF) to Unix format (CRLF) from DOS/Windows.
```bash
sed "s/\r//"

```

22.Delete leading whitespace (tabs and spaces) from each line.
```bash
sed 's/^[ \t]*//'

```

23.Delete trailing whitespace (tabs and spaces) from each line.
```bash
sed 's/[ \t]*$//'

```


24.Delete both leading and trailing whitespace from each line.
```bash
sed 's/^[ \t]*//;s/[ \t]*$//'

```

25.Insert five blank spaces at the beginning of each line.
```bash
sed 's/^/     /'


```



26.Align lines right on a 79-column width.
```bash

sed -e :a -e 's/^.\{1,78\}$/ &/;ta'
```

27.Center all text in the middle of 79-column width.
```bash
sed  -e :a -e 's/^.\{1,77\}$/ & /;ta'

sed  -e :a -e 's/^.\{1,77\}$/ &/;ta' -e 's/\( *\)\1/\1/'

```


28.Substitute (find and replace) the first occurrence of "foo" with "bar" on each line.
```bash
sed 's/foo/bar/'

```

29.Substitute (find and replace) the fourth occurrence of "foo" with "bar" on each line.
```bash

sed 's/foo/bar/4'

```

30.Substitute (find and replace) all occurrence of "foo" with "bar" on each line.
```bash

sed 's/foo/bar/g'

```

31.Substitute (find and replace) the first occurrence of a repeated occurrence of "foo" with "bar".
```bash
sed 's/\(.*\)foo\(.*foo\)/\1bar\2/'

```

32.Substitute (find and replace) only the last occurrence of "foo" with "bar".
```bash
sed 's/\(.*\)foo/\1bar/'
```

33.Substitute all occurrences of "foo" with "bar" on all lines that contain "baz".
```bash
sed '/baz/s/foo/bar/g'

```
34.Substitute all occurrences of "foo" with "bar" on all lines that DO NOT contain "baz".
```bash
sed '/baz/!s/foo/bar/g'
```

35.Change text "scarlet", "ruby" or "puce" to "red".
```bash
sed 's/scarlet/red/g;s/ruby/red/g;s/puce/red/g'

gsed 's/scarlet\|ruby\|puce/red/g'

```

 

36.Reverse order of lines (emulate "tac" Unix command).
```
sed '1!G;h;$!d'
```


37.Reverse a line (emulates "rev" Unix command).
```
sed '/\n/!G;s/\(.\)\(.*\n\)/&\2\1/;//D;s/.//'



 sed '
   /\n/ !G
   s/\(.\)\(.*\n\)/&\2\1/
   //D
   s/.//
 ' 
```

 
38.Join pairs of lines side-by-side (emulates "paste" Unix command).
```
sed '$!N;s/\n/ /'
```
 
39.Append a line to the next if it ends with a backslash "\".
```
sed -e :a -e '/\\$/N; s/\\\n//; ta'
```


40.Append a line to the previous if it starts with an equal sign "=".
```
sed -e :a -e '$!N;s/\n=/ /;ta' -e 'P;D'
```
 
41.Digit group (commify) a numeric string.
```
sed -e :a -e 's/\(.*[0-9]\)\([0-9]\{3\}\)/\1,\2/;ta'
 

gsed ':a;s/\B[0-9]\{3\}\>/,&/;ta'
```

 
```
$ echo "12345 1234 123" | sed 's/[0-9]\{3\}\>/,&/g'
12,345 1,234 ,123

It's clearly wrong. The last 123 got a comma added. Adding the "\B" makes sure we match the numbers only at word boundary:

$ echo "12345 1234 123" | sed 's/\B[0-9]\{3\}\>/,&/g'
12,345 1,234 123
```

 

42.Add commas to numbers with decimal points and minus signs.
```
gsed -r ':a;s/(^|[^0-9.])([0-9]+)([0-9]{3})/\1\2,\3/g;ta'
```
 
43.Add a blank line after every five lines.
```
sed 'n;n;n;n;G;'
```
 




























