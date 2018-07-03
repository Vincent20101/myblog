JPA 学习笔记


##jpa是什么
```java
Java Persistence API：用于对象持久化的 API

Java EE 5.0 平台标准的 ORM 规范，使得应用程序以统一的方式访问持久层


```

## JPA和Hibernate的关系

```text

JPA 是 hibernate 的一个抽象（就像JDBC和JDBC驱动的关系）：

JPA 是规范：JPA 本质上就是一种  ORM 规范，不是ORM 框架 —— 
因为 JPA 并未提供 ORM 实现，它只是制订了一些规范，提供了一些编程的 API 接口，但具体实现则由 ORM 厂商提供实现


Hibernate 是实现：Hibernate 除了作为 ORM 框架之外，它也是一种 JPA 实现


从功能上来说， JPA 是 Hibernate 功能的一个子集


```

## jpa的hello world程序

首先弄个jpa的helloworld程序

persistence.xml 文件。这个要放到resources目录下面的META-INF目录下面.  
JPA规范要求在类路径的META-INF目录下面放persistence.xml，文件名称是固定的。
```xml

<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">
    <persistence-unit name="jpa-1" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>

        <!-- 添加持久化类 -->
        <class>com.mamh.jpa.Customer</class>


        <properties>
            <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql:///mage"/>
            <property name="javax.persistence.jdbc.user" value="mage"/>
            <property name="javax.persistence.jdbc.password" value="123456"/>

            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.hbm2ddl.auto" value="create"/>
            <property name="hibernate.show_sql" value="true"/>
        </properties>

    </persistence-unit>

</persistence>


```

pom.xml 文件,采用maven来管理jar包的依赖，这里加的jar包比较多.  
几个比较关键的jar包， hibernate-entitymanager ,hibernate-jpa-2.1-api ,hibernate-commons-annotations
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>mage</groupId>
    <artifactId>jpa</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>jpa Maven Webapp</name>
    <url>http://maven.apache.org</url>

    <properties>
        <spring.version>5.0.4.RELEASE</spring.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!--spring jar包-->

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/taglibs/standard -->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>

        <!--spring相关包, 在web中需要使用spring-web和spring-webmvc这2个jar包的-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
            <version>1.8.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-websocket</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.6</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-commons-annotations</artifactId>
            <version>3.2.0.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>4.3.11.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate.javax.persistence</groupId>
            <artifactId>hibernate-jpa-2.1-api</artifactId>
            <version>1.0.0.Final</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>4.3.11.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-c3p0</artifactId>
            <version>4.3.11.Final</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>4.3.2.Final</version>
        </dependency>


        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-ehcache</artifactId>
            <version>5.2.15.Final</version>
        </dependency>
        <!--日志jar-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
            <scope>provided</scope>
        </dependency>


        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-core-asl</artifactId>
            <version>1.9.13</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-asl</artifactId>
            <version>1.9.13</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-lgpl</artifactId>
            <version>1.9.13</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-core-lgpl</artifactId>
            <version>1.9.13</version>
        </dependency>

        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.2.4</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>
        <dependency>
            <groupId>jcifs</groupId>
            <artifactId>jcifs</artifactId>
            <version>1.3.17</version>
        </dependency>

        <dependency>
            <groupId>commons-httpclient</groupId>
            <artifactId>commons-httpclient</artifactId>
            <version>3.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-websocket</artifactId>
            <version>8.0.23</version>
            <scope>provided</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/jaxen/jaxen -->
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.1.6</version>
        </dependency>


        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjrt -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>1.8.13</version>
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


        <!-- https://mvnrepository.com/artifact/org.apache.struts/struts2-core -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
            <version>2.5.16</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.struts/struts2-spring-plugin -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-spring-plugin</artifactId>
            <version>2.5</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.9.5</version>
        </dependency>
    </dependencies>
    <build>
        <finalName>jpa</finalName>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.1</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
            </plugin>
        </plugins>
    </build>

</project>


```

持久化类：
```java
package com.mamh.jpa;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;
import javax.persistence.Table;

@Table(name = "jpa_customer")
@Entity
public class Customer {
    private Integer id;
    private String lastName;
    private String email;
    private int age;


    @Id
    @Column(name = "id")
    @GeneratedValue(strategy = GenerationType.AUTO)
    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    @Column(name = "last_name")
    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}


```

测试类
```java
package com.mamh.jpa;

import org.junit.Test;

import javax.persistence.EntityManager;
import javax.persistence.EntityManagerFactory;
import javax.persistence.EntityTransaction;
import javax.persistence.Persistence;

public class CustomerTest {

    @Test
    public void test() {
        String name = "jpa-1";
        EntityManagerFactory entityManagerFactory = Persistence.createEntityManagerFactory(name);

        EntityManager entityManager = entityManagerFactory.createEntityManager();

        EntityTransaction transaction = entityManager.getTransaction();

        transaction.begin();

        Customer customer = new Customer();
        customer.setAge(12);
        customer.setLastName("jerry......");
        customer.setEmail("email.....");

        entityManager.persist(customer);


        transaction.commit();

        entityManager.close();

        entityManagerFactory.close();

    }
}


```
## jpa中常用的注解

@Entity 放到实体类上面
```text
@Entity 标注用于实体类声明语句之前，指出该Java 类为实体类，将映射到指定的数据库表。
如声明一个实体类 Customer，它将映射到数据库中的 customer 表上。


```

@Table(name = "jpa_customer")设置表名
```text
当实体类与其映射的数据库表名不同名时需要使用 @Table 标注说明，该标注与 @Entity 标注并列使用，
置于实体类声明语句之前，可写于单独语句行，也可与声明语句同行。

@Table 标注的常用选项是 name，用于指明数据库的表名

@Table标注还有一个两个选项 catalog 和 schema 用于设置表所属的数据库目录或模式，通常为数据库名。
uniqueConstraints 选项用于设置约束条件，通常不须设置。


```

@Id映射主键的，放到getter方法上
```text
@Id 标注用于声明一个实体类的属性映射为数据库的主键列。该属性通常置于属性声明语句之前，可与声明语句同行，也可写在单独行上。

@Id标注也可置于属性的getter方法之前。


```

@GeneratedValue(strategy = GenerationType.AUTO)设置生成主键的策略
```text
@GeneratedValue  用于标注主键的生成策略，通过 strategy 属性指定。默认情况下，
JPA 自动选择一个最适合底层数据库的主键生成策略：
    SqlServer 对应 identity，
    MySQL 对应 auto increment。

在 javax.persistence.GenerationType 中定义了以下几种可供选择的策略：
    IDENTITY：采用数据库 ID自增长的方式来自增主键字段，Oracle 不支持这种方式；
    AUTO： JPA自动选择合适的策略，是默认选项；
    SEQUENCE：通过序列产生主键，通过 @SequenceGenerator 注解指定序列名，MySql 不支持这种方式
    TABLE：通过表产生主键，框架借由表模拟序列产生主键，使用该策略可以使应用更易于数据库移植。



