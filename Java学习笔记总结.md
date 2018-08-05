
```text
        岗位职责: 主要负责研发效能平台和相关工具的架构与实现，能够准确把握业务需求，参与系统设计，负责核心代码的实现。
        职位要求：
        1. 本科及以上学历，3年以上Java开发经验。 熟悉Java IO、多线程、集合等基础框架，对JVM原理有一定的了解。
        2. 熟悉Spring、Mybatis、Structs等开源框架，对其工作机制和原理有一定程度的了解。
        3. 熟悉分布式设计，了解分布式计算（Hadoop、Storm等）、缓存（Memcache，Redis等）、消息等工作机制，并能应用到实际工作中
        4. 熟悉Mysql数据库，能编写高效的SQL语句并能对SQL进行性能调优，熟悉常见的NoSQL DB，如MongoDB等
        5. 熟悉前端开发，有React/Redux开发经验者优先。
        6. 综合能力：关注技术动态，对技术有热情；有责任心和团队合作精神，心态积极，能主动融入团队。
        7. 其他：有代码管理、CI/CD、测试平台、大数据分析和机器学习等开发经验者优先。

```

Java IO

```text
字节流 InputStream     FilterInputStream,   BufferedInputStream, ByteArrayInputStream,   DataInputStream,  PrintStream  
字节流 OutputStream    FilterOutputStream,  BufferedOutputStream, ByteArrayOutputStream, DataOutputStream, 
字符流 Reader  BufferedReader, LineNumberReader, CharArrayReader, InputStreamReader, FileReader, FilterReader, PushbackReader, PipedReader, StringReader
字符流 Writer  BufferedWriter,                  CharArrayWriter, OutputStreamWriter,  FileWriter,FilterWriter,                 PipedWriter, StringWriter, PrintWriter,

RandomAccessFile 随机访问文件流
```

java IO 之 FilterInputStream和FilterOutputStream
```
java.io包中的字节流。 java.io包中的字节流中的类关系有用到GoF《设计模式》中的装饰者模式，
而这正体现在FilterInputStream和FilterOutputStream和它的子类上。
首先，这两个都分别是InputStream和OutputStream的子类。而且，FilterInputStream和FilterOutputStream 是具体的子类，
实现了InputStream和OutputStream这两个抽象类中为给出实现的方法。
但是，FilterInputStream和FilterOutputStream仅仅是“装饰者模式”封装的开始，它们在各个方法中的实现都是最基本的实现，
都是基于构造方法中传入参数封装的InputStream和OutputStream的原始对象。
比如，在FilterInputStream类中，封装了这样一个属性：
    protected volatile InputStream in;
而对应的构造方法是：
    protected FilterInputStream(InputStream in) {
        this.in = in;
    }
read()方法的实现则为：
    public int read() throws IOException {
        return in.read();
    }

同样的输出流的构造方法是
    public FilterOutputStream(OutputStream out) {
        this.out = out;
    }
write方法的实现：
    public void write(int b) throws IOException {
        out.write(b);
    }

FilterInputStream 的作用是用来“封装其它的输入流，并为它们提供额外的功能”。它的常用的子类有BufferedInputStream和DataInputStream。
BufferedInputStream的作用就是为“输入流提供缓冲功能，以及mark()和reset()功能”。
DataInputStream 是用来装饰其它输入流，它“允许应用程序以与机器无关方式从底层输入流中读取基本 Java 数据类型”。应用程序可以使用DataOutputStream(数据输出流)写入由DataInputStream(数据输入流)读取的数据



FilterOutputStream 的作用是用来“封装其它的输出流，并为它们提供额外的功能”。它主要包括BufferedOutputStream, DataOutputStream和PrintStream。
BufferedOutputStream的作用就是为“输出流提供缓冲功能”。
DataOutputStream 是用来装饰其它输出流，将DataOutputStream和DataInputStream输入流配合使用，“允许应用程序以与机器无关方式从底层输入流中读写基本 Java 数据类型”。
PrintStream 是用来装饰其它输出流。它能为其他输出流添加了功能，使它们能够方便地打印各种数据值表示形式。
```


java IO  之 FileInputStream 和 FileOutputStream
```
FileInputStream 和 FileOutputStream都是直接怼到文件上的，也就是直接访问文件使用的，在这2个流上我们可以包装其他的流。

FileInputStream 是文件输入流，它继承于InputStream。通常，我们使用FileInputStream从某个文件中获得输入字节。

FileInputStream(File file)         // 构造函数1：创建“File对象”对应的“文件输入流”
FileInputStream(FileDescriptor fd) // 构造函数2：创建“文件描述符”对应的“文件输入流”
FileInputStream(String path)       // 构造函数3：创建“文件(路径为path)”对应的“文件输入流”

int      available()             // 返回“剩余的可读取的字节数”或者“skip的字节数”
void     close()                 // 关闭“文件输入流”
FileChannel      getChannel()    // 返回“FileChannel”
final FileDescriptor     getFD() // 返回“文件描述符”
int      read()                  // 返回“文件输入流”的下一个字节
int      read(byte[] buffer, int byteOffset, int byteCount) // 读取“文件输入流”的数据并存在到buffer，从byteOffset开始存储，存储长度是byteCount。
long     skip(long byteCount)    // 跳过byteCount个字节

FileOutputStream 是文件输出流，它继承于OutputStream。通常，我们使用FileOutputStream 将数据写入 File 或 FileDescriptor 的输出流。

FileOutputStream(File file)                   // 构造函数1：创建“File对象”对应的“文件输入流”；默认“追加模式”是false，即“写到输出的流内容”不是以追加的方式添加到文件中。
FileOutputStream(File file, boolean append)   // 构造函数2：创建“File对象”对应的“文件输入流”；指定“追加模式”。
FileOutputStream(FileDescriptor fd)           // 构造函数3：创建“文件描述符”对应的“文件输入流”；默认“追加模式”是false，即“写到输出的流内容”不是以追加的方式添加到文件中。
FileOutputStream(String path)                 // 构造函数4：创建“文件(路径为path)”对应的“文件输入流”；默认“追加模式”是false，即“写到输出的流内容”不是以追加的方式添加到文件中。
FileOutputStream(String path, boolean append) // 构造函数5：创建“文件(路径为path)”对应的“文件输入流”；指定“追加模式”。

void                    close()      // 关闭“输出流”
FileChannel             getChannel() // 返回“FileChannel”
final FileDescriptor    getFD()      // 返回“文件描述符”
void                    write(byte[] buffer, int byteOffset, int byteCount) // 将buffer写入到“文件输出流”中，从buffer的byteOffset开始写，写入长度是byteCount。
void                    write(int oneByte)  // 写入字节oneByte到“文件输出流”中



```
java IO 之  BufferedInputStream  和 BufferedOutputStream
```text
BufferedInputStream 是缓冲输入流。它继承于FilterInputStream。
BufferedInputStream 的作用是为另一个输入流添加一些功能，例如，提供“缓冲功能”以及支持“mark()标记”和“reset()重置方法”。
BufferedInputStream 本质上是通过一个内部缓冲区数组实现的( byte buf[];)。
例如，在新建某输入流对应的BufferedInputStream后，当我们通过read()读取输入流的数据时，BufferedInputStream会将该输入流的数据分批的填入到缓冲区中。
每当缓冲区中的数据被读完之后，输入流会再次填充数据缓冲区；如此反复，直到我们读完输入流数据位置。

说明：
要想读懂BufferedInputStream的源码，就要先理解它的思想。BufferedInputStream的作用是为其它输入流提供缓冲功能。
创建BufferedInputStream时，我们会通过它的构造函数指定某个输入流为参数。BufferedInputStream会将该输入流数据分批读取，每次读取一部分到缓冲中；操作完缓冲中的这部分数据之后，再从输入流中读取下一部分的数据。
为什么需要缓冲呢？原因很简单，效率问题！缓冲中的数据实际上是保存在内存中，而原始数据可能是保存在硬盘或NandFlash等存储介质中；而我们知道，从内存中读取数据的速度比从硬盘读取数据的速度至少快10倍以上。
那干嘛不干脆一次性将全部数据都读取到缓冲中呢？第一，读取全部的数据所需要的时间可能会很长。第二，内存价格很贵，容量不像硬盘那么大。


BufferedOutputStream 是缓冲输出流。它继承于FilterOutputStream。
BufferedOutputStream 的作用是为另一个输出流提供“缓冲功能”。
说明：
BufferedOutputStream的源码非常简单，这里就BufferedOutputStream的思想进行简单说明：B
ufferedOutputStream通过字节数组来缓冲数据，当缓冲区满或者用户调用flush()函数时，它就会将缓冲区的数据写入到输出流中。
 

当写入缓冲区小于实际的缓冲区大小，他不会执行情况缓冲区操作（即，将缓冲数据写入到输出流中），在执行close()后 它会关闭输出流；在关闭输出流之前，会将缓冲区的数据写入到输出流中。
如果手动执行了flush()操作会将缓冲区的数据写入到输出流中。
如果写入缓冲区的大于实际缓冲区大小，他会直接 写入都输出流中，而不经过缓冲区。


```

