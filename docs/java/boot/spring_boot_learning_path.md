#### Best practice

- [https://www.e4developer.com/2018/08/06/spring-boot-best-practices/](https://www.e4developer.com/2018/08/06/spring-boot-best-practices/)

#### Spring Boot

AutoConfiguration
Prod-Ready monitoring

![type:video](https://www.youtube.com/embed/W_E6iSTBqNs)

https://github.com/in28minutes/learn/blob/master/learning-paths/01.md

#### JPA

- Technologies by chronology:
  - JDBC : Allows to perform SQL queries. But it takes 25 lines of code for a single query
      - Query
      - ResultSet
  - SpringJDBC : Same as previous but in 5 lines of code.
  - iBatis: Entities
  - JPA (JavaEE standard): Maps an Entity to a table. Allows to move Business code from SQL to Business Tier.
  - Hibernate (ORM, implementation of JPA): Hides the SQL query from code
      - EntityManager
      - Transactions: Allows to set a context where we can rollback an incomplete transaction.
      - JPQL, Criteria Query: Query language mixing SQL and Java
      - Caching
      - Performance (Lazy loading)
      - Used in 90% of project
  - Spring Data JPA: Avoids to play with the entityManager. You only need to create entity and extend a specific interface.
  - Spring Data Rest: can expose REST api and prototype quickly

#### WebApplication

Browser <-> WebApplication

- WebApp architecture evolution:
  - Model1: Browser interacts with JSP which bears the logic
  - Model2: Browser interacts with a servlet (talking to DB) which provides JSP (for the View)
  - Model2+: Browser interacts with a Front Controller (Dispatcher Servlet) which dispatch to other Controller or Servlet

- JSP, Servlets is the legacy framework that implements:
    - HttpRequest
    - HttpResponse
- MVC
- Design
    - HTML/CSS/JS/BootStrap
    - Layout
    - Logout/Login
    - Session

#### WebServices

Allows to provides technology agnostic service over HTTP:
- Machine to Machine
- Interoperable (cross Platform/cross Language)
- Network enabled

#### REST API

- Features:
  - HateOAS
  - Documentation
      - OpenAPI
  - Security
      - JWT
  - Versioning
      - URL
      - Header
  - Validation

- Best Practices:
  - Consumer First
  - Richardson Maturity Model
      - level0: HTTP
      - level1: Resources
      - level2: HTTP verbs
      - level3: HateOAS  
  - Enterprise standardization
      - Framework (versioning/security)

- Framework:
  - Spring MVC/Rest
  - JAX-RS - Jersey
      - Quarkus / Micronaut

#### IDE / BUILD / UNIT TESTING

- Build
  - jar
  - war
  - maven
  - gradle

- Unit Testing
  - Junit
  - Mockito
  - Spring Unit

- Ide
  - Eclipse
  - IntelliJ
  - VsCode

Become an expert at the IDE will save you hours.