```
@Basic 默认的注解
```text
@Basic 表示一个简单的属性到数据库表的字段的映射,对于没有任何标注的 getXxxx() 方法,默认即为@Basic

fetch: 表示该属性的读取策略,有 EAGER 和 LAZY 两种,分别表示主支抓取和延迟加载,默认为 EAGER.

optional:表示该属性是否允许为null, 默认为true


```

@Column(name = "last_name") 设置列,name映射到数据库表中的列名
```text
当实体的属性与其映射的数据库表的列不同名时需要使用@Column 标注说明，
该属性通常置于实体的属性声明语句之前，还可与 @Id 标注一起使用。

@Column 标注的常用属性是 name，用于设置映射数据库表的列名。此外，
该标注还包含其它多个属性，如：unique 、nullable、length 等。

@Column 标注的 columnDefinition 属性: 表示该字段在数据库中的实际类型.
通常 ORM 框架可以根据属性类型自动判断数据库中字段的类型,但是对于Date类型
仍无法确定数据库中字段类型究竟是DATE,TIME还是TIMESTAMP.此外,String的
默认映射类型为VARCHAR, 如果要将 String 类型映射到特定数据库的 BLOB 或TEXT 字段类型.


@Column标注也可置于属性的getter方法之前


```

@Transient 不需要影响为数据库表的列的方法上加上这个注解
```text
表示该属性并非一个到数据库表的字段的映射,ORM框架将忽略该属性.
如果一个属性并非数据库表的字段映射,就务必将其标示为@Transient,否则,ORM框架默认其注解为@Basic

```

@Temporal(TemporalType.TIMESTAMP) 设置时间日期对应到数据库表列的类型的
```text
在核心的 Java API 中并没有定义 Date 类型的精度(temporal precision).  
而在数据库中,表示 Date 类型的数据有 DATE, TIME, 和 TIMESTAMP 三种精度(即单纯的日期,时间,或者两者 兼备). 
在进行属性映射时可使用@Temporal注解来调整精度.


```

## jpa中常用的类

Persistence类
```text
Persistence  类是用于获取 EntityManagerFactory 实例。该类包含一个名为 createEntityManagerFactory 的 静态方法 。

createEntityManagerFactory 方法有如下两个重载版本。
    带有一个参数的方法以 JPA 配置文件 persistence.xml 中的持久化单元名为参数
    带有两个参数的方法：前一个参数含义相同，后一个参数 Map类型，用于设置 JPA 的相关属性，
    这时将忽略其它地方设置的属性。Map 对象的属性名必须是 JPA 实现库提供商的名字空间约定的属性名。


```

EntityManagerFactory
```text
EntityManagerFactory 接口主要用来创建 EntityManager 实例。该接口约定了如下4个方法：
    createEntityManager()：用于创建实体管理器对象实例。
    
    createEntityManager(Map map)：用于创建实体管理器对象实例的重载方法，Map 参数用于提供 EntityManager 的属性。
    
    isOpen()：检查 EntityManagerFactory 是否处于打开状态。实体管理器工厂创建后一直处于打开状态，除非调用close()方法将其关闭。
    
    close()：关闭 EntityManagerFactory 。 EntityManagerFactory 关闭后将释放所有资源，
    isOpen()方法测试将返回 false，其它方法将不能调用，否则将导致IllegalStateException异常。


```

EntityManager
```text
在 JPA 规范中, EntityManager 是完成持久化操作的核心对象。实体作为普通 Java 对象，
只有在调用 EntityManager 将其持久化后才会变成持久化对象。EntityManager 对象在一
组实体类与底层数据源之间进行 O/R 映射的管理。它可以用来管理和更新 Entity Bean, 
根椐主键查找 Entity Bean, 还可以通过JPQL语句查询实体。
 
 
 实体的状态:
    新建状态:   新创建的对象，尚未拥有持久性主键。
    持久化状态：已经拥有持久性主键并和持久化建立了上下文环境
    游离状态：拥有持久化主键，但是没有与持久化建立上下文环境
    删除状态:  拥有持久化主键，已经和持久化建立上下文环境，但是从数据库中删除。



```


EntityManager.find()方法
```text
find (Class<T> entityClass,Object primaryKey)：返回指定的 OID 对应的实体类对象，
如果这个实体存在于当前的持久化环境，则返回一个被缓存的对象；否则会创建一个新的 Entity, 
并加载数据库中相关信息；若 OID 不存在于数据库中，则返回一个 null。第一个参数为被查询的
实体类类型，第二个参数为待查找实体的主键值。
```
```java



    @Test
    public void testFind(){
        Customer customer = entityManager.find(Customer.class, 1);
        System.out.println(customer);
    }

```

EntityManager.getReference()方法,只有在使用对象的时候才去数据库中查询。会出现懒加载异常。
```text
getReference (Class<T> entityClass,Object primaryKey)：与find()方法类似，不同的是：如果缓存中不存在指定的 Entity, EntityManager 会创建一个 Entity 类的代理，但是不会立即加载数据库中的信息，只有第一次真正使用此 Entity 的属性才加载，所以如果此 OID 在数据库不存在，getReference() 不会返回 null 值, 而是抛出EntityNotFoundException


```
```java

    @Test
    public void testRef() {
        Customer customer = entityManager.getReference(Customer.class, 1);
        System.out.println("----------------------");
        System.out.println(customer);

    }

```

EntityManager.persist()方法,使对象由临时状态变为持久化状态。 
```text
persist (Object entity)：用于将新创建的 Entity 纳入到 EntityManager 的管理。该方法执行后，传入 persist() 方法的 Entity 对象转换成持久化状态。
如果传入 persist() 方法的 Entity 对象已经处于持久化状态，则 persist() 方法什么都不做。
如果对删除状态的 Entity 进行 persist() 操作，会转换为持久化状态。
如果对游离状态的实体执行 persist() 操作，可能会在 persist() 方法抛出 EntityExistException(也有可能是在flush或事务提交后抛出)。


``` 
```java


    @Test
    public void testPersist() {
        Customer customer = new Customer();
        customer.setAge(12);
        customer.setLastName("jerry1......");
        customer.setEmail("email.....");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());

        entityManager.persist(customer);


    }

```
如果这时设置了customer的ID，则会报错的。


EntityManager.remove()方法,删除数据库记录，只能删除持久化对象，游离对象不行。
```text
remove (Object entity)：删除实例。如果实例是被管理的，即与数据库实体记录关联，则同时会删除关联的数据库记录。


```  
```java
    @Test
    public void testRemove() {
        Customer customer = new Customer();
        customer.setId(123);
        entityManager.remove(customer);
    }

```
这样才能删除
```java
    @Test
    public void testRemove() {
        //Customer customer = new Customer();
        //customer.setId(123);
        Customer customer = entityManager.find(Customer.class, 1);
        entityManager.remove(customer);
    }

