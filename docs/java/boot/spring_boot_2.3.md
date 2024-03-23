---
title: "Spring Boot 2.3"
date: 2020-10-01T16:26:32+02:00
draft: false
---

![type:video](https://www.youtube.com/embed/WL7U-yGfUXA)

#### Build an OCI image with maven

New maven goal = build-image

```
mvn spring-boot:build-image
```

https://docs.spring.io/spring-boot/docs/current/maven-plugin/reference/html/#build-image

Provided Docker daemon is launched

will build ```docker.io/library/todo-reactive-data:0.0.1-SNAPSHOT``` from project ```todo-reactive-data```

Image_name = project
TAG = SW version

```sh
docker run -it -p 8080:8080 docker.io/library/todo-reactive-data:0.0.1-SNAPSHOT
```


Utility to navigate in a Docker image:
```
dive <name of the image>
```

Toggle CTRL+U to see modified files in the layer.

You can active this option in maven Spring boot plugin to create a layered jar. 
Only few kB will be updated when code is changed/.

```xml
<project>
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>2.3.4.RELEASE</version>
				<configuration>
					<layers>
						<enabled>true</enabled>
					</layers>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```

if your dependencies remains the same, only few kB will be updated in a separate layer.


#### Jarmode

Some context here:
https://spring.io/blog/2020/08/14/creating-efficient-docker-images-with-spring-boot-2-3

Standard way to launch Spring Boot app.
```
java -jar <my_spring_boot_jar>
```

Alternatively:
```
java -Djarmode=layertools -jar <my_spring_boot_jar>
```
provides 3 choices : list / extract / help

- list:  provides the list of layers
- extract: allows to extract files from a layer to local directory

Allows to create efficient Dockerfile.

In
https://docs.spring.io/spring-boot/docs/current-SNAPSHOT/reference/htmlsingle/#writing-the-dockerfile
we explain how to write an efficient Dockerfile.

```text
FROM adoptopenjdk:11-jre-hotspot as builder
WORKDIR application
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

FROM adoptopenjdk:11-jre-hotspot
WORKDIR application
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/spring-boot-loader/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
```

Allows to make coherent layers to take advantage of layering system
- dependencies does not change so often -> 1 layer
- application change more -> 1 layer

./BOOT-INF:
- classpath.idx: allows to declare the order to load dependencies
- layers.idx: (layer index) declare layers

Create a layer.xml file in src/ can declare 
- each layer
	- package they are related to

- the order to load each layer

#### Graceful Shutdown

If application shutdowns during a long running request, the underlying network connection is stopped.
This is not a great user experience and might be problematic in dynamic environment like Kubernetes, where instance can be moved.
2 properties added
```text
server.shutdown=graceful
spring.lifecycle.timeout-per-shutdown-phase=20s
```
Now, in-flight request will be processed and new request will be refused.

#### Probes

Suppose we are running on Kubernetes. Expose all management apis provided by actuator :
```text
spring.main.cloud-platform=kubernetes
management.endpoints.web.exposure.include=*
```
```text
curl http://localhost:8080/actuator/health

{"status":"UP","groups":["liveness","readiness"]}
```
```text
curl http://localhost:8080/actuator/health/liveness

{"status":"UP"}
```
```text
curl http://localhost:8080/actuator/health/readiness

{"status":"UP"}
```

New class: AvailabilityChangeEvent which carries an interface 
- AvailabilityState.
	- ReadinessState
	- LivenessState

On Component can change the liveness ans readiness state of the application.

AvailibilityChangeEvent.publish(publisher, this, ReadinessState.REFUSING_TRAFFIC)

having a publisher as a dependency

ServerProperties is a core SpringBoot class that maps configuration file .properties to property.
@ConfigurationProperties(prefix = "server", ignoreUnknownFields = true)
```java
	private Shutdown shutdown = Shutdown.IMMEDIATE;
```
