
spring MVC 学习


首先来个hello world例子。

我们在index.jsp中弄一个超链接
```haml
<a href="helloworld">helloworld</a>

```

新建个handler，用来处理请求的url，这里我们使用注解的方式来映射url。
```java

@Controller
public class HelloWorld {


    /**
     * 使用  RequestMapping 来映射请求的 url。
     * 返回值会通过视图解析器来解析为实际的视图。通过 prefix + returnValue + 后缀  得到路径，任何使用转发.
     * @return
     */
    @RequestMapping("/helloworld")
    private String hello() {

        System.out.println("hellow world...................");

        return "success";
    }
}

```

在spingmvc.xml中我们要配置
```xml
    <!-- 配置自动扫描的包 -->
    <context:component-scan base-package="com.mamh.springmvc.demo.handlers"/>


    <!-- 配置视图解析器，如何把hanlder方法返回值解析为物理视图 -->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/WEB-INF/views/"/>
        <property name="suffix" value=".jsp"/>
    </bean>

```


web.xml文件的配置
```xml


<web-app>


    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <url-pattern>/</url-pattern>
    </servlet-mapping>


</web-app>

```

整个工程使用maven来管理，其中pom文件
```xml

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>mamh</groupId>
    <artifactId>spring</artifactId>
    <packaging>war</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>spring Maven Webapp</name>
    <url>http://maven.apache.org</url>

    <properties>
        <spring.version>5.0.4.RELEASE</spring.version>
    </properties>


    <dependencies>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>1.7.21</version>
        </dependency>

        <!--j2ee相关包 servlet、jsp、jstl-->
        <dependency>
            <groupId>javax.servlet</groupId>
            <artifactId>javax.servlet-api</artifactId>
            <version>3.1.0</version>
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

        <!--mysql驱动包-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.35</version>
        </dependency>

        <dependency>
            <groupId>commons-fileupload</groupId>
            <artifactId>commons-fileupload</artifactId>
            <version>1.3.1</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!-- https://mvnrepository.com/artifact/com.mchange/c3p0 -->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.5.2</version>
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

        <!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-core -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>5.2.15.Final</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-c3p0</artifactId>
            <version>5.2.15.Final</version>
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

        <dependency>
            <groupId>com.oracle</groupId>
            <artifactId>ojdbc8</artifactId>
            <version>12.2.0.1.0</version>
        </dependency>

        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-ehcache</artifactId>
            <version>5.2.15.Final</version>
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


    </dependencies>

    <build>
        <finalName>spring</finalName>

        <resources>
            <!--表示把java目录下的有关xml文件,properties文件编译/打包的时候放在resource目录下-->
            <resource>
                <directory>${basedir}/src/main/java</directory>
                <includes>
                    <include>**/*.properties</include>
                    <include>**/*.xml</include>
                </includes>
            </resource>
            <resource>
                <directory>${basedir}/src/main/resources</directory>
            </resource>
        </resources>
        <plugins>
            <!--servlet容器 jetty插件-->
            <plugin>
                <groupId>org.eclipse.jetty</groupId>
                <artifactId>jetty-maven-plugin</artifactId>
                <version>9.3.10.v20160621</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>1.6</source>
                    <target>1.6</target>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>


```


@RequestMapping("/helloworld")详解

这个注解也能加到class上。如果同时class上和method都加的有，那url的映射是2个加到一起的。

类上没有就是相对于web应用的根目录的。方法出的相对于类定义处的。

```java

@RequestMapping("/class")
@Controller
public class HelloWorld {


    /**
     * 使用  RequestMapping 来映射请求的 url。
     * 返回值会通过视图解析器来解析为实际的视图。通过 prefix + returnValue + 后缀  得到路径，任何使用转发.
     * @return
     */
    @RequestMapping("/helloworld")
    private String hello() {

        System.out.println("hellow world...................");

        return "success";
    }
}

```
这样类上加了@RequestMapping("/class") 之后 的url就是这样http://localhost:8080/class/helloworld来访问了。

先前的没有加的是http://localhost:8080/helloworld来访问了


@RequestMapping()请求方式，value，method属性 

@RequestMapping 的 value、method、params 及 heads 
分别表示请求 URL、请求方法、请求参数及请求头的映射条
件，他们之间是与的关系，联合使用多个条件可让请求映射
更加精确化。
 
常用的method的两个值：
RequestMethod.POST ，RequestMethod.GET

