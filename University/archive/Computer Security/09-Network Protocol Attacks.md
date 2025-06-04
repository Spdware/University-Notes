## Network-level Sniffing
Normally, a network interface card (NIC) intercepts and passes to the OS only the packets directed to that host's IP
**Promiscuous mode**: the NIC passess to the OS any packet read off of the wire
**DSniff** tool www.monkey.org/~dugsong/dsniff ARP spoofing, MAC flooding, sniffing
More modern, complete and complex: https://www.bettercap.org/
HW availability is not a problem as is the same you use for your network, as SW you can use Wireshark. 
Promiscuous Mode: mode of network cards that let you listen to all the communication that pass through them, pass every frame that is seen on the card. You can do sniffing on the network card.
## Sniffing vs time
Originally: ethernet networks were on a shared media (BNC cable)
RJ-45 cables changed shape but the medium was still shared because they ended in hubs 
**Hubs** broadcast traffic to every host in broadcast domain
**Switches** selectively relay traffic to the wire corresponding to the correct NIC (Eth address based)
	Performance, not security reasons
Problems is that there were a lot of collisions, so the performance dropped.
There is a lookup table of addresses we have seen and know how to reach(even if partially). 
## ARP Spoofing (& Cache Poisoning)
The ARP maps 32-bits IPv4 addresses to 48-bits
hardware, or MAC, addresses.
- ARP request "where is 192.168.0.1?"
- ARP reply  "192.168.0.1 is at b4:e9:b0:c9:81:03"
First come, first trusted! An attacker can forge replies easily: lack of authentication.
Each host caches the replies: try arp -a
Address Resolution Protocol: let you have an IP address and send to the corresponding MAC address the message.
ARP replies are FCFS: the first response is the one that will be used, no check if it is the correct client. So I can forge an ARP reply and send the unsolicited ARP reply to a single host that I want to disturb or, far worse, to whatever number of machine I want. No one asked but everyone will listen and cache it and continue to send for not being forgotten. I can literally hijack the internet traffic as I am flooding the ARP replies, you will see the network traffic coming to you, you can become a gigantic switch. 
### Possible Mitigations
- Check responses before trusting (if they conflict with exsiting addresses mappings)
- Add a SEQ/ID number in the request
## CAM Table
Switches use CAM tables to know (i.e., cache) which MAC addresses are on which ports
Switches are just as vulnerable to ARP spoofing!
### Filling up a CAM Table
Switches use **CAM tables** to know (i.e., cache) which MAC addresses are on which ports
**Dsniff (macof)** can generate ~155k spoofed packets a minute: fills the CAM table in seconds (MAC flooding)
**CAM table full:** cannot cache ARP replies and must forward everything to every port
(like a hub does).
**Mitigation**: PORT Security (CISCO terminology)
So you just need to fill up a CAM table to be able to sniff on all the ports. You can fill up a CAM table with a script that sends meaningless frame to the switch with different addresses so they are registered in the table. This can be avoided with some logic in the switch that control how many MAC are coming from a port(avoid MAC flooding), if this happens turn off the port. 
## Abusing the Spanning Tree Protocol
The STP (802.1d) avoids loops on switched networks by building a spanning tree (ST).
Switches decide how to build the ST by exchanging BPDU (bridge protocol data unit)
packets to elect the root node.
BPDU packets are not authenticated, so, an attacker can change the shape of the tree for sniffing or ARP spoofing purposes.
You can create STP frames with the content you want. You want to convince all the connected switch to send you frames(can become the backbone switch, problem is that all the traffic goes through you, usually you are on the uplink port of the switch). 
## IP Address Spoofing (UDP/ICMP)
The IP source address is not authenticated.
Changing it in UDP or ICMP packets is easy.
However, the attacker will not see the answers (e.g., he/she is on a different network), because they will be sent to the spoofed host (blind spoofing).
But if the attacker is on the same network, s(he) can sniff the rest, or use ARP spoofing.
For TCP it is not the same as there is a continuous pingpoint of message ACK when I can only send and not receive.
### IP Address Spoofing (TCP): TCP Sequence Number Guessing
TCP uses sequence numbers for reordering and acknowledging packets.
A semi-random Initial Sequence Number (ISN) is chosen.
### TCP Sequence Number Guessing
TCP uses sequence numbers for reordering and acknowledging packets.
A semi-random Initial Sequence Number (ISN) is chosen.
If a blind spoofer can predict the ISN, he can blindly complete the 3-way handshake without seeing the answers.
However, the spoofed source should not receive the response packets, otherwise it might answer with a RST.
If I am doing a blind spoofing I will try to guess sequence numbers continuously(1/$2^{32}$). This is only done if the data are Uniformly random distributed.
Better than using a fixed random draw I can start every time with a random guess on the same codomain(64000 effort spent).
### TCP Session Hijacking
Taking over an active TCP session.
If the attacker (C) can sniff the packets:
1. C follows the conversation of A and B recording the sequence numbers.
2. C somehow disrupts A's connection (e.g., SYN Flood): A sees only a “random” disruption of service.
3. C takes over the dialogue with B by spoofing A address and using a correct ISN. B suspects nothing.
If you can listen to the conversation the hijacking is simpler as you can know the sequence numbers and you can DoS one end and continue to answer as the hijacked one to the other(one endpoint will detect the attack the other will not). 
A lot of tools (e.g., hunt/dsniff) implement this attack automatically.
The attacker can avoid disrupting B's session and just inject things in the flow only if s(he) is a man in the middle
- It can control/resync all the traffic flowing through.
#### MITM: Man In The Middle
A broad category comprising all the attacks where an attacker can impersonate the server with respect to the client and vice-versa.
- physical or logical
- full- or half-duplex (blind)

