1. Bluetooth is an industrial specification for WPANs
2. The WG 802.15.1 adapted the industrial specifications of Bluetooth for the levels 1 and 2
3. ’96-’97: Ericsson internal project
4. ’98: Bluetooth SIG created (Ericsson, IBM, Intel, Toshiba, Nokia)
5. ’99: new members join the SIG (3Com, Lucent Technologies, Microsoft, Motorola)
## Bluetooth characteristics
- Radio technology
- Low cost
- Small range (10-20 m)
- Low complexity
- Small size
- ISM 2.4 GHz band
- Created by an industrial consortium
- Only the first two levels have then been standardized by IEEE 802.15.1
The protocol was created to unify short range communication protocols between different devices of different manufacturers.
Works in an unlicensed part of the frequency bands. Bluetooth range is in the range of 10-20 meters outdoor, indoor is terrible.
## Application scenarios
- Headset/Audio
- Data transfer
- Connectivity sharing
- Proximity marketing (BLE only)
Nowadays is used for audio and proximity marketing.
## Physical layer
Differently from other technologies in its frequency range it uses frequency hopping. The transmitter and receivers hops on a different channel to avoid jamming signals.
1600 hops/s (625 μs per hop)
The FH sequence is pseudo random and determined by the clock and the
address of the ‘master’ device that regulates the access to the channel
The other devices are ‘slaves’ and follows the sequence $f_k$ defined by the master
This continuous jumps let you have a more resilient transmission as, even if some channels are jammed, you can have a continuous transmission.

