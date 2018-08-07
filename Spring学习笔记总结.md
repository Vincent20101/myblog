
Spring   学习

1.什么是spring?
```text

Spring 是个java企业级应用的开源开发框架。Spring主要用来开发Java应用，但是有些扩展是针对构建J2EE平台的web应用。Spring 框架目标是简化Java企业级应用开发，并通过POJO为基础的编程模型促进良好的编程习惯。

Spring的本质是一个bean工厂(beanFactory)或者说bean容器，它按照我们的要求，生产我们需要的各种各样的bean，提供给
我们使用。只是在生产bean的过程中，需要解决bean之间的依赖问题，才引入了依赖注入(DI)这种技术。也就是说依赖注入是
beanFactory生产bean时为了解决bean之间的依赖的一种技术而已。


那么我们为什么需要Spring框架来给我们提供这个beanFactory的功能呢？原因是一般我们认为是，可以将原来硬编码的依赖，
通过Spring这个beanFactory这个工厂来注入依赖，也就是说原来只有依赖方和被依赖方，现在我们引入了第三方——spring这个
beanFactory，由它来解决bean之间的依赖问题，达到了松耦合的效果；这个只是原因之一，还有一个更加重要的原因：在没有
spring这个beanFactory之前，我们都是直接通过new来实例化各种对象，现在各种对象bean的生产都是通过beanFactory来
实例化的，这样的话，spring这个beanFactory就可以在实例化bean的过程中，做一些小动作——在实例化bean的各个阶段进行
一些额外的处理，也就是说beanFactory会在bean的生命周期的各个阶段中对bean进行各种管理，并且spring将这些阶段通过
各种接口暴露给我们，让我们可以对bean进行各种处理，我们只要让bean实现对应的接口，那么spring就会在bean的生命周期
调用我们实现的接口来处理该bean。


```
```text

Spring是一个开源框架，处于MVC模式中的控制层，它能应对需求快速的变化，其主要原因它有一种面向切面编程（AOP）的优势，
其次它提升了系统性能，因为通过依赖倒置机制（IOC），系统中用到的对象不是在系统加载时就全部实例化，
而是在调用到这个类时才会实例化该类的对象，从而提升了系统性能。这两个优秀的性能使得Spring受到许多J2EE公司的青睐，
如阿里里中使用最多的也是Spring相关技术。

Spring的优点：

1、降低了组件之间的耦合性，实现了软件各层之间的解耦。

2、可以使用容易提供的众多服务，如事务管理，消息服务，日志记录等。

3、容器提供了AOP技术，利用它很容易实现如权限拦截、运行期监控等功能。

Spring中AOP技术是设计模式中的动态代理模式。只需实现jdk提供的动态代理接口InvocationHandler，
所有被代理对象的方法都由InvocationHandler接管实际的处理任务。面向切面编程中还要理解切入点、切面、通知、织入等概念。

Spring中IOC则利用了Java强大的反射机制来实现。所谓依赖注入即组件之间的依赖关系由容器在运行期决定。
其中依赖注入的方法有两种，通过构造函数注入，通过set方法进行注入。

```

2.使用Spring框架的好处是什么？
```
轻量：Spring 是轻量的，基本的版本大约2MB。
控制反转：Spring通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。
面向切面的编程(AOP)：Spring支持面向切面的编程，并且把应用业务逻辑和系统服务分开。
容器：Spring 包含并管理应用中对象的生命周期和配置。
MVC框架：Spring的WEB框架是个精心设计的框架，是Web框架的一个很好的替代品。
事务管理：Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。
异常处理：Spring 提供方便的API把具体技术相关的异常（比如由JDBC，Hibernate or JDO抛出的）转化为一致的unchecked 异常。

```

3.Spring由哪些模块组成?
```        
以下是Spring 框架的基本模块：

Core module
Bean module
Context module
Expression Language module
JDBC module
ORM module
OXM module
Java Messaging Service(JMS) module
Transaction module
Web module
Web-Servlet module
Web-Struts module
Web-Portlet module
```

4.核心容器（应用上下文) 模块。
```
这是基本的Spring模块，提供spring框架的基础功能，BeanFactory 是 任何以spring为基础的应用的核心。
Spring 框架建立在此模块之上，它使Spring成为一个容器。
```

5.BeanFactory – BeanFactory 实现举例。
```text
Bean 工厂是工厂模式的一个实现，提供了控制反转功能，用来把应用的配置和依赖从正真的应用代码中分离。
最常用的BeanFactory 实现是 XmlBeanFactory 类。

```

