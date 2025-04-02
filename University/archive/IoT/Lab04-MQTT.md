# MQTT in a nutshell
Publish/Subscriber protocol, optimized for IoT and unreliable, low-bandwidth, high-
latency networks. It is extremely lightweight. Easy client-side implementation. Data agnostic. Few methods: publish, subscribe, unsubscribe.
The topic can be created right before the message is sent.
## MQTT actors
One broker: central entity that distribute the data
Many client:
- Publisher: the one that create data for the various subscriber
- Subscriber: clients that request to be informed when data for a certain topic is published

![](https://i.imgur.com/2FxhsYq.png)
![](https://i.imgur.com/XjpWOWu.png)

### Brokers on the cloud
Addresses:
• test.mosquitto.org
• broker.hivemq.com
Standard Ports:
• 1883
• 8883
## Mosquitto clients: SUBSCRIBER
Subscribe to a topic: mosquitto_sub
• -h “host_name” (default localhost)
• -p “port” (default 1883)
• -t “topic_name”
• -q “qos”
• -d ”debug”
• -v “verbose
**Example**: mosquitto_sub –h localhost –t "/living_room/temperature" -v -d
## Mosquitto clients: PUBLISHER
Subscribe to a topic: mosquitto_pub
• -h “host_name”
• -p “port”
• -t “topic_name”
• -m “message_content”
• -q “qos”
• -d ”debug”
• -r “retain”
**Example**: mosquitto_pub –h localhost –t "/living_room/temperature" –m "33.3" -d
# Topic
• String that the broker uses to filter messages
• Identifies a resource
• Hierarchical structure 
## Topic and Wildcards
Wildcards:
- Single-level: home/groundfloor/+/temperature (to subscribe to all the temperature reading in all the room of the ground floor). Can go everywhere in the string and can match anything
- Multi-level: home/groundfloor/# (to subscribe to all the readings of the ground floor, not only the temperature in the living room). Can go only at the end of the string and match every layer.
Single level: +
- Can be used in the middle or at end of the topic
- Only one level
Multi level: #
- Take any number of levels
- Used only at the end of the topic
Cannot publish with a wildcard as you need to specify all the camps in the topic identifier
## MQTT Subscribe/Unsubscribe
![](https://i.imgur.com/wYja2Ze.png)

## QoS
MQTT needs to implements way to know if a packet arrives at destination as the broker can not be available.
QoS 0: at most once
- Doesn’t survive to failure
- No duplicates

![](https://i.imgur.com/R4bp7C9.png)

QoS 1: at least once
- Survives connection loss
- Duplicates

![](https://i.imgur.com/M3t4EDj.png)

QoS 2: exactly once
- Survives connection loss
- No Duplicates

![](https://i.imgur.com/LlTK1aj.png)


| Publisher QoS | Subscriber QoS | Resulting Sub QoS |
| ------------- | -------------- | ----------------- |
| 0             | 0,1,2          | 0                 |
| 1             | 0              | 0                 |
| 1             | 1,2            | 1                 |
| 2             | 0              | 0                 |
| 2             | 1              | 1                 |
| 2             | 2              | 2                 |
The Subscriber can choose the MAXIMUM QoS value to support. The resulting QoS for the sub  is the MINIMUM value between $QoS_{SUB}$ and $QoS_{PUB}$ 
PINGREQ and PINGRESP permit the confirmation of the QoS 1 and 2.
Subscriber can only decrease the QoS from the one of the publisher. This is done to assure a specific type of delivery and communication survive packet loss.
## Retain message
Messages are normally discarded by the broker if no one is subscribed to that topic
Using retain flag the broker saves the last message on that topic
When a subscriber subscribe on the topic, the broker deliver the message
mosquitto_pub –t “topic” –h localhost –m “my message” **-r**
If the retained message is empty you are saying to delete the retained message.
## Persistent Session
Persistent session stores session status and messages with QoS 1 and 2 that are still to be transmitted or acknowledge
Setting the clean session flag to 0, the broker will reuse the previous session when connecting (It is mandatory to specify the client ID with –i )
mosquitto_pub –t “topic” –h localhost –m “my message” –i 
“pub_cli” – q 1 – c

mosquitto_sub –t “topic” –h localhost –i “sub_cli” – q 1 – c
## Last Will message
Used to notify subs of an unexpected shut down of the publisher
When the broker detects a connection break it sends the last will msg to all subs of a topic
Normal disconnect: NO msgs
Abnormal disconnect: Send Last Will
During the connection the publisher will specify the last will and if the broker detects something is gone wrong it will publish the last will message.
## $SYS topics
Topics created by the broker to keep track of the broker’s status
- Starts with $SYS
- Automatic periodic publish
Some examples:
- $SYS/broker/clients/connected
- $SYS/broker/messages/received
- $SYS/broker/uptime
## CoAP vs MQTT
![](https://i.imgur.com/0H9ZKrv.png)