java IO 之 FileDescriptor
```
FileDescriptor 是“文件描述符”。
FileDescriptor 可以被用来表示开放文件、开放套接字等。
以FileDescriptor表示文件来说：当FileDescriptor表示某文件时，我们可以通俗的将FileDescriptor看成是该文件。
但是，我们不能直接通过FileDescriptor对该文件进行操作；若需要通过FileDescriptor对该文件进行操作，则需要新创建FileDescriptor对应的FileOutputStream，再对文件进行操作。

in, out, err介绍

(01) in  -- 标准输入(键盘)的描述符
 	public static final FileDescriptor in = standardStream(0);

(02) out -- 标准输出(屏幕)的描述符
    public static final FileDescriptor out = standardStream(1);

(03) err -- 标准错误输出(屏幕)的描述符
    public static final FileDescriptor err = standardStream(2);


out 的作用和原理

out是标准输出(屏幕)的描述符。但是它有什么作用呢？
我们可以通俗理解，out就代表了标准输出(屏幕)。若我们要输出信息到屏幕上，即可通过out来进行操作；
但是，out又没有提供输出信息到屏幕的接口(因为out本质是FileDescriptor对象，而FileDescriptor没有输出接口)。怎么办呢？
很简单，我们创建out对应的“输出流对象”，然后通过“输出流”的write()等输出接口就可以将信息输出到屏幕上。如下代码：
try {
    FileOutputStream out = new FileOutputStream(FileDescriptor.out);
    out.write('A');
    out.close();
} catch (IOException e) {
}
执行上面的程序，会在屏幕上输出字母'A'。

为了方便我们操作，java早已为我们封装好了“能方便的在屏幕上输出信息的接口”：通过System.out，我们能方便的输出信息到屏幕上。
因此，我们可以等价的将上面的程序转换为如下代码：
System.out.print('A');


```


java IO之 字节数组流 ByteArrayInputStream 和 ByteArrayOutputStream
```
ByteArrayInputStream 是字节数组输入流。它继承于InputStream。
它包含一个内部缓冲区，该缓冲区包含从流中读取的字节；通俗点说，它的内部缓冲区就是一个字节数组，而ByteArrayInputStream本质就是通过字节数组来实现的。
我们都知道，InputStream通过read()向外提供接口，供它们来读取字节数据；而ByteArrayInputStream 的内部额外的定义了一个计数器，它被用来跟踪 read() 方法要读取的下一个字节。

ByteArrayInputStream实际上是通过“字节数组”去保存数据。
(01) 通过ByteArrayInputStream(byte buf[]) 或 ByteArrayInputStream(byte buf[], int offset, int length) ，我们可以根据buf数组来创建字节流对象。
(02) read()的作用是从字节流中“读取下一个字节”。
(03) read(byte[] buffer, int offset, int length)的作用是从字节流读取字节数据，并写入到字节数组buffer中。offset是将字节写入到buffer的起始位置，length是写入的字节的长度。
(04) markSupported()是判断字节流是否支持“标记功能”。它一直返回true。
(05) mark(int readlimit)的作用是记录标记位置。记录标记位置之后，某一时刻调用reset()则将“字节流下一个被读取的位置”重置到“mark(int readlimit)所标记的位置”；也就是说，reset()之后再读取字节流时，是从mark(int readlimit)所标记的位置开始读取。


ByteArrayOutputStream 是字节数组输出流。它继承于OutputStream。
ByteArrayOutputStream 中的数据被写入一个 byte 数组。缓冲区会随着数据的不断写入而自动增长。可使用 toByteArray() 和 toString() 获取数据。

ByteArrayOutputStream实际上是将字节数据写入到“字节数组”中去。
(01) 通过ByteArrayOutputStream()创建的“字节数组输出流”对应的字节数组大小是32。
(02) 通过ByteArrayOutputStream(int size) 创建“字节数组输出流”，它对应的字节数组大小是size。
(03) write(int oneByte)的作用将int类型的oneByte换成byte类型，然后写入到输出流中。
(04) write(byte[] buffer, int offset, int len) 是将字节数组buffer写入到输出流中，offset是从buffer中读取数据的起始偏移位置，len是读取的长度。
(05) writeTo(OutputStream out) 将该“字节数组输出流”的数据全部写入到“输出流out”中。

 

```

