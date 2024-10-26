# Questions to test the breadth of your knowledge
### 1. Which are the four approaches to tame velocity? List them all before comparing and contrasting two of them

The four approaches that we have seen to tame velocity are:
- Event-Based System tame myriads of tiny flows that you can collect
- Data Stream Management Systems tame continuous massive flows that you cannot stop 
- Complex Event Processing tame continuous numerous flows that can turn into a torrent
- Event Driven Architecture tame the forming of an immense delta made of myriads of flows of any size and speed
Complex Event Processing takes numerous data streams and compact them in a single stream. The problem is that you need to distinguish the various data type that can be heterogeneous. Event Driven Architecture takes a single stream and divide it in numerous stream of various forms and dimensions. The problem that can derive from that is how we decide which data go in which stream.
### 2. Which are the three time-models we introduced? List them all before comparing and contrasting two of them
- Stream-only Time Model
- Absolute Time Model
- Interval-Based Time Model
Stream-only Time Model can exploit the order to perform queries but we cannot have queries taking into account time. The Absolute Time Model correlate the events in the stream to the time so we can compute queries over an interval of time but cannot take in consideration more than one interval at a time. Both have the answer changing over time and once used the events can be forgotten. 
> TODO chiedere chiarimenti su seconda parte di domanda
### 3. What are the basic types of windows? List them all and describe the behaviour of at least two of them
- Tumpling(size): each session sussede the others without time betweenm them
- Hopping(size and stride): two windows can be overlapped
- Session(gap): a window declare a portion of time and look for the events that happens in them
If we consider a stream-only time model, we define physical windows
- E.g., tumbling window: every 10 events 
- E.g., hopping window: the last 10 events every 5 events
- i.e., session windows cannot be specified
If we consider an absolute time model, we define logical windows
- E.g., tumbling window: every 10 minutes 
- E.g., hopping window: the last 10 minutes every 5 minutes
- i.e., session window: events closer than 5 minutes (or inactivity gap larger than 5 minutes)
### 4. What is a context and how does it relate to interval-based time model? Give an example that supports your explanation.
A context is a grouping of events on base of some of their attributes/characteristics. Context enables more flexible and stateful processing in scenarios requiring groupings or conditions that go beyond simple event windows. It can provide state that persist across event streams, persist until terminated, providing a more robust grouping for complex scenarios.
Better Control: context gives fine-grained control over how events are 
processed. Stateful Processing: handles cases where the state is important, e.g., sessions or transactions. Efficient Partitioning: It helps reduce complexity in event partitioning by user, session, or custom logicAn example can be the bocce game where you want to remember all the throws and results even after every throw to define the ranking at the end of the match.
### 5. Why do we say that Complex Event Processor can tame the torrent effect? Support you claim with an example
CEP systems adds the ability to deploy rules that describe how composite events can be generated from primitive (or composite) ones. Typical CEP rules search for sequences of events: Raise C if A→B. Time is a key aspect
in CEP. The fact that it can apply rules helps taming teh torrent effect. An example is a fire alarm system where there is a user consumption policy that let the alarm fire off only the first time the event condition are met(high temp + smoke) even if the same conditions are met more than once. 
### 6. What are latency / throughput? Given an example of both in the same domain
Latency is the time needed for an operation to complete, throughput is the rate at which operations are performed. Examples are a packet takes 20ms to be sent from my computer to Google, a communication link can send 10 million messages per second. 
 ### 7. Describe the Kafka components at a conceptual, system, and physical level. Illustrate them using an example.
