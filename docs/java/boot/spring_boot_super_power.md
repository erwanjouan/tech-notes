![type:video](https://www.youtube.com/embed/J852zGtsE3M)


#### @SpringBootApplication

=

```@SpringBootConfiguration```

```@ComponentScan``` : SpringBoot scans package  of entry point and sub-packages. Best practice conf in base package and beans in sub-packages. 

```@EnableAutoConfiguration```

Tips:
- implements CommandLineRunner allows to perform some action at launch.
- no need to @Autowire when a component is injected through constructor.

Spring makes 2 pass, first user configuration than auto-configuration.


#### AutoConfiguration

Auto configuration class is a configutation class annotated with ```@Configuration```, containing ```@Bean``` annotated method.
In resources folder, create a META-INF folder, create a ```spring.factories``` file.
```
org.springframework.boot.autoconfigure.EnableAutoConfiguration=my.package.MyConfigurationClass
```
Import the maven dependency, the bean will be registered automatically.

But auto configuration overrides bean declared in user configuration.

```@EnableAutoConfiguration``` scans all spring.factories, which lists all auto-configurations (```@Configuration```, ```@ConditionalOn```...). There can be conditioon on methods producing @Bean.


#### Conditional Framework

@ConditionalOnBean
@ConditionalOnMissingBean
@ConditionalOnClass : allows to perfom checks before loading bea,
@ConditionalOnMissingClass
@ConditionalOnProperty
@ConditionalOnExpression
@ConditionalOnSingleCandidate

These annotations are evaluated in a specific order at boot.
```@ConditionalOnClass``` : fastest
...
```@ConditionalOnMissingBean``` : slowest one

Spring boot auto-configurations are filtered by @ConditionalOnClass to eliminate irrelevant beans

##### Autoconfiguration Report

You can create your own condition by extending ```SpringBootCondition```. Then you can plug to the ```CONDITIONS EVALUATION REPORT``` (accessible in DEBUG mode).

```
============================
CONDITIONS EVALUATION REPORT
============================


Positive matches:
-----------------

   AopAutoConfiguration matched:
      - @ConditionalOnProperty (spring.aop.auto=true) matched (OnPropertyCondition)

...

Negative matches:
-----------------


   HelloAutoConfiguration#helloService:
      Did not match:
         - ValidHelloPrefix did not find property 'hello.prefix' (OnValidHelloPrefixCondition)


```

##### Custom condition annotation


```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD})
@Documented
@Conditional(OnValidHelloPrefixCondition.class)
public @interface ConditionalOnValidHelloPrefix {
}
```

#### Traps to avoid

- Cyclic dependency between configuration

- Schedule autoconfiguration ```@AutoConfigureBefore``` ```@AutoConfigureAfter```
Datasource > JdbcTemplatre > DataAccess

- ```@ConditionalOnClass``` on Class and not method annotation.

- 2 steps
    - __User configuration__ : ```@ComponentScan```. Don't use ```@ConditionalOnMissingBean``` in this phase.
    - __Auto configuration__ : ```@EnableAutoConfiguration```

#### Customizing the environment

Property Source: 
- System properties
- Args from cmd
- application.properties
- OS env variables

@PropertySource is an annotation that is evaluated very late in running process, with a low priority, it's content might have been skipped.


#### Spring Boot Events

```ApplicationStartingEvent```

```ApplicationEnvironmentPreparedEvent```

```ApplicationContextInitializedEvent```

(Logging initialized)

```ApplicationEnvironmentPreparedEvent```

(Config files are read)
(Environment post processor are called)
(Logging initialization completes)

```ApplicationPreparedEvent```

(Configuration class are parsed)
(AutoConf is added to context)
(Application Context is refreshed)

```ContextRefreshedEvent``` -> Spring event

(Embedded servlet container connectors are started)

```ServletWebServerInitializedEvent``` -> gives the applicative port.

```ApplicationStartedEvent```

```AvailabilityChangeEvent```

```ApplicationReadyEvent```

```AvailabilityChangeEvent```

or ```ApplicationFailureEvent```


#### Failure Analyzer

You need a typed exception, here InvalidHelloPrefixException, then extend AbstractFailureAnalyzer

```java
public class InvalidHelloPrefixFailureAnalyzer
		extends AbstractFailureAnalyzer<InvalidHelloPrefixException> {

	@Override
	protected FailureAnalysis analyze(Throwable rootFailure,
			InvalidHelloPrefixException cause) {
		return new FailureAnalysis(
				String.format("The hello service could not be auto-configured " +
						"properly: '%s' is an invalid prefix", cause.getPrefix()),
				"A valid prefix must start with an upper-case character", cause);
	}
}
```

```
    public FailureAnalysis(String description, String action, Throwable cause) {
```