java IO 之   管道  PipedInputStream 和 PipedOutputStream 
```
PipedInputStream 是管道输入流，继承自InputStream。
PipedOutputStream 是管道输出流 ， 继承自OutputStream。
它们的作用是让多线程可以通过管道进行线程间的通讯。在使用管道通信时，必须将PipedOutputStream和PipedInputStream配套使用。
使用管道通信时，大致的流程是：我们在线程A中向PipedOutputStream中写入数据，这些数据会自动的发送到与PipedOutputStream对应的PipedInputStream中，
进而存储在PipedInputStream的缓冲中；此时，线程B通过读取PipedInputStream中的数据。就可以实现，线程A和线程B的通信。


```
java IO 之 PipedReader和PipedWriter
```text

PipedWriter 是字符管道输出流，它继承于Writer。
PipedReader 是字符管道输入流，它继承于Writer。
PipedWriter和PipedReader的作用是可以通过管道进行线程间的通讯。在使用管道通信时，必须将PipedWriter和PipedReader配套使用。




```

java IO 之 对象流 ObjectInputStream 和 ObjectOutputStream 
```
ObjectInputStream 和 ObjectOutputStream 的作用是，对基本数据和对象进行序列化操作支持。

创建“文件输出流”对应的ObjectOutputStream对象，该ObjectOutputStream对象能提供对“基本数据或对象”的持久存储；
当我们需要读取这些存储的“基本数据或对象”时，可以创建“文件输入流”对应的ObjectInputStream，进而读取出这些“基本数据或对象”。

通过ObjectInputStream, ObjectOutputStream操作的对象，必须是实现了Serializable或Externalizable序列化接口的类的实例。

序列化，就是为了保存对象的状态；而与之对应的反序列化，则可以把保存的对象状态再读出来。
简言之：序列化/反序列化，是Java提供一种专门用于的保存/恢复对象状态的机制。

一般在以下几种情况下，我们可能会用到序列化：
a）当你想把的内存中的对象状态保存到一个文件中或者数据库中时候； 
b）当你想用套接字在网络上传送对象的时候； 
c）当你想通过RMI传输对象的时候。

(01) 序列化对static和transient变量，是不会自动进行状态保存的。
transient的作用就是，用transient声明的变量，不会被自动序列化。
(02) 对于Socket, Thread类，不支持序列化。若实现序列化的接口中，有Thread成员；在对该类进行序列化操作时，编译会出错！
这主要是基于资源分配方面的原因。如果Socket，Thread类可以被序列化，但是被反序列化之后也无法对他们进行重新的资源分配；再者，也是没有必要这样实现。


“序列化不会自动保存static和transient变量”，因此我们若要保存它们，则需要通过writeObject()和readObject()去手动读写。
事实证明，不能对Thread进行序列化。若希望程序能编译通过，我们对Thread变量添加static或transient修饰即可。


如果一个类要完全负责自己的序列化，则实现Externalizable接口，而不是Serializable接口。
Externalizable接口定义包括两个方法writeExternal()与readExternal()。需要注意的是：声明类实现Externalizable接口会有重大的安全风险。
writeExternal()与readExternal()方法声明为public，恶意类可以用这些方法读取和写入对象数据。如果对象包含敏感信息，则要格外小心。
(01) 实现Externalizable接口的类，不会像实现Serializable接口那样，会自动将数据保存。
(02) 实现Externalizable接口的类，必须实现writeExternal()和readExternal()接口！ 否则，程序无法正常编译！
(03) 实现Externalizable接口的类，必须定义不带参数的构造函数！ 否则，程序无法正常编译！
(04) writeExternal() 和 readExternal() 的方法都是public的，不是非常安全！



```

java IO 之 DataInputStream  和 DataInputStream 
```text
DataInputStream 是数据输入流。它继承于FilterInputStream。
DataInputStream 是用来装饰其它输入流，它“允许应用程序以与机器无关方式从底层输入流中读取基本 Java 数据类型”。
应用程序可以使用DataOutputStream(数据输出流)写入，由DataInputStream(数据输入流)读取的数据。

DataInputStream(InputStream in)
final int     read(byte[] buffer, int offset, int length)
final int     read(byte[] buffer)
final boolean     readBoolean()
final byte     readByte()
final char     readChar()
final double     readDouble()
final float     readFloat()
final void     readFully(byte[] dst)
final void     readFully(byte[] dst, int offset, int byteCount)
final int     readInt()
final String     readLine()
final long     readLong()
final short     readShort()
final static String     readUTF(DataInput in)
final String     readUTF()
final int     readUnsignedByte()
final int     readUnsignedShort()
final int     skipBytes(int count)

DataInputStream 中比较难以理解的函数就只有 readUTF(DataInput in)；
说明：
readUTF()的作用，是从输入流中读取UTF-8编码的数据，并以String字符串的形式返回。

DataOutputStream 是数据输出流。它继承于FilterOutputStream。
DataOutputStream 是用来装饰其它输出流，将DataOutputStream和DataInputStream输入流配合使用，“允许应用程序以与机器无关方式从底层输入流中读写基本 Java 数据类型”。


```
java IO 之 PrintStream(打印输出流)
```text
PrintStream 是打印输出流，它继承于FilterOutputStream。
PrintStream 是用来装饰其它输出流。它能为其他输出流添加了功能，使它们能够方便地打印各种数据值表示形式。
与其他输出流不同，PrintStream 永远不会抛出 IOException；它产生的IOException会被自身的函数所捕获并设置错误标记， 用户可以通过 checkError() 返回错误标记，从而查看PrintStream内部是否产生了IOException。
另外，PrintStream 提供了自动flush 和 字符集设置功能。所谓自动flush，就是往PrintStream写入的数据会立刻调用flush()函数。


我们先看看System.out.println的流程。先看看System.java中out的定义，源码如下：
    public final static PrintStream out = null;


```