```java

    @RequestMapping(value = "/testMethod", method = RequestMethod.POST)
    private String testMethod() {
        System.out.println("hellow world....testMethod...............");


        return "success";

    }

```


@RequestMapping()请求参数，请求头


params  和 headers支持简单的表达式：
```
param1: 表示请求必须包含名为 param1 的请求参数 –
!param1: 表示请求不能包含名为 param1 的请求参数 –
param1 != value1: 表示请求包含名为 param1 的请求参数，但其– 值不能为 value1
{“param1=value1”, “param2”}: 请求必须包含名为 param1 和param2 –的两个请求参数，且 param1 参数的值必须为 value1
```
```java


    @RequestMapping(value = "/testParams", params = {"username", "age!=10"})
    private String testParams() {
        System.out.println("hellow world....testParams...............");


        return "success";

    }

```
http://localhost:8080/testParams?username=mamh&age=101这个就能访问，age改为10就不行了。

```java

    @RequestMapping(value = "/testParams", 
    params = {"username", "age!=10"},
    headers = {"Accept-Language=zh-CN,en-US;q=0.7,en;q=0.3"})
    private String testParams() {
        System.out.println("hellow world....testParams...............");
        return "success";
    }

``` 


@RequestMapping()支持 ant风格统配匹配

Ant 风格资源地址支持 3 种匹配符：  
```
?：匹配文件名中的一个字符 –   
*：匹配文件名中的任意字符 –   
 **：** 匹配多层路径   
```

@RequestMapping 还支持 Ant 风格的 URL ：
```
/user/*/createUser: 匹配  
/user/aaa/createUser、/user/bbb/createUser 等 URL

/user/**/createUser: 匹配  
/user/createUser、/user/aaa/bbb/createUser 等 URL

/user/createUser??: 匹配  
/user/createUseraa、/user/createUserbb 等 URL
```

```java


    @RequestMapping(value = "/testPath/*/abc")
    private String testPath() {
        System.out.println("hellow world....testPath...............");
        return "success";
    }


```
http://localhost:8080/testPath/a/abc可以。  
http://localhost:8080/testPath/b/abc也可以匹配



@PathVariable 映射 URL 绑定的占位符
```java

 
    @RequestMapping(value = "/testPathVariable{id}")
    private String testPathVariable(@PathVariable(value = "id") int id) {
        System.out.println("hellow world....PathVariable............... id = " + id);
        return "success";
    }

```
访问这样http://localhost:8080/testPathVariable2323访问，会得到参数id是2323.  
通过 @PathVariable 可以将 URL 中占位符参数绑定到控制器处理方法的入参中：URL 中的 {xxx} 占位符可以通过
@PathVariable("xxx") 绑定到操作方法的入参中。


REST：即 Representational State Transfer。（资源）表现层状态转化。


```text

REST：即 Representational State Transfer。（资源）表现层状态转化。是目前 •
最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，
所以正得到越来越多网站的采用
资源（Resources）：网络上的一个实体，或者说是网络上的一个具体信息。它 •
可以是一段文本、一张图片、一首歌曲、一种服务，总之就是一个具体的存在。
可以用一个URI（统一资源定位符）指向它，每种资源对应一个特定的 URI 。要
获取这个资源，访问它的URI就可以，因此 URI 即为每一个资源的独一无二的识
别符。
表现层（Representation）：把资源具体呈现出来的形式，叫做它的表现层 •
（Representation）。比如，文本可以用 txt 格式表现，也可以用 HTML 格
式、XML 格式、JSON 格式表现，甚至可以采用二进制格式。
状态转化（State Transfer）：每• 发出一个请求，就代表了客户端和服务器的一
次交互过程。HTTP协议，是一个无状态协议，即所有的状态都保存在服务器
端。因此，如果客户端想要操作服务器，必须通过某种手段，让服务器端发生“
状态转化”（State Transfer）。而这种转化是建立在表现层之上的，所以就是 “
表现层状态转化”。具体说，就是 HTTP 协议里面，四个表示操作方式的动
词：GET、POST、PUT、DELETE。它们分别对应四种基本操作：GET 用来获
取资源，POST 用来新建资源，PUT 用来更新资源，DELETE 用来删除资源。

```

form表单是不支持delete，和put的，不过我们可以添加一个隐藏域使用post方式转换到delete或put

