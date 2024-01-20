---
title: '5. Recovery'
---

The system is designed to be resilient to failure, not through redundant hrardware, but through redundant data.  

Data flows in a circular pattern.  Crawl data becomes processed data, which is loaded into the mariadb database
and index; from which new crawl data can be generated.  This mean that if at least one of the three points of failure
survives (the crawl data, the processed data, or the db+index), the system can be recovered.

The processes in the system operate on a message queue mediated by the mariadb database, and where possible the processes 
resume automatically.  If a process crashes or is terminated, it will resume where it left off when it is restarted.  Most
data processing jobs use some form of journal to keep track of what has been done, and what remains to be done, to avoid
redundant work on a restart. 

The system is designed to be run on a single machine, or a small cluster, and is not designed to be run on a large cluster.
There are central points of failure, where the system can be temporarily brought down by a single machine failing.  This is 
primarily the mariadb database, which is used as a messaging medium.  If the database is down, much of the system will not function.