```

EntityManager.merge()方法 merge (T entity)：merge() 用于处理 Entity 的同步。即数据库的插入和更新操作
 
会创建一个新的对象，把临时对象的属性复制到新的对象中，然后对新的对象执行持久化操作，新的对象中会有id，之前的对象没有id了。
```java

    @Test
    public void testMerge() {
        Customer customer = new Customer();
        customer.setAge(12);
        customer.setLastName("Tom......");
        customer.setEmail("email.....");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());

        Customer merge = entityManager.merge(customer);

        System.out.println(customer);
        System.out.println(merge);
    }

```

若传入的是一个游离对象，也就是说传入的对象有ID值。  

```java

    @Test
    public void testMerge1() {
        Customer customer = new Customer();
        customer.setAge(12);
        customer.setLastName("Tom......");
        customer.setEmail("email.....");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());
        customer.setId(100);

        Customer merge = entityManager.merge(customer);

        System.out.println(customer);
        System.out.println(merge);
    }

```

```text
flush ()：同步持久上下文环境，即将持久上下文环境的所有未保存实体的状态信息保存到数据库中。

setFlushMode (FlushModeType flushMode)：设置持久上下文环境的Flush模式。参数可以取2个枚举
    FlushModeType.AUTO 为自动更新数据库实体，
    FlushModeType.COMMIT 为直到提交事务时才更新数据库记录。


getFlushMode ()：获取持久上下文环境的Flush模式。返回FlushModeType类的枚举值。

refresh (Object entity)：用数据库实体记录的值更新实体对象的状态，即更新实例的属性值。

clear ()：清除持久上下文环境，断开所有关联的实体。如果这时还有未提交的更新则会被撤消。

contains (Object entity)：判断一个实例是否属于当前持久上下文环境管理的实体。

isOpen ()：判断当前的实体管理器是否是打开状态。

getTransaction ()：返回资源层的事务对象。EntityTransaction实例可以用于开始和提交多个事务。

close ()：关闭实体管理器。之后若调用实体管理器实例的方法或其派生的查询对象的方法都将抛出 IllegalstateException 异常，除了getTransaction 和 isOpen方法(返回 false)。不过，当与实体管理器关联的事务处于活动状态时，调用 close 方法后持久上下文将仍处于被管理状态，直到事务完成。



```
EntityTransaction
```text
EntityTransaction 接口用来管理资源层实体管理器的事务操作。通过调用实体管理器的getTransaction方法 获得其实例。

begin ()
用于启动一个事务，此后的多个数据库操作将作为整体被提交或撤消。若这时事务已启动则会抛出 IllegalStateException 异常。

commit ()
用于提交当前事务。即将事务启动以后的所有数据库更新操作持久化至数据库中。

rollback ()
撤消(回滚)当前事务。即撤消事务启动后的所有数据库更新操作，从而不对数据库产生影响。

setRollbackOnly ()
使当前事务只能被撤消。


getRollbackOnly ()
查看当前事务是否设置了只能撤消标志。

isActive ()
查看当前事务是否是活动的。如果返回true则不能调用begin方法，否则将抛出 IllegalStateException 异常；如果返回 false 则不能调用 commit、rollback、setRollbackOnly 及 getRollbackOnly 方法，否则将抛出 IllegalStateException 异常。

```

## 多对一关联关系

多对一在插入数据的情况：
```java
    @Test
    public void testManyToOne() {
        //多对一映射关联关系
        //一个customer可以有多个order。
        Customer customer = new Customer();
        customer.setAge(112);
        customer.setLastName("b");
        customer.setEmail("b");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());

        Order order = new Order();
        order.setOderName("o-ff-1");
        Order order1 = new Order();
        order1.setOderName("o-ff-2");
        order.setCustomer(customer);
        order1.setCustomer(customer);
        entityManager.persist(customer);

        entityManager.persist(order);
        entityManager.persist(order1);
    }

```
```text
Hibernate: 
    insert 
    into
        jpa_customer
        (age, birth, createTime, email, last_name) 
    values
        (?, ?, ?, ?, ?)
Hibernate: 
    insert 
    into
        jpa_order
        (customer_id, order_name) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_order
        (customer_id, order_name) 
    values
        (?, ?)

Process finished with exit code 0

```


保存的时候建议先保存一的那一端对象，也就是先保存customer。
然后再保存多的那一端，也就是order。这样不会多出sql语句。

```java

    @Test
    public void testManyToOne() {
        //多对一映射关联关系
        //一个customer可以有多个order。
        Customer customer = new Customer();
        customer.setAge(112);
        customer.setLastName("b");
        customer.setEmail("b");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());

        Order order = new Order();
        order.setOderName("o-ff-1");
        Order order1 = new Order();
        order1.setOderName("o-ff-2");
        order.setCustomer(customer);
        order1.setCustomer(customer);

        entityManager.persist(order);
        entityManager.persist(order1);
        entityManager.persist(customer);
    }

```
```text
Hibernate: 
    insert 
    into
        jpa_order
        (customer_id, order_name) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_order
        (customer_id, order_name) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_customer
        (age, birth, createTime, email, last_name) 
    values
        (?, ?, ?, ?, ?)
Hibernate: 
    update
        jpa_order 
    set
        customer_id=?,
        order_name=? 
    where
        id=?
Hibernate: 
    update
        jpa_order 
    set
        customer_id=?,
        order_name=? 
    where
        id=?

Process finished with exit code 0

```
多对一在获取的时候：
```java
    @Test
    public void testManyToOne() {
        Order order = entityManager.find(Order.class, 1);
        System.out.println(order);
    }

```
```text
Hibernate: 
    select
        order0_.id as id1_1_0_,
        order0_.customer_id as customer3_1_0_,
        order0_.order_name as order_na2_1_0_,
        customer1_.id as id1_0_1_,
        customer1_.age as age2_0_1_,
        customer1_.birth as birth3_0_1_,
        customer1_.createTime as createTi4_0_1_,
        customer1_.email as email5_0_1_,
        customer1_.last_name as last_nam6_0_1_ 
    from
        jpa_order order0_ 
    left outer join
        jpa_customer customer1_ 
            on order0_.customer_id=customer1_.id 
    where
        order0_.id=?
Order{id=1, oderName='o-ff-1', customer=Customer{id=5, lastName='b', email='b', age=112, createTime=2018-05-31 13:34:20.0, birth=2018-05-31}}

Process finished with exit code 0


```
这里会使用一个左外链接。

在Order类里面可以设置customer  @ManyToOne (fetch = FetchType.LAZY) 为懒加载。




多对一在删除数据的情况
```java
    @Test
    public void testManyToOne2() {
        Customer customer = entityManager.find(Customer.class, 6);
        entityManager.remove(customer);
    }

```
这种情况下是不让删除的，因为有个外键。


##一对多关联关系

一对多关联关系插入数据的情况
```java
    @Test
    public void testOneToMany() {
        Customer customer = new Customer();
        customer.setAge(112);
        customer.setLastName("zz");
        customer.setEmail("zzzzz");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());

        Order order = new Order();
        order.setOderName("o-ff-1");
        Order order1 = new Order();
        order1.setOderName("o-ff-2");

        customer.getOrders().add(order);
        customer.getOrders().add(order1);

        entityManager.persist(customer);
        entityManager.persist(order);
        entityManager.persist(order1);

    }

