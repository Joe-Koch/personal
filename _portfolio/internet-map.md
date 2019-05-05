---
layout: single
title: "Internet Speeds in Rural Areas"
excerpt: "A map showing the fastest internet speeds available in New Hampshire and Vermont."
author_profile: true
sidebar:
  - text: "<a href='https://github.com/ZoeKoch/rural-internet'>View the project's code</a>"
toc: true
tags:
  - geographical data
  - R
  - SQL
  - Geopackage
  - CARTO
---

# Visualizing internet access in rural areas 

## The Map

{% include figure image_path="/assets/images/internet/internet-population-map.png" alt="" caption="Internet speed and population maps." %}

The Center on Rural Innovation was started with the goal of bringing sustainable economic success to small town rural America. They're currently creating tools to help businesses learn more about Opportunity Zones, a new tax-incentive aimed at increasing private investment in low income census tracts. This map of the fastest internet upload speeds available in each census tract was built to help businesses see what's available across Vermont. 

## Data Sources

* Broadband Data Source: the latest [FCC Form 477](https://www.fcc.gov/general/broadband-deployment-data-fcc-form-477) wireline deployment data release
* Population: [FCC estimates](https://www.fcc.gov/reports-research/data/staff-block-estimates) of housing unit, household and population counts for each block for 2010 (US Census) and 2017 (Commission staff estimate)
* Tract shapes: tract-level shapefiles from the [United States Census Bureau](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.2017.html), as of January 1, 2017.
