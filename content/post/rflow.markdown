--- 
title: R Workflow
author: Frank Harrell
date: '2022-05-02'
modified: '2022-06-13'
slug: rflow
tags:
  - 2022
  - data-science
  - graphics
  - r
  - reproducible
summary: 'This article outlines analysis project workflow found to be efficient in making reproducible research reports using R with `Quarto`.  I start by covering the creation of annotated analysis files, discovering missing data patterns, and running descriptive statistics on with goals of understanding the data and the quality and completeness of the data.  Functions in the `Hmisc` package are used to annotate data frames and data tables with labels and units of measurement and to produce tabular and graphical statistical summaries. Several examples of processing and manipulating data using the `data.table` package are given.  Much attention is paid to the use of minimal-assumption methods for describing relationships with continuous variables, avoiding disasters such as computing mean Y as a function of quintiles of body mass index.  Examples of diagramming exclusions of observations from analysis, caching results, doing parallel processing, and running simulations are presented.  This article is a synopsis of the [R Workflow electronic book](https://hbiostat.org/rflow).'
header:
  caption: ''
  image: ''
---
