---
title: '4.1 Model Files'
---


In the `model/` directory, the following files are stored:

File|Description
---|---
English.DICT|RDRPosTagger dictionary
English.RDR|RDRPosTagger model
lid.176.ftz|fasttext language identification model
opennlp-sentence.bin|OpenNLP sentence detector model
opennlp-tokens.bin|OpenNLP tokenizer model
tfreq-new-algo3.bin|Marginalia term frequency model
ngrams.bin|Marginalia n-grams model

The RDRPosTagger models are used for fast part-of-speech tagging.  These models, and additional models are available at the [RDRPOSTagger git repository](https://github.com/datquocnguyen/RDRPOSTagger)

The fasttext language identification model is used to identify the language of a document.  See the [fasttext documentation](https://fasttext.cc/docs/en/language-identification.html) for more information.

The OpenNLP models are used for sentence detection and tokenization.  See the [OpenNLP documentation](https://opennlp.apache.org/) for more information.

`tfreq-new-algo3.bin` is a Marginalia-specific term frequency model.  It contains hashed terms and their frequencies.  These files can be generated from crawl data in the control interface, under `Node N-> Actions-> Export From Crawl Data`, select 'Extract term frequency data'.  The model is used for term frequency calculations in the search engine.  It's also available for download from the [Marginalia Search Downloads](https://downloads.marginalia.nu/) website.

`ngrams.bin` is a binary index of n-grams.  It's used in query processing.  There is currently no way of generating this file from crawl data, it's only available for download from the [Marginalia Search Downloads](https://downloads.marginalia.nu/) website.