```
单向一对多，执行保存时，会多出update语句。因为多的一端在插入的时候不会同时插入外键列。

一对多关联关系查询数据的情况
```java
    @Test
    public void testOneToMany1() {
        Customer customer = entityManager.find(Customer.class, 7);
        System.out.println(customer);

    }

```
默认是懒加载的情况。
通用可以使用    @OneToMany(fetch = FetchType.EAGER) 来修改。然后那个查询会变为左外链接。

## 双向一对多关联关系
```java
    /**
     * 双向一对多关联关系
     */
    @Test
    public void testOneToMany2() {
        Customer customer = new Customer();
        customer.setAge(112);
        customer.setLastName("zz");
        customer.setEmail("zzzzz");
        customer.setBirth(new Date());
        customer.setCreateTime(new Date());

        Order order = new Order();
        order.setOderName("o-ff-1");
        Order order1 = new Order();
        order1.setOderName("o-ff-2");

        customer.getOrders().add(order);
        customer.getOrders().add(order1);
        order1.setCustomer(customer);
        order.setCustomer(customer);

        entityManager.persist(customer);
        entityManager.persist(order);
        entityManager.persist(order1);

    }

```
先插入customer，然后在插入order，只有2个update语句，如果反过来插入，会有4条update语句的。
```text
Hibernate: 
    insert 
    into
        jpa_customer
        (age, birth, createTime, email, last_name) 
    values
        (?, ?, ?, ?, ?)
Hibernate: 
    insert 
    into
        jpa_order
        (customer_id, order_name) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_order
        (customer_id, order_name) 
    values
        (?, ?)
Hibernate: 
    update
        jpa_order 
    set
        customer_id=? 
    where
        id=?
Hibernate: 
    update
        jpa_order 
    set
        customer_id=? 
    where
        id=?

Process finished with exit code 0

```
在进行双向一对多的关联关系时。建议使用多的一方来维护管理关系。

可以在customer上设置    @OneToMany(fetch = FetchType.EAGER,cascade = {CascadeType.REMOVE},mappedBy = "customer")
这样customer就不会维护关联关系了。  
此时这个不能同时使用    @JoinColumn(name = "customer_id") 了。



## 双向一对一的关联关系
```java
package com.mamh.jpa;


import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.OneToOne;
import javax.persistence.Table;

@Entity
@Table(name = "jpa_department")
public class Department {

    private int id;
    private String deptName;

    private Manager manager;

    @Id
    @GeneratedValue
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Column(name = "name")
    public String getDeptName() {
        return deptName;
    }

    public void setDeptName(String deptName) {
        this.deptName = deptName;
    }

    /**
     * 使用  @OneToOne 来映射1-1关联关系。
     * @return
     */
    @JoinColumn(name = "manager_id", unique = true)
    @OneToOne
    public Manager getManager() {
        return manager;
    }

    public void setManager(Manager manager) {
        this.manager = manager;
    }
}


```
```java
package com.mamh.jpa;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.OneToOne;
import javax.persistence.Table;

@Entity
@Table(name = "jpa_manager")
public class Manager {
    private int id;
    private String mgrName;

    private Department deptartment;

    @Id
    @GeneratedValue
    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    @Column(name = "name")
    public String getMgrName() {
        return mgrName;
    }

    public void setMgrName(String mgrName) {
        this.mgrName = mgrName;
    }

    @OneToOne(mappedBy = "manager")
    public Department getDeptartment() {
        return deptartment;
    }

    public void setDeptartment(Department deptartment) {
        this.deptartment = deptartment;
    }
}


```

```java


    @Test
    public void testDepartment(){
        Manager manager = new Manager();
        manager.setMgrName("bright.ma");
        Department department = new Department();
        department.setDeptName("scm");
        department.setManager(manager);
        manager.setDeptartment(department);

        //先保存不维护关联关系的那一方
        entityManager.persist(manager);
        entityManager.persist(department);
    }

```

在添加数据的时候， 先保存不维护关联关系的那一方，这里就是先保存manager。
```text
Hibernate: 
    insert 
    into
        jpa_manager
        (name) 
    values
        (?)
Hibernate: 
    insert 
    into
        jpa_department
        (name, manager_id) 
    values
        (?, ?)

Process finished with exit code 0

```

获取的情况
```java
    @Test
    public void testOnetoOneFind() {
        Department department = entityManager.find(Department.class, 1);
        System.out.println(department);

    }


```

```text
Hibernate: 
    select
        department0_.id as id1_1_0_,
        department0_.name as name2_1_0_,
        department0_.manager_id as manager_3_1_0_,
        manager1_.id as id1_2_1_,
        manager1_.name as name2_2_1_ 
    from
        jpa_department department0_ 
    left outer join
        jpa_manager manager1_ 
            on department0_.manager_id=manager1_.id 
    where
        department0_.id=?
Hibernate: 
    select
        department0_.id as id1_1_1_,
        department0_.name as name2_1_1_,
        department0_.manager_id as manager_3_1_1_,
        manager1_.id as id1_2_0_,
        manager1_.name as name2_2_0_ 
    from
        jpa_department department0_ 
    left outer join
        jpa_manager manager1_ 
            on department0_.manager_id=manager1_.id 
    where
        department0_.manager_id=?
com.mamh.jpa.Department@649f2009

```


```java
    @Test
    public void testOnetoOneFind1() {
        Manager manager = entityManager.find(Manager.class, 1);
        System.out.println(manager);

    }

```
```text
Hibernate: 
    select
        manager0_.id as id1_2_0_,
        manager0_.name as name2_2_0_,
        department1_.id as id1_1_1_,
        department1_.name as name2_1_1_,
        department1_.manager_id as manager_3_1_1_ 
    from
        jpa_manager manager0_ 
    left outer join
        jpa_department department1_ 
            on manager0_.id=department1_.manager_id 
    where
        manager0_.id=?
com.mamh.jpa.Manager@14bb2297

```
## 双向一对一的关联关系
基于主键来做这个一对一关联关系，可以使用“@PrimaryKeyJoinColumn(name = "id")”



## 双向多对多关联关系

```java
package com.mamh.jpa;


import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.JoinColumn;
import javax.persistence.JoinTable;
import javax.persistence.ManyToMany;
import javax.persistence.Table;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "jpa_item")
public class Item {

    private Integer id;
    private String itemName;

    private Set<Category> categories = new HashSet<Category>();

    @GeneratedValue
    @Id
    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    @Column(name = "item_name")
    public String getItemName() {
        return itemName;
    }

    public void setItemName(String itemName) {
        this.itemName = itemName;
    }

    @ManyToMany
    @JoinTable( name = "jpa_item_category",
            joinColumns = {
                    @JoinColumn(name = "item_id", referencedColumnName = "id")
            },
            inverseJoinColumns = {   //对方那个表
                    @JoinColumn(name = "categroy_id", referencedColumnName = "id")
            }
    )
    public Set<Category> getCategories() {
        return categories;
    }

