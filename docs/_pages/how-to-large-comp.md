---
permalink: /faq/how-to-large-comp
title: "How to configure SIRIUS to compute large data sets that contain high mass compounds?"
---
When computing molecular formulas with SIRIUS a few high mass compounds usually need most of the computing time, and some
of them might not finish computing in reasonable time at all and block your whole analysis.

The most straightforward solution is to exclude high mass compounds from the analysis,
by setting a mass threshold. This will usually allow you to annotate the vast majority of your data. However, many of
the higher mass compounds will work just fine, and it would be a pity to not annotate them. **Therefore, the recommended
solution is to set a *per-compound timeout* (`--compound-timeout`) so that a few *hard cases* cannot block your analysis**.
---

There are two main reasons why the running time of the molecular formula identification takes very long for a specific 
compound:

**1. Extremely high number of candidates for the given precursor mass (most common reason).**

Possible solutions:
* If you are only interested in molecular structure database hits, you can only consider formulas present in the molecular structure database. Restricting the number of candidates to a database usually solves all running time problems. Keep in mind that you might miss the correct *novel* molecular formula for some compounds; yet, the confidence score should make it possible to identify these cases. This is **not suggested** if you are interested in CANOPUS results.

* Set a **per-compound** timeout (**not** per-tree) to prevent the malicious cases from blocking your dataset analysis.

**2. Optimizing the fragmentation tree for a candidate takes very long. Running times for solving the optimization problem to compute the fragmentation tree generally increases with the size of the tree but for some rare, extremely hard cases, it may take unreasonably long.**

Possible solutions:
* Use a commercial ILP Solver. Even if multithreading works very well since we switched to CLP as integrated solver, the commercial solvers (Gurobi and CPLEX) are still noticeably, faster especially for the *hard cases*.
* Enable “heuristic-algorithm-only” for compounds above a certain mass (this is possible since SIRIUS version 4.7.0). The resulting tree might not be optimal for certain instances, but still “good enough” for CSI:FingerID; and a heuristic tree is better than no tree.
* Set a timeout per-compound (`--compound-timeout`) or per-tree (less predictable) (`--tree-timeout`), to prevent the malicious cases from blocking your dataset analysis.


**Note:** Selecting many "rare" elements can also dramatically increase the number of formula candidates, which can result in immense running time even for compounds with lower masses.