![](https://i.imgur.com/wt9lTg1.png)

Usually bluetooth is used im a one to one communication, but a bluetooth network can be composed up to 8 devices(1 master and 7 slaves).
It is a polling strategy, the first slot is always occupied by the master.
## Piconet
The simplest network architecture defined in Bluetooth is called piconet
The piconet is an ad hoc network composed of 2 or more devices
A device acts as master and the other as slaves
Communication can take place only between master and slave and not directly between slaves
Up to 7 slaves can be active in a piconet
The others can be in
- Stand-by (not part of the piconet)
- Parked (part of the piconet but not active, up to a maximum of 256 devices)

![](https://i.imgur.com/a3Ap2L5.png)

## Types of connections
Bluetooth considers two types of connections
SCO (Synchronous Connection Oriented)
- Fixed rate bi-directional connection (circuit)
- FEC for improving quality
- Rate of 64 Kbit/s
ACL (Asynchronous ConnectionLess)
- Packet switched connection shared between  master and active slaves based on a polling access scheme
- Several options for packet formats and physical layer codes (1, 3, 5 slots)
- Rate up to 433.9 Kbit/s symmetric (using 5-slot packets in both directions) and 723.2/57.6 Kbit/s asymmetric (using 5-slot packets in one direction and 1-slot packets in the other)

![](https://i.imgur.com/mrnZFVq.png)

## Protocol Architecture
![](https://i.imgur.com/lrDbzqQ.png)

### Packet format
![](https://i.imgur.com/DILsOid.png)

Access code is used to identify who is transmitting.
BT packet includes three parts:
- The access code used for the synchronization and the piconet identification
- The header used for the Link Control (LC) which includes also the retransmission scheme 
- The payload whose format depends on the type of connection and the type of packet (number of slots, PHY protection, etc.)
### Link controller: states
![](https://i.imgur.com/mm0nzyO.png)

 **Stand-by**: the devices is not active and the radio is off
**Connection**: the device is connected with other devices. This state includes other
sub-states
**Inquiry**: the device is looking for other devices in range
**Inquiry Scan**: the device is listening for inquiry requests during small time intervals
(low duty cycle).
**Page**: the device is trying to create a piconet connecting to another specific device with known address
**Page Scan**: the device is listening to the channel for page requests for small internal of time
Inquiry and inquiry scan are used to gain information about other devices. Then you can use page and page scan to become a master and starting a communication.
### Page procedure
- If a device wants to connect to another device of which it knows the address, it runs the page procedure
- From the address, it calculate the Device Access Code (DAC)
- A stand-by device enters periodically into a page scan mode e starts listening for its DAC on the channels
- Due to ISM band usage rule, the page procedure cannot be executed on a fixed channel
- The device in page scan mode follows a pseudo random sequence on 32 channels (frequencies)
-  In order to limit the energy consumption, the page scan is executed for 10 ms on a channel and then the device enters sleep mode for a period of the order of few seconds (from 1.28 to 3.85 s)
- At each new scan period, a new channel is selected according to the pseudo random sequence
- The device in page can calculate the sequence but it usually does not know the phase (clock)
- Therefore, it transmits the DAC sequentially on all channels

![](https://i.imgur.com/Szb4XgU.png)

- The reply is actually the transmission of the DAC itself
- In most of the cases the connection is established within 2 sleep periods
- A third packet is sent by the device in page
- It is the FHS packet which includes all the information on the device, including the clock
- The connection is now active
- The device that was paging takes the role of master and the device that was scanning takes the role of Slave
- An AMA is assigned to the Slave
## Profiles
Basic operation modes characteristic of different applications
Used to guaranteed interoperability at application layer
Bluetooth specify application profiles that are rules that the developer needs to implement to let its device to communicate with the others.
![](https://i.imgur.com/Ajf1fPn.png)

## Bluetooth v2.0
- v2.0 in 2004, v2.1 in 2007
- Adaptive Frequency Hopping (AFH) – v1.2
- extended Synchronous Connections (eSCO)
- Multicast/Broadcast
- Enhanced Data Rate (EDR) – rates up to 3 Mb/s using Differential encoded Phase Shift Keying (DPSK) with 4 and 8 symbols (same band)
## Bluetooth v3.0
- v3.0 in 2009
- Rate up to 24 Mb/s
- ... but using a different MAC/PHY, which is actually WiFi
- BT is basically used only for the negotiation of the connection parameters among the two devices
## Bluetooth v4.0
- v4.0 in 2010
- New revised version of
- Classic BT (Basic Rate, BR)
- BT high speed (EDR)
- And new BT low energy (BLE)
- It’s a new stack that incorporates the work of the WiBree working group; New commercial name is Bluetooth Smart o Low rate (260 kb/s), short range, low power for sensor and small devices o Potential competitor of ZigBee
## Bluetooth Low Energy (BLE)
- Not replacing, but sitting alongside Bluetooth BR/EDR
- Designed to be energy efficient
- Tailored to IoT applications

![](https://i.imgur.com/iFIrA5o.png)

Still remains a complex protocol. But there is a simplification of the stack using the attribute profiles.
### BLE Physical Layer
Same ISM band used in BR/EDR, but:
- 40 2MHz channels
- 3 channels for advertisement/broadcast(messages sent on specific frequency channels, very useful as they enable proximity markets and others short range applications)
- 37 for data transfer via adaptive frequency hopping
Not compatible with BR/EDR!
- GFSK modulation
- Three PHY types:
LE 1M (1Mbps), no FEC, mandatory
LE 2M (2Mbps), no FEC, optional
LE Coded (0.5/0.125Mbps), FEC, optional. Used for extending range 2x or 4x
### BLE Link Layer
Governed by a state machine
Duties:
- Manage packet types
- Addressing
Public / Random
- Channel management
Adaptive Frequency hopping
- Logical link management
LE ACL
Packet ordering/ACK through SN/NESN

![](https://i.imgur.com/gZW4M65.png)

### Central/Peripheral nodes
Similar concept to master/slave
Peripheral nodes transmit advertisments periodically (20ms – 10s)
Centrals scan the adv channels for advertisements
- scan window: for how long to scan
- scan interval: how often to scan
### BLE AFH
BLE uses Adaptive Frequency Hopping on the 37 data channels
Central node selects:
- Channels to use (avoiding the ones with interference). Channel map is shared with peripheral node.
- Hopping algorithm
	Algorithm #1: sequential selection with a fixed hop interval passed during connection
	Algorithm #2: semi-random sequence calculated from address/internal counters
### Advertising types and connections
Directed/undirected: accepts connections only from known devices/all
Connectable/not-Connectable: allows connections or not
Scannable/non-Scannable: allows to receive Scan requests and reply
### BLE Connections
A central can issue a Connection Request to a peripheral sending
connectable advertisements
The peripheral replies with a Connection Reply. Now the two BLE
devices are connected and can exchange data
### BLE Data exchange
Two connected devices (client/server) use the Attribute Protocol (ATT)
The ATT protocol allows to:
- Discovery attributes (resources) available on the server
- Read / Write operations on such attributes
## Bluetooth v5.0
Main improvements over BLE
- 4x range (up to 200m) using lower transmission rates (250 or 125 kbps)
- Max speed 2Mbps
- Mesh topology
This bring bluetooth near other protocols such as ZigBee.