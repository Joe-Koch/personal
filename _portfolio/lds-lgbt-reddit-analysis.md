---
title: "LDS Dicussion on LGBT Issues"
excerpt: "Historical analysis of reddit discourse regarding LGBT issues on the LDS-related subreddits"
layout: single
header:
  overlay_image: /assets/images/lds-lgbt/pride-banner.jpg
sidebar:
  - text: "<a href='https://github.com/Joe-Koch/mormon-queer-analysis'>View the project's code</a>"
author_profile: true
toc: true
tags:
  - Dagster
  - DuckDB
  - data visualization
  - Python
---

<!-- ## Background -->

## Results

<iframe src="https://lds-lgbt-app-algl4vdm3jedzrnpc3alh9.streamlit.app/" width="700" height="400"></iframe>

You can also intereact with the streamlit app on streamlit sharing [here](https://lds-lgbt-app-algl4vdm3jedzrnpc3alh9.streamlit.app/). Clicking on a cluster will show the summary and some sample reddit comments from the cluster.


### Interesting findings:
- Many of the clusters seemed to respond to current events like The Boy Scouts of America rescinding the long-standing ban on openly homosexual youth in the program, publicized suicides and studies on gay suicide, and California's Prop 8. 
- Spikes in calls for no personal attacks immediately follow spikes in sarcastic comments.
- Predictably, r/exmormon had the most discussion on leaving the faith.
- The "homosexuality is a sin" frequency did not decline over the last decade, with spikes as recently as summer 2021. 
- Many of the comments were quite intimate, sharing personal experiences and kindly advice.
- Polygamy comparisons held steady over time.
- OpenAI's embeddings tended to cluster similar but opposing statements very closely. E.g. "I believe in gay marriage" was closer to "I don't believe in gay marriage" than it was to "I support human rights". This made it relatively unhelpful for analysis on support and opposition for LGBT issues. 

## How it's done

In a nutshell, this project uses Dagster to create a pipeline that:
  - Pulls all available historic data from LDS-related subreddits from a data dump API
  - Narrows the posts and comments down to the ones related to LGBT+ issues
  - Puts those through OpenAI's API to get text embeddings
  - Does K-means clustering on those embeddings
  - Uses OpenAI chat API to summarize each cluster 
  - Creates an interactive streamlit app to see cluster frequencies over time 

IMO, the code itself was more interesting than the final visualization. You can check out a detailed description highlighting the best features, as well as the code, [on github](https://github.com/Joe-Koch/mormon-queer-analysis). 

# The stack

- [Dagster](https://dagster.io/): data orchestration. A more modern Airflow alternative, Dagster's doing most of the heavy lifting in this project
- [dbt](https://www.getdbt.com/product/what-is-dbt): data build tool 
- [DuckDB](https://duckdb.org/why_duckdb): an embedded analytical database. Lightweight enough to run locally alongside my pipelines, nimble enough for online analytical processing (OLAP)
- [OpenAI API](https://openai.com/blog/openai-api): text embeddings and summaries of the clusters 
- [Poetry](https://python-poetry.org/): dependency manager. If I ever come back to this project 3 years from now, when all the dependencies are out of date and pip has updated how it resolves conflicts, Poetry will 
- [scikit-learn](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html): K-means clustering
- [Streamlit](https://streamlit.io/): creating and hosting (for free!) an interactive data visualization 
- [Arctic Shift API](https://github.com/ArthurHeitmann/arctic_shift/tree/master/api): source of historic reddit data. No money went towards buying Reddit data in this project.
