---
title: Logback配置详解
date: 2019-08-28 17:28:24
description: logback的一些配置
tags: logback
categories: 开发
---
# logback.xml常用配置详解
```xml
<?xml version="1.0" encoding="utf-8" ?>
<!-- <configuration>：根节点，包含下面3个属性
     scan：      设置为true时，配置文件如果发生改变，将会被重新加载，默认值为true
     scanPeriod: 设置监测配置文件是否有修改的时间间隔，如果没有给出时间单位，默认单位是毫秒。当scan为true时，此属性生效。默认的时间间隔为1分钟。
     debug:      设置为true时，将打印出logback内部日志信息，实时查看logback运行状态，默认值为false。
 -->
<configuration scan="true" scanPeriod="60 seconds" debug="false">
    <!-- <contextName>：用来设置logger上下文名称，每个logger都关联到logger上下文，默认上下文名称为default。
         使用<contextName>设置成其他名字，用于区分不同应用程序的记录。一旦设置，不能修改。
    -->
    <contextName>default</contextName>

    <!-- <timestamp>：获取时间戳字符串，他有两个属性
         key:         标识此<timestamp> 的名字；
         datePattern: 设置将当前时间（解析配置文件的时间）转换为字符串的模式，遵循java.text.SimpleDateFormat的格式。
    -->
    <timestamp key="bySecond" datePattern="yyyyMMdd'T'HHmmss"/>

    <!-- <property> ：  用来定义变量值，通过<property>定义的值会被插入到logger上下文中，可以使“${}”来使用变量。
    -->
    <property name="logPath"    value="d:/test_log" />
    <property name="maxHistory" value="30"/>

    <!-- <appender>：  负责写日志的组件，它有两个必要属性
         name指定appender名称，class指定appender的全限定名配置项。
         通过此节点配置日志输出位置（控制台、文件、数据库）、输出格式等
    -->
    <!-- ConsoleAppender     输出到控制台。
    -->
    <appender name="consoleLog"   class="ch.qos.logback.core.ConsoleAppender">
        <!-- <layout>或者<encoder>对日志进行格式化
        -->
        <layout class="ch.qos.logback.classic.PatternLayout">
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger -%msg%n</pattern>
        </layout>
        <!-- <target>：字符串System.out(默认)或者System.err
        -->
    </appender>

    <!-- RollingFileAppender 输出到文件。滚动记录文件，先将日志记录到指定文件，当符合某个条件时，将日志记录到其他文件
    -->
    <appender name="fileInfoLog"  class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- <encoder> 对记录事件进行格式化。负责两件事，一是把日志信息转换成字节数组，二是把字节数组写入到输出流。
             PatternLayoutEncoder 是唯一有用的且默认的encoder ，有一个<pattern>节点，用来设置日志的输入格式。
         -->
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger -%msg%n</pattern>
        </encoder>

        <!-- <file>：可有可无。被写入的文件名，可以是相对目录，也可以是绝对目录，如果上级目录不存在会自动创建，没有默认值。
        -->
        <!--<file>${logPath}/info/tests.log</file>-->

        <!-- <append>：true，日志被追加到文件结尾；false，清空现存文件。默认是true。
        -->
        <apppend>true</apppend>

        <!-- <rollingPolicy>  滚动策略，决定RollingFileAppender的行为，涉及文件移动和重命名
              TimeBasedRollingPolicy，最常用的滚动策略，它根据时间来制定滚动策略，既负责滚动也负责触发滚动。
        -->
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- <fileNamePattern>：必要节点，包含文件名及“%d”转换符。
                 “%d”可以包含一个java.text.SimpleDateFormat指定的时间格式，如：%d{yyyy-MM}。如果直接使用 %d，默认格式是 yyyy-MM-dd。
                 RollingFileAppender的file字节点可有可无，通过设置file，可以为活动文件和归档文件指定不同位置。
                 当前日志总是记录到file指定的文件（活动文件），活动文件的名字不会改变；如果没设置file，活动文件的名字会根据fileNamePattern 的值，每隔一段时间改变一次。
            -->
            <fileNamePattern>${logPath}/info/%d.log</fileNamePattern>
            <!-- <maxHistory> 可选节点，控制保留的归档文件的最大数量，超出数量就删除旧文件。注意，删除旧文件是，那些为了归档而创建的目录也会被删除。
            -->
            <maxHistory>${maxHistory}</maxHistory>
        </rollingPolicy>

        <!-- <triggeringPolicy>告知 RollingFileAppender 何时激活滚动。
             SizeBasedTriggeringPolicy 查看当前活动文件的大小，如果超过指定大小会告知 RollingFileAppender 触发当前活动文件滚动
        -->
        <triggeringPolicy class="ch.qos.logback.core.rolling.SizeBasedTriggeringPolicy">
            <!-- <maxFileSize>活动文件的大小，默认值是10MB
            -->
            <maxFileSize>5MB</maxFileSize>
            <!-- <prudent>：当为true时，不支持FixedWindowRollingPolicy。支持TimeBasedRollingPolicy，但是有两个限制，1不支持也不允许文件压缩，2不能设置file属性，必须留空。
            -->
            <prudent>true</prudent>
        </triggeringPolicy>

        <!-- FixedWindowRollingPolicy，根据固定窗口算法重命名文件的滚动策略。
             这个设置的有问题。。。。。。。。。
             按照固定窗口模式生成日志文件，当文件大于20MB时，生成新的日志文件。窗口大小是1到3，当保存了3个归档文件后，将覆盖最早的日志。
             该模式下<fileNamePattern>必须包含“%i”,假设最小值和最大值分别为1和3，命名模式为 mylog%i.log，会产生归档文件mylog1.log和mylog2.log。还可以指定文件压缩选项，例如，mylog%i.log.gz 或者mylog%i.log.zip
        <rollingPolicy class="ch.qos.logback.core.rolling.FixedWindowRollingPolicy">
            <minIndex>1</minIndex>
            <maxIndex>3</maxIndex>
            <fileNamePattern>${logPath}/info/%d%i.log</fileNamePattern>
        </rollingPolicy>
        -->
    </appender>
    
    <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 只记录ERROR级别日志，添加范围过滤，可以将该类型的日志特殊记录到某个位置 
        -->
        <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
            <level>ERROR</level>
        </filter>
        <encoder>
            <pattern>%d{yyyy-MM-dd HH:mm:ss} [%thread] %-5level %logger -%msg%n</pattern>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <fileNamePattern>${logPath}/error/%d.log</fileNamePattern>
            <!-- 日志最大的历史 30天 -->
            <maxHistory>${maxHistory}</maxHistory>
        </rollingPolicy>
    </appender>

    <!-- <logger> 设置某一个包或具体的某一个类的日志打印级别、以及指定<appender>。
         name:        用来指定受此<loger>约束的某一个包或者具体的某一个类。
         level:       用来设置打印级别，大小写无关：ALL，TRACE, DEBUG, INFO, WARN, ERROR, OFF（级别从小到大）。
                      还有一个特殊值INHERITED或者同义词NULL，代表强制执行上级的级别。 如果未设置此属性，那么当前loger将会继承上级的级别。
         addtivity:   是否向上级<loger>传递打印信息。默认是true。
         appender-ref 零个或多个，标识这个appender将会添加到这个loger。
    -->

    <!-- <root>  也是<logger>元素，但是它是根<logger>，是所有<logger>的上级。只有一个level属性，因为name已经被命名为"root",且已经是最上级了。
    -->
    <root level="info">
        <appender-ref ref="consoleLog" />
        <appender-ref ref="fileInfoLog" />
        <appender-ref ref="fileErrorLog" />
    </root>
</configuration>

```