```text



<form action="springmvc/testREST/1" method="post">
    <input type="hidden" name="_method" value="PUT"/>
    <input type="submit" value="TestRest PUT"/>
</form>
<br><br>

<form action="springmvc/testREST/1" method="post">
    <input type="hidden" name="_method" value="DELETE"/>
    <input type="submit" value="TestRest DELETE"/>
</form>
<br><br>

<form action="springmvc/testREST" method="post">
    <input type="submit" value="TestRest POST"/>
</form>
<br><br>

<a href="springmvc/testREST/1">Test Rest Get</a>
<br><br>

```
```java


    @RequestMapping(value = "/testREST/{id}", method = RequestMethod.GET)
    private String testGET(@PathVariable(value = "id") int id) {
        System.out.println("testREST  GET : " + id);
        return "success";
    }

    @RequestMapping(value = "/testREST/{id}", method = RequestMethod.PUT)
    private String testPUT(@PathVariable(value = "id") int id) {
        System.out.println("testREST  PUT : " + id);
        return "success";
    }

    @RequestMapping(value = "/testREST/{id}", method = RequestMethod.DELETE)
    private String testDELETE(@PathVariable(value = "id") int id) {
        System.out.println("testREST  DELETE : " + id);
        return "success";
    }

    @RequestMapping(value = "/testREST", method = RequestMethod.POST)
    private String testPOST() {
        System.out.println("testREST   POST : ");
        return "success";
    }

```
这里使用HiddenHttpMethodFilter过滤器来把post请求转换为delete或put请求，

```xml


    <filter>
        <filter-name>hiddenHttpMethodFilter</filter-name>
        <filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>hiddenHttpMethodFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

```
从java中的代码我们可以看到 需要 在post请求中传过滤的参数是DEFAULT_METHOD_PARAM = "_method"
```java
public class HiddenHttpMethodFilter extends OncePerRequestFilter {
    public static final String DEFAULT_METHOD_PARAM = "_method";
    private String methodParam = "_method";

    public HiddenHttpMethodFilter() {
    }

    public void setMethodParam(String methodParam) {
        Assert.hasText(methodParam, "'methodParam' must not be empty");
        this.methodParam = methodParam;
    }

    protected void doFilterInternal(HttpServletRequest request, HttpServletResponse response, FilterChain filterChain) throws ServletException, IOException {
        HttpServletRequest requestToUse = request;
        if ("POST".equals(request.getMethod()) && request.getAttribute("javax.servlet.error.exception") == null) {
            String paramValue = request.getParameter(this.methodParam);
            if (StringUtils.hasLength(paramValue)) {
                requestToUse = new HiddenHttpMethodFilter.HttpMethodRequestWrapper(request, paramValue);
            }
        }

        filterChain.doFilter((ServletRequest)requestToUse, response);
    }

    private static class HttpMethodRequestWrapper extends HttpServletRequestWrapper {
        private final String method;

        public HttpMethodRequestWrapper(HttpServletRequest request, String method) {
            super(request);
            this.method = method.toUpperCase(Locale.ENGLISH);
        }

        public String getMethod() {
            return this.method;
        }
    }
}


```


@RequestParam 映射请求参数  
value值就是请求参数值   
required决定这个请求参数是否是必须的  
defaultValue可以给这个请求参数设置个默认值  

```java

    @RequestMapping("/testRequestParam")
    private String testRequestParam(@RequestParam(value = "username") String username,
                                    @RequestParam(value = "password") String password) {
        System.out.println("hellow world....testRequestParam..............username = " + username + "  password = " + password);

        return "success";
    }

```

http://localhost:8080/springmvc/testRequestParam?username=123&password=abc
访问的时候如果没带请求参数是会报错的。  可以加上required = false来设置是否需要，还可以设置个defaultValue。  


@RequestHeader 绑定请求报头的属性值  
```java
    @RequestMapping("/testRequestHeader")
    private String testRequestHeader(@RequestHeader(value = "Accept-Language") String al) {
        System.out.println("hellow world.....testRequestHeader............\"Accept-Language\" = " + al);
        return "success";
    }
```
http://localhost:8080/springmvc/testRequestHeader

@CookieValue 绑定请求中的 Cookie 值
```java

    @RequestMapping("/testCookieValue")
    private String testCookieValue(@CookieValue(value = "JSESSIONID") String c) {
        System.out.println("hellow world.....testCookieValue....CookieValue =  " + c);
        return "success";
    }

```

使用 POJO 对象绑定请求参数值  
Spring MVC 会按• 请求参数名和 POJO 属性名进行自动匹配，自动为该对象填充属性值。支持级联属性。
如：dept.deptId、dept.address.tel 等