6.XMLBeanFactory 
```text
最常用的就是org.springframework.beans.factory.xml.XmlBeanFactory，它根据XML文件中的定义加载beans。
该容器从XML 文件读取配置元数据并用它去创建一个完全配置的系统或应用。

```

7.解释AOP模块
```
AOP模块用于发给我们的Spring应用做面向切面的开发， 很多支持由AOP联盟提供，这样就确保了Spring和其他AOP框架的共通性。这个模块将元数据编程引入Spring。
```

8.解释JDBC抽象和DAO模块。
```
通过使用JDBC抽象和DAO模块，保证数据库代码的简洁，并能避免数据库资源错误关闭导致的问题，它在各种不同的数据库的错误信息之上，提供了一个统一的异常访问层。它还利用Spring的AOP 模块给Spring应用中的对象提供事务管理服务。
```

9.解释对象/关系映射集成模块。
```
Spring 通过提供ORM模块，支持我们在直接JDBC之上使用一个对象/关系映射映射(ORM)工具，Spring 支持集成主流的ORM框架，如Hiberate,JDO和 iBATIS SQL Maps。Spring的事务管理同样支持以上所有ORM框架及JDBC。
```

10.解释WEB 模块。
```text
Spring的WEB模块是构建在application context 模块基础之上，提供一个适合web应用的上下文。这个模块也包括支持多种面向web的任务，
如透明地处理多个文件上传请求和程序级请求参数的绑定到你的业务对象。它也有对Jakarta Struts的支持。
```

12.Spring配置文件
```text
Spring配置文件是个XML 文件，这个文件包含了类信息，描述了如何配置它们，以及如何相互调用。
```

13.什么是Spring IOC 容器？
```text
Spring IOC 负责创建对象，管理对象（通过依赖注入（DI），装配对象，配置对象，并且管理这些对象的整个生命周期。
```

14.IOC的优点是什么？
```text
IOC 或 依赖注入把应用的代码量降到最低。它使应用容易测试，单元测试不再需要单例和JNDI查找机制。
最小的代价和最小的侵入性使松散耦合得以实现。IOC容器支持加载服务时的饿汉式初始化和懒加载。
```

15.ApplicationContext通常的实现是什么?
```text
FileSystemXmlApplicationContext ：此容器从一个XML文件中加载beans的定义，XML Bean 配置文件的全路径名必须提供给它的构造函数。
ClassPathXmlApplicationContext：此容器也从一个XML文件中加载beans的定义，这里，你需要正确设置classpath因为这个容器将在classpath里找bean配置。
WebXmlApplicationContext：此容器加载一个XML文件，此文件定义了一个WEB应用的所有bean。
```

16.BeanFactory 和 ApplicationContext  有什么区别？
```text
Spring 使用 BeanFactory 来实例化、配置和管理 Bean。
如果说BeanFactory是Spring的心脏，那么ApplicationContext就是完整的躯体了.

ApplicationContext 由 BeanFactory派生而来，提供了更多面向实际应用的功能。
在BeanFactory中，很多功能需要以编程的方式实现，而在ApplicationContext中则可以通过配置实现。


ApplicationContext提供一种方法处理文本消息，一个通常的做法是加载文件资源（比如镜像），它们可以向注册为监听器的bean发布事件。
另外，在容器或容器内的对象上执行的那些不得不由bean工厂以程序化方式处理的操作，可以在Application contexts中以声明的方式处理。
Application contexts实现了MessageSource接口，该接口的实现以可插拔的方式提供获取本地化消息的方法。

1.BeanFactroy采用的是延迟加载形式来注入Bean的，即只有在使用到某个Bean时(调用getBean())，才对该Bean进行加载实例化，这样，我们就不能发现一些存在的Spring的配置问题。
而ApplicationContext则相反，它是在容器启动时，一次性创建了所有的Bean。这样，在容器启动时，我们就可以发现Spring中存在的配置错误。 
相对于基本的BeanFactory，ApplicationContext 唯一的不足是占用内存空间。当应用程序配置Bean较多时，程序启动较慢。

BeanFacotry延迟加载,如果Bean的某一个属性没有注入，BeanFacotry加载后，直至第一次使用调用getBean方法才会抛出异常；
而ApplicationContext则在初始化自身是检验，这样有利于检查所依赖属性是否注入；所以通常情况下我们选择使用 ApplicationContext。
应用上下文则会在上下文启动后预载入所有的单实例Bean。通过预载入单实例bean ,确保当你需要的时候，你就不用等待，因为它们已经创建好了。

2.BeanFactory和ApplicationContext都支持BeanPostProcessor、BeanFactoryPostProcessor的使用，
但两者之间的区别是：BeanFactory需要手动注册，而ApplicationContext则是自动注册。
（Applicationcontext比 beanFactory 加入了一些更好使用的功能。而且 beanFactory 的许多功能需要通过编程实现
而 Applicationcontext 可以通过配置实现。比如后处理 bean ， Applicationcontext 直接配置在配置文件即可而 
beanFactory 这要在代码中显示的写出来才可以被容器识别。 ）

3.beanFactory主要是面对与 spring 框架的基础设施，面对 spring 自己。
而 Applicationcontex 主要面对与 spring 使用的开发者。基本都会使用 Applicationcontex 并非 beanFactory 。

```

