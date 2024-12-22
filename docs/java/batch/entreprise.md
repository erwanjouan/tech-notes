# Batching for the Modern Enterprise

![type:video](https://www.youtube.com/embed/dIx81HYdpq4)

## Concepts

```mermaid
classDiagram
    JobInstance "1" --> "*" JobExecution
    JobInstance : +string jobName
    JobInstance : +date jobDate
    JobExecution "1" --> "*" StepExecution
```

- A JobExecution is the physical run of the JobInstance (Job name + date)
  - Restarts of job lead to several JobExecution 

- Each JobExecution may have several StepExecutions based on how many Steps are configured for the Job

### RunIdIncrementer

In Job Bean definition, the ```.incrementer(new RunIdIncrementer())``` allows to avoid the need to change input parameter.