```java
    @RequestMapping("/testPOJO")
    private String testPOJO(User user) {
        System.out.println("hellow world.....testPOJO..." + user);
        return "success";
    }

```


使用 Servlet API 原生API
```test

HttpServletRequest  
HttpServletResponse  
HttpSession  
java.security.Principal  
Locale  
InputStream  
OutputStream 
Reader  
Writer

```


@SessionAttributes

若希望在多个请求之间共用某个模型属性数据，则可以在 控制器类上标注一个 @SessionAttributes, Spring MVC 将在模型中对应的属性暂存到 HttpSession 中。

@SessionAttributes 除了可以通过属性名指定需要放到会话中的属性外，还可以通过模型属性的对象类型指定哪些模型属性需要放到会话中
```text

@SessionAttributes(types=User.class) 会将隐含模型中所有类型为 User.class 的属性添加到会话中。

@SessionAttributes(value={“user1”, “user2”}) 

@SessionAttributes(types={User.class, Dept.class}) 

@SessionAttributes(value={“user1”, “user2”}, types={Dept.class})

```
```java


    @RequestMapping("/testSessionAttributes")
    public String testSessionAttributes(Map<String, Object> map) {
        User user = new User();
        user.setAge(123);
        user.setUsername("Tom");
        user.setEmail("123@mm.com");
        user.setPassword("123456");
        map.put("user", user);
        //默认这个是放到request里面的
        
        //可以在类上加上 @SessionAttributes(value = {"user"}, types = {Integer.class,String.class, User.class})
        //来把map中对应的类型的,或者对应键值的 对象也放到session中取
        map.put("string1", "xxxxxxxxxxxxxxxxxxx this is a stirng ....");
        map.put("int1", 123321);

        return "success";
    }

```


@ModelAttribute

在方法定义上使用 @ModelAttribute 注解：Spring MVC 在调用目标处理方法前，会先逐个调用在方法级上标注了@ModelAttribute 的方法。

在方法的入参前使用@ModelAttribute 注解可以从隐含对象中获取隐含的模型数据中获取对象，再将请求参数绑定到对象中，再传入入参.将方法入参对象添加到模型中.  

```java

    @ModelAttribute
    public void getUser(@RequestParam(value = "id", required = false) Integer id, Map<String, Object> map) {
        if (id != null) {
            User user = new User(1, "tom", "123456", "tom@123.com", 12, null);

            System.out.println("从数据库获取一个对象" + user);
            map.put("user", user);
        }
    }

    @RequestMapping("/testModelAttribute")
    public String testModelAttribute(User user) {
        System.out.println("testModelAttribute=== " + user);


        return "success";
    }

```
@ModelAttribute注解,可以修身目标方法POJO入参。 当那个键不是类名第一个字母小写的时候，我们可以用  
@ModelAttribute注解修饰入参，里面value就是自定义的那个键名 。例如下面我们把键名改为了“abc”，    
在testModelAttribute(User user)方法中如果不使用注解那个user就是一个新建的，而不是从map中获得的。

```java

    @ModelAttribute
    public void getUser(@RequestParam(value = "id", required = false) Integer id, Map<String, Object> map) {
        if (id != null) {
            User user = new User(1, "tom", "123456", "tom@123.com", 12, null);

            System.out.println("从数据库获取一个对象" + user);
            map.put("abc", user);
        }
    }

    @RequestMapping("/testModelAttribute")
    public String testModelAttribute(@ModelAttribute(value = "abc") User user) {
        System.out.println("testModelAttribute=== " + user);


        return "success";
    }

```

国际化

使用i18n国际化的时候，需要建2个properties文件  
	new file:   src/main/resources/i18n_en_US.properties  
	new file:   src/main/resources/i18n_zh_CN.properties  
```text
i18n.username=username
i18n.passowrd=password


i18n.username=\u7528\u6237\u540D
i18n.passowrd=\u7528\u6237\u540D
```
properties文件中要使用编码后的，也就是中文要变成\u 开头这样的。

```xml

    <bean id="messageSource" class="org.springframework.context.support.ResourceBundleMessageSource">
        <property name="basename" value="i18n"/>
    </bean>

```


view-controller

```xml

    <!-- 配置直接转发的页面，可以直接配置转发的页面，而不需要经过handler方法 -->
    <mvc:view-controller path="/success" view-name="success"/>

```
可以直接访问 http://localhost:8080/success了。

这个时候那个  @RequestMapping() 配置的那个返回的view就会报404错误了。这个时候可以加上
annotation-driven
```xml
    <mvc:annotation-driven/>


```

