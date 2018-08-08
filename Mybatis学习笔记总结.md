Mybatis 学习笔记


mybatis中一对一关联关系
```xml

    <resultMap id="getClassMap" type="Classes">
        <id property="id" column="c_id"/>
        <result property="name" column="c_name"/>
        <association property="teacher" javaType="Teacher">
            <id property="id" column="t_id"/>
            <result property="name" column="t_name"/>
        </association>
    </resultMap>

    <select id="getClass" parameterType="int" resultMap="getClassMap">
        SELECT *
        FROM mb_class, mb_teacher
        WHERE c_id = #{id} AND mb_teacher.t_id = mb_class.teacher_id;
    </select>


    <resultMap id="getClassMap2" type="Classes">
        <id property="id" column="c_id"/>
        <result property="name" column="c_name"/>
        <association property="teacher" javaType="Teacher" column="teacher_id" select="getTeacher"/>
    </resultMap>

    <select id="getTeacher" parameterType="int" resultType="Teacher">
        SELECT
            t_id   id,
            t_name name
        FROM mb_teacher
        WHERE t_id = #{id}
    </select>

    <select id="getClass2" parameterType="int" resultMap="getClassMap2">
        SELECT *
        FROM mb_class
        WHERE c_id = #{id}
    </select>

```
以上可以使用2中方式来处理这个关联关系，一个是连表查询，一个是多次查询。
使用到了<association />

```xml

<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>mage</groupId>
    <artifactId>mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>

    <name>mybatis Maven Webapp</name>
    <url>http://www.example.com</url>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
    </properties>


    <dependencies>
        <!-- https://mvnrepository.com/artifact/cglib/cglib -->
        <dependency>
            <groupId>cglib</groupId>
            <artifactId>cglib</artifactId>
            <version>3.2.7</version>
        </dependency>

        <!-- mybatis核心包 -->
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.2.1</version>
        </dependency>


        <!-- mysql驱动包 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.6</version>
        </dependency>
        <!-- junit测试包 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
        <!-- 日志文件管理包 -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>1.7.12</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.12</version>
        </dependency>
    </dependencies>

    <build>
        <finalName>mybatis</finalName>

        <resources>
            <resource>
                <directory>src/main/java</directory>
                <includes>
                    <include>**/*.xml</include>
                </includes>
            </resource>
        </resources>

    </build>
</project>


```
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="db.prop"/>

    <settings>
        <!--全局性设置懒加载。如果设为‘false’，则所有相关联的都会被初始化加载,默认值为false-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!--当设置为‘true’的时候，懒加载的对象可能被任何懒属性全部加载。否则，每个属性都按需加载。默认值为true-->
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>

    <typeAliases>
        <!-- 其实就是将bean的替换成一个短的名字 -->
        <!--<typeAlias type="com.mamh.mybatis.demo.model.User" alias="User"/>-->
        <package name="com.mamh.mybatis.demo.model"/>
    </typeAliases>

    <!--对事务的管理和连接池的配置-->
    <environments default="dev">
        <environment id="dev">
            <transactionManager type="JDBC"/>

            <dataSource type="POOLED"><!--POOLED：使用Mybatis自带的数据库连接池来管理数据库连接-->
                <property name="driver" value="${jdbc.driverClass}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.user}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>


    </environments>

    <!--mapping文件路径配置-->
    <mappers>
        <mapper resource="UserMapper.xml"/>
        <mapper resource="OrderMapper.xml"/>
        <mapper resource="ClassMapper.xml"/>
        <mapper resource="StudentMapper.xml"/>
        <mapper class="com.mamh.mybatis.demo.mapper.UserMapper"/>
    </mappers>

</configuration>

```


Mybatis 的常见面试题

1、#{}和${}的区别是什么？
```text
#{}是预编译处理，${}是字符串替换。
Mybatis在处理#{}时，会将sql中的#{}替换为?号，调用PreparedStatement的set方法来赋值；
Mybatis在处理${}时，就是把${}替换成变量的值。
使用#{}可以有效的防止SQL注入，提高系统安全性。

```
2、当实体类中的属性名和表中的字段名不一样 ，怎么办 ？
```text
第1种： 通过在查询的sql语句中定义字段名的别名，让字段名的别名和实体类的属性名一致 
    <select id=”selectorder” parametertype=”int” resultetype=”me.gacl.domain.order”> 
       select order_id id, order_no orderno ,order_price price form orders where order_id=#{id}; 
    </select>
    
