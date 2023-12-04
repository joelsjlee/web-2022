---
layout: post
title: Byline Normalization in Newspaper Data
description: Clustering and Normalizing Textual Metadata
---
![The logo of History Unfolded.](../../assets/images/huf-logo.jpg)
*The History Unfolded Logo*

[History Unfolded](https://newspapers.ushmm.org/) is a citizen history project of the United States Holocaust Memorial Museum in Washington, DC. It relies on the power of crowdsourcing to create a unique dataset of newspaper articles so that we can better understand what news Americans had access to throughout the Holocaust.

In this post, we will explore how we can cluster and normalize byline metadata at the textual level.

This is a series that is based on my work through the 2022-2023 year in working with exploratory methods and tools on citizen history data. In this article we will be using Python and Jupyter Lab.

# The Task
When users upload an article in History Unfolded, the text field for inserting the byline is an empty text field. This is to ensure that users are able to accurately write down either the author or the wire service responsible for the article. However, this also poses an issue for organizing the data, in that we end up with different strings for the same byline (ex. 'AP' and 'The Associated Press'). Oftentimes this is not an error of the user; the actual byline printed in the newspaper is reflected in a variety of ways.

# Data Exploration
Let’s start by taking a look at the Byline data in History Unfolded and making note of things that may be useful for us. I have used JupyterLab and the data analysis library Pandas to wrangle with the History Unfolded data to display it sorted decreasingly by its count.

![A list output of the most counted bylines in the HUF dataset.](../../assets/images/byline-list.jpg)
*List output of the most counted bylines in the HUF dataset*

There are a couple things that we can glean from this data by looking at the bylines how the user inputted them:

Broadly speaking, there seems to be three main forms that these bylines take: The wire service, the author of the story, and then the two together.
1. Just the wire service (AP, UP, INS, NEA, …)
    - These bylines are by far the most uniform and populous entries.
    - These bylines also appear in different formats (AP as Associated Press, AP Wirephoto, A.P.)
2. The author of the story (“Dorothy Thompson”)
    - These bylines have two main issues, the primary one being the wire service is not mentioned (Although this is normal for some independent papers or opinion pieces/editorials). Additionally there are often differences in the spelling or casing of the author name (Joseph W. La Bine appears further down the list as Joseph W. LaBine).
3. Both the author of the story and the wire service (“Louis P. Lochner (AP)”)
    - These bylines combine both issues to be the most diverse and complicated kinds of bylines.
    - The problem is finding bylines where 

With so much variability and the issue of connecting authors to their potential wire service, it’s clear that a tailored, rule-based approach is the best way to proceed if we want to try and categorize these bylines.

Let’s first start by splitting off all of the articles into two subcategories: News Articles and other. That way we can leave the potential opinion pieces and other articles that are more likely to be written by authors separate from the wire aside. 


