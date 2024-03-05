Based on flash memory, in the last 10 years we have SSD disks because of the cost(much lower than before). They are electronic, build up of transistors and in case of power loss you don't loss the power state of the disk(no data loss). The system is limited by the speed of the controller. SSDs cost 10% more than HDD and this is why data center use hybrid solutions or use only HDD. 

![](https://i.imgur.com/DzFVe3r.png)

Increasing the layers helps in saving money as you are having more cells in the same space but you are also increasing the time for Reads and you are increasing the reading error. Writes are 10 times slower than reads and Erase are slower than Writes.
They are realised as follows:
- NAND flash is organized into Pages and Blocks
- You can read and write a individuals pages, but you can erase pages at the block level only
- A page contains multiple logical block (e.g. 512B-4KB) addresses (LBAs)
- A block typically consists of multiple pages (e.g. 64) with a total capacity of around 128-256KB
- Block/Page terminology in the SSD context can clash with previous use
- Blocks (or Erase Block): smallest unit that can be erased
	- It consists of multiple pages and can be cleaned using the ERASE command
- Pages: smallest unit that can be read/written.
	- It is a sub-unit of an erase block and consists of the number of bytes which can be read/written in a single operations through the READ or PROGRAM commands. They can be in three states: Empty(or Erased, they don't contain data), Dirty(or Invalid, they contain data but this data is no longer in use(or never used)), In Use(or Valid, the page contains data that can be actually read).
Only empty pages can be written. Only dirty pages can be erased but this must be done at the block level(all the pages in the block must be dirty or empty). It is meaningful to read only pages in the "in use" state. This derives in huge constraints to the use of the SSD.

![](https://i.imgur.com/SaFyN70.png)

Flash cells can wear out due to the erasing of the cells. The silicon is degraded every time electricity flows in it. We don't want to reach the wear out but we want to reach this wear out state equally on all the cells in the system. We can use some empty space to replace weared out cells. The Flash Translation Layer is the component that is relegated to the job of mapping the cells and blocks and the wear state. With a static mapping you are always writing and erasing the same block in various cycle reaching the wear out of the cell faster. The FTL has to perform the data allocation, reduce the Write Amplification problem. Reads and Writes can flip the state of near cells. So you try to write the blocks in order. 
When an existing page is updated old data becomes obsolete! Old version of data are called garbage and (sooner or later) garbage pages must be reclaimed for new writes to take place. Garbage Collection is the process of finding garbage blocks and reclaiming them. Garbage collection is expensive Require reading and rewriting of live data. Ideal garbage collection is reclamation of a block that consists of only dead pages.

