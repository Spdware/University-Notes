# Questions to test the breadth of your knowledge
### 1. Which are the four approaches to tame velocity? List them all before comparing and contrasting two of them

The four approaches that we have seen to tame velocity are:
- Event-Based System tame myriads of tiny flows that you can collect(Kafka)
- Data Stream Management Systems tame continuous massive flows that you cannot stop(Spark) 
- Complex Event Processing tame continuous numerous flows that can turn into a torrent(Esper)
- Event Driven Architecture tame the forming of an immense delta made of myriads of flows of any size and speed(Kafka)

Complex Event Processing takes numerous data streams and compact them in a single stream. The problem is that you need to distinguish the various data type that can be heterogeneous. Event Driven Architecture takes a single stream and divide it in numerous stream of various forms and dimensions. The problem that can derive from that is how we decide which data go in which stream.
### 2. Which are the three time-models we introduced? List them all before comparing and contrasting two of them
- Stream-only Time Model
- Absolute Time Model
- Interval-Based Time Model: beginning and end of the events

Stream-only Time Model can exploit the order to perform queries but we cannot have queries taking into account time. The Absolute Time Model correlate the events in the stream to the time so we can compute queries over an interval of time but cannot take in consideration more than one interval at a time. Both have the answer changing over time and once used the events can be forgotten. 
### 3. What are the basic types of windows? List them all and describe the behaviour of at least two of them
**Use a table of physical and logical as column label and use tumbling, hopping and session as row label and check which one you can do **
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
Relationship with event time model has you can have an event that start the interval and an event that end the interval. A context is a grouping of events on base of some of their attributes/characteristics. Context enables more flexible and stateful processing in scenarios requiring groupings or conditions that go beyond simple event windows. It can provide state that persist across event streams, persist until terminated, providing a more robust grouping for complex scenarios.
Better Control: context gives fine-grained control over how events are 
processed. Stateful Processing: handles cases where the state is important, e.g., sessions or transactions. Efficient Partitioning: It helps reduce complexity in event partitioning by user, session, or custom logicAn example can be the bocce game where you want to remember all the throws and results even after every throw to define the ranking at the end of the match.
### 5. Why do we say that Complex Event Processor can tame the torrent effect? Support you claim with an example
CEP systems adds the ability to deploy rules that describe how composite events can be generated from primitive (or composite) ones. Typical CEP rules search for sequences of events: Raise C if A→B. Time is a key aspect
in CEP. The fact that it can apply rules helps taming the torrent effect. An example is a fire alarm system where there is a user consumption policy that let the alarm fire off only the first time the event condition are met(high temp + smoke) even if the same conditions are met more than once. 
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
- Brokers are the main storage and messaging components of the Kafka cluster. They divide the topics into partitions. The partitions of a single topic can be stored by different brokers.  
- Producers publish messages on brokers
- Consumers groups subscribe to brokers to receive messages

Physical view:
- Topics are partitioned across brokers
- Producers shard messages over the partitions of a certain topic
- Typically, hash partitioning, based on the message’s key, determines the partition a message is assigned to(If the key is null, then roundrobin partitioning is adopted) 
- Each Partition is stored on the Broker’s disk as one or more log files
- Each message in the log is identified by its offset number 
- Messages are always appended
- Consumers can consume from different offset
- Brokers are single thread to guarantee  consistency
- Need to represent the commit logs with the offset and key values.

An example can be represented by article writers that produce articles based on macroarguments(Publishers publishing messages on topics) and store them on websites(brokers that divide the messages in their partition based on topics) and then readers ask the website for articles on specific arguments(Consumer receiving messages only on the topics they are subscribed to)
> TODO chiedere se è un buon esempio
### 8. Illustrate the programming model of spark structured streaming at the logical and physical level. Illustrate them using an example.
The key idea is to treat a live data stream as an unbounded  table that is being continuosly appended resulting in a logical view that is a DAG of standard batch-like queries on static tables and a physical view that runs the DAG incrementally. An example can sussist in counting the total value for each seed of cards drawn from a deck: in the logical view we have the unbounded table of the tuple key-value(card seed-value) and the physical view consist of the continuous evaluation of the two(count the seed and sum the value)
Always a state for each registered query. In some peculiar case the state can be null. Can be answered also with the drawing
### 9. How does Spark Structured Stream treat late arrivals in windowed aggregation? Illustrate it using an example.
Spark Structured Stream use watermarking to handle late arrivals: it tells with a late-threshold T how long to wait for late arrivals; the system tracks the processing time P(i.e. the maximum seen event time) and if the difference between the processing time and the event time of a later arrival is smaller than T the event is considered otherwise it is discarded. An example can consist of the animals seen near a pool, at the end of the event window we take the animal with the greater seen time and use that time to calculate the next watermark threshold. Repeat that procedure for each time the next event window end. If an animal is seen after the process event but before the watermark threshold it is considered as seen.
> TODO chiedere un esempio migliore
### 10. Is there an exact incremental algorithm for computing average, median and distinct? Why?
There isn't as they all require to maintain the memory of all the events in the stream but this is impossible as we would need an infinite amount of memory and also because Spark works in batch of data and so we cannot have a perfect count.
> TODO chiedere una risposta migliore

