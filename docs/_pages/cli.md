---
permalink: /cli/
title: "Commandline Interface"
---

The SIRIUS commandline tool can be called via the "binary/startscript" by
simply running the command in your commandline:
```shell
sirius --help
```

***

**NOTE: It is usually a good idea to use the `--help` option to obtain an overview of the available commands and options. 
This will guarantee that you will have a description of the commands that directly matches your SIRIUS version.**

In the following, the most important commands and options are described shortly.

***


The SIRIUS commandline program is designed as a
toolbox that provides different tools (subcommands) for metabolite
identification. These tools can be concatenated to *toolchains* to
compute multiple analysis steps at once. We distinguish subcommands of
the following categories:

  - **CONFIGURATION:** The config tool can be executed before every
    toolchain or standalone tool to set all configurations available in
    SIRIUS from the command line.

  - [**STANDALONE:**]({{ "/cli-standalone/" | relative_url }}) Tools that run Standalone and cannot be concatenated
    with other subtools. These are usually tools for configuration
    purposes. E.g. modifying `project-space` or exporting `MGF` files. 

  - **PREPROCESSING:** Tools that prepare input data to be compatible
    with SIRIUS. E.g. `lcms-align` feature detection and feature grouping.

  - **COMPOUND TOOL:** Tools that analyze each compound (instance) of
    the dataset individually and can be concatenated with other tools. E.g. `formula` annotation, `structure` database search or `canopus` compound class prediction.

  - **DATASET TOOL:** Tools that analyze all compounds (instances) of
    the dataset simultaneously and can be concatenated with other tools. E.g. dataset-wide molecular formula annotatio with `zodiac`.

Each subtool can also be called with the `--help` option to get a documentation
about the available options and possible follow up commands in a
toolchain. For the `formula` tool the command would be:

```shell
sirius formula --help
```

Results of all tools that produce a project space as output (`--output` option), can be later viewed via the [GUI]({{ "/gui/" | relative_url }}).


## LCMS-align: Feature detection and feature alignment [Preprocessing]

The `lcms-align` tool allows you to import mzML/mzXML files into SIRIUS. It performs
feature detection and feature alignment based on the MS/MS spectra and
creates a SIRIUS project-space which is then used to execute followup
analysis steps:

```shell
sirius -i <mzml(s)> -o <projectspace> lcms-run formula
```


## SIRIUS: Identifying Molecular Formulas [Compound Tool]

One main purpose of SIRIUS is identifying the molecular formula of a
measured ion. For this task SIRIUS provides the `formula` tool. The most basic way
to use the `formula` tool is with the generic text/CSV input:

```shell
sirius [OPTIONS] -1 <MS FILE> -2 <MS/MS FILES comma separated> -z <PARENTMASS> --adduct <adduct> --output <projectspace> formula
```

Where *MS FILE* and *MS/MS FILE* are either CSV or MGF files. If MGF
files are used, you might omit the `-z` option. If you omit the `--adduct` option,
<span>\[</span>M+?<span>\]</span>+ is used as default. It is also
possible to give a list of MS/MS files if you have several measurements
of the same compound with different collision energies. SIRIUS will
merge these MS/MS spectra into one spectrum.

The more common and **recommended** way is using input files in `.ms` or `.mgf`
format (with MSLEVEL and PEPMASS meta information). Such files contain
all spectra for a compound together with their meta data. They can also
contain multiple compounds per file. Further SIRIUS is able to crawl an
input directory for supported files:

```shell
sirius [OPTIONS] --input <inputFile> --output <projectspace> formula [OPTIONS]
```

SIRIUS will pick the meta information (parentmass, ionization etc.) from
the `.ms` files in the given directory. This allows SIRIUS to run in batch
mode (analyzing multiple compounds without starting a new jvm process
every time).

