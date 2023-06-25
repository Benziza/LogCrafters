# 📝 LogCrafters: Java Logging Repository 🚀

Welcome to LogCrafters, your go-to repository for mastering logging in Java! 💻🌟

This repository is a comprehensive collection of resources, code samples, and best practices to help you effectively implement logging in your Java projects. 📚💡

## 1-Overview : Logback

Logback is meant to be an improved version of Log4j, developed by the same developer who made Log4j.

Logback also has a lot more features compared to Log4j, with many of them being introduced into Log4j 2 as well.

Let's start by adding the following dependency to the pom.xml:

```
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
    <version>1.2.6</version>
</dependency>
```

This dependency will transitively pull in another two dependencies, the logback-core and slf4j-api.

## 2-Configuration

Let's now have a look at a Logback configuration example:

```
<configuration>
  # Console appender
  <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
    <layout class="ch.qos.logback.classic.PatternLayout">
      # Pattern of log message for console appender
      <Pattern>%d{yyyy-MM-dd HH:mm:ss} %-5p %m%n</Pattern>
    </layout>
  </appender>

  # File appender
  <appender name="fout" class="ch.qos.logback.core.FileAppender">
    <file>baeldung.log</file>
    <append>false</append>
    <encoder>
      # Pattern of log message for file appender
      <pattern>%d{yyyy-MM-dd HH:mm:ss} %-5p %m%n</pattern>
    </encoder>
  </appender>

  # Override log level for specified package
  <logger name="com.baeldung.log4j" level="TRACE"/>

  <root level="INFO">
    <appender-ref ref="stdout" />
    <appender-ref ref="fout" />
  </root>
</configuration>
```

## 3-Main Class :

```
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Log4jExample {

    private static Logger logger = LoggerFactory.getLogger(Log4jExample.class);

    public static void main(String[] args) {
        logger.debug("Debug log message");
        logger.info("Info log message");
        logger.error("Error log message");
    }
}
```

SLF4J provides a common interface and abstraction for most of the Java logging frameworks. It acts as a facade and provides standardized API for accessing the underlying features of the logging framework.

Logback uses SLF4J as native API for its functionality.