Average yes see question 8 example
Median no as it can be exact because the state for the exact computation is unbounded and need to be computed approximately.
Distinct cannot do even approximated as we need to memorize all the data seen so far. You don't want to keep unbounded state
# Questions to test the depth of your knowledge
### 1. What’s the difference between the stream-only and the absolute time models? Explain it, proposing examples of queries that can be answered in both models and queries that can be answered only with the absolute time model.
Stream-only models work only on events without taking into account time. Instead absolute time models take in consideration events and their time of arrivals. Queries that ask to discriminate if an event arrives after another one can be done by both but only absolute time model can answer to queries that ask if an event arrives after x minutes from another one.
### 2. What’s the difference between the absolute and the interval-based time models? Explain it, proposing examples of queries that can be answered in both models and queries that can be answered only with the interval-based  time model.
Absolute time model works on the time of arrival of the events in a stream instead interval-based model consider the events spanning on intervals of time. Both can answer queries to see if an events happen before x time has passed or to count the events in an interval of time. Interval-based models can also see conflicts in the events(based on the overlapping of the intervals) and see the number of intervals that last x amount of time.
### 3. What are streams and events? How do they relate to each other? Give an example.
Events are the objects that contains the information of the phenomena we want to study. Streams are flux of continuous information. They relate to each other as streams are composed of events. 
We can consider as stream the flux of packets in a router and events the single arrival of a packet.
> TODO rivedere migliore descrizione

The event is a business fact an immutable object that capture something in time 
A stream is a logical container of events of a given type. 
In kafka a topic is a stream.
### 4. What is a tubling logical window? Give an example at the conceptual level and show that you know both EPL’s and Spark Structured Streaming’s syntax
A tumbling logical window is the type of window where the event windows happens one after another without space between them and not overlapping. We consider the time as unit too define the window. an example is every 10 minutes. 
Example: the events received in the last 2 seconds every 2 seconds
**EPL**
select *
from TemperatureSensorEvent.win:time_batch(2 seconds);
**Spark Structured Streaming**
temperature_sdf
.groupBy(window("TS", "2 seconds"),"SENSOR")
### 5. What is a hopping logical window? Give an example at the conceptual level and show that you know both EPL’s and Spark Structured Streaming’s syntax
A hopping logical window is a type of window where the intervals of the events can be overlapped(still no time between two windows). We consider the time window every tot time: last 10 minutes every 5 minutes. 
Example: average temperature of the last 4 seconds every 2 seconds
EPL
select sensor, avg(temperature)
from TemperatureSensorEvent.win:time(4 seconds)
group by sensor
output snapshot every 2 seconds;
Spark Structured Streaming
temperature_sdf
.groupBy(window("TS", "4 seconds", "2 seconds"),"SENSOR")

### 6. What is the role of the output clause in EPL? Give an example that supports your explanation.
The output clause is used to stabilize and control the output all | first | last | snapshot :
-  first ensures that the first result generated by the  query is output immediately
- last outputs the most recent result at the time of the query’s execution, discarding any intermediate results that may have been calculated
- all outputs every result generated by the query,  including both current and previous calculations
- snapshot outputs only the current state of the query at the specified interval. Unlike all, it doesn’t include previous results
An example:
E.g., the average temperature of the last 4 seconds every 2 seconds, a.k.a. 
logical hopping window (Q11)
select sensor, avg(temperature)
from TemperatureSensorEvent.win:time(4 seconds)
group by sensor
output snapshot every 2 seconds;

A way to control the flow
### 7. What’s the role of the followed by operator (->) in EPL? Is it possible to implement it in Spark Structured Streaming? Give an example that supports your explanation.
The followed by operator(->) let you see if an event is followed by another one in the stream. It is a temporal operator that operate on event order.
> TODO example