java IO 之 CharArrayReader 和 CharArrayWriter 
```text
CharArrayReader 是字符数组输入流。它和ByteArrayInputStream类似，只不过ByteArrayInputStream是字节数组输入流，
而CharArray是字符数组输入流。CharArrayReader 是用于读取字符数组，它继承于Reader。操作的数据是以字符为单位！
说明：
CharArrayReader实际上是通过“字符数组”去保存数据。

(01) 通过 CharArrayReader(char[] buf) 或 CharArrayReader(char[] buf, int offset, int length) ，我们可以根据buf数组来创建CharArrayReader对象。
(02) read()的作用是从CharArrayReader中“读取下一个字符”。
(03) read(char[] buffer, int offset, int len)的作用是从CharArrayReader读取字符数据，并写入到字符数组buffer中。offset是将字符写入到buffer的起始位置，len是写入的字符的长度。
(04) markSupported()是判断CharArrayReader是否支持“标记功能”。它始终返回true。
(05) mark(int readlimit)的作用是记录标记位置。记录标记位置之后，某一时刻调用reset()则将“CharArrayReader下一个被读取的位置”重置到“mark(int readlimit)所标记的位置”；也就是说，reset()之后再读取CharArrayReader时，是从mark(int readlimit)所标记的位置开始读取。


CharArrayWriter 用于写入数据符，它继承于Writer。操作的数据是以字符为单位！
说明：
CharArrayWriter实际上是将数据写入到“字符数组”中去。
(01) 通过CharArrayWriter()创建的CharArrayWriter对应的字符数组大小是32。
(02) 通过CharArrayWriter(int size) 创建的CharArrayWriter对应的字符数组大小是size。
(03) write(int oneChar)的作用将int类型的oneChar换成char类型，然后写入到CharArrayWriter中。
(04) write(char[] buffer, int offset, int len) 是将字符数组buffer写入到输出流中，offset是从buffer中读取数据的起始偏移位置，len是读取的长度。
(05) write(String str, int offset, int count) 是将字符串str写入到输出流中，offset是从str中读取数据的起始位置，count是读取的长度。
(06) append(char c)的作用将char类型的c写入到CharArrayWriter中，然后返回CharArrayWriter对象。
注意：append(char c)与write(int c)都是将单个字符写入到CharArrayWriter中。它们的区别是，append(char c)会返回CharArrayWriter对象，但是write(int c)返回void。
(07) append(CharSequence csq, int start, int end)的作用将csq从start开始(包括)到end结束(不包括)的数据，写入到CharArrayWriter中。
注意：该函数返回CharArrayWriter对象！
(08) append(CharSequence csq)的作用将csq写入到CharArrayWriter中。
注意：该函数返回CharArrayWriter对象！
(09) writeTo(OutputStream out) 将该“字符数组输出流”的数据全部写入到“输出流out”中。


```

java IO 之 InputStreamReader 和 OutputStreamWriter
```text
InputStreamReader和OutputStreamWriter 是字节流通向字符流的桥梁：它使用指定的 charset 读写字节并将其解码为字符。
InputStreamReader 的作用是将“字节输入流”转换成“字符输入流”。它继承于Reader。
OutputStreamWriter 的作用是将“字节输出流”转换成“字符输出流”。它继承于Writer。

```
java IO 之 FileReader和FileWriter
```text

FileReader 是用于读取字符流的类，它继承于InputStreamReader。要读取原始字节流，请考虑使用 FileInputStream。
FileWriter 是用于写入字符流的类，它继承于OutputStreamWriter。要写入原始字节流，请考虑使用 FileOutputStream。

```

java IO 之 BufferedReader(字符缓冲输入流) 和 BufferedWriter(字符缓冲输出流)
```text
BufferedReader 是缓冲字符输入流。它继承于Reader。
BufferedReader 的作用是为其他字符输入流添加一些缓冲功能。
// 构造函数
BufferedReader(Reader in)
BufferedReader(Reader in, int size)

void     close()
void     mark(int markLimit)
boolean  markSupported()
int      read()
int      read(char[] buffer, int offset, int length)
String   readLine()
boolean  ready()
void     reset()
long     skip(long charCount)


BufferedWriter 是缓冲字符输出流。它继承于Writer。
BufferedWriter 的作用是为其他字符输出流添加一些缓冲功能。
// 构造函数
BufferedWriter(Writer out) 
BufferedWriter(Writer out, int sz) 
 
void    close()                              // 关闭此流，但要先刷新它。
void    flush()                              // 刷新该流的缓冲。
void    newLine()                            // 写入一个行分隔符。
void    write(char[] cbuf, int off, int len) // 写入字符数组的某一部分。
void    write(int c)                         // 写入单个字符。
void    write(String s, int off, int len)    // 写入字符串的某一部分。


```

java IO 之 PrintWriter (字符打印输出流)
```text
PrintWriter 是字符类型的打印输出流，它继承于Writer。

PrintStream 用于向文本输出流打印对象的格式化表示形式。它实现在 PrintStream 中的所有 print 方法。
它不包含用于写入原始字节的方法，对于这些字节，程序应该使用未编码的字节流进行写入。



```
java IO 之 RandomAccessFile
```text
RandomAccessFile 是随机访问文件(包括读/写)的类。它支持对文件随机访问的读取和写入，即我们可以从指定的位置读取/写入文件数据。
需要注意的是，RandomAccessFile 虽然属于java.io包，但它不是InputStream或者OutputStream的子类；
它也不同于FileInputStream和FileOutputStream。 FileInputStream 只能对文件进行读操作，而FileOutputStream 只能对文件进行写操作；
但是，RandomAccessFile 同时支持文件的读和写，并且它支持随机访问。

RandomAccessFile共有4种模式："r", "rw", "rws"和"rwd"。
"r"    以只读方式打开。调用结果对象的任何 write 方法都将导致抛出 IOException。  
"rw"   打开以便读取和写入。
"rws"  打开以便读取和写入。相对于 "rw"，"rws" 还要求对“文件的内容”或“元数据”的每个更新都同步写入到基础存储设备。  
"rwd"  打开以便读取和写入，相对于 "rw"，"rwd" 还要求对“文件的内容”的每个更新都同步写入到基础存储设备。


```


