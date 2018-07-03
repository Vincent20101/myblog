JPA 学习笔记


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

@Table(name = "jpa_customer")设置表名

@Id映射主键的，放到getter方法上

@GeneratedValue(strategy = GenerationType.AUTO)设置生成主键的策略

@Basic 默认的注解

@Column(name = "last_name") 设置列,name映射到数据库表中的列名

@Transient 不需要影响为数据库表的列的方法上加上这个注解

@Temporal(TemporalType.TIMESTAMP) 设置时间日期对应到数据库表列的类型的

## jpa中常用的类

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



















