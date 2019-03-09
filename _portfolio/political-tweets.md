---
layout: single
title: "Political Social Media"
excerpt: "Text-based classification of politicians' social media posts."
author_profile: true
sidebar:
  - title: "Co-Author"
    text: "<a href='https://www.linkedin.com/in/abecker93/'>Andrew Becker</a>"
  - text: "<a href='https://github.com/ZoeKoch/politicians-tweet-classifier/blob/master/text_modeling.R'>View the project's code</a>"
toc: true
tags:
  - statistical learning
  - text data
  - classification
  - R
---
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [ ['$','$'], ["\\(","\\)"] ],
      processEscapes: true
    }
  });
</script>
<script src='https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML' async></script>

# Classifying Politicians' Social Media Posts

## The Data
The [Kaggle dataset,](https://www.kaggle.com/crowdflower/political-social-media-posts/home) from Crowdflower's Data For Everyone Library, provides text of 5000 messages from politicians' social media accounts, along with human judgments about the purpose, partisanship, and audience of the messages. Contributors looked at thousands of social media messages from US Senators and other American politicians to classify their content. Messages were broken down into audience (national or the tweeter’s constituency), bias (neutral/bipartisan, or biased/partisan), and finally tagged as the actual substance of the message itself (options ranged from informational, announcement of a media appearance, an attack on another candidate, etc.). 

{% include figure image_path="/assets/images/politician/tweet.png" caption="A post with neutral bias." %}
{% include figure image_path="/assets/images/politician/facebook.png" caption="A post with partisan bias." %}

## Methods and Results

We set out to classify political speech in the form of social media messages based on their content. A variety of methods were employed to attempt to do this, including decision trees, naive Bayes classification, and logistic regression, among others. The majority of methods had a variety of issues with the data being input. In the case of naive Bayes it failed to calculate non-zero probabilities and was as a result simply grouping randomly. This same issue occurred with support vector machines.

Initial success, at least in visualizing what words were indicative of certain classes, was seen using simple decision trees. You can see that the presence or absence of the word 'obamacare' was highly indicative of whether a message was partisan or not. Additionally it's clear that 'live' and 'attend' are indicative of a message being meant for an audience of the politicians constituency, whereas the words 'prevent', 'night', and 'world' are indicative of a message being meant for a national audience.

<figure class="half">
    <a href="/assets/images/politician/trees-1-1.png"><img src="/assets/images/politician/trees-1-1.png"></a>
    <a href="/assets/images/politician/trees-2-1.png"><img src="/assets/images/politician/trees-2-1.png"></a>
    <figcaption>Caption describing these two images.</figcaption>
</figure>

We can see some of the differences in the words used in neutral posts, on the left, versus biased posts, on the right.

<figure class="half">
    <a href="/assets/images/politician/neutral-cloud.png"><img src="/assets/images/politician/neutral-cloud.png"></a>
    <a href="/assets/images/politician/neutral-cloud.png"><img src="/assets/images/politician/neutral-cloud.png"></a>
    <figcaption>Caption describing these two images.</figcaption>
</figure>

Unfortunately, these methods barely had classification rates higher than that of the no information rate. In the case of classifying on bias, the No Information Rate was $0.7382$ and the tree correct classification rate was $0.747$. The confusion matrix (Table 1) indicates that it classified nearly all of the messages as being neutral, and picked out those which contained the word obamacare to classify those as partisan. Classifying by audience had even worse results (Table 2) with a classification rate slightly below the no information rate. Again, from the confusion matrix it seems that simply messages with the words 'live' and 'attend' were classified as being for constituency with the rest being classified as nationally targeted messages.


| Table 1: CM for Tree on Bias | Neutral | Partisan |
| Neutral | 914 | 308 |
| Partisan | 8 | 19 |

| Table 2: CM for Tree on Audience | Constituency | National |
| Constituency | 29 | 82 |
| National | 228 | 910 |

A random forest approach was attempted, and little improvement was seen. In the case of classifying by bias, a correct classification rate of $0.755$ was achieved. This is slightly above the previous $0.747$ rate of the single tree, but it is not a significant improvement. The confusion matrix for both this and audience show that they are similar to those of a single tree (Table 3 and Table 4). For classifying by audience the random forest ended up simply classifying all of the messages as being for a national audience. This is the no information rate, but that resulted in an improvement over a single tree.

| Table 3: CM for Random Forest on Bias | Neutral | Partisan |
| Neutral | 908 | 292 |
| Partisan | 14 | 35 |

| Table 4: CM for Random Forest on Audience | Constituency | National |
| Constituency | 0 | 0 |
| National | 257 | 992 |

We eventually tried logistic regression, and after having that fail to converge, tried a form of penalized logistic regression called Firth Logistic Regression. The Firth Logistic Regression is the equivalent of using a Bayesian Jeffrey's Prior on parameters for the regression, and always results in finite and non-zero results through the addition of a fixed value to the parameters that reduces bias from the MLE estimators of the regression coefficients. Using this method we were able to correctly classify into both bias and audience with rates of $0.922$ and $0.972$ respectively (Table 5 and Table 6). The ROC curves show very good True Positive Rates vs False Positive Rates for both Audience and Bias. This means that these models can be used to classify political messages fairly accurately and can likely be applied to political tweets not included in the data set.

<figure class="half">
    <a href="/assets/images/politician/ROC-1-1.png"><img src="/assets/images/politician/ROC-1-1.png"></a>
    <a href="/assets/images/politician/ROC-2-1.png"><img src="/assets/images/politician/ROC-2-1.png"></a>
    <figcaption>Caption describing these two images.</figcaption>
</figure>

| Table 5: CM for FLR on Bias | Neutral | Partisan |
| Neutral | 905 | 80 |
| Partisan | 17 | 247 |

| Table 6: CM for FLR on Audience | Constituency | National |
| Constituency | 238 | 16 |
| National | 19 | 976 |

In this particular case it's difficult to explain _why_ these models are good at classifying, and incredibly difficult to determine what's going on in the model. 38 different variables are taken into account in order to perform the classification (the absence or presence of particular words) and many of them are non-significant in the model, but their removal results in reductions in model accuracy. It's clear that words like 'obamacare', 'job', 'senate', 'vote', and 'bill' are all important when trying to determine whether a particular message is biased or neutral, but saying something like "The presence of the word 'job' in a message, holding all other words constant, increases the log-likelyhood of that message being biased by $0.4795$" doesn't exactly elucidate the nature of bias in these messages. While you could make some conclusions, saying that words we associate with political agendas may be more likely to be associated with bias, the model is a little more complex than that. When it comes to predicting the audience the same seems to hold true, but other words become important in the decision. Again, these models are better suited for prediction than interpretation, and shedding light on what makes political speech targeted towards a particular group or partisan if difficult to do.