    public void setCategories(Set<Category> categories) {
        this.categories = categories;
    }
}


```

```java
package com.mamh.jpa;


import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.ManyToMany;
import javax.persistence.Table;
import java.util.HashSet;
import java.util.Set;

@Entity
@Table(name = "jpa_category")
public class Category {
    private Integer id;
    private String categoryName;

    private Set<Item> items = new HashSet<Item>();


    @GeneratedValue
    @Id
    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    @Column(name = "category_name")
    public String getCategoryName() {
        return categoryName;
    }

    public void setCategoryName(String categoryName) {
        this.categoryName = categoryName;
    }

    @ManyToMany(mappedBy = "categories")
    public Set<Item> getItems() {
        return items;
    }

    public void setItems(Set<Item> items) {
        this.items = items;
    }
}



```

使用@ManyToMany注解来映射多对多关联关系。使用@JoinTable来映射中间表
```text
    @ManyToMany
    @JoinTable( name = "jpa_item_category", //name指定中间表的表名
            joinColumns = {   // joinColumns映射当前类所在表在中间表中的外键。
                    @JoinColumn(name = "item_id", referencedColumnName = "id")
            },
            inverseJoinColumns = {   //对方那个表
                    @JoinColumn(name = "categroy_id", referencedColumnName = "id")
            }
    )

```
```text
    @ManyToMany(mappedBy = "categories")
    public Set<Item> getItems() {
        return items;
    }

```

多对多的保存
```text
    @Test
    public void testManyToMany() {
        Item i1 = new Item();
        i1.setItemName("i - 1");
        Item i2 = new Item();
        i2.setItemName("i - 2");


        Category c1 = new Category();
        c1.setCategoryName("c - 1");
        Category c2 = new Category();
        c2.setCategoryName("c - 2");

        i1.getCategories().add(c1);
        i1.getCategories().add(c2);

        i2.getCategories().add(c1);
        i2.getCategories().add(c2);

        c1.getItems().add(i1);
        c1.getItems().add(i2);

        c2.getItems().add(i1);
        c2.getItems().add(i2);

        entityManager.persist(i1);
        entityManager.persist(i2);
        entityManager.persist(c1);
        entityManager.persist(c2);
    }

```
输出结果
```text
Hibernate: 
    insert 
    into
        jpa_item
        (item_name) 
    values
        (?)
Hibernate: 
    insert 
    into
        jpa_item
        (item_name) 
    values
        (?)
Hibernate: 
    insert 
    into
        jpa_category
        (category_name) 
    values
        (?)
Hibernate: 
    insert 
    into
        jpa_category
        (category_name) 
    values
        (?)
Hibernate: 
    insert 
    into
        jpa_item_category
        (item_id, categroy_id) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_item_category
        (item_id, categroy_id) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_item_category
        (item_id, categroy_id) 
    values
        (?, ?)
Hibernate: 
    insert 
    into
        jpa_item_category
        (item_id, categroy_id) 
    values
        (?, ?)

Process finished with exit code 0


```

多对多的查询
1.先获取维护关联关系的，也就是先获取Item。
```text

    @Test
    public void testManyToManyFind() {
        Item item = entityManager.find(Item.class, 2);
        System.out.println(item.getItemName());
        System.out.println(item.getCategories().size());
    }

```
```text
Hibernate: 
    select
        item0_.id as id1_3_0_,
        item0_.item_name as item_nam2_3_0_ 
    from
        jpa_item item0_ 
    where
        item0_.id=?
i - 2
Hibernate: 
    select
        categories0_.item_id as item_id1_3_0_,
        categories0_.categroy_id as categroy2_4_0_,
        category1_.id as id1_0_1_,
        category1_.category_name as category2_0_1_ 
    from
        jpa_item_category categories0_ 
    inner join
        jpa_category category1_ 
            on categories0_.categroy_id=category1_.id 
    where
        categories0_.item_id=?
2

Process finished with exit code 0

```

2. 获取 不维护关联关系的，也就是先获取Category。

```text

    @Test
    public void testManyToManyFind() {
        Category category = entityManager.find(Category.class, 1);
        System.out.println(category.getCategoryName());
        System.out.println(category.getItems().size());
    }

```

```text
Hibernate: 
    select
        category0_.id as id1_0_0_,
        category0_.category_name as category2_0_0_ 
    from
        jpa_category category0_ 
    where
        category0_.id=?
c - 1
Hibernate: 
    select
        items0_.categroy_id as categroy2_0_0_,
        items0_.item_id as item_id1_4_0_,
        item1_.id as id1_3_1_,
        item1_.item_name as item_nam2_3_1_ 
    from
        jpa_item_category items0_ 
    inner join
        jpa_item item1_ 
            on items0_.item_id=item1_.id 
    where
        items0_.categroy_id=?
2

Process finished with exit code 0


```
通过以上两种方式获取的，效果是一样的，因为是有一张中间表，获取那边都是一样的。

## jpql





```
    @Test
    public void testJpql() {

        String jpsql = "select new Customer(c.lastName,c.age) FROM Customer  c where c.age > ?";

        Query query = entityManager.createQuery(jpsql);
        query.setParameter(1, 1);
        List resultList = query.getResultList();
        System.out.println(resultList);

    }


```


命名查询

需要在实体类上面加上注解@NamedQuery(name = "testNamedQuery", query = "select c from Customer c where c.id = ?")

```java
    @Test
    public void testNamedQuery() {

        Query query = entityManager.createNamedQuery("testNamedQuery");
        query.setParameter(1, 3);
        List resultList = query.getResultList();
        System.out.println(resultList);


    }

```




```java

    @Test
    public void testNativeQuery() {
        String sql = "SELECT age FROM jpa_customer WHERE id = ?";
        Query query = entityManager.createNativeQuery(sql);
        query.setParameter(1, 3);
        List resultList = query.getResultList();
        System.out.println(resultList);
    }

```


## 一级缓存
jpa默认是有一级缓存的。

```java
    @Test
    public void testCache() {
        Customer customer = entityManager.find(Customer.class, 3); //这里一级缓存为什么没有生效呢？？？？

        System.out.println(customer.getLastName());

        System.out.println("");
        Customer customer1 = entityManager.find(Customer.class, 3);
        System.out.println(customer1.getLastName());

    }

```
上面的代码查询2次，只会发送一条sql语句的。特别注意 查询出来的数据要存在，如果不存在也是发送2条sql语句了。

## 二级缓存
```java

    @Test
    public void testCache1() {
        Customer customer = entityManager.find(Customer.class, 3); //这里一级缓存为什么没有生效呢？？？？

        System.out.println(customer.getLastName());
        transaction.commit();
        entityManager.close();
        System.out.println("");

        entityManager = entityManagerFactory.createEntityManager();
        transaction = entityManager.getTransaction();
        transaction.begin();

        Customer customer1 = entityManager.find(Customer.class, 3);
        System.out.println(customer1.getLastName());

    }