Results such as scored molecular formula candidates and corresponding fragmentation trees in `.json` format are written in the `<projectspace>`.
If the command `write-summaries` is used, SIRIUS
will output a summary file on compound level containing among others the 'rank', 'molecularFormula', 'adduct',
'precursorFormula', 'rankingScore', 'SiriusScore', 'TreeScore',
'IsotopeScore', 'numExplainedPeaks', 'explainedIntensity', 'medianMassErrorFragmentPeaks(ppm)',	
'medianAbsoluteMassErrorFragmentPeaks(ppm)', 'massErrorPrecursor(ppm)' and a summary containing the top hits for all compounds on project
level.

The `SiriusScore` is the sum of the `TreeScore` and the
`IsotopeScore`. The tool uses the `SiriusScore` for ranking. If
the `IsotopeScore` is negative, it is set to zero. If at least one
`IsotopeScore` is greater than 10, the isotope pattern is considered
to have *good quality* and only the candidates with best isotope pattern
scores are selected for further fragmentation pattern analysis.

### Computing fragmentation trees

If you already know the correct molecular formula and just want to
compute a fragmentation tree, you can specify the formula using `--formulas`. SIRIUS will then only compute a tree for this molecular
formula. If your input data is in `.ms` format, the molecular formula might be already specified within the file. Note: the `--formulas` options
can also be used to specify a comma-separated list of candidate molecular formulas.

```shell
sirius -i <input> --output <projectspace> formula --formulas <formula>
```

### Instrument-specific parameters

Datasets have different mass errors, level of noise and accuracy of isotope pattern intensities, depending, among others, on instrument type and setup.
By default, SIRIUS uses a profile for `Q-TOF` data with 10 ppm mass deviation. This should not be interpreted as a Q-TOF-only profile, but is often a good default profile even for data from other instruments.
However, if you are certain that your data has mass errors much below 10 ppm - because if was measured on Orbitrap or FT-ICR - you should probably specify more stringent parameters. 
Adjustments are also necessary if the data is expected to have even higher mass errors.
Both can be accomplished by specifying a different profile and mass deviations. 


You may be familiar with the profile option from the GUI. Using the CLI, you can specify `-p <name>` to either select `qtof` (default) or `orbitrap`.
`orbitrap` will mainly use a different mass deviation of 5 ppm and slightly different settings for isotope scoring. 
For FT-ICR data, we recommend to use the `orbitrap` profile and additionally specify a lower mass deviation, as explained in the following.

You can specify the maximum allowed mass deviations for MS1 and MS2 and separately:

```shell
sirius -i <input> --output <projectspace> formula -p orbitrap --ppm-max 2 --ppm-max-ms2 5
```


## ZODIAC: Improve Molecular Formula Identifications [Dataset Tool]

If your input data is derived from a biological sample or any other set
of derivatives, similarities between different compounds can be
leveraged to improve molecular formula annotation of the individual
compounds. ZODIAC builds a similarity network between molecular formula
candidates of all compounds that where computed via the `formula` tool and
re-ranks these candidates using Bayesian statistics (Gibbs Sampling).
This decreases error rates (of top 1 candidates) by approximately 2 fold
— on challenging datasets that contain many large compounds,
improvements can be much more dramatic.

The `zodiac` tool can be executed after the `formula` tool without the need of many
parameters:

```shell
sirius -i <input> -o <projectspace> formula -c 50 zodiac
```

When using ZODIAC, it is reasonable to increase the maximum number of
formula candidates (`-c`) that are stored after running `formula`. These candidates
are input to ZODIAC. If the correct candidate is missing, ZODIAC cannot
recover it. In order to reduce memory consumption and running time,
ZODIAC uses a dynamic number of candidates per compound based on the m/z
— the idea is, for low-mass compounds the correct molecular formula is
much more likely to be in the, say, top 10. By default, ZODIAC uses 10
candidates for compounds with m/z lower equal to 300 (`--considered-candidates-at-300 10`) and 50
candidates for compounds with m/z greater equal to 800 (`--considered-candidates-at-800 50`). 
In between these threshold the number of candidates is calculated by interpolation. 