自定义View
```xml
    <context:component-scan base-package="com.mamh.springmvc.demo.views"/>

    <!-- 配置视图解析器 -->
    <bean id="beanNameViewResolver" class="org.springframework.web.servlet.view.BeanNameViewResolver">
        <property name="order" value="100"/>
    </bean>

```
```java

package com.mamh.springmvc.demo.views;

import org.springframework.lang.Nullable;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.View;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.util.Date;
import java.util.Map;

@Component
public class HelloView implements View {
    @Override
    public void render(@Nullable Map<String, ?> model, HttpServletRequest request,
                       HttpServletResponse response) throws Exception {
        response.getWriter().println("hello view, time: " + new Date());

    }

    @Nullable
    @Override
    public String getContentType() {
        return "text/html";
    }
}

```

CRUD操作

```java

@Controller
public class EmployeeHandler {

    @Autowired
    private EmployeeDao employeeDao;

    @Autowired
    private DepartmentDao departmentDao;

    @RequestMapping(value = "/emp", method = RequestMethod.GET)
    public String input( Map<String, Object> map) {
        System.out.println("input..........");
        map.put("departments", departmentDao.getDepartments());
        map.put("employee",new Employee());

        return "input";
    }

    @RequestMapping("/emps")
    public String list(Map<String, Object> map) {

        map.put("employees", employeeDao.getAll());

        return "list";
    }

}

```
```jsp


<c:if test="${empty requestScope.employees}">
    没有任何信息
</c:if>
<c:if test="${!empty requestScope.employees}">
    <table border="1" cellpadding="10" cellspacing="0">
        <tr>
            <th>ID</th>
            <th>LastName</th>
            <th>Email</th>
            <th>Gender</th>
            <th>Department</th>
            <th>Edit</th>
            <th>Delete</th>
        </tr>
        <c:forEach items="${requestScope.employees}" var="emp">
            <tr>
                <td>${emp.id}</td>
                <td>${emp.lastName}</td>
                <td>${emp.email}</td>
                <td>${emp.gender == 0? 'Female': 'Male'}</td>
                <td>${emp.department.departmentName}</td>
                <td><a href="">Edit</a></td>
                <td><a href="">Delete</a></td>
            </tr>
        </c:forEach>

    </table>
</c:if>

<br/>
<a href="/emp">Add new Employee.........</a>



```

```jsp

<!--
可以更快速的开发出表单页面，方便表单值回显

java.lang.IllegalStateException: Neither BindingResult nor plain target object for bean name 'command' available as request attribute

可以通过 modelAttribute 属性指定绑定的模型属性，
若没有指定该属性，则默认从 request 域对象中读取
command 的表单 bean，如果该属性值也不存在，则会
发生错误。

-->

<form:form action="/emp" method="POST" modelAttribute="employee">
    LastName: <form:input path="lastName"/> <br/>

    Email: <form:input path="email"/> <br/>

    <%
        Map<String, String> genders = new HashMap();
        genders.put("1", "Male");
        genders.put("0", "Female");
        request.setAttribute("genders", genders);
    %>
    Gender: <form:radiobuttons path="gender" items="${genders}"/> <br/>

    Department: <form:select path="department" items="${departments}" itemLabel="departmentName" itemValue="id"/> <br/>


    <input type="submit" value="submit"/>
</form:form>


```
注意在使用```<form:form>```标签的时候，modelAttribute="employee"设置的值要和放到map中的键一致。  
```<form:input>```标签中的path对应html中的name属性，支持级联属性。



删除操作
```java

    @RequestMapping(value = "/emp/{id}", method = RequestMethod.DELETE)
    public String delete(@PathVariable(value = "id") Integer id) {
        System.out.println("delete.........." + id);
        employeeDao.delete(id);
        return "redirect:/emps";
    }

```
注意spingmvc对静态资源的过滤，可以加上
```xml

    <mvc:default-servlet-handler/>

```


mvc:annotation-driven

```<mvc:annotation-driven>```会自动注册3个bean：RequestMappingHandlerMapping,
RequestMappingHandlerAdapter,ExceptionHandlerExceptionResolver。   
支持ConversionService实例对表单参数进行类型转换。  
支持使用@NumberFormat、@DateTimeFormat 注解完成数据类型的格式化。   
支持使用 @Valid 注解对 JavaBean 实例进行 JSR 303 验证。  
支持使用 @RequestBody 和 @ResponseBody 注解。  


@InitBinder

