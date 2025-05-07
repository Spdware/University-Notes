## RFID in Nutshell
To Enhance the concept of “bar-codes” for faster identification of assets (goods, people, animals)
Ingredients:
- Transition to electronic bar codes with wireless communication capabilities
- Transition form optical to wireless “readers”

![](https://i.imgur.com/Z4tb3Kt.png)

A RFID system in a nutshell is a way to automatically read identification information from an object.
Use an external signal and reply back with some information.
## RFID Building Blocks
![](https://i.imgur.com/tl1d98R.png)

The systems are completely passives. Some components(tags) could have batteries but they are used for very little load. The reader is a very complex machine, very costly and powerful, consist of different components and with a very high level transmission module
### Types of Tags (by power supply)
- Passive: Operational power scavenged from reader radiated power. Short range (<1m), Low cost Self sustaining
- Semi-passive: Operational power provided by battery. Medium range (tens of meters), need battery, average cost, long life
- Active: Operational power provided by battery - transmitter built into tag. High range (hundreds of meteres), need battery, Limited lifetime
All the power needed for passive objects is scavenged from the power emitted by the reader. They are useful as they do not need a battery but can be used only in very short range communication(credit card readers)
Semi-passive uses a battery as it needs to transfer information in a short range that is too much to be covered by the energy transmitted by the reader. Still stays in sleep mode forever until it receives a signal from the reader(Example: Telepass)
Active systems we are moving to the wireless sensors world.
### Types of Tags (by storage)
1-bit tags: used in EAS systems
- realized with magnetic material
- when tag gets close to reader, current variation is perceived at the reader
- multple detection impossible
Tags with storage space
- Read only
- Read/write
RX/TX/modulation/demodulation circuitry 
Unique Identifier

## Tag classification
![](https://i.imgur.com/TgF4F1a.png)

As you move to higher classes the capabilities in term of transmission increase.
### The Tags
Tags can be attached to anything:
- pallets or cases of product
- Vehicles
- company assets or personnel
- People or animals
- Electronic appliances
Implementation challenges
Effective Energy Scavenging
Miniaturization/customization
Cost
There are implementation challenges, specially on the protocol layer.
## Reader Radiography
![](https://i.imgur.com/AXodBZs.png)

Most of the time the OS can resolve time information and transmit them using the classical Internet Protocols.
### Reader Implementation Challenges
Reader must deliver enough power from RF field to power the tag
Reader must discriminate backscatter modulation in presence of carrier at same frequency as the answers are transmitted using so little power that could be misinterpreted.
High magnitude difference between transmitted and received signals 
Integration with enterprise solutions
### RFID Backend
Middleware solutions to:
- manage high data volume produced by readers
- filter the data produced by the readers (remove redundancy, eliminate unwanted data, etc.)
- Store the data in a way that is meaningful for the specific application
- Let different readers be interoperable
### Usage Models
![](https://i.imgur.com/3jjvJ3u.png)

### Performance Measures
- Reading Range: how far we can read
- Throughput: how fast we can read
- Robustness: how robust the reading process
Performance depends on:
- Carrier frequency
- Emitted power
- Environment (propagation conditions)
- Concurrency (# of tags to be read simultaneously)
### RFID: Spectrum Snapshot
![](https://i.imgur.com/XFSDukU.png)

Most of the time we will use a non licensed carrier frequency.
### RFID Standards
Different standards available in the fields of:
- animal identification
- cards and personal identification
- containers ID
Standards dedicated to item management application developed by ISO/IEC

![](https://i.imgur.com/n8KWXFM.png)
![](https://i.imgur.com/RwXvhoV.png)

With this standardization you can have a company differentiate between almost 7000000 different pieces
### RFID: Physical Communication
![](https://i.imgur.com/sGXtk4U.png)

Depending on the carrier frequencies of the carrier you will have different regimes.
#### RFID HF – Inductive coupling
Inductive Coupling between two circuits (reader and tag)
Frequency Range 125kHz o 13,56 MHz
Reading range comparable to coil diameter Functioning modes:
- Duplex (concurrent charging and transmission) 
- Sequential (charging and transmission decoupled)
Tag modulates its resistive load with the info to be sent
Voltage changes (modulated by the tag information) are triggered at the reader
### RFID HF – Qualitative performance overview
Tag activation voltage (reading range) depends on:
- Size of coil antennas (the larger the better)
- Tag/reader orientation
- Tag/reader chip hardware
In LF/HF magnetic field is scarcely affected by dielectric materials
Limited reading range BAD
Very sensitive to orientation BAD
Almost immune to the environment GOOD
Moderate cost GOOD
We care about the orientation as it can influence the intensity of the magnetic field.
### RFID UHF/SHF – Electromagnetic coupling
Far field operation model
Energy/power is scavanged by the tag through EM waves emitted by reader
Transmission happens via backscattering by modulating the impedence
Tens of meters of read range
Bipolar antennas (few centimeters)
The reader will need to handle two types of signals that almost overlap:
- a strong signal that it will send to the tag
- a really small signal that comes from the tag
#### RFID UHF - Qualitative performance overview
High reading range GOOD
High bit rate GOOD
High impact of the environment BAD(this is due it uses an electromagnetic field)
## General Problem: Tag Identification/arbitration/singulation
The reading process can either be continuous or manual(reader gun).
If you have a reader with a trigger there could be multiple tags that will reply to the signal(collisions). So we need to solve collisions, need some sort of arbitration/ the tag needs to transmit its information without collide with the others(Medium Access Control).
### Tag Arbitration Peculiarities
Similar to classical Access Control but:
- Fixed unknown population size(scheduling is difficult)
- Tags cannot implement complex protocols (E.g., carrier sense is out)
- Often reader-driven algorithms
### Collision Arbitration Mechanisms: A Classification
Vertical Classification
ALOHA-like access mechanism
- Slotted ALOHA
- Dynamic Frame ALOHA
Tree-based access mechanisms
- Binary Tree
Horizontal Classification
- Centralized/Distributed
- Type of Channel Feedback (S,C,0) Indication if the transmission of a tag is received by a reader
#### Tag Arbitration Efficiency
The efficiency is commonly defined as the tag population size, N, over the length of the arbitration period L(N)
$$Efficiency:\mu =\frac{N}{L_N}$$
### The Frame ALOHA
Extension of the ALOHA protocol where nodes are allowed to transmit once every frame
Frame composed of r slots
Every tag chooses a slot in the frame
If transmission is failed, retry at next frame

![](https://i.imgur.com/YgMrWCr.png)

#### Frame ALOHA: Single Frame
The average throughput is: 
$$E[S]=n(1-\frac{1}{r})^{n-1}$$
Thus, the efficiency is:
$$\mu = \frac{E[S]}{r}=n\frac{1}{r}(1-\frac{1}{r})^{n-1}$$
Which is maximum for r = n:
$$\mu_M(n)=(1-\frac{1}{n})^{n-1}$$
#### Frame Aloha: Multiple frames
The FA efficiency depends on the initial tag population (N), the current backlog (n) and the frame size (r).
Current Frame size r is dynamically set to the current backlog n -> Dynamic Frame Aloha

![](https://i.imgur.com/FJkNpFU.png)

$$Efficiency:\mu =\frac{N}{L_N}$$
The average tag resolution process can be recursively calculated as:
$$L_n=r+\sum_{i=0}^{n-1}P(S=i)L_{n-i}$$
Which leads to:
$$L_n=\frac{r+\sum_{i=0}^{n-1}P(S=i)L_{n-i}}{1-P(S=0)}$$
### Problem
Initial population N and backlogs n are not known
Tag arbitration is actually composed of two modules:
Backlog Estimation Module: to provide and estimate of the backlog nest
Collision Resolution: run Frame Aloha with r= nest

![](https://i.imgur.com/6dGEYrt.png)

### Schoute’Estimate
Assume that any procedure is able to keep the frame size r equal to the current backlog n
Under this assumption, the number of terminals transmitting in a slot is approximated by a Poisson process with intensity 1 [terminal/slot]
The average number of terminals in a collided slot can be consequently calculated as:
$$H=(1-e^{-1})/(1-2e^{-1} )=2.39$$
The backlog is estimated as:
$n_{est}$$=round(Hc), being c the number of collided slots.
### The Binary-Tree
Random Numbers are used to partition the set of colliding tags

![](https://i.imgur.com/kNQHGjH.png)

#### The Binary Tree Implementation
Tags have counters set to 1
The reader broadcasts
Trigger command: sent at the beginning and after successful/empty slots
	tags decrease their counter and transmit if counter is 0
Split commands: sent after collided slots
	tags with counter equal to 0 randomly choose a new counter value in [0,1]
	Tags with counter greater than 0 increase their counter

![](https://i.imgur.com/3XF9deX.png)

#### Binary Tree: Optimizations
More refined feedbacks can be used to steer the splitting
Leverage tag population estimates to steer splitting
In some slots collisions are certain, use Split command other than Trigger one

![](https://i.imgur.com/QMDoeOR.png)
