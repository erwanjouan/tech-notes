# Fault Tolerance Using Skip Policy & Listener

![type:video](https://www.youtube.com/embed/deifDn6FWO0)

## In case of error

A chunk is a unit of work that is processed as a transaction.

- Job
  - Step
    - Chunk
  
- Suppose a file is read with chunks of 10 lines in synchronous executor.
- If row 13 is corrupted
  - First pass
    - Only rows 1-10 will be committed to output (db) with the first chunk
    - Second chunk of rows 11-20 will throw an exception.
    - Job/Step are stopped with failed status
  - If job is relaunched with the same corrupted data
    - Spring batch will acknowledge first 10 rows were processed looking at ```batch_step_execution```
    - First chunk (with an offset of 10 rows) will fail

## Skip Policy

- By default Spring batch will abort a Step if an exception is encountered.
- To ignore a corrupted row
- Specify a predefined skip policy
  - In ```Step``` bean configuration
    - ```.faultTolerant()```  
    - ```.skipLimit(n)```  
    - ```.skip(ExceptionType.class)```
    - ```.noSkip(AnotherExceptionType.class)```
- Write a custom skip policy
  - Implement a class implementing ```SkipPolicy``` interface
    - implement ```boolean shouldSkip(Throwable t, int i)``` method
    - that can check if Throwable is an instance of NumberFormatException then
  - In ```Step``` bean configuration
    - ```.faultTolerant()```
    - ```.skipPolicy(skipPolicy())```

## Skip Listener

- How to keep track of skipped rows ?
- Add a class that implements ```SkipListener<T, U>``` (e.g. SkipListener<Customer, Number>) and define the implementation of:
  - ```onSkipInRead(Throwable t)```
  - ```onSkipInbWrite(Number number, Throwable t)```
  - ```onSkipInProcess(Customer customer, Throwable t)```
- Add the logic
  - e.g Add an entry in log
- Modify the ```Step``` bean declaration in Spring bean config
    - ```.faultTolerant()```
    - ```.skipPolicy(skipPolicy())```
    - ```.listener(skipListener())```