17.一个Spring的应用看起来象什么？
```
一个定义了一些功能的接口。
这实现包括属性，它的Setter ， getter 方法和函数等。
Spring AOP。
Spring 的XML 配置文件。
使用以上功能的客户端程序。
依赖注入
```

18.什么是Spring的依赖注入？
```text
依赖注入，是IOC的一个方面，是个通常的概念，它有多种解释。这概念是说你不用创建对象，而只需要描述它如何被创建。
你不在代码里直接组装你的组件和服务，但是要在配置文件里描述哪些组件需要哪些服务，之后一个容器（IOC容器）负责把他们组装起来。
```


19.有哪些不同类型的IOC（依赖注入）方式？
```text
依赖注入的方式分为 构造函数注入 和 setter方法注入：
构造器依赖注入：构造器依赖注入通过容器触发一个类的构造器来实现的，该类有一系列参数，每个参数代表一个对其他类的依赖。
    <bean id="car" class="com.mamh.spring.demo.beans.Car">
        <constructor-arg value="Audi" index="0"/>      <!-- 通过构造方法设置属性值 -->
        <constructor-arg value="shanghai" index="1"/>  <!-- 按照参数顺序来配置 -->
        <constructor-arg value="123" index="2"/>    <!-- 对于引用 可以使用ref 替换value-->
    </bean>
    
Setter方法注入：Setter方法注入是容器通过调用无参构造器或无参static工厂 方法实例化bean之后，调用该bean的setter方法，即实现了基于setter的依赖注入。
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource" />
      <property name="configLocation" value="classpath:config/mybatis-config.xml" />
      <property name="mapperLocations" value="classpath*:config/mappers/**/*.xml" />
    </bean>

集合等复杂类型的注入
<bean id="moreComplexObject" class="example.ComplexObject">
    <!-- results in a setAdminEmails(java.util.Properties) call -->
    <property name="adminEmails">
        <props>
            <prop key="administrator">administrator@example.org</prop>
            <prop key="support">support@example.org</prop>
            <prop key="development">development@example.org</prop>
        </props>
    </property>
    <!-- results in a setSomeList(java.util.List) call -->
    <property name="someList">
        <list>
            <value>a list element followed by a reference</value>
            <ref bean="myDataSource" />
        </list>
    </property>
    <!-- results in a setSomeMap(java.util.Map) call -->
    <property name="someMap">
        <map>
            <entry key="an entry" value="just some string"/>
            <entry key ="a ref" value-ref="myDataSource"/>
        </map>
    </property>
    <!-- results in a setSomeSet(java.util.Set) call -->
    <property name="someSet">
        <set>
            <value>just some string</value>
            <ref bean="myDataSource" />
        </set>
    </property>
</bean>
 也很简单，list属性就是 <list>里面包含<value>或者<ref>或者<bean>, set也类似。
 map是<map>里面包含<entry>这个也好理解，因为map的实现就是使用内部类Entry来存储key和value.
  Properties是 <props>里面包含<prop>.

<bean> 除了 id 和 class 属性之外，还有一些可选的属性:
1) scope属性，默认<bean> 的 scope就是 singleton="true", 
springmvc和struts2的重要区别之一就是spring的controll是单例的，而struts2的action是：scope="prototype" ，
还有 scope="request" ， scope="session"，scope="globalSession"(仅用于portlet)
2）abstract属性，是否是抽象的bean：
3）depends-on 依赖于某个bean，其必须先初始化：<bean id="xxx" class="xxx" depends-on="refbean" />
4）lazy-init="true" 是否延迟初始化，默认为 false
5) dependency-check 是否对bean依赖的其它bean进行检查，默认值为 none，可取值有：none, simple, object, all等
6）factory-method 和 factory-bean用于静态工厂和非静态工厂：
7）init-method, destory-method 指定bean初始化和死亡时调用的方法，常用于 dataSource的连接池的配置
8) lookup-method 方法注入：
9) autowire 是否启用自动装配依赖，默认为 no, 其它取值还有：byName, byType, constructor


```