java NIO
```
NIO即New IO，这个库是在JDK1.4中才引入的。NIO和IO有相同的作用和目的，但实现方式不同，
NIO主要用到的是块，所以NIO的效率要比IO高很多。在Java API中提供了两套NIO，一套是针对标准输入输出NIO，另一套就是网络编程NIO。

Java IO和NIO之间第一个最大的区别是，IO是面向流的，NIO是面向缓冲区的。 

Java IO面向流意味着每次从流中读一个或多个字节，直至读取所有字节，它们没有被缓存在任何地方。此外，它不能前后移动流中的数据。如果需要前后移动从流中读取的数据，需要先将它缓存到一个缓冲区。 
Java NIO的缓冲导向方法略有不同。数据读取到一个它稍后处理的缓冲区，需要时可在缓冲区中前后移动。这就增加了处理过程中的灵活性。但是，还需要检查是否该缓冲区中包含所有您需要处理的数据。而且，需确保当更多的数据读入缓冲区时，不要覆盖缓冲区里尚未处理的数据。

Java IO的各种流是阻塞的。这意味着，当一个线程调用read() 或 write()时，该线程被阻塞，直到有一些数据被读取，或数据完全写入。该线程在此期间不能再干任何事情了。
Java NIO的非阻塞模式，使一个线程从某通道发送请求读取数据，但是它仅能得到目前可用的数据，如果目前没有数据可用时，就什么都不会获取，而不是保持线程阻塞，所以直至数据变的可以读取之前，该线程可以继续做其他的事情。 非阻塞写也是如此。一个线程请求写入一些数据到某通道，但不需要等待它完全写入，这个线程同时可以去做别的事情。 线程通常将非阻塞IO的空闲时间用于在其它通道上执行IO操作，所以一个单独的线程现在可以管理多个输入和输出通道（channel）。


Java NIO的选择器允许一个单独的线程来监视多个输入通道，你可以注册多个通道使用一个选择器，然后使用一个单独的线程来“选择”通道：这些通道里已经有可以处理的输入，或者选择已准备写入的通道。这种选择机制，使得一个单独的线程很容易来管理多个通道。非阻塞是对于网络io的。


NIO缓冲区，buffer，负责存取数据的，顶层是数组。
allocate() 分配非直接缓冲区，建立在jvm的内存中。
allocateDirect（）分配直接缓冲区。

put（）存入数据到缓冲区
get（）获取缓冲区中的数据
flip()  切换读写模式    先设置limit为当前的position。 然后重置position为0
rewind（）可重复读取数据  重置position为0
clear（）重置position为0，limit为capacity。

capacity 容量，缓冲区最大存储容量，一旦声明不能改变。
limit 界限，缓冲区中可以操作数据的大小，limit后面的数据是不能进行读写操作的。
position 位置，缓冲区中正在操作的数据的位置。
mark 记录当前position的位置的，可以通过mark（）和reset（）来设置和恢复

0 <= mark <= position <= limit <= capacity



通道直接的数据传输
inChannel.transferTo(0, inChannel.size(), outChannel);

outChannel.transferFrom(inChannel, 0, inChannel.size());


分散读取，聚集写入
ByteBuffer[] bufs = {byteBuffer1, byteBuffer2};
channel.read(bufs);
        
ByteBuffer[] bufs = {byteBuffer1, byteBuffer2};
channel.write(bufs);  //GatheringByteChannel

字符编码和解码
编码是  字符串   转换为  字节数组
解码是  字节数组  转换为 字符串
















```


String, StringBuffer, StringBuilder 的区别是什么？String为什么是不可变的？

```text
答：

1、String是字符串常量，StringBuffer和StringBuilder都是字符串变量。后两者的字符内容可变，而前者创建后内容不可变。

2、String不可变是因为在JDK中String类被声明为一个final类。

3、StringBuffer是线程安全的，而StringBuilder是非线程安全的。

ps：线程安全会带来额外的系统开销，所以StringBuilder的效率比StringBuffer高。
如果对系统中的线程是否安全很掌握，可用StringBuffer，在线程不安全处加上关键字Synchronize。

```

集合相关面试分析
```text
java集合分为两类，collection 和  map 两种，两种是并列的。

collection 存的值。
    set 存储无序的不可重复的数据
        HashSet   里面其实是用的HashMap来实现的      (添加的自定义类的对象需要这个类重写equals和hashCode方法)
        LinkedHashSet    存储是按照哈希值无序存放的，遍历的时候可以按照顺序依次遍历。
        TreeSet         (自然排序Comparable接口和定制排序)
    list 存储有序的，可以重复的数据，动态数组（ArrayList, LinkedList, Vector）
        ArrayList  线程不安全的
        LinkedList   对于频繁的插入，和删除操作建议使用这个
        Vector 线程安全的

map存 键和值。
    HashMap
    LinkedHashMap
    HashTable

```
```text
Comparable可以认为是一个内比较器，实现了Comparable接口的类有一个特点，就是这些类是可以和自己比较的，
至于具体和另一个实现了Comparable接口的类如何比较，则依赖compareTo方法的实现，compareTo方法也被称为自然比较方法。
如果开发者add进入一个Collection的对象想要Collections的sort方法帮你自动进行排序的话，那么这个对象必须实现Comparable接口。
compareTo方法的返回值是int，有三种情况：
1、比较者大于被比较者（也就是compareTo方法里面的对象），那么返回正整数
2、比较者等于被比较者，那么返回0
3、比较者小于被比较者，那么返回负整数
接口所在包   java.lang.Comparable    
排序方法    int compareTo(T o);    
触发排序    Collections.sort(List)



Comparator可以认为是是一个外比较器，个人认为有两种情况可以使用实现Comparator接口的方式：
1、一个对象不支持自己和自己比较（没有实现Comparable接口），但是又想对两个对象进行比较
2、一个对象实现了Comparable接口，但是开发者认为compareTo方法中的比较方式并不是自己想要的那种比较方式
Comparator接口里面有一个compare方法，方法有两个参数T o1和T o2，是泛型的表示方式，分别表示待比较的两个对象，方法返回值和Comparable接口一样是int，
有三种情况： 
1、o1大于o2，返回正整数
2、o1等于o2，返回0
3、o1小于o3，返回负整数

接口所在包   java.util.Comparator
排序方法    int compare(T o1, T o2); 
触发排序    Collections.sort(List, Comparator)
```


Vector, ArrayList, LinkedList 的区别是什么？
```text
1、Vector、ArrayList都是以类似数组的形式存储在内存中，LinkedList则以双向链表的形式进行存储。
Vector、ArrayList都是list接口的实现类,也都继承了AbstractList类。

2、List中的元素有序、允许有重复的元素，Set中的元素无序、不允许有重复元素。

3、Vector线程同步,其中有些方法是带synchronized关键字的，ArrayList、LinkedList线程不同步。

4、LinkedList适合指定位置插入、删除操作，不适合查找；ArrayList、Vector适合查找，不适合指定位置的插入、删除操作。

5、ArrayList在元素填满容器时会自动扩充容器大小的50%，而Vector则是100%，因此ArrayList更节省空间。


List l = new ArrayList();
jdk8当中在添加第一个元素的时候才去创建底层的那个数组，jdk7一开始就创建一个默认长度10的数组。


```

