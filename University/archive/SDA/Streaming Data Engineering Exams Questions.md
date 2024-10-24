# Questions to test the breadth of your knowledge
### 1. Which are the four approaches to tame velocity? List them all before comparing and contrasting two of them

The four approaches that we have seen to tame velocity are:
- Event-Based System tame myriads of tiny flows that you can collect
- Data Stream Management Systems tame continuous massive flows that you cannot stop 
- Complex Event Processing tame continuous numerous flows that can turn into a torrent
- Event Driven Architecture tame the forming of an immense delta made of myriads of flows of any size and speed

## 2. Which are the three time-models we introduced? List them all before comparing and contrasting two of them

3. What are the basic types of windows? List them all and describe the behavior of at least two of them

4. What is a context and how does it relate to interval-based time model? Give an example that supports your explanation.

5. Why do we say that Complex Event Processor can tame the torrent effect? Support you claim with an example

6. What are latency / throughput? Given an example of both in the same domain

7. Describe the Kafka components at a conceptual, system, and physical level. Illustrate them using an example.

8. Illustrate the programming model of spark structured streaming at the logical and physical level. Illustrate them using an example.

9. How does Spark Structured Stream treat late arrivals in windowed aggregation? Illustrate it using an example.

10. Is there an exact incremental algorithm for computing average, median and distinct? Why?

# Questions to test the depth of your knowledge
1. What’s the difference between the stream-only and the absolute time models? Explain it, proposing examples of queries that can be answered in both models and queries that can be answered only with the absolute time model.

2. What’s the difference between the absolute and the interval-based time models? Explain it, proposing examples of queries that can be answered in both models and queries that can be answered only with the interval-based  time model.

3. What are streams and events? How do they relate to each other? Give an example.

4. What is a tubling logical window? Give an example at the conceptual level and show that you know both EPL’s and Spark Structured Streaming’s syntax

5. What is a hopping logical window? Give an example at the conceptual level and show that you know both EPL’s and Spark Structured Streaming’s syntax

6. What is the role of the output clause in EPL? Give an example that supports your explanation.

7. What’s the role of the followed by operator (->) in EPL? Is it possible to implement it in Spark Structured Streaming? Give an example that supports your explanation.

8. Explain stream to stream joins. Given an example in EPL that illustrates the different results you obtain using an inner and a left outer join

9. Explain stream to table joins. Given an example in EPL that illustrates the different results you obtain using an inner and a left outer join

10. What makes Kafka scale horizontally? Explain it using an example.

11. What’s the role of a topic in Kafka? How does it relate to partitions and brokers? Explain it using an example.

12. What’s the role of a broker in Kafka? How does it relate to topics and partitions? Explain it using an example.

13. What’s the role of a consumer group in Kafka? How does it relate to topics and partitions? Explain it using an example.

# Exercises on EPL patterns
Suppose you receive the following stream of events: A1@0,C1@1,B1@2,B2@3,A2@4,B3@5,A3@6,B4@10.
Note that A3@6 denotes an event of type A identified by the number 3 that is received at time 6.
Given the patter: every A -> (B and not C where timer:within(3 sec))
Translate the pattern into an English sentence
Which are the events that trigger the matching? Why?
Which are the events that may trigger the matching but are excluded by the semantics of the every and the where timer:within clauses? Why?