20.哪种依赖注入方式你建议使用，构造器注入，还是 Setter方法注入？
```text
你两种依赖方式都可以使用，构造器注入和Setter方法注入。
最好的解决方案是用构造器参数实现强制依赖，setter方法实现可选依赖。
```


Spring Beans
21.什么是Spring beans?
```text
Spring beans 是那些形成Spring应用的主干的java对象。它们被Spring IOC容器初始化，装配，和管理。
这些beans通过容器中配置的元数据创建。比如，以XML文件中<bean/> 的形式定义。

Spring 框架定义的beans都是单件beans。在bean tag中有个属性”singleton”，如果它被赋为TRUE，bean 就是单件，
否则就是一个 prototype bean。默认是TRUE，所以所有在Spring框架中的beans 缺省都是单件。
```

22.一个 Spring Bean 定义 包含什么？
```text
一个Spring Bean 的定义包含容器必知的所有配置元数据，包括如何创建一个bean，它的生命周期详情及它的依赖。
```

23.如何给Spring 容器提供配置元数据?
```text
这里有三种重要的方法给Spring 容器提供配置元数据。

XML配置文件。  就是配置<bean> 这样的节点

基于注解的配置。
注解的方式就是类上加上 注解，可以使用的有@Component，@Controller，@Repository，@Service
还可以配合<context:component-scan/>一起使用，自动扫描哪些包下面的类。配置 <context:annotation-config/>元素开启注解功能。
@Component：一个泛化的概念，表示一个组件（Bean），可作用在任何层次
@Controller：用于对Controller实现类进行标注，目前该功能与Component相同
@Repository：用于对DAO实现类进行标注
@Service：用于对Service实现类进行标注，目前该功能与Component相同

基于java的配置。


基于XML的配置主要使用场景：
    第三方类库，如DataSource、JdbcTemplate等；
    命名空间，如aop、context等；
基于注解的配置主要使用场景：
    Bean的实现类是当前项目开发的，可直接在Java类中使用注解配置
基于Java类的配置主要使用场景：
    对于实例化Bean的逻辑比较复杂，则比较适合用基于Java类配置的方式
    
在日常的开发中我们主要是使用XML配置和注解配置方式向结合的开发方式，一般不推荐使用基于Java类的配置方式。
```


24.你怎样定义类的作用域?
```text
当定义一个<bean> 在Spring里，我们还能给这个bean声明一个作用域。它可以通过bean 定义中的scope属性来定义。
如，当Spring要在需要的时候每次生产一个新的bean实例，bean的scope属性被指定为prototype。
另一方面，一个bean每次使用的时候必须返回同一个实例，这个bean的scope 属性必须设为 singleton。
``` 

25.解释Spring支持的几种bean的作用域。
```text
Spring框架支持以下五种bean的作用域：

singleton : bean在每个Spring ioc 容器中只有一个实例。
prototype：一个bean的定义可以有多个实例。

request：每次http请求都会创建一个bean，该作用域仅在基于web的Spring ApplicationContext情形下有效。
session：在一个HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

global-session：在一个全局的HTTP Session中，一个bean定义对应一个实例。该作用域仅在基于web的Spring ApplicationContext情形下有效。

缺省的Spring bean 的作用域是Singleton.

```


26.Spring框架中的单例bean是线程安全的吗?
```text
不，Spring框架中的单例bean不是线程安全的。
```

