# 1 OpenGrok介绍

> OpenGrok 是一个快速, 便于使用的源码搜索引擎与对照引擎, 它能够帮助我们快速的搜索、定位、对照代码树. 接下来就具体讲解一下 OpenGrok 的安装及使用.



# 2 安装OpenGrok

所需依赖Requirements
```
You need the following:

JDK 1.8 or higher  java 版本需要1.8或者更高的

OpenGrok '''binaries''' from https://github.com/OpenGrok/OpenGrok/releases (.tar.gz file with binaries, not the source code tarball !)
下载的包要是二进制的包，不是源码包，源码包你还要编译的。

https://github.com/universal-ctags for analysis (avoid Exuberant ctags, they are not maintained anymore)
需要ctags命令的

A servlet container like GlassFish or Tomcat 8.0 or later also running with Java at least 1.8
需要servlet容器，推荐，也是我们常用的是tomcat，目前可以使用tomcat8了。

If history is needed, appropriate binaries (in some cases also cvs/svn repository) must be present on the system (e.g. Subversion or Mercurial or SCCS or ... )
如果浏览代码索引的时候，想看代码的 历史记录。例如代码是git管理的，你的环境也要安装git的。

2GB of memory for the indexing process (bigger deployments will need more)
内存比较消耗的，代码很多的情况内存要大。要设置-Xmx参数的

a recent browser for clients - IE, Firefox, recent Chrome or Safari
Optional tuning (see https://github.com/oracle/opengrok/wiki/Tuning-for-large-code-bases)
一个客户端浏览器。IE浏览器，火狐浏览器，谷歌浏览器，Safari浏览器。
```

安装JAVA运行环境
```bash
OpenGrok 是基于 JAVA 的, 因此我们首先需要 JDK 和 JRE 来支持其运行

新版本的opengrok需要jdk8了

Latest Java 1.8


```
安装Web服务器-Tomcat
```
#Ubuntu14.04 的源中已经提供了Tomcat 7 的包

sudo apt-get install tomcat7

# 如果是16.04 可以安装tomcat8了。

```

安装OopenGrok
```
从https://github.com/OpenGrok/OpenGrok/releases 下载最新的安装包，注意下载的是二进制包
而不是源码包。解压放到固定位置上。

```

配置OpenGrok
```

```

建立索引Indexing

```bash
indexer.py -J=-Djava.util.logging.config.file=/var/opengrok/logging.properties \
    -a /opengrok/dist/lib/opengrok.jar -- \
    -s /var/opengrok/src -d /var/opengrok/data -H -P -S -G \
    -W /var/opengrok/etc/configuration.xml -U http://localhost:8080
```

# 配置Opengrok 使用ldap登录

以下是需要修改源码，然后编译的。加上我们自己的ldap登录的功能的。
```xml
修改web.xml文件


    <display-name>Blackshark OpenGrok</display-name>

    <description>黑鲨科技 安卓代码</description>

    <context-param>
        <description>Full path to the configuration file where OpenGrok can read its configuration
        </description>
        <param-name>CONFIGURATION</param-name>
        <param-value>/home/opengrok/etc/configuration.xml</param-value>
    </context-param>


    <context-param>
        <param-name>userSessionKey</param-name>
        <param-value>USER_SESSION_KEY</param-value>
    </context-param>

    <context-param>
        <param-name>redirectPage</param-name>
        <param-value>/login.jsp</param-value>
    </context-param>

    <context-param>
        <param-name>unCheckedUrls</param-name>
        <!-- 类似的白名单，哪些url不需要拦截的 -->
        <param-value>
            /login.jsp,/loginServlet,/favicon.ico,/index.jsp
        </param-value>
    </context-param>

    <listener>
        <listener-class>org.opengrok.web.WebappListener</listener-class>
    </listener>

    <filter>
        <filter-name>LoginFilter</filter-name>
        <filter-class>org.opengrok.web.LoginFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>LoginFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```
>主要就是加上一个login的过滤器，通过过滤器来实现权限的认证。  
>这个LoginFilter过滤器要放到其他过滤器前面。他本来默认有个AuthorizationFilterd 过滤器，  
>感觉也是做权限的。但是看过代码并不是我们想要的ldap登录的一个功能。