HashTable, HashMap，TreeMap 区别？
```text
HashTable存储数据是用的的这个：transient Entry<?,?>[] table

HashMap存储数据是用的的这个：transient Node<K,V>[] table;


1、HashTable线程同步，HashMap非线程同步。

2、HashTable不允许<键,值>有空值，HashMap允许<键,值>有空值。

3、HashTable使用Enumeration，HashMap使用Iterator。

4、HashTable中hash数组的默认大小是11，增加方式的old*2+1，HashMap中hash数组的默认大小是16，增长方式一定是2的指数倍。

5、TreeMap能够把它保存的记录根据键排序，默认是按升序排序。适用于按自然顺序或自定义顺序遍历键(key)

Map主要用于存储健值对，根据键得到值，因此不允许键重复(重复了覆盖了),但允许值重复。

Hashmap 是一个最常用的Map,它根据键的HashCode值存储数据,根据键可以直接获取它的值，具有很快的访问速度，遍历时，取得数据的顺序是完全随机的。
HashMap最多只允许一条记录的键为Null;允许多条记录的值为 Null;HashMap不支持线程的同步，即任一时刻可以有多个线程同时写HashMap;可能会导致数据的不一致。
如果需要同步，可以用 Collections的synchronizedMap方法使HashMap具有同步的能力，或者使用ConcurrentHashMap。

Hashtable与 HashMap类似,它继承自Dictionary类，不同的是:它不允许记录的键或者值为空;它支持线程的同步，
即任一时刻只有一个线程能写Hashtable,因此也导致了 Hashtable在写入时会比较慢。

LinkedHashMap是HashMap的一个子类，保存了记录的插入顺序，在用Iterator遍历LinkedHashMap时，先得到的记录肯定是先插入的.
也可以在构造时用带参数，按照应用次数排序。
在遍历的时候会比HashMap慢，不过有种情况例外，当HashMap容量很大，实际数据较少时，遍历起来可能会比LinkedHashMap慢，
因为LinkedHashMap的遍历速度只和实际数据有关，和容量无关，而HashMap的遍历速度和他的容量有关。

TreeMap实现SortMap接口，能够把它保存的记录根据键排序,默认是按键值的升序排序，也可以指定排序的比较器，当用Iterator 遍历TreeMap时，得到的记录是排过序的。

一般情况下，我们用的最多的是HashMap,在Map 中插入、删除和定位元素，HashMap 是最好的选择。
但如果您要按自然顺序或自定义顺序遍历键，那么TreeMap会更好。
如果需要输出的顺序和输入的相同,那么用LinkedHashMap可以实现,它还可以按读取顺序来排列.

hashMap使用array和list俩表示，根据键的hashcode来计算在数组中哪个位置存放数据，然后在数据存放到list中。
当数据量很大的时候，会把list转为红黑树来存放的（此数组i位置上的链表元素个数大于8且整个数组长度大于64才使用红黑树）。
```

GET，POST区别？
```text
基础知识：Http的请求格式如下。

<request line>   主要包含三个信息：1、请求的类型（GET或POST），2、要访问的资源（如\res\img\a.jif），3、Http版本（http/1.1）

<header>        用来说明服务器要使用的附加信息

<blank line>      这是Http的规定，必须空一行

[<request-body>]   请求的内容数据


1、Get是从服务器端获取数据，Post则是向服务器端发送数据。

2、在客户端，Get方式通过URL提交数据，在URL地址栏可以看到请求消息，该消息被编码过；Post数据则是放在Html header内提交。

3、对于Get方式，服务器端用Request.QueryString获取变量的值；对用Post方式，服务器端用Request.Form获取提交的数据值。

4、Get方式提交的数据最多1024字节，而Post则没有限制。

5、Get方式提交的参数及参数值会在地址栏显示，不安全，而Post不会，比较安全。
```

Session, Cookie区别

```text
答：  

1、Session由应用服务器维护的一个服务器端的存储空间；Cookie是客户端的存储空间，由浏览器维护。

2、用户可以通过浏览器设置决定是否保存Cookie，而不能决定是否保存Session，因为Session是由服务器端维护的。

3、Session中保存的是对象，Cookie中保存的是字符串。

4、Session和Cookie不能跨窗口使用，每打开一个浏览器系统会赋予一个SessionID，此时的SessionID不同，若要完成跨浏览器访问数据，可以使用 Application。

5、Session、Cookie都有失效时间，过期后会自动删除，减少系统开销。



```

Servlet的生命周期
```text
大致分为4部：Servlet类加载-->实例化-->服务-->销毁

1、Web Client向Servlet容器(Tomcat)发出Http请求。

2、Servlet容器接收Client端的请求。

3、Servlet容器创建一个HttpRequest对象，将Client的请求信息封装到这个对象中。

4、Servlet创建一个HttpResponse对象。

5、Servlet调用HttpServlet对象的service方法，把HttpRequest对象和HttpResponse对象作为参数传递给HttpServlet对象中。

6、HttpServlet调用HttpRequest对象的方法，获取Http请求，并进行相应处理。

7、处理完成HttpServlet调用HttpResponse对象的方法，返回响应数据。

8、Servlet容器把HttpServlet的响应结果传回客户端。



其中的3个方法说明了Servlet的生命周期：

1、init()：负责初始化Servlet对象。

2、service()：负责响应客户端请求。

3、destroy()：当Servlet对象推出时，负责释放占用资源。

```
HTTP 报文包含内容
```text
主要包含四部分：

1、request line

2、header line

3、blank line

4、request body

```

Statement与PreparedStatement的区别,什么是SQL注入，如何防止SQL注入
```text
PreparedStatement接口 继承 statment接口。

1、PreparedStatement支持动态设置参数，Statement不支持。

2、PreparedStatement可避免如类似 单引号 的编码麻烦，Statement不可以。

3、PreparedStatement支持预编译，Statement不支持。
preparedStatement = connection.prepareStatement(sql);
preparedStatement.setObject(i + 1, args[i]);

4、在sql语句出错时PreparedStatement不易检查，而Statement则更便于查错。

5、PreparedStatement可防止Sql助于，更加安全，而Statement不行。

 

什么是SQL注入： 通过sql语句的拼接达到无参数查询数据库数据目的的方法。

 如将要执行的sql语句为 select * from table where name = "+appName+"，利用appName参数值的输入，来生成恶意的sql语句，如将['or'1'='1'] 传入可在数据库中执行。

 因此可以采用PrepareStatement来避免Sql注入，在服务器端接收参数数据后，进行验证，此时PrepareStatement会自动检测，而Statement不行，需要手工检测。
```


Redirect, foward区别
```text

1、foward是服务器端控制页面转向，在客户端的浏览器地址中不会显示转向后的地址；sendRedirect则是完全的跳转，浏览器中会显示跳转的地址并重新发送请求链接。

原理：forward是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后再将这些内容返回给浏览器，
浏览器根本不知道服务器发送的这些内容是从哪来的，所以地址栏还是原来的地址。

redirect是服务器端根据逻辑，发送一个状态码，告诉浏览器重新去请求的那个地址，浏览器会用刚才的所有参数重新发送新的请求。


```

