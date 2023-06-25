# 📝 LogCrafters: Java Logging Repository 🚀

Welcome to LogCrafters, your go-to repository for mastering logging in Java! 💻🌟

This repository is a comprehensive collection of resources, code samples, and best practices to help you effectively implement logging in your Java projects. 📚💡

## 1- Overview

Logging is a powerful aid for understanding and debugging program's run-time behavior.

This article discusses the most popular java logging framewloorks, **Log4j 2** and **Logback**, along with their predecessor **Log4j**, and briefly touches upon **SLF4J**, a logging facade that provides a common interface for different logging frameworks.

## 2- Log4j 2

**Log4j 2** is new and improved version of the **Log4j** logging framework. The most compelling improvement is the possibility of asynchronous logging. Log4j 2 requires the following libraries:

### 2-1- Project Directory

![alt text](https://mkyong.com/wp-content/uploads/2019/03/log4j2-project-1.png)

You should have one choice between : **log4j2.properties** or **log4j2.yml** or **log4j2.xml**

### 2-2- Maven

```
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.6.1</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.6.1</version>
</dependency>
```

### 2-3- log4j2.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <Console name="LogToConsole" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
        <Logger name="com.mkyong" level="debug" additivity="false">
            <AppenderRef ref="LogToConsole"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="LogToConsole"/>
        </Root>
    </Loggers>
</Configuration>
```

- The **status** attribute determines the level of internal logging for log4j itself. In this case, only warnings and errors will be logged (level of logging).
- This section defines the appenders, which are responsible for specifying where the log messages should be output. In this case, there is a single appender named "LogToConsole" that logs messages to the console (standard output).
- The pattern defines the format in which the log messages will be displayed. Let's break down the different components of the pattern:

  - %d{HH:mm:ss.SSS}: This represents the timestamp of the log message. %d is used to indicate that it's a date and time pattern. HH:mm:ss.SSS specifies the format as hours (24-hour format), minutes, seconds, and milliseconds.

  - [%t]: This represents the thread name of the thread that generated the log message. %t is used to include the thread name within square brackets.

  - %-5level: This represents the log level of the message. %level is used to include the log level. -5 specifies that the log level should be left-aligned and padded with spaces to a width of 5 characters.

  - %logger{36}: This represents the name of the logger that generated the log message. %logger is used to include the logger name. {36} specifies that the logger name should be truncated to a maximum length of 36 characters.

  - %msg: This represents the actual log message itself. %msg is used to include the message.

  - %n: This represents a platform-dependent line separator. %n is used to add a newline after each log message, ensuring that each message appears on a new line.

- This section defines the loggers, which are responsible for filtering and handling log messages based on their logger name and log level.

  The first <Logger> element specifies that all log messages from the "com.mkyong" package (or any of its sub-packages) should be logged with a log level of "debug". The additivity attribute is set to "false", which means that log messages for this logger will not propagate to higher-level loggers.

  The second <Root> element specifies the root logger, which acts as a catch-all for log messages that don't match any specific logger. In this case, it is configured to log messages with a log level of "error". The <AppenderRef> element is used to reference the "LogToConsole" appender, so that log messages matching the logger configuration will be output to the console.

### 2-4- Hello Log4j 2

```
package com.mkyong;

import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class HelloWorld {

    private static final Logger logger = LogManager.getLogger(HelloWorld.class);

    public static void main(String[] args) {

        logger.debug("Hello from Log4j 2");

        // in old days, we need to check the log level to increase performance
        /*if (logger.isDebugEnabled()) {
            logger.debug("{}", getNumber());
        }*/

        // with Java 8, we can do this, no need to check the log level
        logger.debug("{}", () -> getNumber());

    }

    static int getNumber() {
        return 5;
    }

}
```

Output :

```
19:12:25.337 [main] DEBUG com.mkyong.HelloWorld - Hello from Log4j 2
19:12:25.340 [main] DEBUG com.mkyong.HelloWorld - 5
```

## 3- Log4j 2 Appenders

Some of the common Log4j appenders.

### 3-1- ConsoleAppender

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="DEBUG">
    <Appenders>
        <Console name="LogToConsole" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
    </Appenders>
    <Loggers>
		<!-- avoid duplicated logs with additivity=false -->
        <Logger name="com.mkyong" level="debug" additivity="false">
            <AppenderRef ref="LogToConsole"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="LogToConsole"/>
        </Root>
    </Loggers>
</Configuration>
```

### 3-2- FileAppender

```

<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="DEBUG">
    <Appenders>
        <Console name="LogToConsole" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
        <File name="LogToFile" fileName="logs/app.log">
            <PatternLayout>
                <Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
            </PatternLayout>
        </File>
    </Appenders>
    <Loggers>
        <Logger name="com.mkyong" level="debug" additivity="false">
            <AppenderRef ref="LogToFile"/>
            <AppenderRef ref="LogToConsole"/>
        </Logger>
        <Logger name="org.springframework.boot" level="error" additivity="false">
            <AppenderRef ref="LogToConsole"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="LogToFile"/>
            <AppenderRef ref="LogToConsole"/>
        </Root>
    </Loggers>
</Configuration>
```

### 3-3- RollingFileAppender – Rotate log files daily or when the file size > 10MB.

```
<Configuration status="DEBUG">
    <Appenders>
        <Console name="LogToConsole" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </Console>
        <RollingFile name="LogToRollingFile" fileName="logs/app.log"
                    filePattern="logs/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
			<PatternLayout>
				<Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
			</PatternLayout>
			<Policies>
				<TimeBasedTriggeringPolicy />
				<SizeBasedTriggeringPolicy size="10 MB"/>
			</Policies>
		</RollingFile>
    </Appenders>

    <Loggers>
        <!-- avoid duplicated logs with additivity=false -->
        <Logger name="com.mkyong" level="debug" additivity="false">
            <AppenderRef ref="LogToRollingFile"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="LogToConsole"/>
        </Root>
    </Loggers>
</Configuration>
```

By default, it will create up to 7 archives on the same day.

![alt text](https://mkyong.com/wp-content/uploads/2019/03/log4j2-rolling-file-0.png)
![alt text](https://mkyong.com/wp-content/uploads/2019/03/log4j2-rolling-file-1.png)

We can override the default **7 archives** with this DefaultRolloverStrategy

```
<RollingFile name="LogToRollingFile" fileName="logs/app.log"
			filePattern="logs/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
		<PatternLayout>
			<Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
		</PatternLayout>
		<Policies>
			<TimeBasedTriggeringPolicy/>
			<SizeBasedTriggeringPolicy size="10 MB"/>
		</Policies>
		<DefaultRolloverStrategy max="10"/>
	</RollingFile>
```

Or : RollingRandomAccessFileAppender – Similar to the RollingFileAppender, but faster.

```
<RollingRandomAccessFile name="LogToRollingRandomAccessFile" fileName="logs/app.log"
			filePattern="logs/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
		<PatternLayout>
			<Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
		</PatternLayout>
		<Policies>
			<TimeBasedTriggeringPolicy/>
			<SizeBasedTriggeringPolicy size="1 MB"/>
		</Policies>
		<DefaultRolloverStrategy max="10"/>
	</RollingRandomAccessFile>

```

### 3-4- SMTPAppender – Need javax.mail to send email.

Pom.xml :

```
<dependency>
    <groupId>com.sun.mail</groupId>
    <artifactId>javax.mail</artifactId>
    <version>1.6.2</version>
</dependency>
```

XML file :

```
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Appenders>
        <RollingRandomAccessFile name="LogToRollingRandomAccessFile" fileName="logs/app.log"
                filePattern="logs/$${date:yyyy-MM}/app-%d{MM-dd-yyyy}-%i.log.gz">
            <PatternLayout>
                <Pattern>%d %p %c{1.} [%t] %m%n</Pattern>
            </PatternLayout>
            <Policies>
                <TimeBasedTriggeringPolicy/>
                <SizeBasedTriggeringPolicy size="1 MB"/>
            </Policies>
            <DefaultRolloverStrategy max="10"/>
        </RollingRandomAccessFile>

        <SMTP name="LogToMail" subject="Error Log From Log4j"
              from="from@DOMAIN"
              to="to@DOMAIN"
              smtpHost="smtp.mailgun.org"
              smtpPort="25"
              smtpUsername="abc"
              smtpPassword="123"
              bufferSize="100">
        </SMTP>

    </Appenders>
    <Loggers>
        <Logger name="com.mkyong" level="debug" additivity="false">
            <AppenderRef ref="LogToRollingRandomAccessFile"/>
            <AppenderRef ref="LogToConsole"/>
        </Logger>
        <Root level="error">
            <AppenderRef ref="LogToMail"/>
        </Root>
    </Loggers>
</Configuration>
```

Email :
![alt text](https://mkyong.com/wp-content/uploads/2019/03/log4j2-smtp.png)

### 4-Asynchronous Loggers

https://dzone.com/articles/asynchronous-logging-with-log4j-2
