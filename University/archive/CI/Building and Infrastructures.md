It is the infrastructure that contains our datacenters. The problems we face are cooling, recovery and failure and power distributions.

![](https://i.imgur.com/KW9nFJg.png)

![](https://i.imgur.com/8erFStn.png)

DATA CENTER POWER SYSTEMS: In order to protect against power failure, battery and diesel generators are used to backup the external supply. The UPS typically combines three functions in one system:
- contains some form of energy storage (electrical, chemical, or mechanical) to bridge the time between the utility failure and the availability of generator power
- contains a transfer switch that chooses the active power input (either utility power or generator power)
- conditions the incoming power feed, removing voltage spikes or sags, or harmonic distortions in the AC feed
DATA CENTER COOLING SYSTEMS: IT equipment generates a lot of heat: the cooling system is usually a very expensive component of the datacenter, and it is composed by coolers, heat-exchangers and cold water tanks.
Free cooling, i.e., open-loop, refers to the use of cold outside air to either help the production of chilled water or directly cool servers. It is not completely free in the sense of zero cost, but it involves very low-energy costs compared to chillers
Closed-loop systems come in many forms, the most common being the air circuit on the data center floor
- The goal is to isolate and remove heat from the servers and  transport it to a heat exchanger
- Cold air flows to the servers, heats up, and eventually reaches a heat exchanger to cool it down again for the next cycle through the servers

![](https://i.imgur.com/f7bUuY1.png)

![](https://i.imgur.com/PrOsiwj.png)

Each topology of cooling system presents tradeoffs in complexity, efficiency, and cost: 
- Fresh air cooling can be very efficient but does not work in all climates, requires filtering of airborne particulates, and can introduce complex control problems
- Two-loop systems are easy to implement, relatively inexpensive to construct, and offer isolation from external contamination, but typically have lower operational efficiency
- A three-loop system is the most expensive to construct and has moderately complex controls, but offers contaminant protection and good efficiency
#### Container based Datacenter
![](https://i.imgur.com/yBMyvVY.png)

Datacenter builded inside a container in so you can have everything in a confined space. This solution can also be moved freely as it is capable to be contained in a container.
### Datacenter consumption
The datacenter consumption is really high, the power needed and $CO_2$ emitted is similar to the one of England. 
We use a metric for measure the impact of a datacenter:
$$PUE=\frac{Total FacilityPower}{ITEquipement power}$$ The datacenter efficiency is measured as DCiE and is the inverse of the PUE.

![](https://i.imgur.com/UlzpM5r.png)

### Datacenter Tiers

![](https://i.imgur.com/GBccQBS.png)

