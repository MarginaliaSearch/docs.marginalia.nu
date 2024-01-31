---
title: '1.4 Configuration'
---

After installing, a directory structure will be created in the install directory.  In it, the following files and directories will be created:

path|description
---|---
conf/properties|java-style properties files for configuring the search engine
conf/suggestions.txt|A list of suggestions for the search interface
conf/db.properties|JDBC configuration
env/mariadb.env|Environment variables for the mariadb container
env/service.env|Environment variables for Marginalia Search services
logs/|Log files
model/|Language processing models
index-1/backup|Index backups for index node 1
index-1/index|Index data for index node 1
index-1/storage|Raw and processed crawl data for index node 1
index-1/work|Temporary work directory for index node 1
index-1/uploads|Upload directory for index node 1
index-2/backup|Index backups for index node 2
index-2/index|Index data for index node 2
...|...

For a production-like deployment, you will probably want to move the db and index directories to a separate storage device.  The `index-n/storage` directory wants as much space as can be afforded, and `index-n/index` wants an SSD. 


## `system.properties`

Of principal importance when configuring the system is `conf/properties/system.properties`, which is the main configuration file for the search engine.  It contains the following properties:

### Global

| flag        | values     | description                          |
|-------------|------------|--------------------------------------|
| flyway.disable | boolean | Disables automatic Flyway database migrations |
| log4j2.configurationFile | string | log4j2-test.xml, log4j2-prod.xml |


### Crawler Properties

| flag                        | values     | description                                                                              |
|-----------------------------|------------|------------------------------------------------------------------------------------------|
| crawler.userAgentString     | string | Sets the user agent string used by the crawler                                           |
| crawler.userAgentIdentifier | string | Sets the user agent identifier used by the crawler, e.g. what it looks for in robots.txt |
| crawler.poolSize            | integer | Sets the number of threads used by the crawler, more is faster, but uses more RAM |
| crawler.initialUrlsPerDomain | integer | Sets the initial number of URLs to crawl per domain  (when crawling from spec)              |
| crawler.maxUrlsPerDomain     | integer | Sets the maximum number of URLs to crawl per domain  (when recrawling)                      |
| crawler.minUrlsPerDomain     | integer | Sets the minimum number of URLs to crawl per domain  (when recrawling)                      |
| crawler.crawlSetGrowthFactor | double | If 100 documents were fetched last crawl, increase the goal to 100 x (this value) this time |
| ip-blocklist.disabled       | boolean | Disables the IP blocklist |

### Converter Properties

| flag                        | values     | description                                                                                                                                              |
|-----------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| converter.sideloadThreshold | integer | Threshold value, in number of documents per domain, where a simpler processing method is used which uses less RAM.  10,000 is a good value for ~32GB RAM |

### Marginalia Application Specific

| flag                      | values     | description                                                   |
|---------------------------|------------|---------------------------------------------------------------|
| control.hideMarginaliaApp | boolean | Hides the Marginalia application from the control GUI (used in barebones mode) |


### Experimental Flags (DANGER!)

These flags are provisional and may have unintended side-effects.  These are subject to change, migration paths are not 
guaranteed. 

| flag | values  | description                                                                                                                                                                          
|--|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|system.languageDetectionModelVersion| integer | Choose language detection model. -1 to disable, 0 for default; 1 or 2 to select model between the old crude model and fasttext (which despite its name is slower but more accurate). |
|system.noFlattenUnicode| boolean | if true, the search engine will attempt to support utf-8 keywords.  The index needs to be re-constructed after this has been switched.  Probably also needs new language models.     |