![](https://i.imgur.com/Z6r4rdR.png)

If you MITM the network gateway you can listen to all the conversation(so check it, hardpoint the network gateway in the lookup table to cope with this problem)
## DNS (Cache) Poisoning Attack
Poison the cache of a non authoritative DNS server

![](https://i.imgur.com/GFJB7AS.png)

1. The attacker makes a recursive query to the victim DNS server.
2. The victim (non authoritative) DNS server contacts the authoritative server.
3. The attacker, impersonating the authoritative DNS server, spoofs the answer (before the legitimate one).
4. The victim DNS server trusts and caches the malicious record [POISONED].
In the spoofed answer we need to use the ID of the DNS query initiated by the victim DNS server (step 2.). Guess? Bruteforce? (Kaminsky, 2008)
DNS query have an ID, the length is not specified, it is specified it is  only long enough.
## DHCP Poisoning Attack
DHCP is an unauthenticated protocol
The attacker can intercept the “DHCP requests”, be the first to answer, and client will
believe that answer.
With a (spoofed) “DHCP response”, the attacker can set:
- IP address,
- DNS addresses,
- default gateway of the victim client.
- path for the kernel
SO DHCP should only be used for IP nowadays.
## Internet Control Message Protocol 
Service protocols  that gives ports to the one that needs, but can still be abused
Can hijack the connection by default in the host, one way spoofing as it needs to only know the address of a single router. 
By default OS will ignore redirect nowadays to cope with these attacks.
This mechanism is a good way to do full man in the middle. 
### ICMP Redirect
Tells an host that a better route exists for a given destination, and gives the gateway for that route.
When a router detects a host using a non-optimal route it:
- Sends an ICMP Redirect message to the host and forwards the message.
- The host is expected to then update its routing table.
Nightmare as you are giving writing access to everyone to your routing table(in 2025 do not do this). Still do not drop all the ICMP packets as they are not only used for attacks but for other legal means. 
### ICMP Redirect Attack
The attacker can forge a spoofed ICMP redirect packet to re-route traffic on specific
routes or to a specific host that may be not a router at all.
The attack can be used to:
- Hijack traffic (elect his/her computer as the gateway).
- Perform a denial-of-service attack.
Weak authentication:
- An ICMP message includes the IP header and a portion of the payload (usually the first 8 bytes) of the original IP datagram.
The attacker needs to intercept a packet in the “original” connection in order to forge the reply (i.e., must be in the same network).
Creates a (half-duplex) MITM situation.
Handling of ICMP redirect is OS dependent:
- Windows 9x accepted them adding a temporary host entry in routing tables.
- Linux: default off, configured by value in 
	`/proc/sys/net/ipv4/<int>/accept_redirects`

## Route Mangling
If the attacker can announce routes to a router, s(he) can play a number of magical tricks
- IGRP, RIP, OSPF: no/weak authentication
- EIGRP, BGP: authentication available but seldom used
Availability is paramount and system administrator of ISP do not sacrifice performance for root authentication(even if the check only raise a warning)
Should consider BGP Hijacking protection handling as they are noticeable but can there be problems in not handling them.
In a company network they can happens more as there are internal threaths.
