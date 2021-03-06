---
permalink: /cli/
title: "Commandline Interface"
---

The SIRIUS commandline tool can be called via the "binary/startscript" by
simply running the command in your commandline:
```shell
sirius --help
```

You can always use the `--help` option to get a documentation about the available
commands and options.

Since version 4.4.0 the SIRIUS commandline program is designed as a
toolbox that provides different tools (subcommands) for metabolite
identification. This tools can be concatenated to *toolchains* to
compute multiple analysis steps at once. We distinguish subcommands of
the following categories:

  - **CONFIGURATION:** The config tool can be executed before every
    toolchain or standalone tool to set all configurations available in
    SIRIUS from the command line.

  - [**STANDALONE:**]({{ "/cli-standalone/" | relative_url }}) Tools that run Standalone and cannot be concatenated
    with other subtools. These are usually tools for configuration
    purposes.

  - **PREPROCESSING:** Tools that prepare input data to be compatible
    with SIRIUS.

  - **COMPOUND TOOL:** Tools that analyze each compound (instance) of
    the dataset individually and can be concatenated with other tools.

  - **DATASET TOOL:** Tools that analyze all compounds (instances) of
    the dataset simultaneously and can be concatenated with other tools.

Each subtool can also be called with the `--help` option to get a documentation
about the available options and possible follow up commands in a
toolchain. For the `formula` tool the command would be:

```shell
sirius formula --help
```

## SIRIUS: Identifying Molecular Formulas

One main purpose of SIRIUS is identifying the molecular formula of a
measured ion. For this task SIRIUS provides the `formula` tool. The most basic way
to use the `formula` tool is with the generic text/CSV input:

```shell
sirius [OPTIONS] -1 <MS FILE> -2 <MS/MS FILE> -z <PARENTMASS> --adduct <adduct> formula
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
sirius [OPTIONS] --input demo-data/ms formula [OPTIONS]
```

SIRIUS will pick the meta information (parentmass, ionization etc.) from
the `.ms` files in the given directory. This allows SIRIUS to run in batch
mode (analyzing multiple compounds without starting a new jvm process
every time).

Besides the raw results like the fragmentation trees in `json` format, SIRIUS
will output a summary containing the `rank`, `molecularFormula`, `adduct`,
`precursorFormula`, `rankingScore`, `SiriusScore`, `TreeScore`,
`IsotopeScore`, `numExplainedPeaks`, `explainedIntensity`, `medianMassErrorFragmentPeaks(ppm)`,	
medianAbsoluteMassErrorFragmentPeaks(ppm),	massErrorPrecursor(ppm) on compound
level and a summary containing the top hits for all compounds on project
level.

The `SiriusScore` is the sum of the `TreeScore` and the
`IsotopeScore`. The tool uses the `SiriusScore` for ranking. If
the `IsotopeScore` is negative, it is set to zero. If at least one
`IsotopeScore` is greater than 10, the isotope pattern is considered
to have *good quality* and only the candidates with best isotope pattern
scores are selected for further fragmentation pattern analysis.

### Computing fragmentation trees

If you already know the correct molecular formula and just want to
compute a fragmentation tree, you can specify a single molecular formula
with the option. SIRIUS will then only compute a tree for this molecular
formula. If your input data is in format, the molecular formula might be
already specified within the file. If a molecular formula is specified,
the parent mass can be omitted. However, you still have to specify the
ionization (except for default value `[M+H]+`):

```shell
sirius -f C20H19NO5 -2 demo-data/txt/chelidonine/_msms1.txt demo-data/txt/chelidonine_msms2.txt formula
```

### Analysis Profiles

If you want to analyze spectra measured with `Orbitrap` or `FT-ICR`, you
should specify the appropriate analysis profile. A profile is a set of
configuration options and scoring functions SIRIUS will use for its
analysis. For example, the and profiles having tighter constraints for
the allowed mass deviation but do not rely so much on the intensity of
isotope peaks. You can set the profile with the `-p <name>` option. By default, `qtof` is
used.

See the following examples for running the `formula` sub-tool of the SIRIUS commandline
tool:

## ZODIAC: Improve Molecular Formula Identifications

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
sirius -i <input> -o <output> formula -c 50 zodiac
```

When using ZODIAC, it is reasonable to increase the maximum number of
formula candidates (`-c`) that are stored after running . These candidates
are input to ZODIAC. If the correct candidate is missing, ZODIAC cannot
recover it. In order to reduce memory consumption and running time,
ZODIAC uses a dynamic number of candidates per compound based on the m/z
— the idea is, for low-mass compounds the correct molecular formula is
much more likely to be in the, say, top 10. By default, ZODIAC uses 10
candidates for compounds with m/z lower equal to 300 (`--considered-candidates-at-300 10`) and 50
candidates for compounds with m/z greater equal to 800 (`--considered-candidates-at-800 50`).

The density of the ZODIAC network mainly depends on two parameters: `--edge-threshold`
(default:0.95) and `--minLocalConnections` (default:10). The edge threshold defines the ratio of
all possible edges between candidates that are discarded. Because most
formula candidates are incorrect (there is only one correct candidate
per compound) we assume most edges are spurious and we throw away the
95% with lowest score. However, to prevent compounds being
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
sirius -i <input> -o <output> formula -c 50 zodiac --minLocalConnections 0 --edge-threshold 0.99
```

## CSI:FingerID: Identifying Molecular Structures

With the `structure` tool you can search for molecular structures with CSI:FingerID.
To run CSI:FingerID you need to execute the `formula` tool first. You might also
want to run the `zodiac` tool for improved formula ranking if your data is derived
from a biological sample or any other set of derivatives.

With `--databases` you can specify the database SIRIUS should search in. Available are, among other
`pubchem` and `bio`.

The `structure` tool will generate a `structure_candidates.csv` for each compound containing an ordered
candidate list of structures with the CSI:FingerID score. Furthermore, a `compound_identification.csv`
file will be generated containing the top candidates from all compounds
ordered by their confidence.

```shell
sirius -i demo-data/ms/Bicuculline.ms -o <output>formula -c 10 structure --database pubchem
```

When running `structure` together with `zodiac` the command could look like this:

```shell
sirius -i <input> -o <output> formula -c 50 zodiac structure --database bio
```

## CANOPUS: Predicting Compound Classes without Identification

The `canopus` tool allows you the predict compound classes from the probabilistic
molecular fingerprint predicted by CSI:FingerID. So `canopus` can even provide
compound class information for unidentified compound with no hit in a
structure database:

```shell
sirius -i <input> -o <output> formula -c 10 structure --database pubchem canopus
```

## PASSATUTTO: Decoy Spectra from Fragmentation Trees

The `passattuto` tool allows you to compute high quality decoy spectra from
fragmentation trees provided by the `formula` tool. Assume your are using a
spectral library as input you can easily create a decoy database based
on this spectra:

```shell
sirius -i <spectral-lib> -o <output> formula passatutto
```

If no molecular formulas are annotated to the input spectra the best
scoring candidate will be used for decoy computation instead.

## LCMS-align: Feature detection and feature alignment

The `lcms-align` tool allows you to import mzML/mzXML files into SIRIUS. It performs
feature detection and feature alignment based on the MS/MS spectra and
creates a SIRIUS project-space which is then used to execute followup
analysis steps:

```shell
sirius -i <mzml(s)> -o <output> lcms-run formula
```

