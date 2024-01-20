---
title: 'Documentation'
published: '2024-01-17'
---

This is the documentation for the Marginalia Search Engine Software.  Marginalia Search is both a search engine, and the software it runs.  The software is open source, and can be used to run an internet search engine.

This documentation pertains to the latter, the software, and not the search engine instance itself, although the instance is also running the same software.  It is not "open core" or anything like that, you can clone the repository and run a mirror of the search engine if you want to (although this would require considerable effort and hardware).

The Marginalia Search Engine is an *Internet* search engine.  This is to be understood as different from a document search engine like elasticsearch or mellisearch. An Internet search engine makes different assumptions about the nature of the data being indexded, and different concessions about performance and scaling. 

The Marginalia search engine is integrated with a crawler, and is designed to be used as a search engine for the internet, and is relatively poorly suited for use as a document search engine.

The software is simultaneously the backing sofware for the [Marginalia Search](https://search.marginalia.nu/) project, but can also been run in a barebones mode for use as a general purpose search engine over REST.
