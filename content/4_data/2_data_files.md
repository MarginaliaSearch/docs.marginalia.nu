---
title: '4.2 Data Files'
---

In the `data/` directory, the following files are stored:

File|Description
---|---
adblock.txt|Adblock rules
asn-data-raw-table|CIDR->ASN data
asn-used-autnums|ASN->AS registry data
IP2LOCATION-LITE-DB1.CSV|IP2Location data
atags.parquet|Anchor tags

`adblock.txt` is used to detect ads and other problematic content in the crawled documents.

The asn-files are sourced from [APNIC](https://www.apnic.net/) and used to map IP addresses to the corresponding CIDR and autononous system.

The ip2location data is from [https://lite.ip2location.com/](https://lite.ip2location.com/), and available under CC-BY-SA 4.0.

The atags.parquet file is available from [https://downloads.marginalia.nu](https://downloads.marginalia.nu), and contains anchor texts.  It's also possible to use the Marginalia software to export your own anchor text data.
