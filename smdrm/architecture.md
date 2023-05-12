---
layout: default
title: Architecture
parent: SMDRM
nav_order: 3
---

# Diagram
{: .no_toc }
<br>
![SMDRM Diagram]({{ site.baseurl }}/images/smdrm/smdrm-diagram.drawio.png)


# Components
{: .no_toc }

1. TOC
{:toc}

## API

HTTP REST API service to create, and manage historical and real-time data collections.

The service allows you to:
- extract artifacts given a collection `tag` identifier,
- get place names from our gazetteer,
- publish/unpublish data collections,
- schedule/run realtime data processing pipelines.

## Dashboard

Powered by Kibana, the Dashboard gives you a visualization tool to analyse your data collections.

## Database

Powered by Elasticsearch, the Database is the centralized data storage that provides data to the Dashboard.

## Tasks Queue

The Tasks Queue enables asyncronous execution of time and resource expensive data pipeline jobs scheduled via the REST API.

## Worker

The Worker is responsible for the execution of data pipeline jobs.
In case of a great number of data pipeline jobs, multiple workers may be spawn (horizontal scaling).

## NFS share

The NFS share is a shared location accessible by the services of the swarm via a Docker storage volume.
It is a backup location where the result of the data pipeline jobs is stored.