```

配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">
    <persistence-unit name="jpa-1" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>

        <!-- 添加持久化类 -->
        <class>com.mamh.jpa.Customer</class>
        <class>com.mamh.jpa.Order</class>
        <class>com.mamh.jpa.Department</class>
        <class>com.mamh.jpa.Manager</class>
        <class>com.mamh.jpa.Item</class>
        <class>com.mamh.jpa.Category</class>

        <!--
            配置二级缓存的策略
            ALL：所有的实体类都被缓存
            NONE：所有的实体类都不被缓存.
            ENABLE_SELECTIVE：标识 @Cacheable(true) 注解的实体类将被缓存
            DISABLE_SELECTIVE：缓存除标识 @Cacheable(false) 以外的所有实体类
            UNSPECIFIED：默认值，JPA 产品默认值将被使用
        -->
        <shared-cache-mode>ENABLE_SELECTIVE</shared-cache-mode>

        <properties>
            <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql:///atguigu"/>
            <property name="javax.persistence.jdbc.user" value="atguigu"/>
            <property name="javax.persistence.jdbc.password" value="123456"/>

            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.hbm2ddl.auto" value="update"/>
            <property name="hibernate.show_sql" value="true"/>

            <!--二级缓存相关的 -->
            <property name="hibernate.cache.use_second_level_cache" value="true"/>
            <property name="hibernate.cache.region.factory_class" value="org.hibernate.cache.ehcache.EhCacheRegionFactory"/>
            <property name="hibernate.cache.use_query_cache" value="true"/>
        </properties>

    </persistence-unit>

</persistence>


```

最后实体类上面要加上@Cacheable(true)注解

这样最后的输出就只会发送一个sql语句了
```text
Hibernate: 
    select
        customer0_.id as id1_1_0_,
        customer0_.age as age2_1_0_,
        customer0_.birth as birth3_1_0_,
        customer0_.createTime as createTi4_1_0_,
        customer0_.email as email5_1_0_,
        customer0_.last_name as last_nam6_1_0_ 
    from
        jpa_customer customer0_ 
    where
        customer0_.id=?
Tom

Tom


```

## 查询缓存

需要在配置文件中配置启用查询缓存
```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://java.sun.com/xml/ns/persistence" version="2.0">
    <persistence-unit name="jpa-1" transaction-type="RESOURCE_LOCAL">
        <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>

        <!-- 添加持久化类 -->
        <class>com.mamh.jpa.Customer</class>
        <class>com.mamh.jpa.Order</class>
        <class>com.mamh.jpa.Department</class>
        <class>com.mamh.jpa.Manager</class>
        <class>com.mamh.jpa.Item</class>
        <class>com.mamh.jpa.Category</class>


        <properties>
            <property name="javax.persistence.jdbc.driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="javax.persistence.jdbc.url" value="jdbc:mysql:///atguigu"/>
            <property name="javax.persistence.jdbc.user" value="atguigu"/>
            <property name="javax.persistence.jdbc.password" value="123456"/>

            <property name="hibernate.format_sql" value="true"/>
            <property name="hibernate.hbm2ddl.auto" value="update"/>
            <property name="hibernate.show_sql" value="true"/>

            <!--二级缓存相关的 -->
            <property name="hibernate.cache.use_second_level_cache" value="true"/>
            <property name="hibernate.cache.region.factory_class" value="org.hibernate.cache.ehcache.EhCacheRegionFactory"/>
            <property name="hibernate.cache.use_query_cache" value="true"/>
        </properties>

    </persistence-unit>

</persistence>


```
还有就是jar的版本要对。 hibernate-ehcache的版本要对。
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>mage</groupId>
    <artifactId>jpa</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>jpa Maven Webapp</name>
    <url>http://maven.apache.org</url>

    <properties>
        <spring.version>5.0.4.RELEASE</spring.version>

        <hibernate.version>4.3.11.Final</hibernate.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <!--spring jar包-->

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.0.1</version>
            <scope>provided</scope>
        </dependency>

        <dependency>
            <groupId>javax.servlet.jsp</groupId>
            <artifactId>jsp-api</artifactId>
            <version>2.2</version>
        </dependency>

        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>jstl</artifactId>
            <version>1.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/taglibs/standard -->
        <dependency>
            <groupId>taglibs</groupId>
            <artifactId>standard</artifactId>
            <version>1.1.2</version>
        </dependency>

        <!--spring相关包, 在web中需要使用spring-web和spring-webmvc这2个jar包的-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-web</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-jdbc</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.springframework/spring-orm -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-tx</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
            <version>1.8.0.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aspects</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-websocket</artifactId>
            <version>${spring.version}</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>6.0.6</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-commons-annotations</artifactId>
            <version>3.2.0.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>${hibernate.version}</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate.javax.persistence</groupId>
            <artifactId>hibernate-jpa-2.1-api</artifactId>
            <version>1.0.0.Final</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>${hibernate.version}</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-c3p0</artifactId>
            <version>${hibernate.version}</version>
        </dependency>

        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>4.3.2.Final</version>
        </dependency>


        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-ehcache</artifactId>
            <version>${hibernate.version}</version>
        </dependency>
        <!--日志jar-->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.5</version>
            <scope>provided</scope>
        </dependency>


        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-core-asl</artifactId>
            <version>1.9.13</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-asl</artifactId>
            <version>1.9.13</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-mapper-lgpl</artifactId>
            <version>1.9.13</version>
        </dependency>
        <dependency>
            <groupId>org.codehaus.jackson</groupId>
            <artifactId>jackson-core-lgpl</artifactId>
            <version>1.9.13</version>
        </dependency>

        <dependency>
            <groupId>com.google.code.gson</groupId>
            <artifactId>gson</artifactId>
            <version>2.2.4</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
        </dependency>

        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>
        <dependency>
            <groupId>jcifs</groupId>
            <artifactId>jcifs</artifactId>
            <version>1.3.17</version>
        </dependency>

        <dependency>
            <groupId>commons-httpclient</groupId>
            <artifactId>commons-httpclient</artifactId>
            <version>3.0</version>
        </dependency>

        <dependency>
            <groupId>org.apache.tomcat.embed</groupId>
            <artifactId>tomcat-embed-websocket</artifactId>
            <version>8.0.23</version>
            <scope>provided</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/jaxen/jaxen -->
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>1.1.6</version>
        </dependency>


        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>


        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.8.13</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.aspectj/aspectjrt -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjrt</artifactId>
            <version>1.8.13</version>
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


        <!-- https://mvnrepository.com/artifact/org.apache.struts/struts2-core -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-core</artifactId>
            <version>2.5.16</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/org.apache.struts/struts2-spring-plugin -->
        <dependency>
            <groupId>org.apache.struts</groupId>
            <artifactId>struts2-spring-plugin</artifactId>
            <version>2.5</version>
        </dependency>

        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-core</artifactId>
            <version>2.9.5</version>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-annotations</artifactId>
            <version>2.9.5</version>
        </dependency>
    </dependencies>
    <build>
        <finalName>jpa</finalName>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.1</version>
                    <configuration>
                        <source>1.7</source>
                        <target>1.7</target>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>2.4</version>
            </plugin>
        </plugins>
    </build>

