---
title: "Junit5"
date: 2020-10-02T16:06:01+02:00
draft: false
---

![type:video](https://www.youtube.com/embed/K7g2HUhWbNE)

#### Junit5 = Platform + Jupiter + Vintage

Junit Jupiter is the new programming model and extension for Junit5.

Junit Vintage allows to perform of Junit3/Junit4 tests.

Third party extensions (Cucumber,...):
https://github.com/junit-team/junit5/wiki/Third-party-Extensions

First release of Junit5: September 10th 2017.


#### Jupiter

- Programming model
    - Annotations
    - Assertions
    ...

More Powerful Programming Model.

- Visibility
    - Everything does not have to be ```public```
- Custom display names
    - @DisplayName with emoji support
- Tagging
    @Tag replace @Category
- Assertion based on Timeouts
    - AssertTimeoutPreemtively(...)
- @Nested test class
- @RepeatedTest

#### Tagging

Can combine Junit / Spring Test in tags

#### Parameteriezd

@Parameterized
- @ValueSource
- @MethodSource

#### Dynamic Test

Generate test from lambda expression, providing 

#### Parallel test execution

junit.jupiter.execution.parallel.config.parallel=true

#### Extensibility

Extension API

#### Evolution from Junit4

|Junit4 |Junit 5|
|---|---|
|@org.junit.Test|@org.junit.jupiter.api.Test|
|@Ignore|@Disabled|
|@BeforeClass/@AfterClass|@BeforeAll / @AfterAll|
|@Before / @After|@BeforeEach / @AfterEach|
|org.junit.Assert|org.junit.jupiter.api.Assertions|
|org.junit.Assume|org.junit.jupiter.api.Assumptions|

https://github.com/sbrannen/junit5-demo