27.解释Spring框架中bean的生命周期。
```text
Spring容器 从XML 文件中读取bean的定义，并实例化bean。
Spring根据bean的定义填充所有的属性。
如果bean实现了 BeanNameAware 接口，Spring 传递bean 的ID 到 setBeanName方法。
如果Bean 实现了 BeanFactoryAware 接口， Spring传递beanfactory 给setBeanFactory 方法。
如果有任何与bean相关联的BeanPostProcessors，Spring会在postProcesserBeforeInitialization()方法内调用它们。
如果bean实现IntializingBean了，调用它的afterPropertySet方法，如果bean声明了初始化方法，调用此初始化方法。
如果有 BeanPostProcessors 和bean 关联，这些bean的postProcessAfterInitialization() 方法将被调用。
如果bean实现了 DisposableBean，它将调用destroy()方法。


        实例化BeanFactoryPostProcessor实现类
                    ^
                    |
执行 BeanFactoryPostProcessor 的 postProcessBeanFactory方法
                    ^
                    |
        实例化 BeanPostProcessor 实现类
                    ^
                    |
实例化 InstantiationAwareBeanPostProcessorAdapter 实现类
                    ^
                    |
执行 InstantiationAwareBeanPostProcessor  的 postProcessBeforeInstantiation方法
                    ^
                    |
            执行bean构造方法
                    ^
                    |
执行 InstantiationAwareBeanPostProcessor 的 postProcessPropertyValues 方法
                    ^
                    |
            为 bean 注入属性
                    ^
                    |
    调用 BeanNameAware 的 setBeanName 方法
                    ^
                    |
    调用 BeanFactoryAware 的 setBeanFactory 方法
                    ^
                    |
执行 BeanPostProcessor 的 postProcessBeforeInitialization 方法
                    ^
                    |
调用 initializingBean 的 afterPropertiesSet 方法
                    ^
                    |
    调用bean的 init-method 属性指定的方法
                    ^
                    |
执行  BeanPostProcessor 的 postProcessAfterInitialization 方法
                    ^
                    |
    容器初始化成功，执行正常调用后，下面销毁容器
                    ^
                    |
    调用 DiposibleBean 的 destroy 方法
                    ^
                    |
    调用bean的 destroy-method 属性指定的方法


Bean的完整生命周期经历了各种方法调用，这些方法可以划分为以下几类：

1、Bean自身的方法　　：　　这个包括了Bean本身调用的方法和通过配置文件中<bean>的init-method和destroy-method指定的方法

2、Bean级生命周期接口方法　　：　　这个包括了BeanNameAware、BeanFactoryAware、InitializingBean和DiposableBean这些接口的方法

3、容器级生命周期接口方法　　：　　这个包括了InstantiationAwareBeanPostProcessor 和 BeanPostProcessor 这两个接口实现，一般称它们的实现类为“后处理器”。

4、工厂后处理器接口方法　　：　　这个包括了AspectJWeavingEnabler, ConfigurationClassPostProcessor, CustomAutowireConfigurer等等非常有用的工厂后处理器　　接口的方法。工厂后处理器也是容器级的。在应用上下文装配配置文件之后立即调用。
```


28.哪些是重要的bean生命周期方法？ 你能重载它们吗？
```text
有两个重要的bean 生命周期方法，第一个是setup ， 它是在容器加载bean的时候被调用。第二个方法是 teardown  它是在容器卸载类的时候被调用。

bean 标签有两个重要的属性（init-method和destroy-method）。用它们你可以自己定制初始化和注销方法。它们也有相应的注解（@PostConstruct和@PreDestroy）。
```

29.什么是Spring的内部bean？
```text
当一个bean仅被用作另一个bean的属性时，它能被声明为一个内部bean，为了定义inner bean，在Spring 的 基于XML的 配置元数据中，
可以在 <property/>或 <constructor-arg/> 元素内使用<bean/> 元素，内部bean通常是匿名的，它们的Scope一般是prototype。
```

30.在 Spring中如何注入一个java集合？
```text
Spring提供以下几种集合的配置元素：

<list>类型用于注入一列值，允许有相同的值。
<set> 类型用于注入一组值，不允许有相同的值。
<map> 类型用于注入一组键值对，键和值都可以为任意类型。
<props>类型用于注入一组键值对，键和值都只能为String类型。
```


31.什么是bean装配?
```text
装配，或bean 装配是指在Spring 容器中把bean组装到一起，前提是容器需要知道bean的依赖关系，如何通过依赖注入来把它们装配到一起。
```

32.什么是bean的自动装配？
```text
Spring 容器能够自动装配相互合作的bean，这意味着容器不需要<constructor-arg>和<property>配置，能通过Bean工厂自动处理bean之间的协作。
```

33.解释不同方式的自动装配 。
```text
有五种自动装配的方式，可以用来指导Spring容器用自动装配方式来进行依赖注入。

no：默认的方式是不进行自动装配，通过显式设置ref 属性来进行装配。
byName：通过参数名 自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byname，之后容器试图匹配、装配和该bean的属性具有相同名字的bean。
byType:：通过参数类型自动装配，Spring容器在配置文件中发现bean的autowire属性被设置成byType，之后容器试图匹配、装配和该bean的属性具有相同类型的bean。如果有多个bean符合条件，则抛出错误。
constructor：这个方式类似于byType， 但是要提供给构造器参数，如果没有确定的带参数的构造器参数类型，将会抛出异常。
autodetect：首先尝试使用constructor来自动装配，如果无法工作，则使用byType方式。
```


34.自动装配有哪些局限性 ?
```text
自动装配的局限性是：
重写： 你仍需用 <constructor-arg>和 <property> 配置来定义依赖，意味着总要重写自动装配。
基本数据类型：你不能自动装配简单的属性，如基本数据类型，String字符串，和类。
模糊特性：自动装配不如显式装配精确，如果有可能，建议使用显式装配。
```

35.你可以在Spring中注入一个null 和一个空字符串吗？
```text
可以。 <null/>用于处理null值。 

```

Spring注解

