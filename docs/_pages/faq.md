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

### How should I interpret COSMIC confidence values?
COSMIC confidence scores need to be interpreted with some care. It is important to understand, that they do not represent probabilities, and thus there is no statistical interpretation of a confidence value. If doing large scale analysis, one should focus on the highest confident hits (for example the Top 1/5/10%), (mostly) regardless of their confidence value. 

### An annotation received a low COSMIC score, even though am I sure it is correct!
In this example, the spectrum of Campherol from the SIRIUS demo data receives the low COSMIC score of 0.3, even though the structure annotation is correct. The reason for this is, that the database we searched in contains multiple extremely similar structures, that have very similar fingerprint representations as well as CSI:FingerID scores. If the true structure was not know, each of these highly-similar compounds could be the correct structure. Hence, the hit receives a low confidence. This is not a limitation of CSI:FingerID and COSMIC but rather of mass spectrometry. The MS/MS of these structures will look very similar.

{% capture fig_img %} ![Foo]({{ "/assets/images/camph-1.png" | relative_url }}) {% endcapture %}
{{ fig_img | markdownify | remove: "
" | remove: "
" }} Overview tab. 
