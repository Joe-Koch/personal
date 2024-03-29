---
layout: single
title: Resources
excerpt: "A curated list of SWE resources that made an impact on me."
author_profile: true
toc: true
---

- ### [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/)
  This free course from MIT covers holes that might exist in a CS education, and definitely exist in a math education. Made to "teach you how to master the command-line, use a powerful text editor, use fancy features of version control systems, and much more!", it makes the world outside of user-friendly GUI's more inviting, and answers questions like What do the numbers mean when you're port-forwarding a docker container? What does running `. my_filename` do, and why is it part of so many download instructions? Is learning vim worth it? Going through the resources made me feel more confident as a software engineer.

- ### [The Official Git Tutorial](https://git-scm.com/docs/gittutorial) 
  Learning git can be intimidating because it's one of the first tools you learn, it's hard to tell what's going on from the command line, and any mistake you make is written indelibly in the history, highly visible, with your name attached. I've worked with data scientists who shared jupyter notebooks by emailing them back and forth, or who had another copy of all the code in the same directory and they'd eventually delete the original, and learning how to share code was step 1 in improving code quality. There are 100's of git tutorials out there, and the official one is a good place to start.

  ![Git XKCD](https://imgs.xkcd.com/comics/git.png)

- ### [Software Engineering at Google: Lessons Learned from Programming Over Time. Curated by Titus Winters, Tom Manshreck and Hyrum Wright ](https://abseil.io/resources/swe-book)
  I think about something from this book at least once a week. It's about building a strong culture, highly abstracted descriptions of what tools need to accomplish and why, all with a throughline of how the culture and tools interplay. And it has a lot of one-liners to --steal-- use like "the Beyonce rule": if you liked it then you shoulda put a test on it, "Haunted graveyards": the parts of code that are so ancient, obtuse, or complex that no one dares enter it, or the concept of "psychological safety" at work. At the time of writing, the digital version is free.

- ### [Hidden Technical Debt in Machine Learning Systems by Sculley et al.](https://proceedings.neurips.cc/paper_files/paper/2015/file/86df7dcfd896fcaf2674f757a2463eba-Paper.pdf)
  This paper lists several ways ML systems can quickly rack up tech debt. It was published in 2015, so you'd expect it to be irrelevant by now, but it's painfully (or wonderfully?) prescient. Every ML tool I've loved, from Dagster to MLFlow to Great Expectations, tries to solve some of the system-level anti-patterns they wrote about.

- ### [Kubernetes Deployment Antipatterns](https://codefresh.io/blog/kubernetes-antipatterns-1/) by [Kostis Kapelonis](https://codefresh.io/author/kostiscodefresh-io/)
  I love how this guide was written. 15 extremely specific bad practices, why they're bad, and how to avoid them. A lot of blogs are kind of high level and info-light, where you walk away not exactly remembering what it even said, but this one is highly practical and opinionated. Check if you've implemented any antipatterns in your Kubernetes deployments.

- ### [Unfair Predictions: 5 Common Sources of Bias in Machine Learning by Conor O'Sullivan](https://towardsdatascience.com/algorithm-fairness-sources-of-bias-7082e5b78a2c)  
  A lot of people, even data scientists, believe that so long as your ML model isn't taking in demographic data like sex, race, class, etc., then the model can't be racist/sexist/classist. That belief can even discourage them from collecting that demographic data in the first place, making it impossible to quantify just how biased the models even are. This Towards Data Science article explains things like proxy variables and unbalanced samples in plain English. I don't have a ton of praise for this particular article because it's very introductory-level and tame, but it could be useful for meeting people where they're at. 

- ### This Dilbert Comic
  Wisdom passed down to me from an old mentor.
  {% include figure image_path="assets/images/whatcolordatabase.webp"%}

- ### [Meetup | Find Local Groups, Events, and Activities Near You](https://www.meetup.com)
  The best resource of all: other people. Join groups like Utah Data Engineers, Women Who Go, or MLOps and AI Developers to find meetups near you.