36.什么是基于Java的Spring注解配置? 给一些注解的例子.
```text
基于Java的配置，允许你在少量的Java注解的帮助下，进行你的大部分Spring配置而非通过XML文件。
以@Configuration 注解为例，它用来标记类可以当做一个bean的定义，被Spring IOC容器使用。另一个例子是@Bean注解，它表示此方法将要返回一个对象，作为一个bean注册进Spring应用上下文。
```

37.什么是基于注解的容器配置?
```text
相对于XML文件，注解型的配置依赖于通过字节码元数据装配组件，而非尖括号的声明。
开发者通过在相应的类，方法或属性上使用注解的方式，直接组件类中进行配置，而不是使用xml表述bean的装配关系。
```

38.怎样开启注解装配？
```text
注解装配在默认情况下是不开启的，为了使用注解装配，我们必须在Spring配置文件中配置 <context:annotation-config/>元素。
```

39.@Required  注解
```text
这个注解表明bean的属性必须在配置的时候设置，通过一个bean定义的显式的属性值或通过自动装配，若@Required注解的bean属性未被设置，容器将抛出BeanInitializationException。
```

40.@Autowired 注解
```text
@Autowired 注解提供了更细粒度的控制，包括在何处以及如何完成自动装配。它的用法和@Required一样，修饰setter方法、构造器、属性或者具有任意名称和/或多个参数的PN方法。
```

41.@Qualifier 注解
```
当有多个相同类型的bean却只有一个需要自动装配时，将@Qualifier 注解和@Autowire 注解结合使用以消除这种混淆，指定需要装配的确切的bean。
```

Spring数据访问
42.在Spring框架中如何更有效地使用JDBC? 
```
使用SpringJDBC 框架，资源管理和错误处理的代价都会被减轻。所以开发者只需写statements 和 queries从数据存取数据，
JDBC也可以在Spring框架提供的模板类的帮助下更有效地被使用，这个模板叫 JdbcTemplate  

配置一个数据源
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="driverClass" value="${jdbc.driverClass}"/>
        <property name="initialPoolSize" value="${jdbc.initPoolSize}"/>
        <property name="maxPoolSize" value="${jdbc.maxPoolSize}"/>
    </bean>

配置一个jdbcTemplate就可以了
    <!--配置spring 的jdbctemplate -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
    </bean>

```

43.JdbcTemplate
```
JdbcTemplate 类提供了很多便利的方法解决诸如把数据库数据转变成基本数据类型或对象，
执行写好的或可调用的数据库操作语句，提供自定义的数据错误处理。



```

44.Spring对DAO的支持
```
Spring对数据访问对象（DAO）的支持旨在简化它和数据访问技术如JDBC，Hibernate or JDO 结合使用。这使我们可以方便切换持久层。编码时也不用担心会捕获每种技术特有的异常。
```


45.使用Spring通过什么方式访问Hibernate? 
```
在Spring中有两种方式访问Hibernate：
控制反转  Hibernate Template和 Callback。
继承 HibernateDAOSupport提供一个AOP 拦截器。

配置数据源
    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="${jdbc.user}"/>
        <property name="password" value="${jdbc.password}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="driverClass" value="${jdbc.driverClass}"/>
        <property name="initialPoolSize" value="${jdbc.initPoolSize}"/>
        <property name="maxPoolSize" value="${jdbc.maxPoolSize}"/>
    </bean>
    
配置hibernate的sessionFactory
    <!--配置hibernate的sessionfactory -->
    <bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>

        <!--这个配置文件是可以不要的。使用hibernateProperties 来配置属性 -->
        <property name="configLocation" value="classpath:hibernate.cfg.xml"/>
        <!-- 建议保留hibernate.cfg.xml 配置文件-->

        <!-- -->
        <property name="hibernateProperties">
            <props>
                <prop key="hibernate.dialect">org.hibernate.dialect.MySQL5Dialect</prop>
                <!--<property name="dialect">org.hibernate.dialect.MySQLInnoDBDialect</property>-->

                <!--是否打印sql语句-->
                <prop key="hibernate.show_sql">true</prop>

                <!--是否对sql语句格式化-->
                <prop key="hibernate.format_sql">true</prop>

                <!--指定自动生成数据库表的策略，有4个值-->
                <prop key="hibernate.hbm2ddl.auto">update</prop>
            </props>

        </property>

        <property name="mappingLocations" value="classpath:com/mamh/spring/demo/beans/*.hbm.xml"/>
    </bean>
需要事务，在配置个事务管理器
    <bean id="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory"/>
    </bean>
```


46.Spring支持的ORM
```
Spring支持以下ORM：
Hibernate
iBatis
JPA (Java Persistence API)
TopLink
JDO (Java Data Objects)
OJB
```

