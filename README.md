# 📝 LogCrafters: Java Logging Repository 🚀

Welcome to LogCrafters, your go-to repository for mastering logging in Java! 💻🌟

This repository is a comprehensive collection of resources, code samples, and best practices to help you effectively implement logging in your Java projects. 📚💡

## 1-Overview : Log4J

let's have a look at the venerable Log4j logging framework.

À ce stade, il est bien sûr obsolète, mais mérite d'être discuté car il jette les bases de ses successeurs plus modernes.

## 1-Configuration

First of all you need to add Log4j library to your projects pom.xml:

```
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

Lets take a look at a complete example of simple Log4j configuration with only one console appender:

```
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd" >
<log4j:configuration debug="false">

    <!--Console appender-->
    <appender name="stdout" class="org.apache.log4j.ConsoleAppender">
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern"
              value="%d{yyyy-MM-dd HH:mm:ss} %p %m%n" />
        </layout>
    </appender>

    <root>
        <level value="INFO" />
        <appender-ref ref="stdout" />
    </root>

</log4j:configuration>
```

## 2-Main Class

```
import org.apache.log4j.Logger;

public class Log4jExample {
    private static Logger logger = Logger.getLogger(Log4jExample.class);

    public static void main(String[] args) {
        logger.debug("Debug log message");
        logger.info("Info log message");
        logger.error("Error log message");
    }
}
```
