
![type:video](https://www.youtube.com/embed/J_kTukE7hr8)

#### Auto Conf

https://www.springboottutorial.com/

- In Spring MVC, a lot of things need to be configured. 
  - WebJar
  - Dispatcher Servlet
  - Default error page
  - ....


- And dependencies:
  - Jackson
  - Validator (Server-side)

- In Hibernate also:
  - DataSource
  - EntityManager
  - TransactionManager

- In the world of Microservice, we need to code very fast.  We need to :
  - AutoConfigure DataSource if Hibernate is on the classPath
  - Configure Dispatcher Servlet if Spring MVC is on the classPath

Based on FrameWorks in the classPath, Spring Boot auto configures framework.

Spring Boot initializr allows to start a Spring Boot very fast.

All the magic comes from ``` org.springframework.boot.autoconfigure ``` package.

There are 2 ways to see auto-configuration in action.

#### Debug mode

To see more things go to debug mode:
```
logging.level.org.springframexork:DEBUG
```
An **autoconfiguration report** will be printed, which states if autoconfiguration was performed or not on classes.
If Spring Boot find some classes on ClassPath, it will perform auto-configuration.

Unsuccessful auto-configuration

```text
   FlywayAutoConfiguration:
      Did not match:
         - @ConditionalOnClass did not find required class 'org.flywaydb.core.Flyway' (OnClassCondition)
```

Successful auto-configuration

```text
   LogbackMetricsAutoConfiguration matched:
      - @ConditionalOnClass found required classes 'io.micrometer.core.instrument.MeterRegistry', 'ch.qos.logback.classic.LoggerContext', 'org.slf4j.LoggerFactory' (OnClassCondition)
      - LogbackLoggingCondition ILoggerFactory is a Logback LoggerContext (LogbackMetricsAutoConfiguration.LogbackLoggingCondition)
      - @ConditionalOnBean (types: io.micrometer.core.instrument.MeterRegistry; SearchStrategy: all) found bean 'simpleMeterRegistry' (OnBeanCondition)
```

#### Actuator

Note 
```xml
    <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
      <dependency>
          <groupId>org.springframework.data</groupId>
          <artifactId>spring-data-rest-hal-browser</artifactId>
      </dependency
```



#### Note

Note format:

```sh
mvn spring-javaformat:apply
```
