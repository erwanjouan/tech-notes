- Make a todo list of what needs to be done
- Decide which test needs to be done first
    - start small or not at all
    - Imagine the most simple, expressive interface for the object
    - write and run test event if target class does not exist, test will fail
    - make the test compile with stubs
    - make the test run by committing horrible sins (wrong field visibility...)
    - Gradually genealized the working code, replacing constants with variables


- TDD Short workflow

```mermaid
graph LR;
    write_test(1. Write a test<br/>Invent the interface you wish to add<br/>include all necesseray elements)
    --> run_test(2. Make it run<br />Green bar is the most important<br/>Even with bad code) 
    run_test --> make_it_right(3. Make it Right<br/>Generalize code<br/>Eliminate duplication<br/>Make it rigorous<br/>Make the test pass)
```

Test Driven development = Clean code approach

```mermaid
graph LR;
    run_code(1. Make it run) --> clean_code(2. Solve the clean code part) 
    clean_code --> end_code(end)
```

Is the opposite of Architecture Driven development

```mermaid
graph LR;
    clean_code(1. Solve the clean code part) --> run_code(2. Make it run) 
    run_code --> end_code(end)
```

The handling of side effects is important: Translate feelings (disgust at side effect) into a test

```mermaid
graph LR;
    side_effect(1. Translate a design objection<br/>or side effects into<br/> a test case that failed<br/>because of the objection) --> compile_with_stubs(2. Get the code compile<br/>with stub implementation) 
    compile_with_stubs --> real_implem(3. Made the test work<br/>by typing in what seemed)
```

When choosing a design pattern (ex: Value Object): 
- identify the critical operation to be implemented (ex: equality)
- implement obvious things
- test it further
- refactor when all test cases are set

> When refactoring, fix the visibility of fields/methods/class.

> Expand code by shamelessly duplicate existing class and existing test but commit to remove **duplication** afterwards

> Tackle duplication: Move common code to superclass, Make the second subclass, make sure tests for both subclasses work

Chap9