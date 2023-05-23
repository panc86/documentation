---
layout: default
title: Datapoint
nav_order: 4
parent: SMDRM
---

# Datapoint

A single newline delimited JSON entry that contains the mandatory fields as shown in _table 1_.

# Mandatory Fields

|Field|Type|Description|Note|
|-----|----|-----------|----|
|`created_at`|string|The date and time at which the datapoint is created e.g. `EEE LLL dd HH:mm:ss Z yyyy`|Crucial for temporal analyses.|
|`text`|string|The textual information to be annotated and geocoded|Only textual information that fall inside this field will be considered.|

_Table 1. Mandatory Fields_

# Optional Fields

|Field|Type|Description|
|-----|----|-----------|
|`id`|integer \| string|The unique identifier of a datapoint. It is converted to integer format if provided as string.|
|`lang`|string|The language in which the `text` of a datapoint is expressed.|
|`media`|array[string]|A list of media URLs (e.g. images, videos) attached to a datapoint.|
|`url`|string|The social media platform based public URL of the datapoint.|

# Enriched Fields

|Field|Type|Description|
|-----|----|-----------|
|`floods`|number|Floods annotation score.|
|`impacts`|number|Impacts annotation score.|
|`NER`|object|DeepPavlov Named Entity Recognition tags.|
|`NER.<TAG>`|array[string]|Geographic locations identified by DeepPavlov NER tagger.|
|`places`|object|Geographic attribures placeholder.|
|`places.place_name`|string|The name of the place. Returned only if the GPE exists in the places Geopackage file.|
|`places.region_name`|string|The region name.|
|`places.country_name`|string|The Country name.|
|`places.latitude`|number|Latitude in WSG84 CRS.|
|`places.longitude`|number|Longitude in WSG84 CRS.|
|`text_clean`|string|Normalized textual information|

_Table 3. Data pipeline process generated fields_

[^1]: Facility
[^2]: Geo Political Entity
[^3]: Location