</project>

```


```java

    @Test
    public void testQueryCache() {
        String jpsql = "select new Customer(c.lastName,c.age) FROM Customer  c where c.age > ?";

        //Query query = entityManager.createQuery(jpsql);
        Query query = entityManager.createQuery(jpsql).setHint(QueryHints.HINT_CACHEABLE, true);
        query.setParameter(1, 1);
        List resultList = query.getResultList();
        System.out.println(resultList);

        //query = entityManager.createQuery(jpsql);  //默认是发送2条sql查询语句的
        query = entityManager.createQuery(jpsql).setHint(QueryHints.HINT_CACHEABLE,true);  //默认是发送2条sql查询语句的
        query.setParameter(1, 1);
        resultList = query.getResultList();
        System.out.println(resultList);

    }

```

本来是发送2个sql语句的，现在使用了查询缓存只发送1个sql语句。
```text
Hibernate: 
    select
        customer0_.last_name as col_0_0_,
        customer0_.age as col_1_0_ 
    from
        jpa_customer customer0_ 
    where
        customer0_.age>?
[
Customer{id=null, lastName='Tom', email='null', age=12, createTime=null, birth=null}, 
Customer{id=null, lastName='Jerry', email='null', age=12, createTime=null, birth=null}, 
Customer{id=null, lastName='b', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='dsf', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='zz', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='zz', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='zz', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='aa', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='Tim', email='null', age=12, createTime=null, birth=null}]
[
Customer{id=null, lastName='Tom', email='null', age=12, createTime=null, birth=null}, 
Customer{id=null, lastName='Jerry', email='null', age=12, createTime=null, birth=null}, 
Customer{id=null, lastName='b', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='dsf', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='zz', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='zz', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='zz', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='aa', email='null', age=112, createTime=null, birth=null}, 
Customer{id=null, lastName='Tim', email='null', age=12, createTime=null, birth=null}]

Process finished with exit code 0

```




## order by 和 group by 子句
```java

    @Test
    public void testOrderBy() {
        String jsql = "from Customer c where c.age > ? order by c.id asc";
        Query query = entityManager.createQuery(jsql);
        query.setParameter(1, 1);
        List resultList = query.getResultList();
        System.out.println(resultList);
    }

```
```text

Hibernate: 
    select
        customer0_.id as id1_1_,
        customer0_.age as age2_1_,
        customer0_.birth as birth3_1_,
        customer0_.createTime as createTi4_1_,
        customer0_.email as email5_1_,
        customer0_.last_name as last_nam6_1_ 
    from
        jpa_customer customer0_ 
    where
        customer0_.age>? 
    order by
        customer0_.id asc
[
Customer{id=3, lastName='Tom', email='tom@tom.com', age=12, createTime=2018-05-31 09:32:04.0, birth=2018-05-30}, 
Customer{id=4, lastName='Jerry', email='jerry@163.com', age=12, createTime=2018-05-31 09:32:12.0, birth=2018-05-30}, 
Customer{id=5, lastName='b', email='b', age=112, createTime=2018-05-31 13:34:20.0, birth=2018-05-31}, 
Customer{id=6, lastName='dsf', email='adfa', age=112, createTime=2018-05-31 13:36:33.0, birth=2018-05-31}, 
Customer{id=7, lastName='zz', email='zzzzz', age=112, createTime=2018-05-31 16:02:17.0, birth=2018-05-31}, 
Customer{id=8, lastName='zz', email='zzzzz', age=112, createTime=2018-06-04 11:33:16.0, birth=2018-06-03}, 
Customer{id=9, lastName='zz', email='zzzzz', age=112, createTime=2018-06-04 11:34:05.0, birth=2018-06-03}, 
Customer{id=10, lastName='aa', email='aa', age=112, createTime=2018-06-04 11:41:09.0, birth=2018-06-03}, 
Customer{id=100, lastName='Tim', email='fdadfa', age=12, createTime=2018-05-31 09:33:39.0, birth=2018-05-30}]


```


```java

    @Test
    public void testGroupBy() {
        String jsql = "select o.customer FROM  Order o group by o.customer HAVING count(o.id)>2";
        Query query = entityManager.createQuery(jsql);
        List resultList = query.getResultList();
        System.out.println(resultList);
    }


```
```text
Hibernate: 
    select
        customer1_.id as id1_1_,
        customer1_.age as age2_1_,
        customer1_.birth as birth3_1_,
        customer1_.createTime as createTi4_1_,
        customer1_.email as email5_1_,
        customer1_.last_name as last_nam6_1_ 
    from
        jpa_order order0_ 
    inner join
        jpa_customer customer1_ 
            on order0_.customer_id=customer1_.id 
    group by
        order0_.customer_id 
    having
        count(order0_.id)>2
[
Customer{id=10, lastName='aa', email='aa', age=112, createTime=2018-06-04 11:41:09.0, birth=2018-06-03}]


```



## 关联查询
```java
    @Test
    public void testLeftOuterJoinFetch() {
        //String jsql = "from Customer  c left outer join fetch c.orders where c.id=?";
        String jsql = "from Customer  c   where c.id=?";
        Query query = entityManager.createQuery(jsql);
        query.setParameter(1, 10);
        Customer customer = (Customer) query.getSingleResult();
        System.out.println(customer.getLastName());
        System.out.println(customer.getOrders().size());
    }


```
如上代码没有使用left outer join fetch，这样先获取customer会发送一条sql语句。然后在获取oder又会发送一条sql语句的。
```text
Hibernate: 
    select
        customer0_.id as id1_1_,
        customer0_.age as age2_1_,
        customer0_.birth as birth3_1_,
        customer0_.createTime as createTi4_1_,
        customer0_.email as email5_1_,
        customer0_.last_name as last_nam6_1_ 
    from
        jpa_customer customer0_ 
    where
        customer0_.id=?
aa
Hibernate: 
    select
        orders0_.customer_id as customer3_1_0_,
        orders0_.id as id1_6_0_,
        orders0_.id as id1_6_1_,
        orders0_.customer_id as customer3_6_1_,
        orders0_.order_name as order_na2_6_1_ 
    from
        jpa_order orders0_ 
    where
        orders0_.customer_id=?
3

```


以下代码加上left outer join fetch，效果就不一样了。
```java

    @Test
    public void testLeftOuterJoinFetch() {
        String jsql = "from Customer  c left outer join fetch c.orders where c.id=?";
        Query query = entityManager.createQuery(jsql);
        query.setParameter(1, 10);
        Customer customer = (Customer) query.getSingleResult();
        System.out.println(customer.getLastName());
        System.out.println(customer.getOrders().size());
    }

```
```text

