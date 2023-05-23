---
layout: default
title: Data Processing Pipeline
grand_parent: SMDRM
parent: Pipelines
nav_order: 0
---

# Data Processing Pipeline
{: .no_toc }

{: .important}
Data processing pipeline requires a number of components to run.
<br>
For more information, go to the [install]({{ site.baseurl }}/smdrm/install) page.

The default data processing pipeline takes a file,
applies several operations, and returns an enriched output.

1. TOC
{:toc}

## Input File

A compressed zipfile archive containing one [Newline Delimited JSON](http://ndjson.org/) (NDJSON) file.

## Arguments

A [`disaster type`]({{ site.baseurl }}/smdrm/glossary#disaster-types) argument selects
the correct annotation model to annotate datapoints related to a specific natural disaster. 

## Output File

A compressed zipfile archive containing one [Newline Delimited JSON](http://ndjson.org/) (NDJSON) file,
enriched with `places` object, and one of disaster type probability annotations.

For more informations, go to the [Enriched Fields]({{ site.baseurl }}/smdrm/datapoint#enriched-fields)
in the Datapoint section.

## Pipeline

The data processing pipeline is made of the following components executed sequentially.

### Validator

Validates existence and type of required features.

### Features Extractor

Applies the following operations:

* if input is Twitter
    * anonymize `text`
    * remove retweets
    * extract media URLs
    * extract extended text
    * build tweet URL
* extract required [features](https://github.com/panc86/smdrm_prod/blob/08700006ca5f057f35bc7848ef697e77782a72a3/smdrm/config.py#L8-L14)
* normalize URLs
* normalize hashtags
* remove punctuation
* normalize white spaces
* remove new lines
* remove dates
* merge neighbouring word duplicates

### Annotators

Annotate batches of pre-processed multilingual texts with an array of probabilities
using binary models trained on natural disasters related texts.

Available publications:
* [Floods](https://zenodo.org/record/6351658)
* [Impacts](https://zenodo.org/record/6577394)


### Geocoder

Matches geographic locations identified from texts using DeepPavlov Named Entity Recognizer (NER) against a given gazetteer.

{: .note}
You can load your own gazetteer CSV file.
Place it into the NFS share and export the `GAZETTEER_PATH` environment variable.

|Feature ID|Group|Description|
|----------|-----|-----------|
|`country_name`|Country|Official name of a Country.|
|`country_code`|Country|ISO Alpha 3 Country code.|
|`region_name_N`|Region|Any administrative level between Country and city ordered by level.|
|`place_name`|City|Official name of a city.|
|`place_altname_N`|City|Alternative names of a city.|
|`latitude`|City|WSG84 latitude of a city.|
|`longitude`|City|WSG84 longitude of a city.|

_Table 1. Required features of a gazetteer_

### Artifacts

Serialize NDJSON object to compressed zipfile artifact.
