


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


## Repository 接口概述
```java



Repository是一个空接口，即一个标记接口。

若我们继承了Repository接口，则该接口会被IOC容器识别为一个Repository bean，
纳入到IOC容器中，进而可以在该接口中定义满足一端规范的方法。

纳入IOC容器需要满足 base-package扫描的repository的包名. 和 extends Repository.
另外一个方式：//@RepositoryDefinition(domainClass = People.class, idClass = Integer.class)


Repository 接口是 Spring Data 的一个核心接口，它不提供任何方法，开发者需要在自己定义的接口中声明需要的方法 
    public interface Repository<T, ID extends Serializable> { } 
Spring Data可以让我们只定义接口，只要遵循 Spring Data的规范，就无需写实现类。  
与继承 Repository 等价的一种方式，就是在持久层接口上使用 @RepositoryDefinition 注解，并为其指定 domainClass 和 idClass 属性。如下两种方式是完全等价的

```

## Repository 的子接口
```
基础的 Repository 提供了最基本的数据访问功能，其几个子接口则扩展了一些功能。它们的继承关系如下： 

Repository： 仅仅是一个标识，表明任何继承它的均为仓库接口类

CrudRepository： 继承 Repository，实现了一组 CRUD 相关的方法 

PagingAndSortingRepository： 继承 CrudRepository，实现了一组分页排序相关的方法 

JpaRepository： 继承 PagingAndSortingRepository，实现一组 JPA 规范相关的方法 
自定义的 XxxxRepository 需要继承 JpaRepository，这样的 XxxxRepository 接口就具备了通用的数据访问控制层的能力。

JpaSpecificationExecutor： 不属于Repository体系，实现一组 JPA Criteria 查询相关的方法 


```

## Repository 的接口方法声明规范
```java

简单条件查询: 查询某一个实体类或者集合 
按照 Spring Data 的规范，查询方法以 find | read | get 开头
涉及条件查询时，条件的属性用条件关键字连接，要注意的是：条件属性以首字母大写。 

例如：定义一个 Entity 实体类 
class User｛ 
    private String firstName; 
    private String lastName; 
｝

使用And条件连接时，应这样写： 
    findByLastNameAndFirstName(String lastName, String firstName); 

条件的属性名称与个数要与参数的位置与个数一一对应 


查询方法解析流程

假如创建如下的查询：findByUserDepUuid()，框架在解析该方法时，首先剔除 findBy，然后对剩下的属性进行解析，假设查询实体为Doc
先判断 userDepUuid （根据 POJO 规范，首字母变为小写）是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，继续第二步；
从右往左截取第一个大写字母开头的字符串(此处为Uuid)，然后检查剩下的字符串是否为查询实体的一个属性，如果是，则表示根据该属性进行查询；如果没有该属性，则重复第二步，继续从右往左截取；最后假设 user 为查询实体的一个属性；
接着处理剩下部分（DepUuid），先判断 user 所对应的类型是否有depUuid属性，如果有，则表示该方法最终是根据 “ Doc.user.depUuid” 的取值进行查询；否则继续按照步骤 2 的规则从右往左截取，最终表示根据 “Doc.user.dep.uuid” 的值进行查询。
可能会存在一种特殊情况，比如 Doc包含一个 user 的属性，也有一个 userDep 属性，此时会存在混淆。可以明确在属性之间加上 "_" 以显式表达意图，比如 "findByUser_DepUuid()" 或者 "findByUserDep_uuid()"
特殊的参数： 还可以直接在方法的参数上加入分页或排序的参数，比如：
Page<UserModel> findByName(String name, Pageable pageable);
List<UserModel> findByName(String name, Sort sort);


```
## 使用@Query自定义查询

这种查询可以声明在 Repository 方法中，摆脱像命名查询那样的约束，
将查询直接在相应的接口方法中声明，结构更为清晰，这是 Spring data 的特有实现。

```java

//@RepositoryDefinition(domainClass = People.class, idClass = Integer.class)
public interface PeopleRepository extends Repository<People, Integer> {

    /**
     * 通过last name获取people对象
     *
     * @param lastName
     * @return
     */
    People getByLastName(String lastName);


    //where lastName like ?% and id < ?
    List<People> getByLastNameStartingWithAndIdLessThan(String lastName, Integer id);

    //where lastName like %? and id < ?
    List<People> getByLastNameEndingWithAndIdLessThan(String lastName, Integer id);

    //where email in (?, ?, ?)  or birth < ?
    List<People> getByEmailInOrBirthLessThan(List<String> emails, Date birth);


    //查询id最大的people
    @Query("select p from People p where p.id = (select max(p2.id) from People p2)")
    People getMaxIdPeople();


    //使用占位符的方式传递参数
    @Query("select p from People p where p.lastName = ?1 and p.email = ?2")
    List<People> getPeople(String last, String email);
    
    //命名参数的方式
    @Query("select p from People p where p.lastName = :lastName and p.email = :email")
    List<People> getPeople1(@Param("lastName") String last, @Param("email") String email);
    
    
    @Query("select p from People p where p.lastName like %:lastName% and p.email like %:email%")
    List<People> getPeople2(@Param("lastName") String last, @Param("email") String email);

}

```





