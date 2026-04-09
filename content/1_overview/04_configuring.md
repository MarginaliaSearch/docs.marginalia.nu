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

## Recommended Tweaks for constrained hardware

If you intend to run the system on a machine with limited RAM, especially on a relatively high-powered CPU with many
cores, some configuration changes are recommended.

In `conf/properties/system.properties`:

* Add `system.conserveMemory = true`. This will make the system use additional memory 
conservation strategies where available, at the expense of disk I/O and speed.

* Optionally, you might want to tweak `converter.poolSize`.  The default is automatically reduced by `system.conserveMemory`, 
 and affects the number  of threads used by the converter process in turn increasing RAM requirements.  It should be in the range between 1 and
  the number of logical cores in your CPU.  If set lower than necessary processing will take longer time.

* You might also want to lower `converter.sideloadThreshold` to something smaller than the default 10,000 -- maybe down to 1000.  This will make the 
converter perform a simpler processing operation on domains with more than this number of documents.   It's 
generally a fair bit slower, but drastically reduces the amount of RAM required.

**Optional:**

By default, Java sets a maximum heap size of 25% of your system RAM.  In general this is fine, but in extremely
resource constrained systems, this is a bit low.  

If ${MEMORY} is half your system ram in Gb, edit `env/service.env` and add `-Xmx${MEMORY}Gb` to CONVERTER_PROCESS_OPTS.

That is, if you have 32 GB of RAM, it could look like this:

`CONVERTER_PROCESS_OPTS="-Dservice-name=converter -Dservice-host=0.0.0.0 -Xmx16G"`

This is not generally necessary.

## `system.properties` overview

Of principal importance when configuring the system is `conf/properties/system.properties`, which is the main configuration file for the search engine.  It contains the following properties:

### Global

| flag                     | values     | description                          |
|--------------------------|------------|--------------------------------------|
| flyway.disable           | boolean | Disables automatic Flyway database migrations |
| log4j2.configurationFile | string | log4j2-test.xml, log4j2-prod.xml |
| system.conserveMemory    | boolean | Apply extra effort to conserve RAM. Useful on low-powered hardware |
| system.profile           | boolean | Automatically generate JFR profiling data for the crawler, converter, and other batch processes | 

### Crawler Properties

| flag                        | values     | description                                                                                                                                                                             |
|-----------------------------|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| crawler.userAgentString     | string | Sets the user agent string used by the crawler                                                                                                                                          |
| crawler.userAgentIdentifier | string | Sets the user agent identifier used by the crawler, e.g. what it looks for in robots.txt                                                                                                |
| crawler.poolSize            | integer | Sets the number of threads used by the crawler, more is faster, but uses more RAM.  This should generably higher than the number of cores, since crawler threads idle most of the time. |
| crawler.initialUrlsPerDomain | integer | Sets the initial number of URLs to crawl per domain  (when crawling from spec)                                                                                                          |
| crawler.maxUrlsPerDomain     | integer | Sets the maximum number of URLs to crawl per domain  (when recrawling)                                                                                                                  |
| crawler.midUrlsPerDomain     | integer | Sets breakpoint for when crawling set slows down per domain (when recrawling) |
| crawler.minUrlsPerDomain     | integer | Sets the initial number of URLs to crawl per domain  (when recrawling)                                                                                                                  |
| crawler.crawlSetGrowthFactor | double | If 100 documents were fetched last crawl, increase the goal to 100 x (this value) this time                                                                                             |
| ip-blocklist.disabled       | boolean | Disables the IP blocklist                                                                                                                                                               |

### Converter Properties

| flag                        | values     | description                                                                                                                                              |
|-----------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| converter.sideloadThreshold | integer | Threshold value, in number of documents per domain, where a simpler processing method is used which uses less RAM.  2500 is the default, and good for ~32 GB of RAM. |
| converter.poolSize | integer | Number of threads used by the converter process, should probably be in the range 2 - 32                                                                  |
| converter.lenientProcessing | boolean | Disable language, quality and length checks |
| converter.ignoreDomSampleData | boolean | Bypass DOM sample quality penalties |

### Loader Properties

| flag                        | values     | description                                                                                                                                              |
|-----------------------------|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| loader.insertFoundDomains | boolean | inserts all domains found in the crawl data into the database (defaults to true) |


### Marginalia Application Specific

| flag                      | values     | description                                                   |
|---------------------------|------------|---------------------------------------------------------------|
| control.hideMarginaliaApp | boolean | Hides the Marginalia application from the control GUI (used in barebones mode) |


### Experimental Flags (DANGER!)

These flags are provisional and may have unintended side effects.  These are subject to change, migration paths are not 
guaranteed. 

| flag | values  | description                                                                                                                                                                          
|--|---------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|system.noSunMiscUnsafe| boolean | Disable the use of sun.misc.Unsafe, use MemorySegment instead.  This is slower but has better memory safety |



## Language Support

The search engine has first-class support for English, and experimental support for other languages.  Languages need to be configured and defined via an XML file `conf/languages.xml`.  A template can be found in [functions/language-processing/resources/languages-experimental.xml](https://github.com/MarginaliaSearch/MarginaliaSearch/blob/master/code/functions/language-processing/resources/languages-experimental.xml),  and a basic introduction in [functions/language-processing](https://github.com/MarginaliaSearch/MarginaliaSearch/tree/master/code/functions/language-processing).

The simplest language configuration that can be implemented, that does no POS tagging or stemming, looks like

```
    <language isoCode="iso-code" name="Language Name">
        <keywordHash algorithm="utf8" />
        <stemmer algorithm="none" />
        <sentenceDetector algorithm="opennlp"/>
        <unicodeNormalization algorithm="minimal" />
    </language>
```

There are more elaborate examples in the experimental config file linked above.  New POS tagging data can be acquired from the [rdrpostagger](https://github.com/datquocnguyen/rdrpostagger) repository, and will be downloaded automatically by the system.

## NSFW Filter

The system has support for NSFW filtering.   To enable this, first a machine learning model needs to be trained.  This can be done by running `./gradlew trainNsfwModel`.  This will take a few minutes, depending on your CPU.

Once this is done, the model needs to be copied from `run/model/nsfw-model` in the Marginalia source directory, into `models/nsfw-model` in the deployment directory.

Once installed, after restarting the system (or just restarting `query-service`), passing `&nsfw=2` to the API will filter out NSFW content. 

## Headless Browser

The system can optionally interact with a headless browser to capture a rendered DOM sample, and website screenshots.

The docker image for this does not build by default (as this requires considerable resources and time),
it must first be enabled via `gradle properties`, by setting `wantHeadless=true`. 
After doing so, running `./gradlew docker` will create such an image along with images for the rest of the system's services.

**Caveat:**
Running a headless browser also requires vigilant patching and maintenance, as well as considerable RAM and CPU.
Running `./gradlew code:tools:headless:docker` will recreate the docker image for the headless browser against the latest upstream image
to keep you up to date with patches.

To enable this integration, add a section like the below to the docker-compose file.

```
  headless:
    image: "marginalia-headless"
    container_name: "headless"
    restart: always
    networks:
    - wmsa
    env_file:
    - "env/headless.env"
```

The healthcheck will restart the container daily to avoid runaway resource consumption. 

Next add a line like the below to `env/headless.env`

```
TOKEN=my-secret-password
```

and a few lines like these to `conf/properties/system.properties`

```
live-capture.headless-uri=http://headless:3000/
live-capture.headless-token=my-secret-password
live-capture.headless-sample-threads=4
```
