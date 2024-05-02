---
permalink: /changelog/
title: "Changelog"
---

### SIRIUS 6


### SIRIUS 5
#### 5.8.4 (2023-11-04)
- fixed problem with summary files writing of CANOPUS predictions
- fixed bug where filtering of features would result in omitting features with no available confidence score
- added option to ignore molecular formulas during file import in the GUI

#### 5.8.0 (2023-07-01)
- breaking: rankings and ranking cloumn names of summary files have changed
- improvement: added feature id support from several input formats. 
- improvement: added feature id as separate fields to project wide summary files
- improvement: change display name of ConfidenceScore from COSMIC to Confidence
- improvement: added support for .mat PEAKID field
- improvement: added support of 1-/1+ charge notation in mgf parser

- fix wrong ranks in summary files
- fix: missing adducts in CanopusSummaryWriter
- fix: missing adducts in FormulaSummaryWriter
- fix: bug with wrong NPC classes in Canopus summary files
- fix: null value problem with MS-Dial msp/mat files

#### 5.7.3 (2023-06-14)
- improvement: Buffered parallelization for Fingerprinter subtool
- improvement: use internal browser as fallback for User Portal if system browser not available.
  - fix: fixes bug where account creation and management not possible with conda version of SIRIUS  

#### 5.7.2 (2023-05-26)
- build: SIRIUS docker Image not on dockerhub
- fix: Fixed maxRetentionError getting too small, introduced minError

#### 5.7.1 (2023-05-20)
- feature: Added compound quality flags to Scripting API
- improvement: db import dialog, error handling and description
- Fix: Fixed MS1<->MS2 switch bug

#### 5.7.0 (2023-05-19)
//todo coming soon

#### 5.6.3 (2023-01-12)
- improvement: compounds can now be filtered by cosmic/confidence score (GUI)
- improvement: added `.version` file to project-space that contains SIRIUS version information.
- fix: unreachable availability check url for authentication server which prevented the login request message from occurring.
- fix: fixed  bug when selecting `all` database flag in cli. I behaves now like selecting all databases in the GUI.
- fix: fixed some missing default values in the GUI.
- fix: fixed handling of preset molecular formulas. `.ms` and `.mgf` inputs should now behave equally.
- fix: fixed custom db handling in sirius/formula step. custom database can now be used as formula candidate list instead of denovo.
- fix: fixed custom database import which did crash in 5.6.2
- fix: fixed some typos
#### 5.6.2 (2022-11-03)
- fix: bug that prevents accepting license terms caused by mising content-length header in the request. 
#### 5.6.1 (2022-10-28)
- feature: improved progress information for background computations.
- feature: new scheduling of remote jobs that reduces computation time and improves local cpu utilization.
- **feature: A Beta version of the new SIRIUS background service is now included in the respective build (_service_ suffix).**
- feature: support for MS1 only data in GUI (Isotope Pattern analysis only).
  - MS1 only data from Agilent CEF format can now be imported
- feature: command line autocompletion support for Linux and MacOS. See `sirius --help` for details. 
- feature: allow to write uncompressed (legacy) project-spaces via `--no-compression` pearameter
- improvement: improvements on feature finding and feature alignment
- improvement: more robust custom database importer. Support to import existing DBs into the GUI.
- fix: fixed bug in feature finding that changed negative ion mode data to positive ion mode
- fix: fixed inconsistent indexing of aligned and not aligned mzml/mzxml data that caused an invalid FBMN output.
   - **possibly breaking change**: SIRIUS internal compound index does now start at 1 instead of 0
- fix: several smaller fixes and improvements

#### 5.5.2
- fix: Collsion energy parsing bug
- feature: Update checker 

