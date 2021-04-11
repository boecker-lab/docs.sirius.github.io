---
permalink: /faq/
title: "Frequently asked questions (FAQ)"
---

### How can I export result in SIRIUS 4.4 GUI?
SIRIUS 4.4 saves all results and data immediately to disk. This output is a well organized folder structure (project-space). 
If you have not specified any custom location (`SaveAs` button), SIRIUS stores everything in a temporary location per default. 
You can see the path of current project-space location in the header of the SIRIUS window.

### How can I access the Probability of [ClassyFire](http://classyfire.wishartlab.com/) classes predicted by [CANOPUS]({{ "/cli/#canopus-predicting-compound-classes-without-identification" | relative_url }})
In your project space directory there should be a `canopus.tsv` file. 
This file lists all compound classes with meta information, and their *relative index*. 
The relative index (starting with 0) tells you which line in the canopus `.fpt` files belongs to which compound class.

Alternatively, you can use the [canopus_treemap](https://github.com/kaibioinfo/canopus_treemap) python library 
which contains code for parsing the compound classes from the project space.