47.如何通过HibernateDaoSupport将Spring和Hibernate结合起来？
```
用Spring的 SessionFactory 调用 LocalSessionFactory。集成过程分三步：

配置the Hibernate SessionFactory。

继承 HibernateDaoSupport 实现一个DAO。

在AOP支持的事务中装配。

```

48.Spring支持的事务管理类型
```
Spring支持两种类型的事务管理：

编程式事务管理：这意味你通过编程的方式管理事务，给你带来极大的灵活性，但是难维护。

声明式事务管理：这意味着你可以将业务代码和事务管理分离，你只需用注解和XML配置来管理事务。
    <bean id="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory"/>
    </bean>
```

49.Spring框架的事务管理有哪些优点？
```
它为不同的事务API  如 JTA，JDBC，Hibernate，JPA 和JDO，提供一个不变的编程模式。
它为编程式事务管理提供了一套简单的API而不是一些复杂的事务API如
它支持声明式事务管理。
它和Spring各种数据访问抽象层很好得集成。
```

50.你更倾向用那种事务管理类型？
```
大多数Spring框架的用户选择声明式事务管理，因为它对应用代码的影响最小，因此更符合一个无侵入的轻量级容器的思想。
声明式事务管理要优于编程式事务管理，虽然比编程式事务管理（这种方式允许你通过代码控制事务）少了一点灵活性。
```

Spring面向切面编程（AOP）
```
什么是AOP
AOP（Aspect-OrientedProgramming，面向方面编程），可以说是OOP（Object-Oriented Programing，面向对象编程）的补充和完善。OOP引入封装、继承和多态性等概念来建立一种对象层次结构，用以模拟公共行为的一个集合。当我们需要为分散的对象引入公共行为的时候，OOP则显得无能为力。也就是说，OOP允许你定义从上到下的关系，但并不适合定义从左到右的关系。例如日志功能。日志代码往往水平地散布在所有对象层次中，而与它所散布到的对象的核心功能毫无关系。对于其他类型的代码，如安全性、异常处理和透明的持续性也是如此。这种散布在各处的无关的代码被称为横切（cross-cutting）代码，在OOP设计中，它导致了大量代码的重复，而不利于各个模块的重用。

AOP使用场景
AOP用来封装横切关注点，具体可以在下面的场景中使用:

 

Authentication 权限

Caching 缓存

Context passing 内容传递

Error handling 错误处理

Lazy loading 懒加载

Debugging 调试

logging, tracing, profiling and monitoring　记录跟踪　优化　校准

Performance optimization　性能优化

Persistence 持久化

Resource pooling 资源池

Synchronization 同步

Transactions 事务
```

51.解释AOP
```
面向切面的编程，或AOP， 是一种编程技术，允许程序模块化横向切割关注点，或横切典型的责任划分，如日志和事务管理。

AOP相关概念
切面（Aspect）：一个关注点的模块化，这个关注点实现可能另外横切多个对象。事务管理是J2EE应用中一个很好的横切关注点例子。方面用Spring的 Advisor或拦截器实现。

连接点（Joinpoint）: 程序执行过程中明确的点，如方法的调用或特定的异常被抛出。

通知（Advice）: 在特定的连接点，AOP框架执行的动作。各种类型的通知包括“around”、“before”和“throws”通知。
通知类型将在下面讨论。许多AOP框架包括Spring都是以拦截器做通知模型，维护一个“围绕”连接点的拦截器链。
Spring中定义了四个advice: BeforeAdvice, AfterAdvice, ThrowAdvice和DynamicIntroductionAdvice

切入点（Pointcut）: 指定一个通知将被引发的一系列连接点的集合。AOP框架必须允许开发者指定切入点：
例如，使用正则表达式。 Spring定义了Pointcut接口，用来组合MethodMatcher和ClassFilter，可以通过名字很清楚的理解， 
MethodMatcher是用来检查目标类的方法是否可以被应用此通知，而ClassFilter是用来检查Pointcut是否应该应用到目标类上

引入（Introduction）: 添加方法或字段到被通知的类。 Spring允许引入新的接口到任何被通知的对象。
例如，你可以使用一个引入使任何对象实现 IsModified接口，来简化缓存。Spring中要使用Introduction, 
可有通过DelegatingIntroductionInterceptor来实现通知，通过DefaultIntroductionAdvisor来配置Advice和代理类要实现的接口

目标对象（Target Object）: 包含连接点的对象。也被称作被通知或被代理对象。POJO

AOP代理（AOP Proxy）: AOP框架创建的对象，包含通知。 在Spring中，AOP代理可以是JDK动态代理或者CGLIB代理。

织入（Weaving）: 组装方面来创建一个被通知对象。这可以在编译时完成（例如使用AspectJ编译器），也可以在运行时完成。Spring和其他纯Java AOP框架一样，在运行时完成织入。


```