Conceptual view:
- Producers send messages on topics(publishers in pub-sub terms)
- Consumers read messages from topics(subscribers in pub-sub terms)
- Topics are streams of messages
- A message is a key-value pair + metadata
- A kafka cluster manages topics(no content-based routing)
System view:
- Brokers are the main storage and messaging components of the Kafka cluster 
- Producers publish messages on brokers
- Consumers subscribe to brokers to receive messages
Physical view:
- Topics are partitioned across brokers
- Producers shard messages over the partitions of a certain topic
- Typically, hash partitioning, based on the message’s key, determines the partition a message is assigned to(If the key is null, then roundrobin partitioning is adopted) 
- Each Partition is stored on the Broker’s disk as one or more log files
- Each message in the log is identified by its offset number 
- Messages are always appended
- Consumers can consume from different offset
- Brokers are single thread to guarantee  consistency
An example can be represented by article writers that produce articles based on macroarguments(Publishers publishing messages on topics) and store them on websites(brokers that divide the messages in their partition based on topics) and then readers ask the website for articles on specific arguments(Consumer receiving messages only on the topics they are subscribed to)
> TODO chiedere se è un buon esempio
### 8. Illustrate the programming model of spark structured streaming at the logical and physical level. Illustrate them using an example.
The kkey idsea is to treat a live data stream as an unbounded  table that is being continuosly appended resulting in a logical view that is a DAG of standard batch-like queries on static tables and a physical view that runs the DAG incrememtally. An exam,ple can sussist in counting the total value for each seed of cards drawn from a deck: in the logical view we have the unbounded table of the tuple key-value(card seed-value) and the physical view consist of the continuous evaluation of the two(count the seed and sum the value)
### 9. How does Spark Structured Stream treat late arrivals in windowed aggregation? Illustrate it using an example.
Spark Structured Stream use watermarking to handle late arrivals: it tells with a late-threshold T how long to wait for late arrivals; the system tracks the processing time P(i.e. the maximum seen event time) and if the difference between the processing time and the event time of a later arrival is smaller than T the event is considered otherwise it is discarded. An example can consist of the animals seen near a pool, at the end of the event window we take the animal with the greater seen time and use that time to calculate the next watermark threshold. Repeat that procedure for each time the next event window end. If an animal is seen after the process event but before the watermark threshold it is considered as seen.
> TODO chiedere un esempio migliore
### 10. Is there an exact incremental algorithm for computing average, median and distinct? Why?
There isn't as they all require to maintain the memory of all the events in the stream but this is impossible as we would need an infinite amount of memory and also because Spark works in batch of data and so we cannot have a perfect count.
> TODO chiedere una risposta migliore
# Questions to test the depth of your knowledge
### 1. What’s the difference between the stream-only and the absolute time models? Explain it, proposing examples of queries that can be answered in both models and queries that can be answered only with the absolute time model.

### 2. What’s the difference between the absolute and the interval-based time models? Explain it, proposing examples of queries that can be answered in both models and queries that can be answered only with the interval-based  time model.

### 3. What are streams and events? How do they relate to each other? Give an example.

### 4. What is a tubling logical window? Give an example at the conceptual level and show that you know both EPL’s and Spark Structured Streaming’s syntax

### 5. What is a hopping logical window? Give an example at the conceptual level and show that you know both EPL’s and Spark Structured Streaming’s syntax

### 6. What is the role of the output clause in EPL? Give an example that supports your explanation.

### 7. What’s the role of the followed by operator (->) in EPL? Is it possible to implement it in Spark Structured Streaming? Give an example that supports your explanation.

### 8. Explain stream to stream joins. Given an example in EPL that illustrates the different results you obtain using an inner and a left outer join

### 9. Explain stream to table joins. Given an example in EPL that illustrates the different results you obtain using an inner and a left outer join

### 10. What makes Kafka scale horizontally? Explain it using an example.

### 11. What’s the role of a topic in Kafka? How does it relate to partitions and brokers? Explain it using an example.

### 12. What’s the role of a broker in Kafka? How does it relate to topics and partitions? Explain it using an example.

### 13. What’s the role of a consumer group in Kafka? How does it relate to topics and partitions? Explain it using an example.

# Exercises on EPL patterns
Suppose you receive the following stream of events: A1@0,C1@1,B1@2,B2@3,A2@4,B3@5,A3@6,B4@10.
Note that A3@6 denotes an event of type A identified by the number 3 that is received at time 6.
Given the patter: every A -> (B and not C where timer:within(3 sec))
Translate the pattern into an English sentence
Which are the events that trigger the matching? Why?
Which are the events that may trigger the matching but are excluded by the semantics of the every and the where timer:within clauses? Why?