谈谈Hibernate的理解，一级和二级缓存的作用，在项目中Hibernate都是怎么使用缓存的。
```text

Hibernate是一个开发的对象关系映射框架（ORM）。它对JDBC进行了非常对象封装，Hibernate允许程序员采用面向对象的方式来操作关系数据库。

Hibernate的优点：
    
    1、程序更加面向对象
    
    2、提高了生产率
    
    3、方便移植
    
    4、无入侵性。

缺点：
    
    1、效率比JDBC略差
    
    2、不适合批量操作
    
    3、只能配置一种关联关系

Hibernate有四种查询方式：
    
    1、get、load方法，根据id号查询对象。
    
    2、Hibernate query language   (HQL)
    
    3、标准查询语言
    
    4、通过sql查询


Hibernage工作原理：

    1、配置hibernate对象关系映射文件、启动服务器
    
    2、服务器通过实例化Configuration对象，读取hibernate.cfg.xml文件的配置内容，并根据相关的需求建好表以及表之间的映射关系。
    
    3、通过实例化的Configuration对象建立SeesionFactory实例，通过SessionFactory实例创建Session对象。
    
    4、通过Seesion对象完成数据库的增删改查操作。
    
    
Hibernate中的状态转移

临时状态（transient）
    
    1、不处于session缓存中
    
    2、数据库中没有对象记录
    
    java是如何进入临时状态的：
        1、通过new语句创建一个对象时。
        2、刚调用session的delete方法时，从seesion缓存中删除一个对象时。

持久化状态(persisted)

    1、处于session缓存中
    
    2、持久化对象数据库中没有对象记录
    
    3、seesion在特定的时刻会保存两者同步
    
    java如何进入持久化状态：
        1、seesion的save()方法。
        2、seesion的load().get()方法返回的对象。
        3、seesion的find()方法返回的list集合中存放的对象。
        4、Session的update().save()方法。

流离状态（detached）

    1、不再位于session缓存中
    
    2、游离对象由持久化状态转变而来，数据库中还没有相应记录。
    
    java如何进入流离状态：
        1、Session的close()。Session的evict()方法，从缓存中删除一个对象。

 

Hibernate中的缓存主要有Session缓存（一级缓存）和SessionFactory缓存（二级缓存，一般由第三方提供）。 

Session的缓存是内置的，不能被卸载，也被称为Hibernate的第一级缓存（Session的一些集合属性包含的数据）。在第一级缓存中，持久化类的每个实例都具有唯一的OID。

 

SessionFactory的缓存又可以分为两类：

    内置缓存（存放SessionFactory对象的一些集合属性包含的数据，SessionFactory的内置缓存中存放了映射元数据和预定义SQL语句，
    映射元数据是映射文件中数据的拷贝，而预定义SQL语句是在Hibernate初始化阶段根据映射元数据推导出来，SessionFactory的内置缓存是只读的，
    应用程序不能修改缓存中的映射元数据和预定义SQL语句，因此SessionFactory不需要进行内置缓存与映射文件的同步。）

    外置缓存。SessionFactory的外置缓存是一个可配置的插件。在默认情况下，SessionFactory不会启用这个插件。
    外置缓存的数据是数据库数据的拷贝，外置缓存的介质可以是内存或者硬盘。SessionFactory的外置缓存也被称为Hibernate的第二级缓存。

 

缓存的范围分为三类。
    1 事务范围：缓存只能被当前事务访问。
    2 进程范围：缓存被进程内的所有事务共享。
    3 集群范围：在集群环境中，缓存被一个机器或者多个机器的进程共享。

 

持久化层可以提供多种范围的缓存。如果在事务范围的缓存中没有查到相应的数据，还可以到进程范围或集群范围的缓存内查询，如果还是没有查到，那么只有到数据库中查询。事务范围的缓存是持久化层的第一级缓存，通常它是必需的；进程范围或集群范围的缓存是持久化层的第二级缓存，通常是可选的。

在进程范围或集群范围的缓存，即第二级缓存，会出现并发问题。因此可以设定以下四种类型的并发访问策略，每一种策略对应一种事务隔离级别。

 

事务型、读写型、非严格读写型、只读型

    什么样的数据适合存放到第二级缓存中？
    1 很少被修改的数据
    2 不是很重要的数据，允许出现偶尔并发的数据
    3 不会被并发访问的数据
    4 参考数据
    不适合存放到第二级缓存的数据？
    1 经常被修改的数据
    2 财务数据，绝对不允许出现并发
    3 与其他应用共享的数据。

    Hibernate的二级缓存策略的一般过程如下：
    1) 条件查询的时候，总是发出一条select * from table_name where …. （选择所有字段）这样的SQL语句查询数据库，一次获得所有的数据对象。
    2) 把获得的所有数据对象根据ID放入到第二级缓存中。
    3) 当Hibernate根据ID访问数据对象的时候，首先从Session一级缓存中查；查不到，如果配置了二级缓存，那么从二级缓存中查；查不到，再查询数据库，把结果按照ID放入到缓存。
    4) 删除、更新、增加数据的时候，同时更新缓存。


关系映射：对象之间的对应数量关系

1 对 1：（双向外键关联的意思：在程序中两边都设置对方的引用，但是在数据库中一样的，注意设置 mappedBy属性）
     单向（主键、外键）

       外键关联：@OneToOne
            属性：fetch，cascade…
            @JoinColumn(name=”wife”)

    双向（主键、外键）
       外键关联：互有引用
           @OneToOne(mappedBy=”wife”)
       规律：凡是双向关联，必设mappedBy
   
多对1：（数据库中两张表，并在多的那个表中加上1的外键）
     2.1单： 
     在程序中在多方类中增加单方类的引用，并在引用对象的get方法上增加 @Many2One


    2.2双：
    1) 在多方类(User) 的单方引用的get方法上设置：@Many2One

    2) 在单方类（group）的多方类引用哈希表对象的get方法上设置一个注解：
      @One2Many(mappedBy=”group”)

      
1对多：（数据库表中有三张表，还生成了一张中间表（默认当成多对多））
     3.1 单：在单方类中增加多的引用的HashSet<User>
          @One2Many 注解

          @JoinColumn(name=”groupId”)
          Ps:感觉不太好理解。。。

             

     3.2双      

多对多：
      5.1 单：

    在数据库中增加一张中间关联表，有两个字段：两个表的id，且这两个id构成联合主键、每个id分别为其中一张表的主键作为外键。

       @ManyToMany

 
      5.2 双：
  
  
Eager 加载 和 lazy 加载的区别：
eager 加载: 会将需要加载的类(user)以及其关联的对象（group）一起放入内存中。如果session关闭了仍能处理其关联的对象(group)
lazy  加载: 只有当要访问关联的对象(user)时，才从数据库中取得。如果session关闭后被关联的对象不在内存中，此时是无法再取得被关联对象(group) 的。 

 
```
谈谈Hibernate与Ibatis的区别，哪个性能会更高一些
```text
1、Hibernate偏向于对象的操作达到数据库相关操作的目的；而ibatis更偏向于sql语句的优化。

2、Hibernate的使用的查询语句是自己的hql，而ibatis则是标准的sql语句。

3、Hibernate相对复杂，不易学习；ibatis类似sql语句，简单易学。

性能方面：

1、如果系统数据处理量巨大，性能要求极为苛刻时，往往需要人工编写高性能的sql语句或存储过程，此时ibatis具有更好的可控性，因此性能优于Hibernate。

2、同样的需求下，由于hibernate可以自动生成hql语句，而ibatis需要手动写sql语句，此时采用Hibernate的效率高于ibatis。

```

