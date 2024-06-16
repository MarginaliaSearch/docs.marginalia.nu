---
title: '1.1 Hardware and Configuration'
published: '2024-01-16'
---

The Marginalia Search Engine is designed to be run on a single server.  The server should be an x86-64 machine, have at least 16GB of RAM, and at least 4 cores.  It is designed to run on physical hardware, and will likely be very expensive to run in the cloud.

Overall the system is designed to be run on a single server, but it is possible to run the index nodes on separate servers.  This will likely require a bit of configuration, and is not recommended for most users.  

Crawling requires a decent network connection, ideally at least 1 Gbps. 100 Mbps will work, but will be slower.  It's unlikely the crawler will saturate an 1 Gbps connection, as disk I/O will be the bottleneck at that point.

Storage requirements are highly dependent on the size of the index, and the number of documents being indexed.  For 100,000 documents, you can probably get away with 2 TB of SSD storage, and 4 TB of mechanical storage for the crawl data.  

Note that all SSDs are not created equal.  Enterprise SSDs are strongly recommended, as they have far better sustained I/O performance, and won't wear out as quickly.  The software is very I/O intensive, and may chew through consumer grade SSDs in a matter of months.

All relevant hard drive partitions should be mounted with the `noatime` option.  This will reduce the amount of I/O required to run the software, and will increase the lifetime of your drives.  

If you are using mechanical drives, you should probably also disable the spindown timer, `hdparm -S0 /dev/sdX`. 


