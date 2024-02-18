---
title: '2.2 Recrawling'
published: '2024-01-16'
---

The work flow with a crawl spec was a one-off process to bootstrap the search engine.  To keep the search engine up to date,
it is preferable to do a recrawl.  This will try to reduce the amount of data that needs to be fetched.

To trigger a Recrawl, go to `Nodes->Node N->Actions->Re-crawl`.  This will bring you to a page that looks similar to the 'new crawl page', where you can select a set of existing crawl data to use as a source.  

Select the crawl data you want, and press `[Trigger Recrawl]`. 

<figure>
    <img src="recrawl.webp">
    <figcaption>Recrawl Dialog</figcaption>
</figure>

Crawling will proceed as before, but this time, the crawler will try to fetch only the data that has changed since the last crawl, increasing the number of documents by a percentage.  This will typically be much faster than the initial crawl.

The recrawl will update the set of crawl data it was based on, so that the next recrawl will be based on the latest data.  This means that the first recrawl will be a full crawl, but subsequent recrawls will be incremental.

**Warning** If you download sample data into the system, these domains will also be included in the re-crawl.  This is a strongly undesirable state, and there is no means of restoring the data contamination.

**Warning** Prior to version v24.01.2 a foot-gun existed: If a re-crawl was performed on crawl data from a crawl spec immediately after the initial crawl, before the data was loaded into the system, the crawl log would be overwritten and the 
re-crawl would do nothing, essentially losing the ability to load the data.  After v24.01.2, this is no longer possible.  This is an unlikely scenario in a production setting, but all the more likely when testing the software.  An upgrade is 
advisable.