52.Aspect 切面
```
AOP核心就是切面，它将多个类的通用行为封装成可重用的模块，该模块含有一组API提供横切功能。
比如，一个日志模块可以被称作日志的AOP切面。根据需求的不同，一个应用程序可以有若干切面。在Spring AOP中，切面通过带有@Aspect注解的类实现。
```

52.在Spring AOP 中，关注点和横切关注的区别是什么？
```
关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。
横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。
```

54.连接点 Joinpoint
```
连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个AOP切面，它实际上是个应用程序执行Spring AOP的位置。
```

55.通知
```
通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过SpringAOP框架触发的代码段。

Spring切面可以应用五种类型的通知：

before：前置通知，在一个方法执行前被调用。
after: 在方法执行之后调用的通知，无论方法执行是否成功。
after-returning: 仅当方法成功完成后执行的通知。
after-throwing: 在方法抛出异常退出时执行的通知。
around: 在方法执行之前和之后调用的通知。
```

56.切点 Pointcut
```
切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。
```

57.什么是引入? 
```
引入允许我们在已存在的类中增加新的方法和属性。
```

58.什么是目标对象?
``` 
被一个或者多个切面所通知的对象。它通常是一个代理对象。也指被通知（advised）对象。
```


59.什么是代理?
```
代理是通知目标对象后创建的对象。从客户端的角度看，代理对象和目标对象是一样的。
```

60.有几种不同类型的自动代理？
```
BeanNameAutoProxyCreator

DefaultAdvisorAutoProxyCreator

Metadata autoproxying
```


61.什么是织入。什么是织入应用的不同点？
```
织入是将切面和到其他应用类型或对象连接或创建一个被通知对象的过程。

织入可以在编译时，加载时，或运行时完成。
```


62.解释基于XML Schema方式的切面实现。
```
在这种情况下，切面由常规类以及基于XML的配置实现。
```

63.解释基于注解的切面实现
```
在这种情况下(基于@AspectJ的实现)，涉及到的切面声明的风格与带有java5标注的普通java类一致。
```

Spring 的MVC
64.什么是Spring的MVC框架？
```
Spring 配备构建Web 应用的全功能MVC框架。Spring可以很便捷地和其他MVC框架集成，
如Struts，Spring 的MVC框架用控制反转把业务对象和控制逻辑清晰地隔离。
它也允许以声明的方式把请求参数和业务对象绑定。
```

65.DispatcherServlet
```
Spring的MVC框架是围绕 DispatcherServlet 来设计的，它用来处理所有的HTTP请求和响应。
```

66.WebApplicationContext
```
WebApplicationContext 继承了ApplicationContext  并增加了一些WEB应用必备的特有功能，
它不同于一般的ApplicationContext ，因为它能处理主题，并找到被关联的servlet。
```

67.什么是Spring MVC框架的控制器？
```
控制器提供一个访问应用程序的行为，此行为通常通过服务接口实现。
控制器解析用户输入并将其转换为一个由视图呈现给用户的模型。
Spring用一个非常抽象的方式实现了一个控制层，允许用户创建多种用途的控制器。
```

68.@Controller 注解
```
该注解表明该类扮演控制器的角色，Spring不需要你继承任何其他控制器基类或引用Servlet API。
```

69.@RequestMapping 注解
```
该注解是用来映射一个URL到一个类或一个特定的方处理法上。 
```


# Spring  概念

1.spring是开源的轻量级的框架

2.spring核心主要两部分  

- aop：面向切面编程，扩展功能不是修改源代码实现。  

- ioc：控制反转。比如一个类，在类里面有个方法（不是静态方法），调用类里面的方法步骤是：创建类的对象，使用对象来调用这个方法，这里创建类对象需要使用new的方式来创建。
把对象的创建交给spring，采用配置文件的方式来创建对象，这个就是控制反转。


   

3.spring是一站式框架

- spring在java EE三层结构中，每一层都提供不同的解决技术
- web层： spring MVC
- service层： spring IOC
- dao层： spring jdbcTemplate

4.spring版本

- hibernate用的5.x版本
- spring用的4.x版本

# Spring的IOC操作

1.把对象的创建交给spring进行管理

2.ioc操作两种方式

- 配置文件的方式
- 注解的方式

3.IOC底层原理

ioc底层原理使用的技术

- xml配置文件
- dom4j解析xml
- 工厂设计模式
- 反射 



























