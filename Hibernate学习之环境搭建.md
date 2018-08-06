----------


马哥的淘宝店:https://shop592330910.taobao.com/



什么是Hibernate

 1. 一个框架
 2. 一个java领域持久化框架
 3. 一个ORM框架

什么是ORM
（object/relation mapping）对象关系映射
类  -- 表
对象--表的行
属性--表的列
将关系数据库表中的记录映射成为对象，以对象的形式展现，程序员可以把对数据库的操作转化为对对象的操作。
orm采用元数据来描述对象关系映射细节，元数据采用xml格式，存放在对象关系映射文件中。

马哥的淘宝店:https://shop592330910.taobao.com/

hibernate与jdbc代码对比

```
//这个是hibernate保存一个对象message的代码
public void save(Session sess, Message m){
    sess.save(m);
}

//下面是使用jdbc保存一个对象message的代码
public void save(Connection conn, Message m){
    PreparedStatement ps=null;
    String sql = "insert into message values(?, ?)";
    try{
        ps = conn.prepareStatement(sql);
        ps.setString(1, m.getTitle());
        ps.setString(2, m.getContent());
        ps.execute();
    }catch(){
        e.printStackTrace();
    }finally{
        if (ps != null){
            ps.close();
        }

    }
}

```
马哥的淘宝店:https://shop592330910.taobao.com/

hibernate开发步骤

1.通过hibernate  api编写访问数据库的代码
 

```
    @Test
    public void test() {
        //1.创建一个SessionFactory 对象，创建session的工厂的一个类
        SessionFactory sessionFactory = null;
        //1.1创建一个Configuration对象，对应hibernate的基本配置信息和对象关系映射信息
        Configuration configuration = new Configuration().configure();
        //1.2创建一个ServiceRegistry对象，hibernate4.x新添加的对象，hibernate任何的配置和服务都要在该对象中注册才能有效。
        ServiceRegistry serviceRegistry = new ServiceRegistryBuilder().applySettings(configuration.getProperties()).buildServiceRegistry();
        //1.3
        sessionFactory = configuration.buildSessionFactory(serviceRegistry);


        //2.创建一个Session对象
        Session session = sessionFactory.openSession();


        //3.开启事务
        Transaction transaction = session.beginTransaction();

        //4.执行保存操作
        News news = new News("title:java", "author: mamh", new Date(new java.util.Date().getTime()));
        session.save(news);

        //5.提交事务
        transaction.commit();

        //6.关闭session对象
        session.close();

        //7.关闭SessionFactory 对象
        sessionFactory.close();

    }
```
马哥的淘宝店:https://shop592330910.taobao.com/

2.创建持久化类
 必须提供一个无参数的构造器，因为需要用的反射的机制。

```
package com.mamh.hibernate.demo;

import java.sql.Date;

public class News {
    private Integer id;
    private String title;
    private String author;

    private Date date;


    public News() {//必须提供一个无参数的构造器
    }

    public News(String title, String author, Date date) {
        this.title = title;
        this.author = author;
        this.date = date;
    }

    public News(Integer id, String title, String author, Date date) {
        this.id = id;
        this.title = title;
        this.author = author;
        this.date = date;
    }

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public Date getDate() {
        return date;
    }

    public void setDate(Date date) {
        this.date = date;
    }

    @Override
    public String toString() {
        return "News{" +
                "id=" + id +
                ", title='" + title + '\'' +
                ", author='" + author + '\'' +
                ", date=" + date +
                '}' + '\n';
    }
}

```
马哥的淘宝店:https://shop592330910.taobao.com/

3.创建对象关系映射文件

```
<class name="com.mamh.hibernate.demo.News" table="hb_news" schema="atguigu">
指定类名和数据库表名的对应
```

```
<id name="id">
    <column name="hb_id" sql-type="int(11)"/>
    <!--指定主键的生成方法，native使用数据库本地方式，mysql就是自动增长的数字-->
    <generator class="native"/>
</id>
```

```
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>

    <class name="com.mamh.hibernate.demo.News" table="hb_news" schema="atguigu">
        <id name="id">
            <column name="hb_id" sql-type="int(11)"/>
            <!--指定主键的生成方法，native使用数据库本地方式，mysql就是自动增长的数字-->
            <generator class="native"/>
        </id>
        <property name="title">
            <column name="hb_title" sql-type="varchar(1024)" length="1024" not-null="true"/>
        </property>
        <property name="author">
            <column name="hb_author" sql-type="varchar(2048)" length="2048" not-null="true"/>
        </property>
        <property name="date">
            <column name="hb_date" sql-type="date" not-null="true"/>
        </property>
    </class>
</hibernate-mapping>
```
马哥的淘宝店:https://shop592330910.taobao.com/

4.创建hibernate配置文件
这个配置文件的默认名是/hibernate.cfg.xml

```
通过configure()方法中的代码可以看到这个默认的配置文件名称。
    public Configuration configure() throws HibernateException {
        configure( "/hibernate.cfg.xml" );
        return this;
    }
```

```
<!--是否打印sql语句-->
<property name="show_sql">true</property>

<!--是否对sql语句格式化-->
<property name="format_sql">true</property>
配置了这2个属性就会有下面的输出。方便调试sql语句。
Hibernate: 
    insert 
    into
        atguigu.hb_news
        (hb_title, hb_author, hb_date) 
    values
        (?, ?, ?)
```

```
<!--指定自动生成数据库表的策略，有4个值-->
<property name="hbm2ddl.auto">update</property>```
update,数据库表个原来一样就不动，
create,每次都创建新的数据库表
create_drop,每次创建，用完删除表
validate
```


```
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!--  链接数据库的基本信息 -->
        <property name="hibernate.connection.username">test</property>
        <property name="hibernate.connection.password">123456</property>
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql://10.0.63.43/test</property>

        <!-- 配置数据库的方言，注意这个有的版本好像不一样的。视频中使用的是这个MySQLInnoDBDialect，不过我直接照着做的时候好像这个值已经废弃，可能是hibernate/mysql的jar包版本问题把。[Maven: org.hibernate:hibernate-core:4.1.1.Final] org.hibernate.dialect中好像有个这个值。 -->
        <property name="dialect">org.hibernate.dialect.MySQL5Dialect</property>

        <!--是否打印sql语句-->
        <property name="show_sql">true</property>

        <!--是否对sql语句格式化-->
        <property name="format_sql">true</property>

        <!--指定自动生成数据库表的策略，有4个值-->
        <property name="hbm2ddl.auto">update</property>

        <!-- 这里使用的路径 -->
        <mapping resource="News.hbm.xml"/>
        
    </session-factory>
</hibernate-configuration>
```
maven. pom.xml 文件，使用maven来管理依赖的jar包文件。用idea新建一个空的maven工程，然后使用下面的pom.xml 内容就可以了。

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>hibernate</groupId>
    <artifactId>mamh</artifactId>
    <version>1.0-SNAPSHOT</version>

    <dependencies>
        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>4.1.1.Final</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/mysql/mysql-connector-java -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.6</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.9.1</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/commons-dbutils/commons-dbutils -->
        <dependency>
            <groupId>commons-dbutils</groupId>
            <artifactId>commons-dbutils</artifactId>
            <version>1.7</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>



        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
        </dependency>

    </dependencies>

</project>
```
马哥的淘宝店:https://shop592330910.taobao.com/





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