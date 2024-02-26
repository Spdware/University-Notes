Concepts: we want to evaluate the performance using the quantification of the power use, efficiency and speed of the architectures.
Different architectures are based on different needs. The focus of an architecture specialise in the needing it is going to satisfy. Every time we have an issue we have a possibility to design a system to resolve this issue. We can have more computational power but we will have an higher power consumption and we need a software that utilise a multi-core HW. So we need to do this process every time we design a HW:

![400](https://i.imgur.com/an3oDYS.png)

We can identify a faster computer with two methods:
- Minimize elapsed time for program ececution: response time: execution time = time_end – time_start
- Computer Center Manager: Maximize completion rate = number of jobs/sec and  throughput: total amount of work done in a given time

##### Overview of Factors Affecting Performance
- Algorithm complexity and data set
- Compiler
- Instruction set
- Available operations
- Operating system
- Clock Rate
- Memory system performance
- I/O system performance and overhead

You can have a performance, consequence of speedup and this is important to understand what is your solution(Speedup(x,y)= Performance(x)/Performance(y))

__Amdahl's Law__: you cannot accelerate everything and your rates are dependent from what you aren't accelerating.

![](https://i.imgur.com/fXCYtLf.png)

### Benchmarks
Important as they evaluate your performance