第2种： 通过<resultMap>来映射字段名和实体类属性名的一一对应的关系 
    <select id="getOrder" parameterType="int" resultMap="orderresultmap">
        select * from orders where order_id=#{id}
    </select>
   <resultMap type=”me.gacl.domain.order” id=”orderresultmap”> 
        <!–用id属性来映射主键字段–> 
        <id property=”id” column=”order_id”> 
        <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–> 
        <result property = “orderno” column =”order_no”/> 
        <result property=”price” column=”order_price” /> 
    </reslutMap>    

```

3、 模糊查询like语句该怎么写?
```text
第1种：在Java代码中添加sql通配符。
string wildcardname = “%smi%”; 
    list<name> names = mapper.selectlike(wildcardname);

    <select id=”selectlike”> 
     select * from foo where bar like #{value} 
    </select>
    
    
第2种：在sql语句中拼接通配符，会引起sql注入
    string wildcardname = “smi”; 
    list<name> names = mapper.selectlike(wildcardname);

    <select id=”selectlike”> 
     select * from foo where bar like "%"#{value}"%"
    </select>
    
        

```
4、通常一个Xml映射文件，都会写一个Dao接口与之对应，请问，这个Dao接口的工作原理是什么？Dao接口里的方法，参数不同时，方法能重载吗？
```text
Dao接口，就是人们常说的Mapper接口，接口的全限名，就是映射文件中的namespace的值，接口的方法名，
就是映射文件中MappedStatement的id值，接口方法内的参数，就是传递给sql的参数。Mapper接口是没有实现类的，
当调用接口方法时，接口全限名+方法名拼接字符串作为key值，可唯一定位一个MappedStatement，
举例：com.mybatis3.mappers.StudentDao.findStudentById，
可以唯一找到namespace为com.mybatis3.mappers.StudentDao下面id = findStudentById 的 MappedStatement。
在Mybatis中，每一个<select>、<insert>、<update>、<delete>标签，都会被解析为一个MappedStatement对象。

Dao 接口里的方法，是不能重载的，因为是全限名 + 方法名的保存和寻找策略。

Dao接口的工作原理是JDK动态代理，Mybatis运行时会使用JDK动态代理为Dao接口生成代理proxy对象，
代理对象proxy会拦截接口方法，转而执行MappedStatement所代表的sql，然后将sql执行结果返回。

```

5、Mybatis是如何进行分页的？分页插件的原理是什么？
```text
Mybatis使用RowBounds对象进行分页，它是针对ResultSet结果集执行的内存分页，而非物理分页，
可以在sql内直接书写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页。

分页插件的基本原理是使用Mybatis提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的sql，
然后重写sql，根据dialect方言，添加对应的物理分页语句和物理分页参数。

```
6、Mybatis是如何将sql执行结果封装为目标对象并返回的？都有哪些映射形式？
```text
答：第一种是使用<resultMap>标签，逐一定义列名和对象属性名之间的映射关系。
第二种是使用sql列的别名功能，将列别名书写为对象属性名，比如T_NAME AS NAME，
对象属性名一般是name，小写，但是列名不区分大小写，Mybatis会忽略列名大小写，
智能找到与之对应对象属性名，你甚至可以写成T_NAME AS NaMe，Mybatis一样可以正常工作。

有了列名与属性名的映射关系后，Mybatis通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。

