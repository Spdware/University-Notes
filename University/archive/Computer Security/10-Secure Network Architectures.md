## Firewalls

**Firewall**: network access control system that verifies all the packets flowing through it. Its main functions are usually:
- IP packet filtering
- Network address translation (NAT)
Must be the single enforcement point between a screened network and outside networks.
It is a filter that can happens at each level and can do network address translation
> A firewall (construction) is a wall designed to partition a building and stop fire spreading, “Wall of fire” is a 4th level spell.

Should be a single point of contact between two network with different level of trust(usually inside the network and outside network)
### Firewalls are not Omnipotent
A firewall checks **all** the traffic flowing through it, and **only** the traffic flowing through it.

![](https://i.imgur.com/oVsmmA2.png)

Powerless against **insider attacks** (unless the network is partitioned somehow).

![](https://i.imgur.com/R09HksB.png)
![](https://i.imgur.com/2gKcXTX.png)

In general, powerless against unchecked paths:
- E.g. a modem or 5G connection of machine connected also to a LAN
These attacks can be either unintentional or intentional
### Firewalls are Computers
The firewall itself is a computer: it could have vulnerabilities and be violated.
Most of the times:
- It's a single purpose machine, an embedded appliance with just a firmware.
- Usually offers few or no services 
Smaller attack surface
Choose to maintain the minimum number of service available and possible do only physical access and keep the firewall updated.
### Security Policies = Firewall Rules
A firewall is a stupid “bouncer at the door”
- Just applies rules
- Bad rules = no protection
Firewall rules are essentially the implementation of higher-level security policies.
- E.g. “I want no clients to be able to download email from external email servers!”
Policy must be built on a default deny base:
- I.e., "deny all, except ..."
Usually commercial firewalls come with the rule pass all as some laws impose you to have a firewall, for some company the law specify a configured firewall.
Once setup is done firewall are really low maintenance.
### Firewall Taxonomy
We divide them depending on their packet inspection capability (from low to high layers).
Network layer firewalls:
- Packet Filters firewall (network layer)
- Stateful Packet Filters (SPF) firewall (network and transport layer)
Application layer firewalls:
- Circuit level firewalls (transport-application)
- Application proxies (application)
The richer the rules are the more time is needed for a packet to pass through the firewall.
Firewall is an HTTP interpreter and can decide to approve or not a request. Usually we use network layer firewall as they are the more cheap and faster.
## Packet Filters
Packet by packet processing.
Decodes the IP (and part of the TCP) header:
- SRC and DST IP
- SRC and DST port
- Protocol type
- IP options
Stateless
- Cannot track TCP connections.
- No (full) payload inspection.
Mostly found on routers (“ACLs” on Cisco)
No tracking if there is a sensible way of floating. 
Tracking connection is annoying
### Expressiveness
Predicate only on what we can decode. E.g.:
- Block any incoming packet ("default deny")
	 iptables -P INPUT DROP # P = policy
- Allow incoming packet if going to 10.0.0.1
	 iptables -A INPUT -d 10.0.0.1 -j ALLOW
- Block anything out except SMTP (port 25)
	 iptables -P OUTPUT DROP
	 iptables -A OUTPUT --dport 25 -j ALLOW
What happens with packets with spoofed IP?
Do not filter in pre or post routing as you do not know where the packet need to go(pre routing) or you didn't stop before why now?(post routing)
### Rule ~> Reaction
Regardless of the specific syntax, every network packet filter allows to express the
following concept:
if (packet matches certain condition) do this, e.g.:
- block
- allow
- log
- (other actions, e.g., rate limit)
### Stateful (or Dynamic) Packet Filters
Include network packet filters, plus:
- keep track of the TCP state machine
	 after SYN, SYN-ACK must follow
- we can **track connections** without adding a response rule.
- Make deny rule safer
Need to have a connection track table, track the TCP state machine and so we have easier rules to implements. Destination and Source address can be anyone as everyone can connect to the server.
#### Stateless Packet Filter
![](https://i.imgur.com/Nj3oTjg.png)

#### STATEFUL Packet Filter
![](https://i.imgur.com/33SDvMQ.png)

#### STATEFUL vs STATELESS Packet Filter
![](https://i.imgur.com/xuPBdLc.png)

Cost of a stateless packet filter is constant with the size of the routing table, a stateful packet filter cost is linear with the number of connections. 
### Stateful (or Dynamic) Packet Filters
Include network packet filters, plus:
- keep track of the TCP state machine
	 after SYN, SYN-ACK must follow
- we can track connections without adding a response rule.
- Make deny rule safer
CONS:
Performance bounded on a per-connection basis, not on a per-packet basis.
- The number of simultaneous connections are just as important as packets per second.
## Better Expressiveness and Pluses
Tracking (Logging and accounting on) connections.
Deeper content inspection:
- Reconstruct application-layer protocols
- Application-layer filtering, e.g. ActiveX objects in
HTTP responses disallowed.
Network Address Translation (NAT) offered as embedded feature. Firewall use connection tracking table to do that. So if you do NAT stateful packet filtering is almost free. 
**Packet defragmenting and reassembly** (helps avoiding pathological fragmented packets, e.g., teardrop). Reassemble on the firewall the IP packet, you have extra load in the table but is more secure against attacks.
UDP connection are tolerated for a timer, if there is no communication and the timer end the row is cancelled. 
## Session Handling
A session is an atomic, transport-layer exchange of application data between 2 hosts.
Main transport protocols:
- TCP (Transmission Control Protocol)
	 session =~ TCP connection
- UDP (User Datagram Protocol)
	 session = this concept does not exist
Session-handling is fundamental for NAT.
Heuristic that punches holes in NAT as NAT is not a security feature. For example as the NAT keep track of the UDP session anyone that spoof the UDP packet that you send can use that connection as they want. You can do private to private IP spoofing if the NAT is not smart enough to check the source address.
## Application-layer Inspection for NAT
Some protocols (e.g., DCC, RDT, instant messengers, file transfer) transmit network
information data (e.g., port) at application layer.
For instance, FTP uses dynamic connections:
- Allocated for file uploads, downloads, output of commands
- "PORT" application command 
Stateful firewalls must take care of these.
Application layer protocol should have no knowledge on what happens outside them. FTP can be broken if we do not look inside the port that is used. 
Use FTP only on passive mode so you only have outgoing connection from the client and maintain the connection to the server that give you a port where it is expecting connections. 
## Circuit Firewalls (Legacy)
Relay TCP connections.
Client connects to a specific TCP port on the firewall, which then connects to the address and port of the desired server (not transparent!).
Essentially, a TCP-level proxy.
Only historical example: SOCKS
They do not only keep track of an IP connection but keep the entire connection. Do connection tracking at application level. Allow more flexibility in handling thing but is now dead as if the connection is not using TCP/IP connection and use a network overlay and use another network protocol.
SSH client is a full SOCKS proxy having a connection to the server.
## Application Proxies
Same as circuit firewalls, but at application layer.
Almost never transparent to clients:
- May require modifications
- Each protocol needs its own proxy server
Inspect, validate, manipulate protocol application data (e.g., rewrite HTTP frames)
Inspecting the application layer weed out a lot of attack but is a privacy problem as you are reading the traffic in clear and all the traffic.
### Functionalities & Additional Benefits
Can authenticate users, apply specific filtering policies, perform advanced logging, content filtering or scanning (e.g., anti-virus/spam).
Can be used to expose a subset of the protocol:
- to defend clients,
- to defend servers (“reverse proxy”).
Usually implemented on COTS OSs.
## Deep Inspection and Intrusions
Modern firewalls can analyze packets and sessions even more in depth.
- Recognize MIME multipart attachments in SMTP flows and send data to antiviruses
- Recognize a et of known attack packets patterns to be blocked
# Architectures for secure networks
## Dual- or Multi-zone Architectures
In most cases, the perimeter defense works on the assumption that what is “good” is inside, and what's outside should be kept outside if possible.
There are two counterexamples:
- Access to resources from remote (i.e., to a web server, to FTP, mail transfer) .
- Access from remote users to the corporate network.
**Problem**: if we mix externally accessible servers with internal clients, we lower the
security of the internal network.
**Solution**: we allow external access to the accessible servers, but not to the internal
network.
**General idea**: split the network by privileges levels. Firewalls to regulate access.
In practice, we create a semi-public zone called DMZ (demilitarized zone) .
The DMZ will host public servers (web, FTP, public DNS server, intake SMTP).
On the DMZ no critical or irreplaceable data. The DMZ is almost as risky as the Internet.
DMZ are zones that are not as untrustworthy as zones outside the firewall but they are not as secure as the zones inside the firewall. 
### Example DMZ + Private Zone
![](https://i.imgur.com/q7mtdzW.png)

Typically outbound servers are the ones that only talk to the outside of the network so no one should try to talk to them from the outside. 
