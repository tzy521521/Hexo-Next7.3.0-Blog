---
title: SpringMVC框架配置
date: 2019-06-14 19:40:13
tags: SpringMVC
categories: 开发
---
# web.xml介绍和版本修改
　　web.xml的模式文件是由Sun公司定义的，每个web.xml文件的根元素<web-app>中，都必须标明这个 web.xml使用的是哪个模式文件。其它的元素都放在<web-app></web-app>之中。
　　web.xml的模式文件中定义的标签并不是定死的，模式文件也是可以改变的，一般来说，随着web.mxl模式文件的版本升级，里面定义的功能会越来越复杂，标签元素的种类肯定也会越来越多，但有些不是很常用的，我们只需记住一些常用的并知道怎么配置就可以了。
　　使用IDEA中创建maven web项目中的web.xml的版本比较低。打开WEB-INF\下的web.xml文件，根据需要更改一下web.xml的版本，可以支持更高级的一些语法，下面是web.xml的3.1版本：
```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">

</web-app>
```
　　可以参考[idea中web.xml默认版本问题解决](https://blog.csdn.net/senar59/article/details/80538821)来解决这个问题。
　　一般的web工程中都会用到web.xml，web.xml主要用来配置Filter、Listener、Servlet等。但是要说明的是web.xml并不是必须的，一个web工程可以没有web.xml文件。
# web工程加载web.xml过程
　　web工程加载的顺序与元素节点在文件中的配置顺序无关。即不会因为 filter 写在 listener 的前面而会先加载 filter。WEB容器的加载顺序是：ServletContext -> context-param -> listener -> filter -> servlet。并且这些元素可以配置在文件中的任意位置。加载过程顺序如下：
<!-- more -->
# web.xml配置
## 配置servlet
　　servlet标签用于指定此Web应用的servlet相关配置，这个配置相当重要。servlet-name标签指定此servlet的名字，servlet-class指定servlet的类，这个类开发者可以自己写，一般会继承HttpServlet类，用来初始化整个Web项目和接受http请求并处理，SpringMVC项目中用DispatcherServlet类(前端控制器)。init-param标签里面可以配置一些参数。load-on-startup标签指定当前Web应用启动时装载Servlet的次序，它的内容必须是整数，当这个数>=0时，容器会按数值从小到大依次加载。如果数值<0或没有指定，容器将在用户首次访问时加载这个servlet类。
　　servlet-mapping标签可定义servlet映射，里面的servlet-name必须与前面的名字一致，url-pattern指定servlet映射的路径。
　　在用Tomcat启动整个web项目时，当配置了load-on-startup标签并且里面的数字>=0时，会加载此servlet类，创建类的实例，调用init()方法初始化init-param标签里面的配置信息，此初始化在整个servlet生命周期中只会进行一次。如果未配置load-on-startup标签或数字<0时，Tomcat启动时不会加载此servlet类，当然也就不会调用init()方法进行初始化，当用户首次访问时会加载类并初始化，所以此时第一次访问时可能会加载很慢。
```xml
    <servlet>
        <servlet-name>mvc-dispatcher</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--定义Servlet的初始化参数(包括参数名和参数值),一个<servlet>元素里可以有多个<init-param>元素。
            contextConfigLocation配置SpringMVC加载的配置文件(配置处理器、映射器和适配器等等)。
            若不配置，默认加载WEB-INF/servlet名称-servlet.xml(mvc-dispatcher-servlet.xml)。
        -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
    <!--<servlet-mapping> 用来定义servlet所对应的URL-->
    <servlet-mapping>
        <servlet-name>mvc-dispatcher</servlet-name>
        <!--指定servlet所对应的URL
            第一种: *.action,访问以.action结尾，由DispatcherServlet进行解析
            第二种: /,所有访问的地址由DispatcherServlet进行解析，对静态文件的解析需要配置不让DispatcherServlet进行解析，
                     使用此种方式和实现RESTful风格的url
            第三种: /*,这样配置不对，使用这种配置，最终要转发到一个jsp页面时，仍然会由DispatcherServlet解析jsp地址，
                     不能根据jsp页面找到handler，会报错
        -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```
## 配置filter
　　filter标签内部配置过滤器，filter-name标签指定此过滤器的名字，filter-class标签指定此过滤器指向的类，此类必须实现javax.servlet.Filter接口。filter-mapping标签用来关联一个或多个servlet或jsp页面。注意无论有多少filter-mapping，他们的filter-name必须与前面的名字一致。
```xml
    <!--为了能够处理中文的post请求，配置一个encodingFilter，以避免post请求中文出现乱码情况-->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
        <init-param>
            <param-name>forceEncoding</param-name>
            <param-value>true</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

# SpringMVC配置文件
　　在classpath下(resources文件夹下)的springmvc.xml中或者web.xml同级目录下的 mvc-dispatcher-servlet.xml中配置
## 注解开发
　　开启springmvc注解模式，由于我们利用注解方法来进行相关定义，可以省去很多的配置：
```xml
    <!--注解映射器 -->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>
    <!--注解适配器 -->
    <bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>
    <!-- 在实际开发，使用<mvc:annotation-driven>代替上边处理器映射器和适配器配置。 -->
    <mvc:annotation-driven/>
```
## 静态资源访问
```xml
    <!-- 静态资源(js、image等)的访问 -->
    <mvc:default-servlet-handler/>
```
## 配置处理器
　　首先加入component-scan标签，指明controller所在的包，并扫描其中的注解
```xml
    <!--指明 controller 所在包，并扫描其中的注解-->
    <context:component-scan base-package="com.gaussic.controller"/>
```
## 视图解析器
```xml
    <!--ViewResolver 视图解析器,用于支持Servlet、JSP视图解析-->
    <bean id="jspViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="viewClass" value="org.springframework.web.servlet.view.JstlView"/>
        <property name="prefix" value="/WEB-INF/pages/"/>
        <property name="suffix" value=".jsp"/>
    </bean>
```
　　关于controller如何找到视图文件，这里需要详细的说明。在 controller 的一个方法中，返回的字符串定义了所需访问的jsp的名字（如上面的index）。在jspViewResolver中，有两个属性，一个是prefix，定义了所需访问的文件路径前缀，另一是suffix，表示要访问的文件的后缀，这里为 .jsp。那么，如果返回字符串是 xxx ，SpringMVC就会找到 /WEB-INF/pages/xxx.jsp 文件。