EPL tries to avoid the torrent effect. Can be implemented partially in Spark  Structured Streaming using every statement and where timer:within: every A -> every B where timer:within(10 sec). The timestamps  
### 8. Explain stream to stream joins. Given an example in EPL that illustrates the different results you obtain using an inner and a left outer join
The stream to stream joins are operations that search for events in both the streams(they require a time window), searching for some specific interaction depending on the type:
- Inner joins return results only if two corresponding records appears in the two streams in the time interval
- Left outer joins start a computation each time an event arrives on both streams but only save results if the event is on the left stream. The right stream events can be saved if and only if a corresponding event arrived on the left stream before
- Full outer stream will emit an output each time an event is processed in either stream. The join method will be applied to both elements if the window state already contains an element with the same key in the other stream. If not it will only apply the incoming element
Inner join query:
```EPL
select * 
from View#time(9 sec) as v 
inner join 
Click#time(9 sec) as c 
on v.id = c.id;
```
Left Outer join:
```EPL
select * 
from View#time(9 sec) as v 
left outer join 
Click#time(9 sec) as c 
on v.id = c.id;
```
### 9. Explain stream to table joins. Given an example in EPL that illustrates the different results you obtain using an inner and a left outer join
The stream to table joins use the unidirectional keyword to identify the stream that will provide the events to execute the joins. The other streams in the from clause become passive and events from them don't generate join results. This make the stream to table join asymmetric. The join is not windowed the left input stream is stateless thus join lookups from table records to stream records are impossible.
Inner join:
```EPL
select *
from View as v
	unidirectional inner join
	Click#unique(id) as c
	on v.id=c.id;
```
Left outer join:
```EPL
select *
from View as v
	unidirectional left outer join
	Click#unique(id) as c
	on v.id=c.id;
```
### 10. What makes Kafka scale horizontally? Explain it using an example.
Kafka can scale horizontally thanks to the suddivision of the topics in partitions in the brokers and the aggregation of consumers in consumer groups where we aggregate consumers interested in the same topics so that brokers can send the related topic to a single group faster. 
We can think of a highway where the brokers send the cars to the consumers. If we add more consumer to the consumer group we add more lanes to the highway scaling up the system.
You need to scale brokers and machines in a way that the brokers have direct and exclusive approach to the disks. The broker is a single thread, can only listen to one producer at a time.
### 11. What’s the role of a topic in Kafka? How does it relate to partitions and brokers? Explain it using an example.
A topic represent a stream of messages. These relate to the partitions and brokers as the brokers collect the messages and divide them in partitions in base of the topic to enable a faster access to them by the consumers. An example can be chefs publishing their recipes to a server. In the server these recipes are divided by the authors and the clients can request recipes from a specific author.
### 12. What’s the role of a broker in Kafka? How does it relate to topics and partitions? Explain it using an example.
The role of a broker is to store the information produced by the producers and to send them to the consumer. The brokers divide their information in partition based on topics to permit a parallel requests from the consumers related to different topics. 
> TODO trovare un esempio
### 13. What’s the role of a consumer group in Kafka? How does it relate to topics and partitions? Explain it using an example.
A consumer group is a logical aggregation of consumers all interested in the same topic. It relates to partitions as the partitions on different brokers can be accessed by different consumers in the consumer group at the same time or a consumer can access different partition on the same topic.
> TODO  example 

Play two roles: scalability(more consumer up to the number of partition) and fault tolerance as you can reassign the partition to another consumer.
Look for chaos monkey/ chaos engineering
# Exercises on EPL patterns
Suppose you receive the following stream of events: A1@0,C1@1,B1@2,B2@3,A2@4,B3@5,A3@6,B4@10.
Note that A3@6 denotes an event of type A identified by the number 3 that is received at time 6.
Given the patter: every A -> (B and not C where timer:within(3 sec))
Translate the pattern into an English sentence
Which are the events that trigger the matching? Why?
Which are the events that may trigger the matching but are excluded by the semantics of the every and the where timer:within clauses? Why?

Translation: for every event A that is followed by event B and not Event C within 3 seconds.
A1@0,B1@2  A2@4 , B3@5 as it's the only event where A is followed by B and not C where the time between the events is less than 3.
where timer:within doesn't let A3@6,B4@10 to trigger because the two events are too distant in time. The and operator exclude A1@0,C1@1 to be considered as A cannot be followed by event C.