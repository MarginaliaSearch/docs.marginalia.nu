---
title: '2. Crawling and Loading'
---

This section pertains to basic operations of the search engine.  It will explain how to run a crawl, and other ways to load data into the system.

## What is a crawler?

An internet crawler (or spider) is a piece of software that traverses the internet, and downloads pages as it encounters them.  This is the basis for all internet search engines.  

During the first run, the crawler needs to be seeded with a list of domains.  A freshly installed search engine does not know anything about the world, and needs to be told which domains exist.  Such a crawl will fetch every document it encounters. 

Subsequent runs can be configured to only fetch documents that have been updated since the last run.  In the software this is called a recrawl.  Elsewhere it's sometimes called a differential crawl.  This is generally faster than a full crawl, and is the recommended mode of operation whenever available.

After the crawl is complete, the documents need to be processed and indexed.  After this the search engine is ready to serve queries.

Documents can also be sideloaded from other sources, such as a WARC file, a Wikipedia dump, from stackexchange data, etc.  This is useful as these sources are generally very large, and difficult to crawl. 

## Important disclaimer about web crawling

Please note that the crawler will be accessing real servers from your connection, and you may end up on IP greylists that temporarily block your access to those servers for up to a few weeks; on rare occasions **permanently**.  You will likely end up solving a lot of CAPTCHAS for several weeks after running the crawler on a domestic connection.

The crawler is generally polite and well-behaved, but it is still a crawler, and it will be accessing a lot of pages. You run the risk of getting in trouble with your ISP, the law, your partner, your landlord, and/or your parents; depending on terms of service, jurisdiction, marital status, living conditions and age.

**Crawling is performed at your own risk**.  Be sure to configure an user agent that identifies your crawler, to help webmasters reach you if there is a problem.  

The Internet is already very hostile toward independent search engines due to all the reckless bot activity that is going on around the clock.  Be a good citizen, and don't make it worse, or we all suffer for it.