##@Modifying 注解和事务
@Query 与 @Modifying 这两个 annotation一起声明，可定义个性化更新操作，例如只涉及某些字段更新时最为常用，示例如下：   
注意：  
方法的返回值应该是 int，表示更新语句所影响的行数  
在调用的地方必须加事务，没有事务不能正常执行  

```java
public interface PeopleRepository extends Repository<People, Integer> {    
    
    @Modifying
    @Query("update People  p set p.email = :email where  p.id = :id")
    void updatePeopleEmail(@Param("id") String id,@Param("email") String email);
}

```

在使用修改的情况下面，如果不加上@Modifying就会报错“Not supported for DML operations ”

加上了@Modifying还是会报另外一个错误nested exception is javax.persistence.TransactionRequiredException: Executing an update/delete query
这个因为需要事务，我需要把这个update（）放到service中，其中加上    @Transactional注解


```

Spring Data 提供了默认的事务处理方式，即所有的查询均声明为只读事务。
对于自定义的方法，如需改变 Spring Data 提供的事务默认方式，可以在方法上注解 @Transactional 声明 

进行多个 Repository 操作时，也应该使它们在同一个事务中处理，按照分层架构的思想，
这部分属于业务逻辑层，因此，需要在 Service 层实现对多个 Repository 的调用，并在相应的方法上声明事务。 

默认情况下，spring data的每个方法上有事务，但都是一个只读的事务，他们不能完成修改操作！

```


## CrudRepository 接口

```java
CrudRepository 接口提供了最基本的对实体类的添删改查操作 

T save(T entity);//保存单个实体 

Iterable<T> save(Iterable<? extends T> entities);//保存集合        

T findOne(ID id);//根据id查找实体         

boolean exists(ID id);//根据id判断实体是否存在         

Iterable<T> findAll();//查询所有实体,不用或慎用!         

long count();//查询实体数量         

void delete(ID id);//根据Id删除实体         

void delete(T entity);//删除一个实体 

void delete(Iterable<? extends T> entities);//删除一个实体的集合         


void deleteAll();//删除所有实体,不用或慎用! 




```
保存还是需要放到service，加上事务的。
```java

public interface PeopleCrudRepository extends CrudRepository<People, Integer> {

}


@Service
public class PeopleService {

    @Autowired
    private PeopleRepository peopleRepository;

    @Autowired
    private PeopleCrudRepository peopleCrudRepository;

    @Transactional
    public void savePeoples(List<People> peoples) {
        peopleCrudRepository.saveAll(peoples);

    }

    @Transactional
    public void update(Integer id, String email) {
        peopleRepository.updatePeopleEmail(id, email);
    }
}
```



## PagingAndSortingRepository接口

分页和排序相关的一个接口
```java


/**
 * Extension of {@link CrudRepository} to provide additional methods to retrieve entities using the pagination and
 * sorting abstraction.
 * 
 * @author Oliver Gierke
 * @see Sort
 * @see Pageable
 * @see Page
 */
@NoRepositoryBean
public interface PagingAndSortingRepository<T, ID> extends CrudRepository<T, ID> {

	/**
	 * Returns all entities sorted by the given options.
	 * 
	 * @param sort
	 * @return all entities sorted by the given options
	 */
	Iterable<T> findAll(Sort sort);

	/**
	 * Returns a {@link Page} of entities meeting the paging restriction provided in the {@code Pageable} object.
	 * 
	 * @param pageable
	 * @return a page of entities
	 */
	Page<T> findAll(Pageable pageable);
}





public interface PeoplePagingAndSortingRepository extends PagingAndSortingRepository<People, Integer> {


}




```

测试代码
```java
    @Test
    public void testPageing() {
        int pageNo = 1 - 1;
        int pageSize = 5;

        PageRequest pageRequest = PageRequest.of(pageNo, pageSize);
        Page<People> page = peoplePagingAndSortingRepository.findAll(pageRequest);

        System.out.println("总记录数： " + page.getTotalElements());
        System.out.println("当前第几页： " + (page.getNumber() + 1));
        System.out.println("当前页面的list： " + page.getContent());
        System.out.println("当前页面的记录数： " + page.getNumberOfElements());

    }

```

排序
```java
    @Test
    public void testSorting() {
        int pageNo = 2 - 1;
        int pageSize = 5;
        Sort.Order order1 = new Sort.Order(Sort.Direction.ASC, "id");
        Sort.Order order2 = new Sort.Order(Sort.Direction.DESC, "email");
        Sort sort = Sort.by(order1, order2);

        PageRequest pageRequest = PageRequest.of(pageNo, pageSize, sort);
        Page<People> page = peoplePagingAndSortingRepository.findAll(pageRequest);

        System.out.println("总记录数： " + page.getTotalElements());
        System.out.println("当前第几页： " + (page.getNumber() + 1));
        System.out.println("当前页面的list： " + page.getContent());
        System.out.println("当前页面的记录数： " + page.getNumberOfElements());

    }

```
注意输出的sql语句就带有order by 子句的。
```java
Hibernate: 
    select
        people0_.id as id1_1_,
        people0_.address_id as address_5_1_,
        people0_.birth as birth2_1_,
        people0_.email as email3_1_,
        people0_.last_name as last_nam4_1_ 
    from
        jpa_people people0_ 
    order by
        people0_.id asc,
        people0_.email desc limit ?,
        ?
Hibernate: 
    select
        count(people0_.id) as col_0_0_ 
    from
        jpa_people people0_

```

