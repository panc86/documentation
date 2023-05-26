---
layout: default
title: Realtime Data Collection with Twitter
grand_parent: SMDRM
parent: Examples
nav_order: 0
---

# Realtime Data Collection with Twitter
{: .no_toc }
---
#### TABLE OF CONTENTS
{: .no_toc }

1. TOC
{:toc}

## Context

Few hours ago, a 7.8 Mw earthquake has struck [southern and central Turkey](https://www.gdacs.org/Earthquakes/report_shakemap.aspx?eventid=1357372&episodeid=1487096&eventtype=EQ).

## Problem

Monitor the aftermath of the earthquake in realtime, and where
its impact is significant with regards to deaths, and damages.

## Create Rule

You want to filter datapoints from Twitter that contains a specific set of keywords.

Thus, you select the keywords `earthquake` and `deprem` because of the event type,
and the cities of `Gaziantep`, `Hatay`, and `Kahraman Maras` given their proximity to
the epicenter of the earthquake.

{: .important}
Read [how to build a rule](https://developer.twitter.com/en/docs/twitter-api/enterprise/rules-and-filtering/building-a-rule)
on the Twitter API documentation to understand the syntax of the `query` field.

```shell
# create a rule
curl -X 'PUT' \
  'http://10.169.138.109/api/v1/twitter/rule/' \
  -H 'accept: application/json' \
  -H 'Content-Type: application/json' \
  -d '{
  "tag": "SMDRM_turkey_example",
  "query": "(earthquake OR deprem) (Gaziantep OR Hatay OR \"Kahraman Maras\")",
  "description": "Example rule earthquake turkey"
}'
```

For more information, check the [PUT Create a rule](http://10.169.138.109/api/v1/redoc#tag/Twitter/operation/create_rule__twitter_rule__put)
resource in the Twitter section on the SMDRM API Reference.

## Publish Rule

Publish the rule by `tag` to start the collection of raw Twitted data.  

```shell
# publish a rule
curl -X 'POST' \
  'http://10.169.138.109/api/v1/twitter/rule/publish/prod?tag=SMDRM_turkey_example' \
  -H 'accept: application/json' \
  -d ''
```

## Schedule Pipeline Jobs

Schedule the realtime data processing `impacts` pipeline jobs that
fetches, extracts, enriches, and packages Twitter raw data every
`job_frequency` (15) minutes, for `duration` (48) hours.

```shell
curl -X 'POST' \
  'http://10.169.138.109/api/v1/twitter/schedule/impacts/prod?tag=SMDRM_turkey_example&duration=48&job_frequency=15&job_timeout=180' \
  -H 'accept: application/json' \
  -d ''
```

For more information, check the [POST Schedule frequency based data pipeline job](http://10.169.138.109/api/v1/redoc#tag/Twitter/operation/schedule_realtime_pipeline_jobs_twitter_schedule__disaster_type___product__post) resource in the Twitter section on the SMDRM API Reference.

## Export Artifacts

After `job_frequency` minutes, you extract the first artifacts of the data processing pipeline,
and start your analyses.

```shell
curl -X 'GET' \
  'http://10.169.138.109/api/v1/artifacts/export/SMDRM_turkey_example?lastn=0' \
  -H 'accept: application/json'
```
