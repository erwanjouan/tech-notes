# High Performance Batch Processing

![type:video](https://www.youtube.com/embed/J6IPlfm7N6w)

- [Github](https://github.com/mminella/scaling-demos){:target="_blank"}

## Spring Batch 101

- Job
  - Flow that is going to be executed within batch processing
  - Made up of steps

- Step
  - Can be sequential or parallel
  - Conditional
  - Tasklets are unit of work inside a Step

- Tasklet
  - Interface provided by Spring batch
  - Executed within the scope of a transaction
  - E.g: chunk oriented tasklet 

- Chunk
  - 6:19