#### 5.4.1
- **breaking**: User Authentication. A user account and license is now needed to user the online features of SIRIUS. 
The license is free and automatically available for non-commercial use, details [here]({{ "/install/#creating-a-user-account-since-v410" | relative_url }}). 
- **breaking**: New [project-space]({{ "/io/#output" | relative_url }}) compression. Method level directories are now compressed archives to reduce number of files and save storage. 
- **breaking**: Summary writing has been moved to a separate sub-tool (`write-summaries`). Som summary files have slightly changed. Usually just additional columns if at all.
- **breaking**: The `fingerid`/`structure` sub-tool has been split into a  `fingerprint` (fingerprint prediction)
and a `structure` (structure db search) sub-tool. This allows the user to recompute the database search 
without having to recompute the fingerprint and compound class predictions. It further allows to compute `canopus`
compound class prediction without having to perform structure db search.
- **breaking**: Updated Fingerprint vector. Fingerprint related results of SIRIUS 4 projects might have to be recomputed 
to perform some modification (e.g. recompute db-search). Reading the projects is still possible and formula results do not affected.  
- **breaking**: Custom database format has changed. Custom databases need to be re-imported

- feature: **Tool** - **El Gordo** lipid class annotation. 
- feature: **GUI Tool** - **Epimetheus** sub-structure annotation. Combinatorial fragmentation of CSI:FingerID candidates to assign fragment peaks to sub-structures of the candidate.
- feature: **GUI View** - New feature rich **spectrum viewer**. Mirror-plot to compare Isotope pattern and Simulated isotope pattern (e.g. ). 
- feature: **GUI View** - **LC-MS** view to review chromatographic peak and data quality report for a given feature/compound
- feature: CANOPUS now fully supports **NPC** classes (prediction, GUI and output)
- feature: **GUI** advanced filtering options
- feature: **GUI** scaling factor can be defined by the user (setting panel)
- feature: Additional file formats for spectrum import supported (`.msp`, massbank, `.mat`)

### SIRIUS 4

#### 4.9.3
- fix: prevent Null-pointer in CANOPUS View filter box
- improvement: allow storing project-space copy as directory (not compressed)

