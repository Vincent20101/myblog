
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






















