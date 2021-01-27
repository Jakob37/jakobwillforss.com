---
title: "Snakemake - A Friendly Introduction"
author: "Jakob Willforss"
date: '2021-01-26'
output:
  html_document:
    df_print: paged
Description: ''
categories:
- dataprocessing
- tutorial
DisableComments: no
Tags: []
slug: snakemake-a-friendly-introduction
tags:
- beginner
- tutorial
- snakemake
Categories: []
---

I made a tutorial in Snakemake for RSG-Nordic and [RSG-Sweden](rsg-sweden.iscbsc.org). You can access the full tutorial with data, scripts and solutions at [GitHub](https://github.com/Jakob37/SnakemakeAFriendlyIntroduction).

Presentation slides are available [here](https://docs.google.com/presentation/d/1j3qtPVz4MRulzcZBKeumWgLvsTPlTl577YVqc6KDHPY/edit)

But, why learn Snakemake? In brief:

Snakemake is a workflow-based system built to make reproducible analyses easy to build, extend and rerun.

There are several arguments for using it in your bioinformatics analyses:

* Reproducibility - preserve the exact sequence of commands used to generate certain results.
* Modularity - giving your analysis flexibility, and making it easy to follow the analysis flow.
* Ease of parallel processing - optimally use your resources with minimal effort.
* Support for managing versions in containers such as [Docker](https://www.docker.com) or [Singularity](https://sylabs.io).

With that said, I think you will find Snakemake a powerful addition in your set of analysis-tools, well worth the relatively limited time and effort needed to pick up.