The density of the ZODIAC network mainly depends on two parameters: `--edge-threshold`
(default:0.95) and `--minLocalConnections` (default:10). The edge threshold defines the ratio of
all possible edges between candidates that are discarded. Because most
formula candidates are incorrect (there is only one correct candidate
per compound) we assume most edges are spurious and we throw away the
95% edges with lowest score. However, to prevent compounds being
disconnected completely from the rest of the network, we discard edges
in such a way that one candidate per compound is connected to at least
`--minLocalConnections` other compounds. This introduces an individual edge score threshold for
each compound. However, when using `--minLocalConnections`, ZODIAC first has to create the
complete network and filter edges afterwards. Thus, ZODIAC may consume a
large amount of system memory.

For very large datasets, the ZODIAC network may not fit in 1TB system
memory and more. Please, perform a feature alignment between your
LC-MS/MS runs to reduce the number of compounds and thus reduce the size
of the ZODIAC network. If this is still not sufficient, memory
consumption can be dramatically decreased by setting `--minLocalConnections=0`. 
This will allow ZODIAC to filter low weight edges on the fly when creating the network.
**Use this setting with care, since it can result in a badly connected
network that may decrease performance:**

```shell
sirius -i <input> -o <projectspace> formula -c 50 zodiac --minLocalConnections 0 --edge-threshold 0.99
```

## CSI:FingerID: Predicting molecular fingerprints [Compound Tool]

Molecular fingerprints can be predicted via `fingerprints` command after molecular formula candidates
have been calculated running `formula`. Fingerprint prediction is part of CSI:FingerID.
A fingerprint is predicted based on a specific molecular formula candidate
(with corresponding fragmentation tree). By default, fingerprints are predicted for multiple good scoring formula candidates by applying a soft score-threshold on the SIRIUS score.

```shell
sirius -i <input> -o <projectspace> formula fingerprint
```


## CSI:FingerID: Identifying Molecular Structures [Compound Tool]

Using the `structure` tool you can search with CSI:FingerID for molecular structures in a structure database.
To run structure database search, molecular fingerprints need to be predicted in advance by running the `fingerprint` tool first. You might also
want to run the `zodiac` tool for improved formula ranking if your data is derived
from a biological sample or any other set of derivatives.

With `--databases` you can specify the database CSI:FingerID should search in. Available are, among other
`pubchem` and `bio`.

When structure search was performed, the `write-summaries` tool will generate a `structure_candidates.csv` for each compound containing an ordered
candidate list of structures with the CSI:FingerID score. Furthermore, a projectspace-wide `compound_identification.csv`
file will be generated containing the top candidate structure for each compound
ordered by their confidence score.

```shell
sirius -i <input> -o <projectspace> formula structure --database pubchem
```

When running `structure` together with `zodiac` the command could look like this:

```shell
sirius -i <input> -o <projectspace> formula -c 50 zodiac structure --database bio
```

## CANOPUS: Database-free Compound Classes Prediction [Compound Tool]

The `canopus` tool allows you to directly predict compound classes based on the probabilistic
molecular fingerprint that was predicted by CSI:FingerID (`fingerprint` command). Notably, `canopus` can even provide
compound class information for unidentified compounds with no hit in a structure database:

```shell
sirius -i <input> -o <projectspace> formula fingerprint canopus
```


## PASSATUTTO: Decoy Spectra from Fragmentation Trees [Compound Tool]

The `passattuto` tool allows you to compute high quality decoy spectra from
fragmentation trees provided by the `formula` tool. Assume your are using a
spectral library as input you can easily create a decoy database based
on these spectra:

```shell
sirius -i <spectral-lib> -o <projectspace> formula passatutto
```

If no molecular formulas are annotated to the input spectra the best
scoring candidate will be used for decoy computation instead.


