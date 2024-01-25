---
title: '4.3 Crawl Data, Processed Data (WIP)'
---

The system stores crawl data in `index-n/storage/`, along with processed data and other various long-term data.

## Crawl Data

The name of the directory reflects its content.  Crawl data is stored in directories with names like `crawl_data_...`.

In the root of the directory is a file called `crawler.log`, which is a journal of what has been crawled, and where it is stored.  It will relate domain names to subdirectories.

To get around file system limitations, the crawl data is stored in a subdirectory tree.  Each domain is stored in a parquet file.  These can be interrogated with SQL using e.g. [duckdb](https://duckdb.org/).

Processed data follows a similar storage structure like crawl data. It also has a log file, directory structure, and parquet files.
