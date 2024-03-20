A Direct Attached Storage (DAS) is a storage system directly attached to a server or workstation. They are visible as disks/volumes by the client OS
A Network Attached Storage (NAS) is a computer connected to a network that provides only file-based data storage services (e.g., FTP, Network File System and SAMBA) to other devices on the network and is visible as File Server to the client OS
Storage Area Networks (SAN) are remote storage units that are connected to servers using a specific networking technology (e.g., Fiber Channel) and are visible as disks/volumes by the client OS

![500](https://i.imgur.com/449lr4s.png)

### DAS Direct Attached Storage
DAS is a storage system directly attached to a server or workstation
The term is used to differentiate non-networked storage from SAN and NAS (that will be described later). 

![](https://i.imgur.com/TTCR9W6.png)

Main features: 
- limited scalability
- complex manageability
- to read files in other machines, the “file sharing” protocol of the OS must be used
Internal and external:
- DAS does not necessary mean “internal drives”
- All the external disks, connected with a point-to-point protocol to a PC can be considered as DAS
This model is not ideal for large solution as it is not scalable enough(backup has to backup all the machine attached at the network)
### NAS Network Attached Storage
A NAS unit is a computer connected to a network that provides only file-based data storage services to other devices on the network
NAS systems contain one or more hard disks, often organized into logical redundant storage containers or RAID
Provide file-access services to the hosts connected to a TCP/IP network though Networked File Systems/SAMBA
Each NAS element has its own IP address
Good scalability (incrementing the devices in each NAS element or incrementing the number of NAS elements)

![500](https://i.imgur.com/bdI9Bx3.png)

The key differences between direct-attached storage (DAS) and NAS are:
- DAS is simply an extension of an existing server and is not necessarily networked
- NAS is designed as an easy and self-contained solution for sharing files over the network
The performance of NAS depends mainly on the speed of and congestion on the network
### SAN Storage Area Network
Storage Area Networks, are remote storage units that are connected to PCs/servers using a specific networking technology.
SANs have a special network devoted to the accesses to storage devices 
- Two distinct networks (one TCP/IP and one dedicated network, e.g., Fiber Channel)
- High scalability (simply increasing the storage devices connected to the SAN network)

![](https://i.imgur.com/EK8CmM5.png)

The protocol must guarantee not out of order and packet loss.
#### NAS vs SAN
NAS provides both storage and a file system
This is often contrasted with SAN which provides only block-based storage and leaves file system concerns on the "client" side 
One way to loosely conceptualize the difference between a NAS and a SAN is that: 
- NAS appears to the client OS (operating system) as a file server (the client can map network drives to shares on that server)
- a disk available through a SAN still appears to the client OS as a disk: it will be visible in the disks and volumes management utilities (along with client's local disks), and available to be formatted with a file system
Traditionally:
- NAS is used for low-volume access to a large amount of storage by many users 
- SAN is the solution for petabytes (1012) of storage and multiple, simultaneous access to files, such as streaming audio/video

![](https://i.imgur.com/dcyuYdP.png)

