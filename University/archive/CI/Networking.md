Various way to implement networking in data center: Switch-centric architectures, Server-centric and hybrid architectures. 
At the start when computers become usable, everything was done inside the machine except some periferical devices. Minimal network demands, proprietary devices.
First network technology was applied to a LAN then, when it was needed to exchange data in long distance, it shifted to TCP/IP+Proprietary protocols.
The next step was the introducing of Web Applications and access from anywhere. Servers are broken in multiple units.
Then we passed to microservices where infrastructure moved to cloud providers, increases in server-to-server traffic, Datacenter are born. 

![](https://i.imgur.com/hQtRAnW.png)

In the networking part you have to increase the interfaces and the capacity behind the interfaces to utilise everything at the maximum efficiency. 
All the traffic in the front panel has to be transferred somewhere(You have to upgrade the front and the back together). 
BIsection Bandwidth: bandwidth across the narrowest line that equally divides the cluster into two parts. It characterizes network capacity since randomly communicating processors must send data across the middle of the network.

![](https://i.imgur.com/oAVFMKl.png)

### Classes of DCN(Data Center Network)
DCNs can be classified into three main categories:
- Switch-centric architectures: Uses switches to perform packet forwarding
- Server-centric architecture: Uses servers with multiple Network Interface Cards (NICs) to act as switches in addition to performing other computational functions
- Hybrid architectures: Combine switches and servers for packet forwarding
Creates their traffic when communicating but they will also handle the communication between the elements in the network. You can also virtualize switches having the capability of a switch but it is implemented in a specific CPU inside the network interface
### Switch centric architecture

![](https://i.imgur.com/aIORNNx.png)

Two traffic flows direction: 
- Northbound-Southbound: communication between server and internet
- East-West: traffic consistent, communication between data center. it also include all the network function virtualization. It can be of different type: unicast(point to point communication, VM migration, data backup, stream data processing), multicast(one-to-many communication, software update, data replication (≥ 3 copies per content) for reliability, OS image provision for VM), incast(many-to-one communication, reduce phase in MapReduce, merging tables in database)
#### Classic three tier architecture

![](https://i.imgur.com/dAmVwAR.png)

When you go to the top you start aggregating traffic so you need more capacity. 
This architecture handles link between layers and between the same layer. 
Each switches of a level is connected to multiple switches of the next layer.

![](https://i.imgur.com/cE7wpeN.png)

![](https://i.imgur.com/TASh5YK.png)

Structure frequently used as the interconnection used is one for every racks and one for every switch. One alternative is to skip one layer to save on switches

![](https://i.imgur.com/xyqPUCC.png)

You need different type of switches for every layer, the cost of this architecture is very high as it requires very efficient switches. This is a problem as cost in term of acquisition, management, spare part stocks and energy consumption.
### Leaf Spine Architecture
Leaf and Spine layer:
- Leaf layer is the equivalent of the Top of Rack layer
- Spine layer is the equivalent of the Core and Aggregation layer

![](https://i.imgur.com/SfU1jP2.png)

No interconnection between the switches of the same layer to simplify the paths of the data.

![](https://i.imgur.com/0lpaBtR.png)

The set of links is determined by the number of matrixs in the stage. 
Let k be the number of middle stage switches(ensure that you have the bisectional bandwidth related to the number of server connected)
Let n be the number of input and outputs of switches of side stages(exploited by Leaf and Spine architecture)
If k ≥ n there is always a way to rearrange communications to free a path between any pair of idle input/output
If k ≥ 2n - 1 there is always a free path between any pair of idle input/output 
Notice that t is a free design parameter so the total number of input/output $N=t*n$ can scale freely (by increasing the size of middle-stage switches). But a DCN is a PACKET-SWITCHED network!!
If you always find a free path the architecture is called Freeblocking, if you find a path but it is used you can rearrange the connection and this architecture is called Moving Block architecture.

![](https://i.imgur.com/tJTcwVv.png)

In the folded architecture there are only two stages as the last one is collapsed on the middle one.
The advantages is also we can use a single type of switch, control protocol, interfaces and so on. This resolve the problem seen in the 3 tier model. Advantages also in routing as we can use protocol using multiple routing cost path exploiting all the bisectional bandwidth of the network. 
If we have a great number of servers we can reintroduce the 3-tier architecture but with some modification as using a Two tier Clos using four port switches. This model is a PoD based model aka the Fat Tree

![](https://i.imgur.com/KkL2vbp.png)

A PoD is a Point of Delivery, a module or group of network, compute, storage and application components that work together to deliver a network service. It is a repeatable pattern, its components increase the modularity, scalability and manageability of data. Full interconnectivity if you consider the pod, it is single connectivity if you consider the switch.  
### Fat Tree network
At the edge layer there are 2k PoDs each with $k^2$ servers. Each edge switch is directly connected to k servers in a pod and k aggregation switches. A fat tree network with 2k-port commodity switches can accomodate $2k^3$ servers in total. k^2 core switches with 2k-port each, each one connected to 2k pods. Each aggregation switch is connected to k core switches(Note the partial connectivity at switch level)

![](https://i.imgur.com/wWhiUvK.png)

An example of a cost effective hierarchical fat tree based DCN architecture with high bisection bandwidth is VL2 Network. It uses three types of switches: intermediate, aggregation, and top-of-rack (ToR) switches. It uses $D_A/2$ intermediate switches $D_I$ aggregation switches and $D_A*D_I/2$ ToR switches. The number of servers in a VL2 network is $20(D_A*D_I)/2$.  It uses a load-balancing technique called valiant load balancing (VLB).

![](https://i.imgur.com/84E6pUq.png)

