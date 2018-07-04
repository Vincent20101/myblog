


##spring data jpa的hello world

1.配置数据源
```xml
    <!-- 配置数据源,数据库账户密码存放这个db.prop文件中 -->
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

```

配置完我们就要写一个单元测试
```java
public class SpringDataTest {

    private ApplicationContext context = null;

    @Before
    public void init() {
        context = new ClassPathXmlApplicationContext("spring.xml");
    }





    @Test
    public void test(){
        DataSource dataSource = context.getBean(DataSource.class);
        System.out.println(dataSource);
    }
}

```

2.配置jpa的entityManagerFactory
```xml

    <!--配置jpa的entityManagerFactory -->
    <bean id="entityManagerFactory" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="jpaVendorAdapter">
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"/>
        </property>

        <property name="packagesToScan" value="com.mamh.jpa.springdata"/>

        <property name="jpaProperties">
            <props>
                <prop key="hibernate.format_sql">true</prop>
                <prop key="hibernate.show_sql">true</prop>
                <prop key="hibernate.hbm2ddl.auto">update</prop>
            </props>
        </property>

    </bean>

```
如果配置好了，执行测试代码，会在数据库中新建一个实体类对应的表格的。
```java

    @Test
    public void test(){

    }

```
People实体类
```java
package com.mamh.jpa.springdata;

import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.Id;
import javax.persistence.Table;
import java.util.Date;

@Entity
@Table(name = "jpa_people")
public class People {
    private Integer id;
    private String lastName;
    private String email;
    private Date birth;


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

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }

    public Date getBirth() {
        return birth;
    }

    public void setBirth(Date birth) {
        this.birth = birth;
    }
    
    @Override
    public String toString() {
        return "\nPeople{" +
                "id=" + id +
                ", lastName='" + lastName + '\'' +
                ", email='" + email + '\'' +
                ", birth=" + birth +
                '}';
    }
}


```

3.配置事务管理器 
```xml
    <!-- 配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactory"/>
    </bean>

    <!-- 配置支持基于注解事务配置 -->
    <tx:annotation-driven transaction-manager="transactionManager"/>

```
以上spring和jpa的已经配置好了。




4.配置spring data
```xml
首先要导入jar的。 

    <!-- 配置spring data -->
    <!-- 配置jpa的命名空间， base-package扫描的repository的包名 -->
    <jpa:repositories base-package="com.mamh.jpa.springdata" entity-manager-factory-ref="entityManagerFactory"/>



```
再声明一个接口，和一个方法
```java
public interface PeopleRepository extends Repository<People, Integer>{

    /**
     * 通过last name获取people对象
     * @param lastName
     * @return
     */
    People getByLastName(String lastName);

}

```
测试代码
```java

    @Test
    public void test(){
        PeopleRepository peopleRepository = context.getBean(PeopleRepository.class);
        People people = peopleRepository.getByLastName("aa");
        System.out.println(people);

    }

```
结果输出
```java
Hibernate: 
    select
        people0_.id as id1_0_,
        people0_.birth as birth2_0_,
        people0_.email as email3_0_,
        people0_.last_name as last_nam4_0_ 
    from
        jpa_people people0_ 
    where
        people0_.last_name=?

People{id=1, lastName='aa', email='aa', birth=2018-07-05 03:14:42.0}

```

























