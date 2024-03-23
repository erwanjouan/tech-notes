#### Spring Reactor Tutorial

https://stackabuse.com/spring-reactor-tutorial/

- Old paradigm in traditionnal Servlet servers (Tomcat servers):
  - Each request warrants a new thread and waits for the whole background process to finish in order to send the response back
  - Data tiers blocks the Web Tier

Java 8 Asynchronous still makes blocking calls, as it waits for .join() method to complete.

- We want the program to be asynchronous and non-blocking => Reactive streams specifications. Based on the following:
  - **Publisher** - A Publisher is a provider of a potentially unbounded number of elements.
  - **Subscriber** - A Subscriber listens to that Publisher, asking for new data. Sometimes, it's also referred to as a Consumer.
  - **Backpressure** - The ability of the Subscriber to let the Publisher how many requests can it handle at the time. So it's the Subscriber that is responsible for the flow of the data, not the Publisher as it just provides the data.


- The Reactor Project offers 2 types of publishers. These are considered the main building blocks of Spring Webflux:
  - **Flux** - is a publisher that produces 0 to N values. It could be unbounded. Operations that return multiple elements use this type.
  - **Mono** - is a publisher that produces 0 to 1 value. Operations that return a single element use this type.

Had we used the traditional approach with Spring MVC, objects would keep on accumulating in your RAM and once it collects everything it would return it to the client. This might exceed our RAM capacity and also blocks any other operation from getting processed in the meantime.

When we use Spring Webflux, the whole internal dynamics get changed. The framework starts subscribing to these records from the publisher and it serializes each item and sends it back to the client in chunks.

##### Server Sent Events


#### Get Started with Reactive Programming in Spring


https://developer.okta.com/blog/2018/09/21/reactive-programming-with-spring

Spring WebFlux is built on top of project Reactor.
```java
this.repository.findAll();
```
returns a **Flux<T>**, which is a Reactive Stream Publisher with Rx Operators that emits
0 or N elements and then completes.

**Mono<T>**  is a **Publisher<T>** that produces zero or one value.

Build Reactive APIs with Spring WebFlux

https://developer.okta.com/blog/2018/09/24/reactive-apis-with-spring-webflux


https://www.codingame.com/playgrounds/929/reactive-programming-with-reactor-3/Intro

