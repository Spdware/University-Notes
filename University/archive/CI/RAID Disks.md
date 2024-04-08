Disk can only reach a certain performance in a given time space. Combining several disk together you can go beyond the maximum capabilities of the singular disk. These are single large and fast disks. Starting as JBOD(Just a Bunch Of Disk), they are a way to separate the disks from the computer. RAID combine the disk together to make them larger and faster. RAID does so dividing a single information between all the disks available to use the parallelism of the structure to speed up the transfer rate(Striping method). The decreasing in side is a side effect of the increasing in performance. The parallelism introduce a new way to something goes wrong.
Striping works letting know the computer using a RAID disks as a single disk and not a collection of disks(I/O virtualization). 

![](https://i.imgur.com/RoH5kxr.png)

To put the blocks of a file in a RAID structure I can divide the blocks in the various disks in the structure or I can divide the blocks in smaller blocks and save them in parallel in the RAID structure
- Striping: data are written sequentially (a vector, a file, a table, ...) in units (stripe unit: bit, byte, blocks) on multiple disks according to a cyclic algorithm (round robin)
- Stripe unit: dimension of the unit of data that are written on a single disk
- Stripe width: number of disks considered by the striping algorithm
	1. multiple independent I/O requests will be executed in parallel by several disks decreasing the queue length (and time) of the disks
	2. single multiple-block I/O requests will be executed by multiple disks in parallel increasing of the transfer rate of a single request
As I increase the number of disks used I increase the risk of failure of my RAID disk so I need to introduce __REDUNDANCY__
Redundancy: error correcting codes (stored on disks different from the ones with the data) are computed to tolerate loss due to disk failures
Since write operations must update also the redundant information, their performance is worse than the one of the traditional writes. To enable all of this I use the usual checksum to assure that I have all the data correctly updated in my disk.
# RAID
Redundant Array of Inexpensive Disks: use multiple disks to create the illusion of a large, faster, more reliable disk. Externally, RAID looks like a single disk(it is Trasparent), Data blocks are read/written as usual, No need for software to explicitly manage multiple disks or perform error checking/recovery. Internally, RAID is a complex computer system: Disks managed by a dedicated CPU + software,  RAM and non-volatile memory, Many different configuration options(RAID levels)
### RAID Levels
- RAID 0 striping only
- RAID 1 mirroring only
	- RAID 0+1 (nested levels)
	- RAID 1+0 (nested levels)
- RAID 2 bit interleaving (not used)
- RAID 3 byte interleaving - redundancy (parity disk)
- RAID 4 block interleaving - redundancy (parity disk)
- RAID 5 block interleaving - redundancy (parity block distributed)
- RAID 6 greater redundancy (2 failed disks are tolerated)
#### RAID level 0: striping, no redundancy
Data are written on a single logical disk and splitted in several blocks distributed
across the disks according to a striping algorithm used where performance and capacity, rather than reliability, are the primary concerns, minimum two drives required:
+ lowest cost because it does not employ redundancy (no error-correcting codes are computed and stored)
+ best write performance (it does not need to update redundant data and it is parallelized)
- single disk failure will result in data loss

![](https://i.imgur.com/7VcAbWv.png)

Since I am cutting data and using the disks in parallel I have the maximum performance I can. My speed can increase up to the number of disks I am using. This is also the lowest cost as I am exploiting as much as possible my data structure. 
Key idea: present an array of disks as a single large disk
Maximize parallelism by striping data cross all N disks
How do you access specific data blocks?
- Disk = logical_block_number % number_of_disks
- Offset = logical_block_number / number_of_disks
Chunk size impacts array performance:
- Smaller chunks $\rightarrow$ greater parallelism
- Big chunks $\rightarrow$ reduced seek times
Typical arrays use 64KB chunks
As usual, we focus on sequential and random workloads. Assume disks in the array have random transfer rate R. Doing a single operation is different to do millions of small operation. 
Capacity: Nx Lap One DIsk(All space on all drives can be filled with data)
Reliability: 0(If any drive fails, data is permanently lost, MTTDL = MTTF/N)
Sequential read and write: N * S(Full parallelization across drives)
Random read and write: N * R(Full parallelization across all drives)
#### RAID level 1: mirroring
whenever data is written to a disk it is also duplicated (mirrored) to a second disk (there are always two copies of the data), minimum 2 disk drives
Positive:
+ high reliability: when a disk fails the second copy is used
+ read of a data: it can be retrieved from the disk with the shorter queueing, seek, and latency delays
+ fast writes (no error correcting code should be computed) – but still slower than standard disks (due to duplication)
Negative:
- high costs (50% of the capacity is used)

![](https://i.imgur.com/vUfNg6T.png)

In principle, a RAID 1 can mirror the content over more than one disk. This give resiliency to errors even if more than one disk breaks. It allows with a voting mechanism to identify errors not reported by the disk controller.  In practice this is never used, because the overhead and costs are too high.
The idea is that RAID 0 offers high performance, but zero error recovery so the key idea is to make two copies of all data. But if several disks are available(in even number) disks could be coupled and the total capacity is halved as each disk has a mirror. I need to combine the RAID level 0 and 1. So usually whenever you have 20 disks you can have 10 disks of storage and 10 disks for recovery(You can dispose the disks in RAID 0+1 or RAID 1+0). 

![](https://i.imgur.com/J9RyGDR.png)

RAID 10 means you are combining RAID 1 and RAID 0.

![](https://i.imgur.com/wDyQtF8.png)

![](https://i.imgur.com/MrQkW0r.png)

The main difference is the fault tolerance level:
On most implementations of RAID controllers RAID 0+1 fault tolerance is less and RAID 1+0 fault tolerance is larger.
Capacity: N / 2(Two copies of all data, thus half capacity)
Reliability: 1 drive can fail, sometime more(If you are lucky, N / 2 drives can fail without data loss)
Sequential write: (N / 2) * S(Two copies of all data, thus half throughput)
Sequential read: (N / 2) * S(Half of the read blocks are wasted, thus halving throughput)
Random read: N * R(Best case scenario for RAID 1, Reads can parallelize across all disks)
Random write: (N / 2) * R(Two copies of all data, thus half throughput)
Mirrored writes should be atomic
- All copies are written, or none are written
However, this is difficult to guarantee
Example: power failure 
Many RAID controllers include a write-ahead log, Battery backed, non-volatile
storage of pending writes ,A recovery procedure ensures to recover the out-of-sync mirrored copies
Decreasing the Cost of Reliability
-  RAID 1 offers highly reliable data storage
- But, it uses N / 2 of the array capacity
- Can we achieve the same level of reliability without wasting so much capacity?
-  Yes!
- Use information coding techniques to build light-weight error recovery mechanisms
#### RAID 4
![](https://i.imgur.com/6ffuKdm.png)

