---
title: MyBatis_Generator配置详解
date: 2019-06-27 12:10:50
description: MyBatis_Generator的一些配置
tags: MyBatis
categories: 开发
---
# XML配置详解
　　在MBG中，最主要也最重要的就是XML配置文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE generatorConfiguration PUBLIC
        "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd" >
<generatorConfiguration>
    <!-- <generatorConfiguration>(有严格的配置顺序) 下可以配置3种子元素<properties>、<classPathEntry> 和<context>
         (特别注意: 该配置文件只能放在classpath目录下，放在classpath下的任何包中都不可以。)
    -->
    
    <!-- <properties>      可选，最多配置1个
         该元素用于指定一个需要在配置中解析使用的外部属性文件。配置中的任何属性将接受${property}这种形式的属性。从指定的属性文件中搜索匹配的值，配置的值将会被替换。属性文件时普通的Java属性文件的格式。
         该元素有2个必选属性(但是只能选择一个)：
                 resource：属性文件的全限定名称。当指定了resource属性，将会从classpath下面搜索属性文件。当指定为com/myproject/generatorConfig.properties时，它必须存在com.myproject 包下面。
                 url：     属性文件的URL值。这可以用于指定一个属性文件在文件系统上的特定位置，例如 file:///C:/myfolder/generatorConfig.properties。
     -->
    <properties resource="properties/jdbc.properties"/>

    <!-- <classPathEntry>  可选，可配置多个(Maven项目中不建议使用,Maven项目中在mybatis-generator-maven-plugin中添加依赖)
         该元素用于将MBG运行时需要的jar包(或者zip格式)添加到classpath下。MBG在以下情况下从这些路径中加载类：
                 当为映射的数据库加载JDBC驱动时
                 当加载<JavaModelGenerator>中的根类以便检查重写的方法时
         该元素有1个必选元素：
                 location：要添加到classpath中的JAR/ZIP文件的完整路径名称或目录。
         <classPathEntry location="E:\mysql\mysql-connector-java-5.1.29.jar"/>
     -->

    <!-- <context>         必选，最少配置一个
         该元素用于指定生成一组对象的环境。子元素用于指定要连接到的数据库、要生成模型的类型和要处理的数据库中的表。多个<context>元素可以在<generatorConfiguration>元素中列出来，这样可以在同一个MBG中从不同的数据库或者使用不同的生成生成器参数生成对象。
         该元素有1个必选属性：
                 id：      用来唯一确定一个<context>元素。该值将用于某些错误的信息。(注意： 运行MBG的时候还可以指定要运行的<context>)。
         该元素有3个可选属性：
                 defaultModelType： 这个属性用来设置生成模型类型的默认值。模型类型定义了MBG如何生成领域类。对某些模型类型，MBG会给每一个表生成一个单独的领域类。对另外一些模型类型，MBG会根据表结构生成不同的类。这个属性有以下可选值：
                         conditional：  默认值。这个模型和hierarchical模型类似，除了如果那个单独的类将只包含一个字段，将不会生成一个单独的类。因此,如果一个表的主键只有一个字段,那么不会为该字段生成单独的实体类,会将该字段合并到基本实体类中。
                         flat：         该模型为每一张表只生成一个实体类。这个实体类包含表中的所有字段。
                         hierarchical： 如果表有主键,那么该模型会产生一个单独的主键实体类,如果表还有BLOB字段，则还会为表生成一个包含所有BLOB字段的单独的实体类，然后为所有其他的字段生成一个单独的实体类。MBG会在所有生成的实体类之间维护一个继承关系(注：BLOB类 继承 其他字段类 继承 主键类)。
                 targetRuntime：     此属性用于指定生成的代码的运行时环境（~java模型生成器~<javaModelGenerator>）。该属性支持以下可选值：
                         MyBatis3：  默认值。使用这值的时候，MBG生成的对象会生成兼容MyBatis 3.0或更高版本，兼容JSE5.0或更高版本的对象（例如，Java模型类和Mapper接口会使用泛型）。这些生成对象中的"by example"方法将支持几乎不受限制的动态的where子句。另外，这些生成的Java对象支持更多的JSE5.0特性，包含泛型和注解。
                         MyBatis3Simple：    使用这值的时候，和MyBatis3类似，除了生成的mapper仅仅具有非常基本的CRUD操作，但是不会有"by example"一类的方法和只有少量的动态SQL。
                         MyBatis3DynamicSql：使用这值的时候，MBG生成的对象会生成兼容MyBatis 3.4.2或更高版本，兼容Java8或者更高版本（例如，Java模型类和Mapper接口会使用泛型和其他Java8的特性像接口中的默认方法）。
                                                            注意：生成的Java代码运行时需要依赖 "MyBatis Dynamic SQL"支持的jar包。
                                                            其他：defaultModelType值为flat时，意味着没有“with BLOBs”和“with BLOBs”方法。
                                                                  生成mapper作为注解mapper,不管<javaClientGenerator>的设置的类型是何值。
                                                                  没有XML文件生成。<sqlMapGenerator>不是必须的，并且如果配置了会被忽略。
                                                                  MyBatis Dynamic SQL 在运行时支持表注释，在每个查询方式而不是所有的方式。基于这个原因，<table>中的<alias>设置会被忽略。
                         Ibatis2Java2：      使用这值的时候，MBG会生成兼容iBATIS 2.2.0或更高版本（除了iBATIS 3），兼容还有Java2。 这些生成对象中的"by example"方法将支持几乎不受限制的动态的where子句。这些生成的对象不能100%和原生的Abator或其他的代码生成器生成的对象兼容。
                         Ibatis2Java5：      使用这值的时候，MBG会生成兼容iBATIS 2.2.0或更高版本（除了iBATIS 3），兼容JSE 5.0或更高版本的对象（例如Java模型类和Dao类会使用泛型）。这些生成对象中的"by example"方法将支持几乎不受限制的动态的where子句。 另外，这些生成器生成的Java对象支持JSE 5.0特性，包含泛型和注解。 这些生成的对象不能100%和原生的Abator或其他的代码生成器兼容。
                         如果您想创建一个完全不同的代码生成器，使用一个继承了org.mybatis.generator.api.IntrospectedTable接口的全限定类名替换该值。通过这个值，您可以创建您自己的代码生成器，然后插入到代码生成器引擎中。查阅 Extending MyBatis Generator页面获取更多信息。
                 introspectedColumnImpl：使用这个值去指定一个继承了org.mybatis.generator.api.IntrospectedColumn接口的全限定类名称。这可以修改代码生成器计算列信息时候的行为。 查阅 Extending MyBatis Generator页面获取更多信息。
    -->
    <context id="context" targetRuntime="MyBatis3Simple">
        <!-- <context>（有严格的配置顺序）下可以配置9种子元素<property>、<plugin>、<commentGenerator>、<jdbcConnection>、<javaTypeResolver>、<javaModelGenerator>、<sqlMapGenerator>、<javaClientGenerator>、<table>-->

        <!-- <property>           可选，在<context>下可以配置0个或者多个。支持的属性有autoDelimitKeywords、beginningDelimiter、endingDelimiterjava、FileEncoding、javaFormatter、xmlFormatter-->
        <!-- autoDelimitKeywords：默认值是false。如果是true，那么MGB会分隔SQL关键字，如果他们被用作表中的列名。 MBG维护了许多不同数据库的的SQL关键字列表。 然而，列表可能不是非常的全面。如果一个特殊的关键字没有在MBG的关键字列表中。您需要通过<columnOverride>(table元素中涉及到)强制分隔列。查看org.mybatis.generator.internal.db.SqlReservedWords这个类的源码查看MGB包含的关键字列表。 -->
        <property name="autoDelimitKeywords" value="true"/>
        <!-- beginningDelimiter： 默认值是双引号 (")。用作需要分隔符的SQL标识符开头的标识符分隔符的值。MBG会自动分割包含空格的SQL标识符。MGB还会自动分隔在 <table> 或 <columnOverride>中配置了具体要求的标识符。在Mysql中需要这两个默认值改为反单引号(`) -->
        <!-- endingDelimiter：    默认值是双引号 (")。用作需要分隔符的SQL标识符结束的标识符分隔符的值。MBG会自动分割包含空格的SQL标识符。MGB还会自动分隔在 <table> 或 <columnOverride>中配置了具体要求的标识符。在Mysql中需要这两个默认值改为反单引号(`) -->
        <property name="beginningDelimiter" value="`"/>
        <property name="endingDelimiter" value="`"/>
        <!-- javaFileEncoding：   设置要使用的Java文件的编码。新生成的Java文件会用这个编码写入到文件系统，如果Java文件已经存在并且执行合并操作，将使用这个编码进行读取。如果没有指定，就会使用平台的默认编码。查阅 java.nio.charset.Charset 获取可以选择的编码的信息。  -->
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 最后两个javaFormatter和xmlFormatter属性可能会很有用，如果你想使用模板来定制生成的java文件和xml文件的样式，你可以通过指定这两个属性的值来实现。 -->
        <!-- javaFormatter 使用此属性来指定用户提供全限定类名用于生成Java文件。这个类必须继承 org.mybatis.generator.api.JavaFormatter 而且必须有一个默认（不含参数）的构造方法。每一个context都持有一个单独的javaFormatter的实例。默认的javaFormatter是org.mybatis.generator.api.dom.DefaultJavaFormatter。 默认的格式使用内置的Java DOM类的格式。  -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
        <!-- xmlFormatter  使用此属性来指定用户提供全限定类名用于生成XML文件。 这个类必须继承 org.mybatis.generator.api.XmlFormatter  而且必须有一个默认（不含参数）的构造方法。每一个context都持有一个单独的xmlFormatter的实例。 默认的xmlFormatter 是org.mybatis.generator.api.dom.DefaultXmlFormatter。  默认的格式使用内置的xml DOM类的格式。  -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>

        <!-- <plugin>             元素可以配置0个或者多个，不受限制。
             该元素用来定义一个插件。插件用于扩展或修改通过MBG生成的代码。插件将按在配置中配置的顺序执行。
                   关于插件开发的信息，   请参考Implementing Plugins页面。
                   关于MBG提供的插件信息，请参考Supplied Plugins页面。
             该元素有1个必选属性：
                     type： 实现插件的类的完全限定名。该类必须实现该接口 org.mybatis.generator.api.Plugin, 必须有一个公开默认的构造函数。注意，继承org.mybatis.generator.api.PluginAdapter这个适配器类比继承接口更容易扩展。
        -->

        <!-- <commentGenerator>  可选，在<context>下最多配置一个。
            该元素用来定义~注释生成器~的属性。~注释生成器~是用来给通过MBG生成的多种元素（Java属性，Java方法，XML元素等等）生成注释。默认的~注释生成器~将JavaDoc注释添加到所有生成的Java元素上，从而使Eclipse插件可以使用合并Java的功能。 此外，注释还会添加到每一个生成的XML元素中。 注释的目的还在于告诉用户这个元素是生成的，并且有可能重新生成（也就是说 - 他们不应该被修改）。
            默认的实现是org.mybatis.generator.internal.DefaultCommentGenerator，默认的实现被设计成可扩展的，如果你想修改某些行为。
            该元素有1个可选属性：
                    type：可以指定用户的实现~注释生成器类~。这个类必须继承org.mybatis.generator.api.CommentGenerator接口。而且必须有一个默认的公共的构造函数。
                          这个属性接收默认的特殊值DEFAULT，会使用默认的实现类org.mybatis.generator.internal.DefaultCommentGenerator（这和不指定该属性的效果一样）。
        -->
        <commentGenerator>
            <!--
                默认的~注释生成器~提供了几个可以用<property>设置的属性：
                suppressAllComments： 这个属性用来指定MBG生成的代码中是否包含任何注释。这个属性有以下可选值：
                        false：   默认值。当这个属性是false或者没有指定时，所有生成的元素都会包含用来说明这是这些元素是生成的元素。
                        true：            当这个属性是true时，不会往生成的元素中添加任何注释。警告：如果suppressAllComments值设为true，那么所有的代码合并都会被禁用。
                suppressDate：        这个属性用来指定MBG生成的注释中是否包含生成的时间戳。这个属性有以下可选值：
                        false:    默认值。当这个属性是false或者没有指定时，所有元素生成注释时都会带着生成时间。
                        true:             当这个属性是true时，注释中不会添加时间戳。
                        警告，如果suppressAllComments值是true,这个选项将会被忽略
                addRemarkComments：   这个属性用来指定MBG生成的代码中是包含数据库中表和列注释信息。这个属性有以下可选值：
                        false：    默认值,当这个属性是false或者没有指定时，所有元素生成注释时都不会包含数据库中表和字段注释。
                        true：            当这个属性是true时，数据库中表和字段注释将会添加到生成的注释中。
                        警告，如果suppressAllComments值是true,这个选项将会被忽略并且1.3.3以上版本有效。
                dateForma：           将日期写入生成的注释时要使用的日期格式字符串。这个字符串将用于构造java.text.SimpleDateFormat对象。这个对象的任何有效的字符串形式都可以在这里指定。默认情况下，日期字符串将使用来自java.util.Date上的toString()方法。
                        警告，如果suppressAllComments或者suppressDate的值是true,这个选项将会被忽略并且1.3.4以上版本有效。
            -->
            <property name="addRemarkComments" value="true"/>
            <property name="dateFormat" value="该注释于yyyy年MM月dd日HH时mm分ss秒生成"/>
        </commentGenerator>

        <!-- <jdbcConnection>    必选，在<context>下只能有1个。
             该元素用于指定需要去映射表的数据库连接属性。MBG使用JDBC的数据库元数据类来发现您在配置中指定的表的属性。
             （应该是使用默认的实现类org.mybatis.generator.internal.JDBCConnectionFactory）
             该元素有2个必选属性：
                     driverClass：    访问数据库的JDBC驱动程序的完全限定类名
                     connectionURL：  访问数据库的JDBC连接URL
             该元素有2个可选属性：
                     userId：         访问数据库的用户ID
                     password：       访问数据库的密码
        -->
        <jdbcConnection userId="${jdbc.username}" password="${jdbc.password}" driverClass="${jdbc.driver}" connectionURL="${jdbc.url}">
            <!--此外该元素还可以接受多个<property>子元素，这里配置的<property>属性都会添加到JDBC驱动的属性中。-->
        </jdbcConnection>
        <!-- <connectionFactory>   和<jdbcConnection>二选一
             该元素用于指定需要映射表的数据库连接的连接工厂。MBG使用JDBC的数据库元数据类来发现您在配置中指定的表的属性。
             每一个<context>都必须有一个<jdbcConnection>元素或者<connectionFactory>元素。
             该元素有1个可选属性：
                     type： 用于指定用户提供的数据库连接工厂，这个类必须继承org.mybatis.generator.api.ConnectionFactory接口。而且必须有一个默认的公共的构造函数。
                            这个属性接收默认的特殊值DEFAULT，会使用默认的实现类org.mybatis.generator.internal.JDBCConnectionFactory（这和不指定该属性的效果一样）。
        IDEA中会有红色的提示，但是没有错。
        <connectionFactory>
            <property name="driverClass" value="${jdbc.driver}"/>
            <property name="connectionURL" value="${jdbc.url}"/>
            <property name="userId" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
        </connectionFactory>
        -->

        <!-- <javaTypeResolver>  可选，在<context>下最多可以配置一个。
             该元素用来定义~Java类型转换器~。~Java类型转换器~用来计算数据库中列信息的Java类型。默认的~Java类型转换器~试图将JDBC的DECIMAL和NUMERIC类型转换成用于使用的整数类型（Long, Integer, Short等）。如果这种行为不可取，可以将属性forceBigDecimals设置为true。如果你想要不同的默认行为，可以替换成自己实现的类。
             该元素有1个可选属性：
                     type：可以指定用户实现的~Java类型转换器~。这个类必须继承org.mybatis.generator.api.JavaTypeResolver接口。而且必须有一个默认的公共的构造函数。
                           这个属性接收默认的特殊值DEFAULT，会使用默认的实现类org.mybatis.generator.internal.types.JavaTypeResolverDefaultImpl（这和不指定该属性的效果一样）。
         -->
        <javaTypeResolver>
            <!-- 默认的实现类中提供了1个可选属性：
                 forceBigDecimals: 该属性用来指定MBG是否强制将DECIMAL和NUMERIC类型的字段转换为Java类型的java.math.BigDecimal而不是简单的整数类型。有以下2个可选值：
                      false：默认值，转换规则为：
                             如果精度>0或者长度>18，就会使用java.math.BigDecimal
                             如果精度=0并且10<=长度<=18，就会使用java.lang.Long
                             如果精度=0并且5<=长度<=9，就会使用java.lang.Integer
                             如果精度=0并且长度<5，就会使用java.lang.Short
                      true： 一定会使用java.math.BigDecimal
             -->
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>

        <!-- <javaModelGenerator>必选，在<context>下只能有1个。
             该元素用于定义~java模型生成器~的属性。~java模型生成器~创建主键类、纪录类和 by Example查询类来匹配映射的表。
             ~java模型生成器~根据IntrospectedTable(IntrospectedTable是MBG提供的一个比较基础的扩展类)来创建主键类、纪录类和 by Example查询类。
             在MBG中，提供了几种默认的IntrospectedTable的实现，其实在context上设置的targetRuntime对应的就是不同的IntrospectedTable的实现，下面就是几种runtime和对应的IntrospectedTable关系：
             MyBatis3 (默认)：org.mybatis.generator.codegen.mybatis3.IntrospectedTableMyBatis3Impl
             MyBatis3Simple： org.mybatis.generator.codegen.mybatis3.IntrospectedTableMyBatis3SimpleImpl
             Ibatis2Java2：   org.mybatis.generator.codegen.ibatis2.IntrospectedTableIbatis2Java2Impl
             Ibatis2Java5：   org.mybatis.generator.codegen.ibatis2.IntrospectedTableIbatis2Java5Impl
             注意，其中的IntrospectedTableMyBatis3Impl和IntrospectedTableMyBatis3SimpleImpl两个类是值得我们在自己扩展前仔细阅读的；当扩展了自己的IntrospectedTable之后，就可以在context的targetRuntime中配置自己的实现类的全限定名即可。
             该元素有2个必选属性：
                     targetPackage：生成实体类存放的包名，一般就是放在该包下。实际还会受到其他配置的影响(<table>中会提到)。
                     targetProject：指定目标项目路径，可以是绝对路径或相对路径（如 targetProject="src/main/java"）。
        -->
        <javaModelGenerator targetPackage="com.tzy777.testssm.entity" targetProject="src/main/java">
            <!-- 该元素支持以下5个<property>子元素属性：
                 constructorBased:  默认为false。该属性只对MyBatis3有效，如果true就会使用构造方法入参，如果false就会使用setter方式。这个属性会被<table>中的<corresponding>属性重写。
                 enableSubPackages: 默认为false。如果true，MBG会根据catalog和schema来生成子包。如果false就会直接用targetPackage属性。
                 immutable:         默认为false。该属性只对MyBatis3有效，用来配置实体类属性是否可变，如果设置为true，那么constructorBased不管设置成什么，都会使用构造方法入参，并且不会生成setter方法。如果为false，实体类属性就可以改变。该属性可以被<table>中的 <corresponding>属性重写。
                 rootClass:         设置所有实体类的基类。如果设置，需要使用类的全限定名称。并且如果MBG能够加载rootClass，那么MBG不会覆盖和父类中完全匹配的属性。匹配规则：属性名完全相同、属性类型相同、属性有getter方法、属性有setter方法
                 trimStrings:       默认值为false。是否对数据库查询结果进行trim操作，如果设置为true就会生成类似这样public void setUsername(String username) {this.username = username == null ? null : username.trim();}的setter方法。可以被<table>或者<columnOverride>重写。
             -->
            <property name="enableSubPackages" value="false"/>
            <property name="immutable" value="false"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- <sqlMapGenerator>   可选，该元素最多可以配置一个。
             该元素被用于定义~SQL map生成器~属性。~SQL map生成器~为每一个映射的表创建一个MyBatis/iBATIS格式的SQL map的XML文件。
             如果targetRuntime是iBATIS2，该元素必须配置一个。
             如果targetRuntime是MyBatis3，只有当<javaClientGenerator>需要XML时，该元素必须配置一个。
             如果没有配置<javaClientGenerator>，则使用以下的规则：
                     如果指定了一个<sqlMapGenerator>，那么MBG将只生成XML的SQL映射文件和实体类。
                     如果没有指定<sqlMapGenerator>，  那么MBG将只生成实体类。
             该元素有2个必选属性：（和前面提过的<javaModelGenerator>的属性含义一样）。
                     targetPackage:生成SQL Map存放的包名，一般就是放在该包下。实际还会受到其他配置的影响(<table>中会提到)。
                     targetProject:指定目标项目路径，可以是绝对路径或相对路径（如 targetProject="src/main/resources"）。
         -->
        <sqlMapGenerator targetPackage="config/mapper" targetProject="src\main\resources">
            <!-- 该元素支持1个<property>子元素：
                 enableSubPackages：  默认为false。如果true，MBG会根据catalog和schema来生成子包。如果false就会直接用targetPackage属性。
             -->
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>

        <!-- <javaClientGenerator>可选，最多配置一个。
             该元素该元素被用于定义~Java client生成器~属性。Java client生成器创建java接口和类，便于使用java model和XML map文件。
             如果targetRuntime是iBATIS2，这些生成的对象采用DAO接口和实现类的形式
             如果targetRuntime是MyBatis，生成的对象采用mapper接口的形式。
             如果不配置该元素，就不会生成Mapper接口。
             该元素有3个必选属性：
                   targetPackage：用于存放生成接口的包名，一般就是放在该包下。
                   targetProject：指定目标项目路径，可以是绝对路径或相对路径（如 targetProject="src/main/java"）。
                   type:可以指定用户的实现~Java client生成器类~。这个类必须继承org.mybatis.generator.codegen.AbstractJavaClientGenerator接口。而且必须有一个默认的公共的构造函数。
                        <context> targetRuntime 是 MyBatis3:
                                type="XMLMAPPER",       生成的对象是 MyBatis 3.x 映射器基础结构的 Java接口。 这些接口将会依赖于生成的XML文件。
                                type="ANNOTATEDMAPPER", 生成的对象是 MyBatis 3.x 映射器基础结构的 Java接口。 这些接口将会基于注解和 MyBatis 3.x SqlProviders。不会有XML文件生成。（注：就是纯接口使用注解的形式，不会有XML文件）。                  依赖 MyBatis 3.0.4 或更高版本。
                                type="MIXEDMAPPER",     生成的对象是 MyBatis 3.x 映射器基础结构的 Java接口。 这些接口将基于注解和XML的混合形式。 注解将会用在简单注解可以实现的地方。 此客户端（接口中）不会生成SqlProvider，所有复杂的动态SQL都会生成在XML中。依赖 MyBatis 3.0.4 或更高版本。
                        <context> targetRuntime 是 MyBatis3Simple::
                                type="XMLMAPPER",       生成的对象是 MyBatis 3.x 映射器基础结构的 Java接口。 这些接口将会依赖于生成的XML文件。
                                type="ANNOTATEDMAPPER", 生成的对象是 MyBatis 3.x 映射器基础结构的 Java接口。 这些接口将会基于注解和 MyBatis 3.x SqlProviders。不会有XML文件生成。（注：就是纯接口使用注解的形式，不会有XML文件）。                  依赖 MyBatis 3.0.4 或更高版本。
                        <context> targetRuntime 是 Ibatis2Java2 or Ibatis2Java5::
                                type="IBATIS",          生成的对象符合iBATIS的DAO框架（不建议使用）。
                                type="GENERIC-CI",      生成的对象将只依赖于SqlMapClient，通过构造方法注入。生成的对象是DAO接口和实现类。
                                type="GENERIC-SI",      生成的对象将只依赖于SqlMapClient，通过setter方法注入。生成的对象是DAO接口和实现类。
                                type="SPRING"           生成的对象符合Spring的DAO接口
              该元素有1个可选属性：
                    implementationPackage:如果指定了该属性，实现类就会生成在这个包中。这默认的生成器中属性（property）enableSubPackages控制着如何去创建包。
        -->
        <javaClientGenerator targetPackage="com.tzy777.testssm.dao" type="XMLMAPPER" targetProject="src/main/java">
            <!--该元素支持以下5个<property>子元素属性：
                    enableSubPackages：       默认值是 false。这个属性用来选择MGB是否根据catalog和schema的introspected表来生成不同的Java包。例如，假设某个表MYTABLE在MYSCHMA的schema中，也假设targetPackage属性的值设置为"com.mycompany"。 如果此属性为true，为这个表生成的DAO接口和类将被放置在包"com.mycompany.myschema"中。 如果此属性是false，生成的SQL映射将被放在"com.mycompany" schema中。
                    exampleMethodVisibility： 此属性用来设置不同"ByExample"方法的可见性 - selectByExample, deleteByExample等等。 如果没有指定，这些方法将会是公开的的，并将在接口中声明。 此属性使您可以隐藏这些方法如果您只想使用它们来执行其他专门的方法。如果目标运行环境是MyBatis3，则将忽略此属性。
                        public：   默认值，生成实现类中的方法将是公开的，并将该接口中声明的方法。
                        private：          生成实现类中的方法将是private的，也不会在接口中声明。
                        protected：        生成实现类中的方法将是protected的，也不会在接口中声明。
                        default：          生成实现类中的方法将是private的，也不会在接口中声明。
                    methodNameCalculator：    此属性用于设置一个~方法名称计算器~。 ~方法名称计算器~可以用来为DAO方法提供不同的名称。 您可以选择一个预定义的值。 如果提供的选项都适合您的环境， 也可以指定一个实现了org.mybatis.generator.api.DAOMethodNameCalculator接口的完全限定名称。如果目标运行环境是MyBatis3，则将忽略此属性。
                               default这是默认值生成的方法名称会很简单("insert", "updateByPrimaryKey"等等)。
                               extended 生成的方法名称将包括与方法关联的实体对象的名称("insertWidget", "updateWidgetByPrimaryKey"等等)。
                    rootInterface：           此属性用于指定一个所有生成的接口都继承的父接口。 这个值可以通过<table>配置的 rootInterface 属性覆盖。重要: MBG 不验证接口是否存在或者是否是一个有效的接口。如果指定，这个属性值应该是一个全限定接口名称(例如 com.mycompany.MyRootInterface)。
                    useLegacyBuilder，        默认值是 false。如果为true，那么注解客户端（接口）将使用SqlBuilder为MyBatis生成动态SQL。 在MyBatis 3.2或以后版本，这个SqlBuilder被废弃了，用了一个新的SQL类替代。 如果是false，MBG会使用新的SqlBuilder生成客户端代码。
            -->
            <property name="enableSubPackages" value="false"/>
            <!--如果目标运行环境是MyBatis3，则将忽略此属性。-->
            <property name="methodNameCalculator" value="extended"/>
        </javaClientGenerator>

        <!-- <table>              必选，该元素至少要配置一个，可以配置多个。
             该元素用来从映射的数据库中选择一个表。选定的表会生成如下的这些对象：
             一个 MyBatis/iBATIS 格式的SQL映射文件
             一组根据“model”生成的表的类：
                     一个和表的主键匹配的类(如果表有主键)。
                     一个包含了非主键字段和非BLOB字段的类，如果存在主键类，这个类就会继承主键类。
                     如果表中包含BLOB字段，就会有一个包含了BLOB字段的类。这个类会根据表的配置继承上面的一个或两个类。
                     一个用于不同的"by example"方法(selectByExample, deleteByExample)生成动态查询条件的类。
             (可选) 一个DAO接口或者类
             数据库标识符：
             该元素有1个必选的属性：
                   tableName: 数据库表的名称(不包括schema或catalog)。如果需要，指定的值可以包含SQL通配符。
             该元素18个可选属性：
                   schema：    数据库schema，如果你的数据库不使用schema，或者有一个默认的schema您不需要设置schema。如果需要，指定的值可以包含SQL通配符。如果设置了该值，生成SQL的表名会变成如schema.tableName的形式。
                   catalog：   数据库catalog，如果您的数据库不使用 catalog，或者有一个默认的catalog，您就不需要设置catalog。如果设置了该值，生成SQL的表名会变成如catalog.tableName的形式。
                   alias：     如果指定，这个值会用在任何生成的SQL查询语句的表的别名和列名上。列名会被别名化，用alias_actualColumnName(别名_实际列名)这种模式。
                   domainObjectName：生成对象的基本名称。如果没有指定，MBG会自动根据表名来生成名称。
                   mapperName：     MyBatis3生成的mapper       类和XML文件名称，如果没有指定，无论domain object name是什么，都会在后面加上"Mapper"。     版本1.3.4以上有效。
                   sqlProviderName：MyBatis3生成的SQL provider 类，             如果没有指定，无论domain object name是什么，都会在后面加上"SqlProvider"。版本1.3.4以上有效。
                   enableXXX： XXX代表多种SQL方法，共有10个。该属性用来指定是否生成对应的XXX语句。
                   selectByPrimaryKeyQueryId: 这个值会以"'<value>' as QUERYID"这种形式被添加到通过   主键查询的语句的select列中。 这可能对在运行时的DBA跟踪工具中标记查询有用。 如果您使用这个值，您需要为MBG生成的每一个查询指定一个唯一的id。
                   selectByExampleQueryId:    这个值会以"'<value>' as QUERYID"这种形式被添加到通过Example查询的语句的select列中。 这可能对在运行时的DBA跟踪工具中标记查询有用。 如果您使用这个值，您需要为MBG生成的每一个查询指定一个唯一的id。
                   modelType:如果您需要，这个值可以用来重写默认的模型类型。如果没有指定，MBG将会生成基于<context>的defaultModelType值的实体对象。模型类型定义了MBG如何生成实体类。有一些模型类型MGB会为每个表生成一个单独的实体类。 另外一些模型，MGB会根据表结构生成不同的一些类。 这个属性有以下可选值：
                             hierarchical :如果表有主键,那么该模型会产生一个单独的主键实体类,如果表还有BLOB字段,则会为表生成一个包含所有BLOB字段的单独的实体类,然后为所有其他的字段生成一个单独的实体类。MBG会在所有生成的实体类之间维护一个继承关系（注：BLOB类 继承 其他字段类 继承 主键类）。
                             conditional  :这个模型和hierarchical类似，除了如果那个单独的类将只包含一个字段，将不会生成一个单独的类。 因此,如果一个表的主键只有一个字段,那么不会为该字段生成单独的实体类,会将该字段合并到基本实体类中。
                             flat         :该模型为每一张表只生成一个实体类。这个实体类包含表中的所有字段。
                   escapeWildcards：    默认false。这个属性表示当查询列时，是否对schema和表名中的SQL通配符 ('_' and '%') 进行转义。 对于某些驱动当schema或表名中包含SQL通配符时（例如，一个表名是MY_TABLE，有一些驱动需要将下划线进行转义）是必须的。
                   delimitIdentifiers： 默认false。是否给标识符增加分隔符。但是当catalog,schema或tableName中包含空白时，默认为true
                   delimitAllColumns：  默认false。是否对所有列添加分隔符。 这是一种给每个列添加<columnOverride>来指定列需要被分隔的替代方式。 这对类似PostgreSQL这种使用小写标识符的数据库很有用。分隔符在 <context> 元素上指定。
        -->
        <table tableName="%" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false">
            <!-- 该元素包含13个可用的<property>子元素，可选属性为：
                 constructorBased:            默认值是false。此属性用于选择代码生成器是否生成接受类中的每个字段的值的类的构造函数。此外，SQL结果映射也会为每个字段生成使用构造方法而不是"setter"的方式。 和<javaModelGenerator>中的属性含义一样。
                                              此属性仅适用于MyBatis3，iBATIS2将会被忽略。如果"immutable"属性设置为"true"，这个属性将会被忽略。
                 ignoreQualifiersAtRuntime:   默认值是false。如果设置为true，MBG不会将schema或catalog添加到生成SQL中的表名上。如果您有几个schemas中的表具有相同的名称，这是非常有用的。您可以使用MBG生成基于在一个schema上的表，但是运行时不包含schema。
                 immutable:                   和<javaModelGenerator>中的属性含义一样。
                 modelOnly:                   此属性用于配置是否为表只生成实体类。如果设置为true就不会有Mapper接口。如果配置了<sqlMapGenerator>，并且modelOnly为true，那么MBG只会为表在SQL XML映射文件中生成实体对象的映射元素(<resultMap>)。如果为true还会覆盖属性中的enableXXX方法，将不会生成任何CRUD方法。
                 rootClass:                   和<javaModelGenerator>中的属性含义一样。
                 rootInterface:               和<javaClientGenerator>中的属性含义一样。
                 runtimeCatalog:              如果你给这个属性设定了值，那么BGM用会用这个Catalog在生成的SQL语句中而不是使用之前设定的值。这是非常有用的。
                 runtimeSchema:               如果你给这个属性设定了值，那么BGM用会用这个Schema在生成的SQL语句中而不是使用之前设定的值。这是非常有用的。
                 runtimeTableName：           如果你给这个属性设定了值，那么BGM用会用这个tableName在生成的SQL语句中而不是使用之前设定的值。这是非常有用的，。
                 selectAllOrderByClause:      该属性值会追加到selectAll方法后的SQL中，会直接跟order by拼接后添加到SQL末尾。这个值只有在MyBatis3Simple时才有效
                 trimStrings                  和<javaModelGenerator>中的属性含义一样。这个默认的值从 <javaModelGenerator>继承下来，否则为false。
                 useActualColumnNames:        如果设置为true,那么MBG会使用从数据库元数据获取的列名作为生成的实体对象的属性。 如果为false(默认值)，MGB将会尝试将返回的名称转换为驼峰形式。 在这两种情况下，可以通过<columnOverride>元素显示指定，在这种情况下将会忽略这个（useActualColumnNames）属性。
                 useCompoundPropertyNames:    默认是false。如果是true,那么MBG生成属性名的时候会将列名和列备注接起来. 这对于那些通过第四代编程语言自动生成列(例如:FLD22237),但是备注包含有用信息(例如:"customer id")的数据库来说很有用.在这种情况下,MBG会生成属性名FLD2237_CustomerId。
            -->


            <!-- 除了<property>子元素外，<table>还包含以下子元素：
                 1.<generatedKey>元素,这个元素最多可以配置一个。这个元素用来指定从（identity字段或者sequences序列）自动生成主键的值。如果指定这个元素，MBG在生成insert的SQL映射文件中插入一个<selectKey>元素。
                        这个元素非常重要，这个元素包含下面2个必选属性：
                               column:       生成列的列名。
                               sqlStatement: 将返回新值SQL语句。如果这是一个identity列，您可以使用其中一个预定义的的特殊值。预定义值如下：
                                            Cloudscape、DB2、DB2_MF、Derby、HSQLDB、Informix、MySql、SqlServer、SYBASE、
                                            JDBC:这会配置MBG使用MyBatis3支持的JDBC标准的来生成主键。这是一个独立于数据库获取标识列中的值的方法。 重要: 只有当目标运行为MyBatis3时才会产生正确的代码。 如果与iBATIS2一起使用目标运行时会产生运行时错误的代码。
                        这个元素包含下面2个必选属性：
                               identity:   默认值是false。当设置为true时,该列会被标记为identity列，并且<selectKey>元素会被插入在insert后面。当设置为false时，<selectKey>会插入到insert之前（通常是序列）。重要: 即使您type属性指定为post，您仍然需要为identity列将该参数设置为true。 这将标志MBG从插入列表中删除该列。
                               type:      type=post and identity=true的时候生成的<selectKey>中的order=AFTER,当type=pre的时候，identity只能为false，生成的<selectKey>中的order=BEFORE。可以这么理解，自动增长的列只有插入到数据库后才能得到ID，所以是AFTER,使用序列时，只有先获取序列之后，才能插入数据库，所以是BEFORE。
                               https://blog.csdn.net/u014365133/article/details/78884751有不同的配置对应的生成的结果。
                  2.<columnRenamingRule> 该元素最多配置一个
                       MBG使用<columnRenamingRule>元素在生成映射表中相应的属性之前可以对列进行重命名。这对那些存在同一前缀的字段想在生成属性名时去除前缀的表非常有用。
                       例如假设一个表包含以下的列：CUST_BUSINESS_NAME，CUST_STREET_ADDRESS，CUST_CITY，CUST_STATE
                       如果生成的所有属性名中如果都包含CUST的前缀可能会让人不爽。这些前缀可以通过如下方式定义重命名规则来移除：<columnRenamingRule searchString="^CUST_" replaceString="" />
                       注意，在内部，MBG使用java.util.regex.Matcher.replaceAll方法实现这个功能。请参阅正则表达式在Java中的方法和例子的相关文档。
                       当<columnOverride>匹配一列时，元素<columnRenamingRule>会被忽略。<columnOverride>优先于重命名的规则。
                       如果指定该选项，重命名规则会在生成属性名前替换列名。 计算出的属性名可能不同，这取决于 <table> 元素上的 "useActualColumnNames"属性。 下表显示了如果重命名规则用于字段集上时不同的值:
                       Column Name          Property if useActualColumnNames="true"   Property if useActualColumnNames="false"
                       CUST_BUSINESS_NAME         BUSINESS_NAME                             businessName
                       CUST_STREET_ADDRESS        STREET_ADDRESS                            streetAddress
                       CUST_CITY                  CITY                                      city
                       CUST_STATE                 STATE                                     state
                       该元素有1个必选属性：
                                searchString:定义将被替换的字符串的正则表达式。
                       该元素有1个可选属性：
                                replaceString:这是一个用来替换搜索字符串列每一个匹配项的字符串。如果没有指定，就会使用空字符串。
                  5.<domainObjectRenamingRule>
                       MBG使用<domainObjectRenamingRule>元素在生成的映射的domain object名称(pojo对象，实体类对象)时可以重命名表的名称。这对那些存在同一前缀的表想在生成domain object名称时去除前缀的非常有用。
                       例如假设存在如下几张表：SYS_USER、SYS_ROLE、SYS_FUNCTION
                       如果生成的所有domain object名称(pojo对象，实体类对象)中如果都包含CUST的前缀可能会让人不爽。这些前缀可以通过如下方式定义重命名规则来移除：<domainObjectRenamingRule searchString="^Sys" replaceString="" />
                       注意，在内部，MBG使用java.util.regex.Matcher.replaceAll方法实现这个功能。请参阅正则表达式在Java中的方法和例子的相关文档。
                       如果指定该选项，这个重命名对象将会重命名所有基于domain object的名称。假设存在一个表SYS_USER :
                       Class             Before using rename rule     After using rename rule
                       Domain Object            SysUser                     User
                       Key                      SysUserKey                  UserKey
                       Blob                     SysUserWithBLOBs            UserWithBLOBs
                       Example                  SysUserExample              UserExample
                       Mapper                   SysUserMapper               UserMapper
                       该元素有1个必选属性：
                                searchString:定义将被替换的字符串的正则表达式。
                       该元素有1个可选属性：
                                replaceString:这是一个用来替换搜索字符串列每一个匹配项的字符串。如果没有指定，就会使用空字符串。
                  3.<columnOverride> 该元素最多配置一个
                       MBG使用<columnOverride>元素将映射表中某些属性默认计算的值更改为指定的值。
                       该元素有1个必选属性:
                                column:   要重写的列名。
                       该元素6个可选属性：
                                property: 要使用的Java属性的名称。如果没有指定，MBG会根据列名生成。例如，如果一个表的一列名为STRT_DTE，MBG会根据<table>的useActualColumnNames属性生成STRT_DTE或strtDte。
                                javaType: 该列属性值为完全限定的Java类型。如果需要，这可以覆盖由<JavaTypeResolver>计算出的类型。对某些数据库来说，这是必要的用来处理“奇怪的”数据库类型(例如MySql的unsigned bigint类型需要映射为java.lang.Object)。
                                jdbcType: 该列的JDBC类型(INTEGER, DECIMAL, NUMERIC, VARCHAR等等)。 如果需要，这可以覆盖由<JavaTypeResolver>计算出的类型。 对某些数据库来说，这是必要的用来处理怪异的JDBC驱动(例如DB2的LONGVARCHAR类型需要映射为VARCHAR..(iBATIS))。
                                typeHandler: 用户定义的需要用来处理这列的类型处理器。它必须是一个继承iBATIS的TypeHandler类或TypeHandlerCallback接口（该接口很容易继承）的全限定的类名。如果没有指定或者是空白，iBATIS会用默认的类型处理器来处理类型。重要:MBG不会校验这个类型处理器是否存在或者可用。MGB只是简单的将这个值插入到生成的SQL映射的配置文件中。
                                delimitedColumnName: 指定是否应在生成的SQL的列名称上增加分隔符。 如果列的名称中包含空格，MGB会自动添加分隔符，所以这个重写只有当列名需要强制指定为一个合适的名字或者列名是数据库中的保留字时才是必要的。
                                isGeneratedAlways: 默认值是false,1.3.4以上版本有效。指定该列是否是数据库中的GENERATED ALWAYS列。如果这个列是GENERATED ALWAYS列，MBG不会映射这个列的insert或者update语句。
                       该元素包含1个可用的<property>子元素，可选属性为：
                                trimStrings: 默认值从<javaModelGenerator>或者中<table>的属性中继承，否则默认值是false。此属性用于MBG时否对从数据库返回的字符字段进行trim操作。这对您的数据库使用CHAR字段而不是VARCHAR字段中存储数据是非常有用的。当这个值是true时MBG会对列进行trim操作，这个值会重写<javaModelGenerator>或者<table>层级上的设置。
                  4.<ignoreColumn> 该元素可选。
                       <ignoreColumn>元素用来告诉MBG忽略映射表的的某些列。没有SQL语句会反应到该列，Java模型对象不会有改属性。
                       该元素有一个必选属性：
                                column:要忽略的列名。
                       该元素有一个必选属性：
                                delimitedColumnName: 匹配列名的时候是否区分大小写。如果为true则区分。默认值为false，不区分大小写。
                  5.<ignoreColumnsByRegex>
             -->
        </table>
    </context>
</generatorConfiguration>

```