```java

package org.opengrok.web;


public class LoginFilter implements Filter, FilterConfig, Serializable {

    private static final Logger LOGGER = LoggerFactory.getLogger(LoginFilter.class);
    private String sessionKey;
    private String redirectUrl;
    private String unCheckUrls;
    private List<String> urlList;


    @Override
    public void init(FilterConfig fc) {
        ServletContext servletContext = fc.getServletContext();
        sessionKey = servletContext.getInitParameter("userSessionKey");
        redirectUrl = servletContext.getInitParameter("redirectPage");
        unCheckUrls = servletContext.getInitParameter("unCheckedUrls");
        urlList = Arrays.asList(unCheckUrls.split(","));
    }

    @Override
    public void doFilter(ServletRequest sr, ServletResponse sr1, FilterChain fc) throws IOException, ServletException {
        HttpServletRequest httpReq = (HttpServletRequest) sr;
        HttpServletResponse httpRes = (HttpServletResponse) sr1;
        String servletPath = httpReq.getServletPath();//             /login/login.jsp

        // All RESTful API requests are allowed for now (also see LocalhostFilter).
        // The /search endpoint will go through authorization via SearchEngine.search()
        // so does not have to be exempted here.
        if (servletPath.startsWith(RestApp.API_PATH) ||
                servletPath.startsWith("/default/") ||
                servletPath.startsWith("/js/")) {
            fc.doFilter(httpReq, httpRes);
            return;
        }

        //2. 检查1中获取的servletPath是否在白名单中，在白名单中的就放行，不做过滤处理
        if (urlList.contains(servletPath)) {
            fc.doFilter(httpReq, httpRes);
            return;
        }
        LOGGER.log(Level.INFO, "servletPath=" + servletPath);
        //3. 这里是需要检查的url
        //3.1 从serssion中获取对应的username，这个username存在就说明登陆过了，放行，允许访问。若不存在重定向到login.jsp
        String username = (String) httpReq.getSession().getAttribute(sessionKey);
        if (StringUtils.isEmpty(username)) {
            LOGGER.log(Level.INFO, "Session中usernames 是空的情况");
            httpRes.sendRedirect(httpReq.getContextPath() + redirectUrl);
            return;
        }

        fc.doFilter(httpReq, httpRes);
    }

    @Override
    public void destroy() {
    }

    @Override
    public String getFilterName() {
        return null;
    }

    @Override
    public ServletContext getServletContext() {
        return null;
    }

    @Override
    public String getInitParameter(String name) {
        return null;
    }

    @Override
    public Enumeration<String> getInitParameterNames() {
        return null;
    }
}

```

```java
package org.opengrok.web;

@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    private static final Logger LOGGER = LoggerFactory.getLogger(LoginServlet.class);

    private static final long serialVersionUID = 1L;
    private String sessionKey;

    @Override
    public void init() throws ServletException {
        super.init();
        sessionKey = getServletContext().getInitParameter("userSessionKey");

    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws IOException {
        HttpSession session = request.getSession();
        String username = request.getParameter("username");
        String password = request.getParameter("password");
        if (StringUtils.isEmpty(username) || StringUtils.isEmpty(password)) {
            return;
        }
        Properties env = new Properties();
        String ldapURL = "LDAP://ip:port";//ip:port
        env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
        env.put(Context.SECURITY_AUTHENTICATION, "simple");//"none","simple","strong"
 
        env.put(Context.SECURITY_PRINCIPAL, "公司域名\\" + username);
        env.put(Context.SECURITY_CREDENTIALS, password);
        env.put(Context.PROVIDER_URL, ldapURL);
        try {
            LdapContext ctx = new InitialLdapContext(env, null);
            SearchControls searchCtls = new SearchControls();
            searchCtls.setSearchScope(SearchControls.SUBTREE_SCOPE);
            String searchFilter = "sAMAccountName=" + username;
            String searchBase = "OU=公司群组名,OU=公司群组名,DC=公司域名,DC=com";
            NamingEnumeration<SearchResult> answer = ctx.search(searchBase, searchFilter, searchCtls);
            while (answer.hasMoreElements()) {
                SearchResult sr = answer.next();
                LOGGER.log(Level.INFO, sr.getName());
            }
            LOGGER.log(Level.INFO, "存储在Session中");
            session.setAttribute(sessionKey, username); //存储在Session中

            ctx.close();
        } catch (NamingException e) {
            e.printStackTrace();
            return;
        }

        response.sendRedirect(getServletContext().getContextPath() + "/index.jsp");
        //request.getRequestDispatcher("index.jsp").forward(request, response);
    }
}
>以上是loginServelt的代码，里面有配置公司的ldap ip，然后通过输入过来的账号密码来判断是否登录成功了。  
>登录成功就把用户名放到sesson中。  
>优化的地方 可以把     String ldapURL = "LDAP://ip:port";//ip:port 配置到 web.xml 中。  
>优化的地方 可以把     String searchBase = "OU=公司群组名,OU=公司群组名,DC=公司域名,DC=com"; 配置到 web.xml 中。   

```

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>$Title$</title>
</head>
<body>
<form action="${pageContext.request.contextPath}/loginServlet" method="post">
    <table>
        <tr>
            <td> username</td>
            <td><input type="text" name="username" value="${param.username}"/></td>
        </tr>
        <tr>
            <td> password</td>
            <td><input type="password" name="password" value="${param.password}"/></td>
        </tr>

        <tr>
            <td><input type="submit" value="登陆"/></td>
        </tr>

    </table>
</form>
</body>
</html>

```
