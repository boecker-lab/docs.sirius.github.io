---
permalink: /cli-standalone/
title: "Standalone CLI tools"
---
Standalone tools provide additional SIRIUS related tasks that do not fit into the SIRIUS identification workflow.
These can e.g. be configuration tasks, file conversion tasks or features that might be helpful for downstream analysis.
These tool cannot be part of a toolchain and have to be executed in a separate command. Each of these tools has its own
help message (`sirius <TOOLNAME> -h`).


## Custom database tool
The `custom-db` tool allows you to import custom structure databases from a `csv/tsv` (tab separated) file with 
structures given in `SMILES` format. Optionally a database `id` and a `name` can be given. 

```
CN1CCCC1C2=C[N+](=CC=C2)[O-]	id-01	Nicotin
CN1C=NC2=C1C(=O)N(C(=O)N2C)C	id-03	Caffein
CN1CCC2=CC3=C(C=C2C1C4C5=C(C6=C(C=C5)OCO6)C(=O)O4)OCO3 id-05 Bicculine
```

You can import multiple files with compounds as SMILES into one DB. If a given structure can be found in
SIRIUS' internal structure DB then fingerprint is downloaded from there, otherwise it is computed locally on your computer 
which might take some time for many structures.


```shell
sirius -i <structure.tsv> cistom-db --name myDB --output /some/dir
```

Note, that we usually use PubChem standardized SMILES to represent the structures for our machine learning methods. 
PubChem standardization is not yet part of this import process. For best possible results we recommend standardizing
your SMILES using the PubChem standardization before importing them, but this step is **not** mandatory.


## Similarity tool
The `similarity` tool allows you to compute different similarity measures between compounds.
It takes a SIRIUS project-space (or any input format SIRIUS can convert into a project such as `ms`, `mgf` or `cef`) 
as input (`sirius -i <INPUT>`) and calculates all against all similarity matrices for the compounds
in the project-space and stores them in the given output directory given by `-d`.

```shell
sirius -i <project-space> similarity --cosine --ftalign --ftblast <SPECTRA_LIB> --tanimoto -d <OUTPUT>
```

### Cosine Similarity   (`--cosine`)
Computes the cosine similarity of the merged MS/MS between all compounds.
Just the spectra are needed no additional computation have to be performed beforehand.

### Fragmentation Tree alignment Similarity  (`--ftalign`)
Computes the tree alignment score of the top ranked fragmentation tree between all compounds.
To perform this computation the input project-space needs to contain the fragmentation trees from the `formula`/`sirius`
subtool. So the `formula` subtool has to be executed beforehand. The Alignment method is described in
["*Identifying the Unknowns by Aligning Fragmentation Trees*"](https://doi.org/10.1021/ac300304u)

### FT-Blast (`--ftblast`)
Computes fragmentation tree alignments between all compounds in the dataset, incorporating the given fragmentation
tree library as described in ["*Identifying the Unknowns by Aligning Fragmentation Trees*"](https://doi.org/10.1021/ac300304u).
The input project-space needs to contain fragmentation trees computed with the `formula`/`sirius` subtool. 
So the `formula` subtool has to be executed beforehand. The given library (`--ft-blast=<LIB_PATH>`) can either be another
SIRIUS project-space containing fragmentation trees, or a directory containing fragmentation trees in `json` format.

### Tanimoto Similarity (`--tanimoto`)
Computes the tanimoto similarity of the top ranked predicted fingerprints between all compounds.
The input project-space needs to contain the predicted fingerprints from the `structure`/`fingerid`
subtool. So the `formula` and the `structure` subtool have to executed beforehand.
Note that the fingerprints compared are probabilistic. The tanimoto computation for two probabilistic fingerprints 
$F$ and $F'$ of length $n$ is computed as follows:

$$\frac{ \sum_{i=1}^{n} F_i \cdot F'_i } { \sum_{i=1}^{n} 1 - (1 - F_i) \cdot (1 - F'_i) }$$

##### Example 1
Let's say we want to compute  `--cosine`, `--ftalign`  and `--tanimoto` similarities. For that we need a proeject-space
that contains spectra, fragmetations trees and fingerprints. So we start computing them with the following command:
```shell
sirius -i <input-data.mgf> -o <my-project> formula structure
```

We can now use the resulting project-space (`my-project`) as input for the similarity computation:
```shell
sirius -i <my-project> similarity --cosine --ftalign --tanimoto --d <output-dir>
```

##### Example 2
Let's say we want to compute `--ftblast` similarities. For that we need one project-space (my-project)
that contains fragmentation trees and another project-space that contains fragmentation trees for our spectral library 
(library-project). Assuming we use `.mgf` format for both, our input spectra and the library spectra, we have to execute 
the following commands.

Compute fragmentation trees for input data:
```shell
sirius -i <input-data.mgf> -o <my-project> sirius
```

Compute fragmentation trees for the library spectra:
```shell
sirius -i <library-data.mgf> -o <library-project> sirius
```

No we have the input we need for the `--ftblast` similarity computation
```shell
sirius -i <my-project> similarity --ftblast <library-project> -d <output-dir>
```


## Mass Decomposition tool
The `decomp` tool provides the SIRIUS internal ["*Efficient mass decomposition*"](https://doi.org/10.1145/1066677.1066715) 
algorithm as standalone tool to decompose masses with given deviation, ionization, chemical alphabet and chemical filter.

## MGF export tool
The `mgf-export` tool exports the spectra of a given input project-space as `.mgf` for use with other tools like [GNPS](https://gnps.ucsd.edu/ProteoSAFe/static/gnps-splash.jsp).
The `--quant-table` option allows to export an additional feature quantification table (`csv`),
e.g. to export a SIRIUS project-space for [GNPS Feature Based Molecular Networking](https://ccms-ucsd.github.io/GNPSDocumentation/featurebasedmolecularnetworking/):
```shell
sirius --input <project-space> MGF --merge-ms2 --quant-table <table.csv> --output <spectra.mgf>
```
Note, quantification information are only available if the source of the project-space was in `mzml`(`mzxml`).

## Fragmentation tree export tool
The `ftree-export` tool exports the fragmentation trees of a given project-space (`sirius -i <INPUT>`) in
various formats (`--json`, `--dot`) to a given output directory (`--output <DIR>`). The `--all` option specifies whether
 fragmentation trees of *all* formula candidates (of a compound) or only of the top formula candidate will be exported.

The following example will export the top ranked fragmentation tree of all compounds in the project space in `dot` format.
```shell
sirius --input <project-space> ftree-export --dot --output <output-dir>
```

The commandline tool [Graphviz](https://www.graphviz.org/) can transform `.dot` files into image formats (PDF, SVG, PNG etc.). 
After [installing Graphviz](https://graphviz.org/download/) you can create a `.pdf` files as follows:

```shell
dot -Tpdf <tree-file.dot> > <tree-file.pdf>
```

{% capture fig_img %}
![Foo]({{ "/assets/images/bicculine_frag_tree.svg" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Fragmentation Tree exported as .dot and converted to svg.</figcaption>
</figure>

Note: The [SIRIUS GUI]({{ "/gui/#export-tree-visualization" | relative_url }}) allows to directly export the rendered 
tree as vector or pixel graphics. 

## Project-space tool
The `project-space` tool Modifies a given project-space (e.g. merging, splitting, filtering, version conversion). 
Read project(s) with `--input`, apply modification and write the result via `--output`. If either only `--input` or 
`--output` is given the modifications will be made in-place.