反射讲一讲，主要是概念,都在哪需要反射机制，反射的性能，如何优化
```text



```

对Spring的理解，项目中都用什么？怎么用的？对IOC、和AOP的理解及实现原理
```text

Spring是一个开源框架，处于MVC模式中的控制层，它能应对需求快速的变化，其主要原因它有一种面向切面编程（AOP）的优势，
其次它提升了系统性能，因为通过依赖倒置机制（IOC），系统中用到的对象不是在系统加载时就全部实例化，
而是在调用到这个类时才会实例化该类的对象，从而提升了系统性能。这两个优秀的性能使得Spring受到许多J2EE公司的青睐，如阿里里中使用最多的也是Spring相关技术。

Spring的优点：

1、降低了组件之间的耦合性，实现了软件各层之间的解耦。

2、可以使用容易提供的众多服务，如事务管理，消息服务，日志记录等。

3、容器提供了AOP技术，利用它很容易实现如权限拦截、运行期监控等功能。

Spring中AOP技术是设计模式中的动态代理模式。只需实现jdk提供的动态代理接口InvocationHandler，
所有被代理对象的方法都由InvocationHandler接管实际的处理任务。面向切面编程中还要理解切入点、切面、通知、织入等概念。

Spring中IOC则利用了Java强大的反射机制来实现。所谓依赖注入即组件之间的依赖关系由容器在运行期决定。
其中依赖注入的方法有两种，通过构造函数注入，通过set方法进行注入。

```

多线程,线程同步，并发操作怎么控制 
```text
1)现在有T1、T2、T3三个线程，你怎样保证T2在T1执行完后执行，T3在T2执行完后执行？
这个线程问题通常会在第一轮或电话面试阶段被问到，目的是检测你对”join”方法是否熟悉。这个多线程问题比较简单，可以用join方法实现。

Java中可在方法名前加关键字syschronized来处理当有多个线程同时访问共享资源时候的问题。syschronized相当于一把锁，当有申请者申请该
资源时，如果该资源没有被占用，那么将资源交付给这个申请者使用，在此期间，其他申请者只能申请而不能使用该资源，当该资源被使用完成后将释放该资源上的锁，其他申请者可申请使用。
并发控制主要是为了多线程操作时带来的资源读写问题。如果不加以控制可能会出现死锁，读脏数据、不可重复读、丢失更新等异常。
并发操作可以通过加锁的方式进行控制，锁又可分为乐观锁和悲观锁。

悲观锁：
悲观锁并发模式假定系统中存在足够多的数据修改操作，以致于任何确定的读操作都可能会受到由个别的用户所制造的数据修改的影响。
也就是说悲观锁假定冲突总会发生，通过独占正在被读取的数据来避免冲突。但是独占数据会导致其他进程无法修改该数据，进而产生阻塞，读数据和写数据会相互阻塞。

乐观锁：
乐观锁假定系统的数据修改只会产生非常少的冲突，也就是说任何进程都不大可能修改别的进程正在访问的数据。
乐观并发模式下，读数据和写数据之间不会发生冲突，只有写数据与写数据之间会发生冲突。即读数据不会产生阻塞，只有写数据才会产生阻塞

```

描述struts的工作流程
```text
1、在web应用启动时，加载并初始化ActionServlet，ActionServlet从struts-config.xml文件中读取配置信息，将它们存放到各个配置对象中。

2、当ActionServlet接收到一个客户请求时，首先检索和用户请求相匹配的ActionMapping实例，如果不存在，就返回用户请求路径无效信息。

3、如果ActionForm实例不存在，就创建一个ActionForm对象，把客户提交的表单数据保存到ActionForm对象中。

4、根据配置信息决定是否需要验证表单，如果需要，就调用ActionForm的validate()方法，
如果ActionForm的validate()方法返回null或返回一个不包含ActionMessage的ActionErrors对象，就表示表单验证成功。

5、ActionServlet根据ActionMapping实例包含的映射信息决定请求转发给哪个Action，如果相应的Action实例不存在，
就先创建一个实例，然后调用Action的execute()方法。

6、Action的execute()方法返回一个ActionForward对象，ActionServlet再把客户请求转发给ActionForward对象指向的JSP组件。

7、ActionForward对象指向的JSP组件生成动态网页，返回给客户。


```
```text

1.读取配置（初始化ModuelConfig对象）
2.发送请求
3.填充FORM（实例化，复位，填充数据，校验，保存）
4.派发请求
5.处理业务
6.返回响应
7.查找响应（翻译响应）
8.响应用户



```



Tomcat的session处理，如果让你实现一个tomcat server，如何实现session机制 
```text

   答：   没有找到合适的答案。

```

jvm垃圾回收原理
```text
1. 引用计数（Reference Counting） 
比较古老的回收算法。原理是此对象有一个引用，即增加一个计数，删除一个引用则减少一个计数。
垃圾回收时，只用收集计数为0的对象。此算法最致命的是无法处理循环引用的问题。 


2. 标记-清除（Mark-Sweep） 
此算法执行分两阶段。第一阶段从引用根节点开始标记所有被引用的对象，
第二阶段遍历整个堆，把未标记的对象清除。此算法需要暂停整个应用，同时，会产生内存碎片。


3. 复制（Copying） 
此算法把内存空间划为两个相等的区域，每次只使用其中一个区域。垃圾回收时，遍历当前使用区域，
把正在使用中的对象复制到另外一个区域中。次算法每次只处理正在使用中的对象，因此复制成本比较小，
同时复制过去以后还能进行相应的内存整理，不过出现“碎片”问题。当然，此算法的缺点也是很明显的，就是需要两倍内存空间。

 
4. 标记-整理（Mark-Compact） 
此算法结合了 “标记-清除”和“复制”两个算法的优点。也是分两阶段，第一阶段从根节点开始标记所有被引用对象，
第二阶段遍历整个堆，清除未标记对象并且把存活对象 “压缩”到堆的其中一块，按顺序排放。
此算法避免了“标记-清除”的碎片问题，同时也避免了“复制”算法的空间问题。


 
5. 增量收集（Incremental Collecting） 
实施垃圾回收算法，即：在应用进行的同时进行垃圾回收。不知道什么原因JDK5.0中的收集器没有使用这种算法的。 
 
6. 分代（Generational Collecting） 
基于对对象生命周期分析后得出的垃圾回收算法。把对象分为年青代、年老代、持久代，对不同生命周期的对象使用
不同的算法（上述方式中的一个）进行回收。现在的垃圾回收器（从J2SE1.2开始）都是使用此算法的。



```


