Hibernate: 
    select
        customer0_.id as id1_1_0_,
        orders1_.id as id1_6_1_,
        customer0_.age as age2_1_0_,
        customer0_.birth as birth3_1_0_,
        customer0_.createTime as createTi4_1_0_,
        customer0_.email as email5_1_0_,
        customer0_.last_name as last_nam6_1_0_,
        orders1_.customer_id as customer3_6_1_,
        orders1_.order_name as order_na2_6_1_,
        orders1_.customer_id as customer3_1_0__,
        orders1_.id as id1_6_0__ 
    from
        jpa_customer customer0_ 
    left outer join
        jpa_order orders1_ 
            on customer0_.id=orders1_.customer_id 
    where
        customer0_.id=?
aa
3

```

如果不加fetch，返回的结果会是多个
```java
    @Test
    public void testLeftOuterJoinFetch1() {
        String jsql = "from Customer  c left outer join  c.orders where c.id=?";
        //String jsql = "from Customer  c   where c.id=?";
        Query query = entityManager.createQuery(jsql);
        query.setParameter(1, 10);
        List list = query.getResultList();

        System.out.println(list);
    }

```
```text
Hibernate: 
    select
        customer0_.id as id1_1_0_,
        orders1_.id as id1_6_1_,
        customer0_.age as age2_1_0_,
        customer0_.birth as birth3_1_0_,
        customer0_.createTime as createTi4_1_0_,
        customer0_.email as email5_1_0_,
        customer0_.last_name as last_nam6_1_0_,
        orders1_.customer_id as customer3_6_1_,
        orders1_.order_name as order_na2_6_1_ 
    from
        jpa_customer customer0_ 
    left outer join
        jpa_order orders1_ 
            on customer0_.id=orders1_.customer_id 
    where
        customer0_.id=?
[[Ljava.lang.Object;@6622a690, [Ljava.lang.Object;@30b9eadd, [Ljava.lang.Object;@497570fb]



```


## 子查询
```java

    @Test
    public void testSubQuery() {
        String jsql = "select o from Order  o where o.customer = (select c from  Customer c where c.lastName = ?)";

        Query query = entityManager.createQuery(jsql);
        query.setParameter(1, "aa");
        List list = query.getResultList();

        System.out.println(list);
    }


```
```text
Hibernate: 
    select
        order0_.id as id1_6_,
        order0_.customer_id as customer3_6_,
        order0_.order_name as order_na2_6_ 
    from
        jpa_order order0_ 
    where
        order0_.customer_id=(
            select
                customer1_.id 
            from
                jpa_customer customer1_ 
            where
                customer1_.last_name=?
        )
[Order{id=11, oderName='o-ff-1'}, Order{id=12, oderName='o-ff-2'}, Order{id=13, oderName='o-ff-3'}]

Process finished with exit code 0

```
## jpql的udpate操作
```java
    @Test
    public void testUpdate(){
        String jsql = "update  Customer  c set c.lastName = ? where c.id = ?";
        Query query = entityManager.createQuery(jsql);
        query.setParameter(1, "aaa");
        query.setParameter(2, 10);
        query.executeUpdate();

    }

```

## jpql的delete操作
```java


```


## spring整合JPA

有这么三种整合方式
```java
LocalEntityManagerFactoryBean：适用于那些仅使用 JPA 进行数据访问的项目，
该 FactoryBean 将根据JPA PersistenceProvider 自动检测配置文件进行工作，
一般从“META-INF/persistence.xml”读取配置信息，这种方式最简单，但不能设置 
Spring 中定义的DataSource，且不支持 Spring 管理的全局事务


从JNDI中获取：用于从 Java EE 服务器获取指定的EntityManagerFactory，
这种方式在进行 Spring 事务管理时一般要使用 JTA 事务管理


LocalContainerEntityManagerFactoryBean：适用于所有环境的 FactoryBean，
能全面控制 EntityManagerFactory 配置,如指定 Spring 定义的 DataSource 等等。


```

```java
package com.mamh.jpa;


public class JPATest {

    private ApplicationContext context = null;
    private PersonService personService;

    @Before
    public void init() {
        context = new ClassPathXmlApplicationContext("spring.xml");
        personService = context.getBean(PersonService.class);
    }

    @Test
    public void testPersonService(){
        Person p1 = new Person();
        p1.setAge(12);
        p1.setEmail("aa@163.com");
        p1.setLastName("aa");

        Person p2 = new Person();
        p2.setAge(13);
        p2.setEmail("BB@163.com");
        p2.setLastName("bb");

        System.out.println(personService.getClass().getName());
        personService.savePerson(p1);
        personService.savePerson(p2);
    }

    @Test
    public void testDataSource() {
        DataSource dataSource = context.getBean(DataSource.class);
        EntityManagerFactory entityManagerFactory = (EntityManagerFactory) context.getBean("entityManagerFactory");

        try {
            System.out.println(dataSource.getConnection());
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }


}


```

```java

package com.mamh.jpa.service;

@Service
public class PersonService {

    @Autowired
    private PersonDao personDao;

    @Transactional
    public void savePerson(Person p){
        personDao.save(p);
    }
}


```
```java
package com.mamh.jpa.dao;

@Repository
public class PersonDao {

    /**
     * 如何获取到和当前事务关联的entitymanger
     * 对象呢？
     */
    @PersistenceContext
    private EntityManager entityManager;

    public void save(Person person) {
        entityManager.persist(person);
    }
}


```
```java

package com.mamh.jpa.entities;

@Table(name = "jpa_person")
@Entity
public class Person {
    private Integer id;
    private String lastName;
    private String email;
    private int age;

    @Id
    @GeneratedValue
    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    @Column(name = "last_name")
    public String getLastName() {
        return lastName;
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
    }

    @Column(name = "email")
    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    @Column(name = "age")
    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}


```

```xml

<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd http://www.springframework.org/schema/cache http://www.springframework.org/schema/cache/spring-cache.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">


    <context:property-placeholder location="classpath:db.prop"/>
    <context:component-scan base-package="com.mamh.jpa.dao"/>
    <context:component-scan base-package="com.mamh.jpa.service"/>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="driverClass" value="${jdbc.driverClass}"/>
        <property name="initialPoolSize" value="${jdbc.initPoolSize}"/>
        <property name="maxPoolSize" value="${jdbc.maxPoolSize}"/>
    </bean>


    <!-- 配置 EntityMangerFactory -->
    <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!-- 配置jpa提供商适配器 -->
        <property name="jpaVendorAdapter">
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"/>
        </property>
        <property name="packagesToScan" value="com.mamh.jpa.entities"/>

        <property name="jpaProperties">
            <props>
                <prop key="hibernate.format_sql">true</prop>
                <prop key="hibernate.hbm2ddl.auto">update</prop>
                <prop key="hibernate.show_sql">true</prop>
            </props>
        </property>
    </bean>

    <!-- 配置jpa使用的事务管理器 -->
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactory"/>
    </bean>

    <!-- 配置支持基于注解事务配置 -->
    <tx:annotation-driven transaction-manager="transactionManager"/>


</beans>

```








