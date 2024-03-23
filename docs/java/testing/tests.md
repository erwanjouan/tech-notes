---
title: "Assertion frameworks"
date: 2020-09-24T20:43:17+02:00
draft: false
---

Frameworks d'assertion

#### AssertJ

Framework that provides fluent assertion statements for java.
Is a fork of Fest assertion.

https://assertj.github.io/doc/

```java
// JUnit style
assertNotNull(list);
assertTrue(list.isEmpty());

// AssertJ
assertThat(list).isNotNull().isNotEmpty();
```

The base method for AssertJ assertions is the assertThat method followed by the assertion.

#### Hamcrest

Hamcrest is a framework for software tests. Hamcrest allows checking for conditions in your code via existing matchers classes. It also allows you to define your custom matcher implementations.

To use Hamcrest matchers in JUnit you use the assertThat statement followed by one or several matchers.

```java
// all statements test the same
assertThat(a, equalTo(b));
assertThat(a, is(equalTo(b)));
assertThat(a, is(b));
```

For Lists

```java
public class HamcrestListMatcherExamples {
    @Test
    public void listShouldInitiallyBeEmpty() {
        List<Integer> list = Arrays.asList(5, 2, 4);
        assertThat(list, hasSize(3));
        // ensure the order is correct
        assertThat(list, contains(5, 2, 4));
        assertThat(list, containsInAnyOrder(2, 4, 5));
        assertThat(list, everyItem(greaterThan(1)));
    }
}
```