```

7、如何执行批量插入?
```text
首先,创建一个简单的insert语句: 
    <insert id=”insertname”> 
     insert into names (name) values (#{value}) 
    </insert>
然后在java代码中像下面这样执行批处理插入: 
    list<string> names = new arraylist(); 
    names.add(“fred”); 
    names.add(“barney”); 
    names.add(“betty”); 
    names.add(“wilma”); 

    // 注意这里 executortype.batch 
    sqlsession sqlsession = sqlsessionfactory.opensession(executortype.batch); 
    try { 
     namemapper mapper = sqlsession.getmapper(namemapper.class); 
     for (string name : names) { 
     mapper.insertname(name); 
     } 
     sqlsession.commit(); 
    } finally { 
     sqlsession.close(); 
    }

```
Mybatis中如何执行批处理？
```text
答：使用BatchExecutor完成批处理。
```

Mybatis都有哪些Executor执行器？它们之间的区别是什么？
```text

答：Mybatis有三种基本的Executor执行器，SimpleExecutor、ReuseExecutor、BatchExecutor。

SimpleExecutor：每执行一次update或select，就开启一个Statement对象，用完立刻关闭Statement对象。

ReuseExecutor：执行update或select，以sql作为key查找Statement对象，存在就使用，不存在就创建，用完后，不关闭Statement对象，而是放置于Map<String, Statement>内，供下一次使用。简言之，就是重复使用Statement对象。

BatchExecutor：执行update（没有select，JDBC批处理不支持select），将所有sql都添加到批处理中（addBatch()），等待统一执行（executeBatch()），它缓存了多个Statement对象，每个Statement对象都是addBatch()完毕后，等待逐一执行executeBatch()批处理。与JDBC批处理相同。

作用范围：Executor的这些特点，都严格限制在SqlSession生命周期范围内。

```
Mybatis中如何指定使用哪一种Executor执行器？
```text
答：在Mybatis配置文件中，可以指定默认的ExecutorType执行器类型，也可以手动给DefaultSqlSessionFactory的创建SqlSession的方法传递ExecutorType类型参数。
```


8、如何获取自动生成的(主)键值?
```text
insert 方法总是返回一个int值 - 这个值代表的是插入的行数。 
而自动生成的键值在 insert 方法执行完后可以被设置到传入的参数对象中。 
示例: 
    <insert id=”insertname” usegeneratedkeys=”true” keyproperty=”id”> 
     insert into names (name) values (#{name}) 
    </insert>

    name name = new name(); 
    name.setname(“fred”); 

    int rows = mapper.insertname(name); 
    // 完成后,id已经被设置到对象中 
    system.out.println(“rows inserted = ” + rows); 
    system.out.println(“generated key value = ” + name.getid());

```
9、在mapper中如何传递多个参数?
```text
第1种：

Public UserselectUser(String name,String area);  
//对应的xml,#{0}代表接收的是dao层中的第一个参数，#{1}代表dao层中第二参数，更多参数一致往后加即可。

<select id="selectUser"resultMap="BaseResultMap">  
    select *  fromuser_user_t   whereuser_name = #{0} anduser_area=#{1}  
</select> 


第2种：    使用 @param 注解: 
        public interface usermapper { 
            user selectuser(@param(“username”) string username, @param(“hashedpassword”) string hashedpassword); 
        }
        然后,就可以在xml像下面这样使用(推荐封装为一个map,作为单个参数传递给mapper): 
    <select id=”selectuser” resulttype=”user”> 
         select id, username, hashedpassword 
         from some_table 
         where username = #{username} 
         and hashedpassword = #{hashedpassword} 
    </select>
```
10、Mybatis动态sql是做什么的？都有哪些动态sql？能简述一下动态sql的执行原理不？
```text
Mybatis动态sql可以让我们在Xml映射文件内，以标签的形式编写动态sql，完成逻辑判断和动态拼接sql的功能。
Mybatis提供了9种动态sql标签：trim|where|set|foreach|if|choose|when|otherwise|bind。
其执行原理为，使用OGNL从sql参数对象中计算表达式的值，根据表达式的值动态拼接sql，以此来完成动态sql的功能。

```
11、Mybatis的Xml映射文件中，不同的Xml映射文件，id是否可以重复？
```text

不同的Xml映射文件，如果配置了namespace，那么id可以重复；如果没有配置namespace，那么id不能重复；毕竟namespace不是必须的，只是最佳实践而已。
原因就是namespace+id是作为Map<String, MappedStatement>的key使用的，如果没有namespace，就剩下id，
那么，id重复会导致数据互相覆盖。有了namespace，自然id就可以重复，namespace不同，namespace+id自然也就不同。


```
12、为什么说Mybatis是半自动ORM映射工具？它与全自动的区别在哪里？
```text

Hibernate属于全自动ORM映射工具，使用Hibernate查询关联对象或者关联集合对象时，可以根据对象关系模型直接获取，所以它是全自动的。
而Mybatis在查询关联对象或关联集合对象时，需要手动编写sql来完成，所以，称之为半自动ORM映射工具。


```
