# Cloud Native Batch Processing (2020)

![type:video](https://www.youtube.com/embed/1NZVwv1cmMc)

## Circuit Breaker

- Use case: 
  - Processor is using an external Rest Service
  - That may not answer and needs a fallback

- An implementation of Circuit Breaker comes with Spring Batch
  - ```@CircuitBreaker```
    - annotation on the method that calls potentially failing external service
    - will make retries
  - ```@Recover```
    - fallback processing in case previous method fails

- There is a retry mechanism in Spring batch
  - See ```.retry()``` method in Step configuration
  - In case of error it
    - Rollbacks the transation 
    - Changes the commit size to 1
    - Tries to process individual records
  - Performance penalty