由 @InitBinder 标识的方法• ，可以对 WebDataBinder 对
象进行初始化。WebDataBinder 是 DataBinder 的子类，用
于完成由表单字段到 JavaBean 属性的绑定

@InitBinder方法不能有返回值，它必须声明为void 。

@InitBinder方法的参数通常是是 WebDataBinder

```java

    @InitBinder
    public void initBinder(WebDataBinder webDataBinder){
        webDataBinder.setDisallowedFields("lastName");
    }

```

JSR 303验证标准  

* hibernate validator验证框架  
* 在springMVC配置文件中添加   <mvc:annotation-driven/>  


@ResponseBody

这个注意版本问题，有时候版本不对会报错的。
```xml

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

```

```java

    @ResponseBody
    @RequestMapping("/testJson")
    public Collection<Employee> testJson( ) {
        System.out.println("testJson......." );

        return  employeeDao.getAll();
    }

```
使用@ResponseBody，返回值可以直接显示到页面上，使用@RequestBody可以直接得到请求内容。 

```java

    @ResponseBody
    @RequestMapping("/testHttpMessageConverter")
    public String testHttpMessageConverter(@RequestBody String body) {
        System.out.println("testHttpMessageConverter.......\n" + body + "\n\n");

        return "hello world! " + new Date();
    }

```



文件的上传

springmvc.xml中需要配置个 multipartResolver。 这个ID必须是这个名称，如果不是会报错的。
```
    <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
        <property name="defaultEncoding" value="utf-8"/>
        <property name="maxUploadSize" value="2000000"/>
        <property name="maxInMemorySize" value="40960"/>
    </bean>
```

```java

    @RequestMapping("/testFileUpload")
    public String testFileUpload(@RequestParam(value = "desc", required = false) String desc,
                                 @RequestParam(value = "file",required = false) MultipartFile file) {
        System.out.println("testFileUpload.......\n");
        System.out.println(desc);
        System.out.println(file.getOriginalFilename());
        System.out.println(file.getSize());

        return "success";
    }

```

html中的表单需要设置 enctype="multipart/form-data"。

```html

<form action="testFileUpload" method="POST" enctype="multipart/form-data">
    File: <input type="file" name="file"/>
    Desc: <input type="text" name="desc"/>
    <input type="submit" value="Submit"/>
</form>

```




自定义拦截器
```java

public class FirstInterceptor implements HandlerInterceptor {


    /**
     * 该方法在目标方法之前被调用。
     * 若返回是true，则继续调用后续的拦截器和目标方法。
     * 若返回是false，则不会继续调用后续拦截器，也不会调用目标方法了
     *
     * 这个方法可以考虑做权限，日志，事务
     * @param request
     * @param response
     * @param handler
     * @return
     * @throws Exception
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle");
        return true;
    }

    /**
     * 在调用目标方法之后，渲染视图之前被调用
     *
     * 这个方法可以用来对请求域中属性或者视图做出修改。
     *
     * @param request
     * @param response
     * @param handler
     * @param modelAndView
     * @throws Exception
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle");
    }

    /**
     * 渲染视图之后被调用
     *
     * 这个方法可以用来释放资源用的
     *
     * @param request
     * @param response
     * @param handler
     * @param ex
     * @throws Exception
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, @Nullable Exception ex) throws Exception {
        System.out.println("afterCompletion");
    }
}


```

配置多个拦截器
```xml

    <mvc:interceptors>
        <!-- 配置自定义拦截器 -->
        <bean id="firstInterceptor" class="com.mamh.springmvc.demo.interceptor.FirstInterceptor"/>

        <mvc:interceptor>
            <mvc:mapping path="/emps"/>
            <bean class="com.mamh.springmvc.demo.interceptor.SecondInterceptor"/>
        </mvc:interceptor>
    </mvc:interceptors>

```




Redirect, foward区别
```text

1、foward是服务器端控制页面转向，在客户端的浏览器地址中不会显示转向后的地址；sendRedirect则是完全的跳转，浏览器中会显示跳转的地址并重新发送请求链接。

原理：forward是服务器请求资源，服务器直接访问目标地址的URL，把那个URL的响应内容读取过来，然后再将这些内容返回给浏览器，
浏览器根本不知道服务器发送的这些内容是从哪来的，所以地址栏还是原来的地址。

redirect是服务器端根据逻辑，发送一个状态码，告诉浏览器重新去请求的那个地址，浏览器会用刚才的所有参数重新发送新的请求。


```



