#### 4.9.2
- fix: error when using external path for custom dbs in the CLI ([#4](https://github.com/boecker-lab/sirius/issues/44))

#### 4.9.1
- fix: wrong log directory that might prevent SIRIUS from starting.

#### 4.9.0
- feature: improved filtering options for the compound list in the GUI
- feature: MS1 only data can now be imported using the CLI (`--allow-ms1-only`) to perform isotope pattern based molecular formula identification [#28](https://github.com/boecker-lab/sirius/issues/25). 
- change: changed tanimoto algorithm form probabilistic to rounded to dramatically reduce running time for large structure candidate lists, see [#43](https://github.com/boecker-lab/sirius/issues/43)
- change: `--db=BIO` is now the default in the CLI, see [#43](https://github.com/boecker-lab/sirius/issues/43)
- fix: wrong adduct for adduct switch annotated spectra in project-space output.

- dev feature: hidden parameter to skip project validation for testing purposes see, [#42](https://github.com/boecker-lab/sirius/issues/42)

- minor bug fixes and improvementss

#### 4.8.2
- critical-fix: custom database importer wrote cache files to working directory instead to the usual casche dir

#### 4.8.1
- fix: problems with the deletion and creation of custom databases (GUI)
- improvement: local database cached can be cleare from the settings panel (GUI)

#### 4.8.0
- feature: **COSMIC - confidence score**
- improvement: new Object storage baackend
- improvement: resizeable compound list (GUI)
- improvement: CSI:FingerID and SIRIUS/ZODIAC scores in formula selector (GUI)
- fix: new CLP dll should improve compatibility on windows (NO Solver found problem)

#### 4.7.4
- fix: intensity bug in FBMN export
 
##### 4.7.3
- fix: CONFIG already exists error during background computations (FBMN export, custom-db import) 
- fix: `.cef` file extension missing in file import dialog 

#### 4.7.2
- fix: `java.util.ConcurrentModificationException` when computing subtool separately with the CLI

#### 4.7.1
- fix: fixed missing jar/zip provider in bundled jre -> **Zipped project-spaces are now working again**
- fix: fixed ignored `custom.config` and config inheritance

#### 4.7.0
- feature: Heuristic computation for fragmentation trees to improve running times for high mass compounds (no ILP has to be computed)
- feature/improvement: Summaries
    - new `formula_identifications_adducts.tsv` summary file
    - new `canopus_summary_adducts.tsv` summary file
    - added `fromulaRank` to `compound_identifications.tsv` and `compound_identifications_adducts.tsv`
    - fix: inconsistent ranking in `compound_identifications.tsv` and `compound_identifications_adducts.tsv`
- improvement: more information about running jobs (compound information), improved log panel
- improvement: All SIRIUS distros ship with start scripts that should handle env variables properly
- improvement: rounded values for similarity matrix output
- improvement: better progress information during import
- improvement: CSI:FingerID adn CANOPUS summaries with multiple adducts (like the formula summary)
- improvement/fix: Multithreading and performance issues of integrated CLP ILP solver
- improvement/fix: GUI Job cancelling now works properly, even under high load
- improvement/fix: improved caching and update mechanisms prevents GUI freezes and  reduces GUI memory consumption when computing large data sets
- improvement/fix: much lower memory consumption when writing summaries


- fix: memory leak in jjob job manager lib - dramatically improves performance on large datasets.
- fix: correct handling of `CHARGE=-0` in mgf
- fix: corrupted project-space caused by empty adduct detection results after mgf import
- fix: empty project dir after wrong command
- fix: wrong compound name in compound edit panel
- fix: invalid valence filter error when computing positive and negative ion mode together
  fix: compound timeout not working reliable
- fix: Commercial ILP solver not detected correctly even if the correct env variable was set
- fix: CEF format import - "No proper interval given" error
- fix: multiOS build architecture now OpenMS compatible
- several minor bug fixes

- upgrade: SIRIUS ships now with JRE-15 which should fix jvm crashes during heavy multi threading on linux 

#### 4.6.1
- fix: CSI:FingerID results were not refreshed correctly after recomputing with different parameters in the GUI
- fix: Parameters were not always handled correctly when recomputing with the GUI
- fix: Index bug in fragmentation tree scoring 

#### 4.6.0
- feature: standalone subtool (`ftree-export`) to export fragmentation trees from CLI
- feature: `--noCite` command to disable bibliography print in CLI
- improvement: bibliography is not printed when showing help message or command parsing error
- fix: another problem with un-importable projects due to detected adducts

#### 4.5.3
- fix: ZODIAC crash caused by empty spectra

#### 4.5.2
- fix: invalid project-space (not importable) due to empty detected adducts
- fix: uncatched exception during adduct resolution (GitHub issue [#9](https://github.com/boecker-lab/sirius/issues/19))  
- Merry X-Mas

#### 4.5.1
- improvement: CLP native libs are now compatible with glibc 2.12+ (instead of 2.18+) 
- fix: project-space with outdated fingeprint versions (e.g. from SIRIUS 4.4) are now handled correctly and can be converted. 
- fix: database formulas could be used if candidates even if they were incompatible with the adduct 
- fix: mzml/mzxml files are now shown in input file selector

#### 4.5.0
- **feature: [CANOPUS:](https://www.biorxiv.org/content/10.1101/2020.04.17.046672v1) for negative ion mode data**
- feature: [Bayesian (individual tree)](https://doi.org/10.1093/bioinformatics/bty245) scoring is now the default for ranking structure candidates
- **update: Structure DB update due to major changes in PubChem standardization since the last one.**
  - feature: COCONUT, NORMAN and Super Natural are now officially supported
- feature: Custom-DB importer View (GUI)

- feature: mgf export for Feature Based Molecular Networking is now available in the GUI

- **breaking:**  additional columns (`ionMass`, `retentionTimeInSeconds`) have been added to project wide summary files
such as `formula_identifications.tsv`, `compound_identifications.tsv` and `compound_identifications_adducts.tsv`
- **breaking:** column names in `formula_candidates.tsv` have changed: `massError(ppm)` to `massErrorPrecursor(ppm)`, `explainedPeaks` to `numExplainedPeaks`, `medianAbsoluteMassError(ppm)` to `medianAbsoluteMassErrorFragmentPeaks(ppm)`
- **breaking:** column names describing scores now use camel case instead of underscores: `ConfidenceScore`, `SiriusScore`, `ZodiacScore`,`TreeScore`,`IsotopeScore`, `CSI:FingerIDScore`

- fix: incompatibility with recent MaOSX version caused by gatekeeper. We now provide an installable packages.
- fix: missing SCANS annotation in mgf-export subtool - creates now a valid input for FBMN
- fix: un-parsed retention times in CEF format.   
- fix: Structure DB linking (wrong ids, missing link flags, duplicate entries, etc.)
- fix: reduced memory consumption of CLI and GUI 

- JRE is now included in all version of SIRIUS
- Many more bug fixes and performance improvements 

**NOTE: SIRIUS versions will now follow semantic versioning (all upcoming releases)** 
regarding the command line interface and project-space output.  

#### 4.4.29
- fix: Error when parsing FragTree json with non numeric double values
- fix: layout of screener progress bar on Mac  

#### 4.4.28
- feature: Retention time will now be imported by SIRIUS 
  - RT is shown in the Compound list in the SIRUS GUI and the list can be sorted by RT
  - RT is part of the compound.info file in the project-space 
- feature: Loglevel can now be changed from CLI
- feature: Summaries can not be written without closing SIRIUS GUI
  - Improvement: Better progress reporting when Summary writing summaries (GUI)  
- fix: Agilent CEF files without CE can now be imported

#### 4.4.27
- feature: coin-or ilp solver (CLP) is now included. This allows parallel computation of FragTrees without the need for a commercial solver.
- improvement: Compounds without given charge are can now be imported. SIRIUS tries to guess the charge from the name (keyword: pos/neg) or falls back to positive.
- improvement: additional parameters in compute dialog
- improvement: commands of the 'show command' dialog can now be copied
- fix: error when writing/reading fragmentation trees with new Jackson parser
- fix: mgf exporter (CLI) now outputs feature name properly
- fix: deadlock during connection check without internet connection
- fix: tree rendering bug on non linux systems
- fix: crash when aborting recompute dialog
- upgrade (GUI): included JRE to `zulu11.41.23-ca-fx-jre11.0.8`

#### 4.4.26
- fix: deadlock and waiting time due to webservice connections
- fix/improvement: Adduct Settings and Adduct detection
- fix: memory leak in third party json lib -> Zodiac memory consumption has been reduced dramatically 
- fix: several minor bug fixes in the sirius libs

#### 4.4.25
- fix: removed spring boot packaging to
  - solve several class not found issues, 
  - solve github issue [#7](https://github.com/boecker-lab/sirius/issues/7)
  - errors when importing and aligning mzml files. 
  - improve startup time
- fix: cosine similarity tool ignores instances without spectra (failed before)
- fix: mgf-export tool skips invalid instances if possible (failed before)
- instance validation after lcms-align tool

#### 4.4.24
- feature: ms2 istotope scorer now available in cli and gui

#### 4.4.23
- fix: wrong missing value handling in xlogp filter (some candidates were invisible)
- improvement: less cores for computations if gui is running to have mor cpu time for GUI tasks
- improvement:  show deviation to target ion in FragTree root if precursor is missing in MS/MS 

#### 4.4.22
- fix: Classloader exceptions when using CLI from the GUI version
- fix: Wrong mass deviation for trees with adducts
- fix: misplaced labels when exporting svg/pdf fragtrees
- fix: some minor GUI bugs

#### 4.4.21
- fix: incompatibilities with existing configs from previous versions (.sirius)
- fix: CANOPUS detail view has size zero
- fix: failing CSI:FingerID computation with Zodiac re-ranking and existing Adducts
- improvement: errors that occur before GUI is started are now reported
- improvement: minor GUI improvements

#### 4.4.20
- fix: some more fixes on MacOS GUI freezes 

#### 4.4.18
- fix: GUI Deadlock on MacOS X fixed. **Mac version is now available**.
- improvement: Character separated files in project-space have now .tsv extension for better excel compatibility.
- feature: Windows headless executable respects `%JAVA_HOME%` as JRE location.
- improvement: Improved packaging and startup of the GUI version
- fixes GitHub issues: [4](https://github.com/boecker-lab/sirius/issues/4) and [6](https://github.com/boecker-lab/sirius/issues/6)

#### 4.4.16
- feature: **CSI:FingerID for negative ion mode is available**
  - NOTE: CANOPUS for negative mode data is not ready yet and will still take some time.
- fix: Too small Heapsize on Windows
- improvement: better GUI performance 

#### 4.4.15
- feature: CLI Sub-Tool to export projects to mgf.
- feature: multiple candidate number for Zodiac.
- fix: zodiac score rendering.
- fix: deadlock project-space import
- fixes: tree rendering
- improvement: import and deletion performance
- improvement: import progress now shown

#### 4.4.14
- fix: MacOS included JRE not found.
- fix: ignored parameters.
- fix: recompute does not correctly invalidate and delete previous results.
- fix: UI now correctly update when data will by deleted by the computations.

#### 4.4.(0-13)
- **New (and newly integrated) tools:**
  - [**CANOPUS:**](https://www.biorxiv.org/content/10.1101/2020.04.17.046672v1): A tool for the comprehensive annotation of compound classes from MS/MS data.
  - [**ZODIAC:**](https://www.biorxiv.org/content/10.1101/842740v1) Builds upon the SIRIUS molecular formula identifications and uses, say, its top 50 molecular formula 
    annotations as candidates for one compound. It then re-ranks molecular formula candidates using Bayesian statistics.
  - [**PASSATUTTO:**](https://www.nature.com/articles/s41467-017-01318-5) Is now part of SIRIUS and allows you to generate dataset specific decoy databases from computed fragmentation trees. 
  - Other handy standalone tools e.g. compound similarity calculation, mass decomposition, custom-db creation and project-space manipulation. 

- [**Project-Space:**](https://link.springer.com/protocol/10.1007/978-1-0716-0239-3_11) A standardized persistence layer shared by CLI and GUI that makes both fully compatible.
  - Save and reimport your projects with all previously calculated results.
  - Review your results computed with the CLI in the GUI.
  - Handy project-space summary CSV and mzTab-M files for downstream analysis.
  - Preojects can be stored and modified as directory structure or as compressed archive. 
    
- **LCMS-Runs:** SIRIUS can now handle full LCMS-Runs given in mzML/mzXML format and performs automatic feature detection. 
  - The **lcms-align** preprocessing tool performs feature detection and feature alignment for multiple LCMS-Runs based on the available the MS/MS spectra.
    
- Redesigned **Command line interface**: SIRIUS is now a toolbox containing many 
subtools that may be combined to ToolChains based on the project-space.

- **CSI:FingerID** had some massive updates, including more and larger molecular properties. 
  - **Structure DBs** New version of the CSI:FingerID PubChem copy that now uses **PubChem standardized structures**.
  - [**NORMAN**](https://www.norman-network.com/nds/common/) is now available as search DB
  - All available database filters can now be combined to arbitrary subsets for searching (even with custom databases).      

- **Interactive fragmentation tree viewer** with vector graphics export in the GUI.
- New REST service with [openAPI](https://www.csi-fingerid.uni-jena.de/v1.4.2-SNAPSHOT/v2/api-docs) specification and [Swagger-UI](https://www.csi-fingerid.uni-jena.de/v1.4.2-SNAPSHOT/swagger-ui.html).
- **Java 11** or higher is now mandatory
  - **GUI** version ships with an **integrated JRE**
- Many minor improvements and Bugfixes

#### 4.0.1
-   **Java 9 and higher are now supported**
-   **CSI:FingerID trainings structures available**
    - Trainings structures available via WebAPI.
    - Trainings structures are flagged in CSI:FingerID candidate list.
-   **SMARTS filter for candidate list (GUI)**
-   **Molecular Property filter for candidate list (GUI)**    
-   **Available prediction workers of the CSI:FingerID webservice can be listed from SIRIUS**
-   Improved connection handling and auto reconnect to Webservice
-   Improved error messaged    
-   Improved stability and load balancing of the CSI:FingerID webservice
-   Several bug fixes

#### 4.0
-   **Fragmentation tree heuristics**
-   **Negative ion mode data is now supported**
-   **Polished and more informative GUI**
    - **Sirius Overview:** Explained intensity, number of explained peaks, median mass deviation  
    - **Fragmentation trees:** Color coding of nodes by intensity/mass deviation,
      more informative Fragmentation tree nodes
    - **CSI:FingerID Overview:** Number of Pubmed publication with pubmed linking for each Candidate,
      Visualization of CSI:FingerID score.
    - **Predicted Fingerprints:** Visualisation of prediction (posterior probability), predictor quality (F1)
      and number of training examples.    
    - Several small improvements                
-   **CPLEX** ILP solver support
-   Consider a specific list of **ionizations for Sirius**
-   Consider a specific list of **adducts for CSI:FingerID**
-   Custom ionizations/adducts can be specified (CLI and GUI)
-   **Full-featured** standalone **command line version** (headless
    version)
-   Improved **parallelization** and task management
-   Improved stability of the CSI:FingerID webservice
-   Time limit for fragmentation tree computations
-   Specify fields to import name and ID from .sdf into a custom
    database (GUI).
-   CSI:FingerID results can be **filtered by Custom databases** (GUI).
-   Better filtering performance (GUI)
-   Bug fix in Database filtering view (GUI)
-   Error Reporter bug fixed (GUI)
-   Logging bugs fixed
-   Many minor bug fixes
### SIRIUS 3
#### 3.5
-   **Custom databases** can be imported by hand or via csv file. You
    can manage multiple databases within Sirius.
-   New **Bayesian Network scoring** for CSI:FingerID which takes
    dependencies between molecular properties into account.
-   **CSI:FingerID Overview** which lists results for all molecular
    formulas.
-   **Visualization of the predicted fingerprints**.
-   **ECFP fingerprints** are now also in the CSI:FingerID database and
    do no longer have to be computed on the users side.
-   Connection error detection and refresh feature. No restart required
    to apply Sirius internal proxy settings anymore.
-   **System wide proxy** settings are now supported.
-   Many minor bug fixes and small improvements of the GUI

#### 3.4
-   **element prediction** using isotope pattern
-   CSI:FingerID now predicts **more molecular properties** which
    improves structure identification
-   improved structure of the result output generated by the command
    line tool **to its final version**

#### 3.3
-   fix missing MS2 data error
-   MacOSX compatible start script
-   add proxy settings, bug reporter, feature request
-   new GUI look

#### 3.2
-   integration of CSI:FingerID and structure identification into SIRIUS
-   it is now possible to search formulas or structures in molecular
    databases
-   isotope pattern analysis is now rewritten and hopefully more stable
    than before

#### 3.1.3
-   fix bug with penalizing molecular formulas on intrinsically charged
    mode
-   fix critical bug in CSV reader

#### 3.1.0
-   Sirius User Interface
-   new output type *-O sirius*. The .sirius format can be imported into
    the User Interface.
-   Experimental support for in-source fragmentations and adducts

#### 3.0.3
-   fix crash when using GLPK solver

#### 3.0.2
-   fix bug: SIRIUS uses the old scoring system by default when *-p*
    parameter is not given
-   fix some minor bugs

#### 3.0.1
-   if MS1 data is available, SIRIUS will now always use the parent peak
    from MS1 to decompose the parent ion, instead of using the peak from
    an MS/MS spectrum
-   fix bugs in isotope pattern selection
-   SIRIUS ships now with the correct version of the GLPK binary

#### 3.0.0
-   release version
