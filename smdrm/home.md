---
layout: default
title: Home
parent: SMDRM
nav_order: 1
---

# Introduction
{: .no_toc }

Social Media for Disaster Risk Management (SMDRM) is a realtime data
processing pipeline for natural disaster risk management.

SMDRM enables analysis aim to study the occurrence of environmental disasters through its location,
and urban impact using social media [datapoints]({{ site.baseurl }}/smdrm/datapoint).

Datapoints are enriched with a probability score, and a geographic location that are extracted
from datapoint text using Named Entity Recognition (NER) algorithms.

# Architecture
{: .no_toc }
<br>
![Diagram]({{ site.baseurl }}/images/smdrm/smdrm-diagram.drawio.png)

1. TOC
{:toc}

## REST API

- manages rules to collect raw data from external sources
- schedules realtime data processing pipelines asyncronously
- extracts data processing pipeline compressed artifacts

## Queue

Holds a list of resource expensive data processing pipeline jobs
scheduled by the REST API at specific interval of time in the future.

## Workers

Workers execute data processing pipeline jobs that await in the queue,
and store jobs result in a shared NFS drive.

Multiple workers may be spawn to execute data processing pipeline jobs
in parallel during high probability disaster periods e.g. seasonal monsoons or rains.

## Docker

SMDRM components are distributed on multiple Docker hosts i.e. virtual machines
using [Docker in Swarm mode](https://docs.docker.com/engine/swarm/).

# What's Next?
{: .no_toc }

Let us [get started]({{ site.baseurl }}/smdrm/get_started)!
