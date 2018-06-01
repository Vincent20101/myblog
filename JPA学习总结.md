JPA 学习笔记


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


@Entity 放到实体类上面

@Table(name = "jpa_customer")设置表名

@Id映射主键的，放到getter方法上

@GeneratedValue(strategy = GenerationType.AUTO)设置生成主键的策略

@Basic 默认的注解

@Column(name = "last_name") 设置列,name映射到数据库表中的列名

@Transient 不需要影响为数据库表的列的方法上加上这个注解

@Temporal(TemporalType.TIMESTAMP) 设置时间日期对应到数据库表列的类型的


Persistence类

EntityManagerFactory

EntityManager


EntityManager.find()方法
```java

    @Test
    public void testFind(){
        Customer customer = entityManager.find(Customer.class, 1);
        System.out.println(customer);
    }

```

EntityManager.getReference()方法,只有在使用对象的时候才去数据库中查询。会出现懒加载异常。
```java

    @Test
    public void testRef() {
        Customer customer = entityManager.getReference(Customer.class, 1);
        System.out.println("----------------------");
        System.out.println(customer);

    }

```

EntityManager.persist()方法,使对象由临时状态变为持久化状态。  
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

EntityManager.merge()方法  
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
这种情况下是不让删除的，应为有个外键。

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






















