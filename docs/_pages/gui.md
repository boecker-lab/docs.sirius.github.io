---
permalink: /gui/
title: "Graphical User Interface"
---

<span>**<span style="color: red">\[update needed!\]</span>**</span>

## Overview

Starting with version 3.1, our software ships with a Graphical User
Interface. On top of the screen you find the toolbar (1). On the left
side is the compound list (2) displaying all imported compounds. Each
*compound* lists MS and MS/MS spectra corresponding to a single measured
compound. If a compound has been processed successfully, you will see a
tick mark on the right (3); if something goes wrong during computation
you will see a cross symbol (4). The output of a computation is an
ordered list of suggested molecular formula candidates. After selecting
a compound an overview is displayed. It shows a list of all molecular
formula candidates (5), sorted by score, the corresponding spectrum (6)
and the fragmentation tree of the selected candidate molecular
formula (7). Explained peaks are highlighted in the spectrum. Nodes in
the fragmentation tree are colored according to their score. In the
upper right corner are settings and bug report dialogs (8).


<span>**<span style="color: red">\[TODO: filter options etc\]</span>**</span>

## Data import

SIRIUS offers two modes for data import: *Single import* and *Batch
import*. The single import is triggered when clicking on the *Import*
button in the toolbar. It allows you to import **one** compound. (We
will use the term “compound” as a description of MS and MS/MS spectra
belonging to a single compound.) The single import mode is recommended
if your data consists of several CSV (comma separated values) files,
such as the data from the CASMI challenges. First press on *Import* to
start the import dialog.

For each spectrum you have to select the MS level (either MS1 or MS/MS).
If you have MSn spectra you can just import them as MS/MS spectra. You
can select a name for the compound as well as an ionization mode. The
collision energy is an optional attribute as it does not affect the
computation.

You can import and files using the *Batch Import*. In this mode SIRIUS
will read all attributes (MS level, ionization, parent mass) directly
from the file. You can, however, change these attributes afterward by
selecting the imported compound and clicking on the *Edit* button.

See section *Supported Input Formats* for a description of the file
formats and .

### Drag and drop

SIRIUS supports Drag and Drop: Just move your input files into the
application window. This is usually the easiest way to import data into
SIRIUS. Supported file formats for Drag and Drop are , , and .

## Identifying molecular formulas with SIRIUS

As for importing data SIRIUS offers two computation modes: *Single
Computation* and *Batch Computation*. The Single Computation allows you
to setup different parameters for each compound. You can trigger it by
right-clicking on a compound and choosing *Compute* in the context menu.
The Batch Computation will compute all compounds in the workspace.
Besides, you can select multiple compounds and choose *Compute* to only
compute a subset of your imported compounds.

### Parent mass

The exact m/z of the parent peak. If MS1 data is present, the m/z of the
monoisotopic peak is presented as default. Otherwise, an autocompletion
offers a list of high intensive peaks from the MS/MS spectra.

### Elements besides CHNOPS

SIRIUS will use the elements carbon (C), hydrogen (H), nitrogen (N),
oxygen (O), phosphorus (P) and sulfur (S) by default. Additional
elements can be selected within the *Select elements* dialog. Adding
additional elements will increase running time. Using (too many)
elements that do not occur in the correct molecular formula of the
compound might worsen the results.

The automated detection of a set of “uncommon elements” is available if
the isotope pattern is provided. These elements are sulfur (S), chlorine
(Cl), bromine (Br), boron (B), and selenium (Se). Using *Auto detect*
will clear your element selection and will set new values based on the
detection. Autodetection is usually quite sensitive and rather
overpredicts the actual quantity of an element.

### Other

The ionization mode determines the polarity of the measurement (positive
or negative) as well as the adduct (e.g. protonation or sodium adduct).
If you choose *Unknown Positive* or *Unknown Negative* SIRIUS will not
care about the adduct, but report the molecular formula of the **ion**
in the candidate list. Otherwise, SIRIUS will subtract the adducts
formula from the ions formula and report neutral molecular formulas in
the candidate list as well as in the fragmentation trees.

Choose either *Q-TOF*, *Orbitrap* or *FT-ICR* in the instrument field.
The chosen instrument affects only very few parameters of the method
(mainly the allowed mass deviation). If your instrument is not one of
these three then just select the Q-TOF instrument.

You can change the maximal allowed mass deviation in the *ppm* field.
SIRIUS will only consider molecular formulas which mass deviations below
the chosen ppm; for masses below 200 Da, the allowed mass deviation is
$(200 \cdot \frac{ppm_{max}}{10^6})$.


Finally, you can select the number of molecular formula candidates that
should be reported in the output, and what molecular formulas are
considered as candidates: If you select option *all possible molecular
formulas* then SIRIUS will enumerate over all molecular formulas that
match the ion mass, filtering out only molecular formulas with negative
ring double bond equivalent. If you choose *all PubChem formulas* then
SIRIUS will select all molecular formulas from PubChem. Option *organic
PubChem formulas* ignores molecular formulas containing elements
untypical for organic compounds such as Si or Mg; molecular formulas
pass this filter if they are composed solely of . When choosing
*formulas from biomolecule databases*, SIRIUS will use all formulas
contained in databases with biological compounds or compounds that could
be expected in biological experiments (e.g. KEGG, BioCyc, HMDB, but also
MaConDa).

  - We never search in these databases directly, but rather in our local
    database copies. Although we regularly update our database, it may
    happen that some new compound in, say, ChEBI is not already
    contained in our local copy.

  - When choosing a *molecular fromulas from a database* option, SIRIUS
    will ignore your element restrictions and instead allow all
    elements.

  - We do not recommend to restrict molecular formula searching to
    biomolecule databases, but doing so significantly speeds up
    computations, as SIRIUS has to consider significantly less molecular
    formulas and download significantly smaller candidate structure
    lists.

## Identifying molecular structure with CSI:FingerID

After computing the fragmentation trees you can search these in a
structure database. Again we provide a *single mode* and a *batch mode*.
The single mode is available by clicking on the molecular formula of
interest, then switching to the *CSI:FingerID* tab and pressing on the
*Search online with CSI:FingerID* button. The batch mode can be
triggered by pressing on the *CSI:FingerID* in the toolbar.

When starting the CSI:FingerID search you are again asked to choose
between PubChem or biomolecule databases. This is mainly a performance
issue because you can filter your result lists afterwards by any
database you want to. Our biomolecule database is several magnitudes
smaller than PubChem and downloading and searching structure lists from
biomolecule databases is significantly faster. However, when searching
in biomolecule databases you might never see if there are structures
with possibly much better score from PubChem. Therefore, we recommend to
search in PubChem and filter the result list if you expect the result to
be contained in biomolecule databases.

## Visualization of the results

Each compound has an *Overview* panel to display the most important
information. The candidate list contains the best candidate molecular
formulas ordered by score. <span>**<span style="color: red">\[all these
new ionizations\]</span>**</span> Molecular formulas are always written
in neutral form, except for compounds with unknown ionization mode. For
the selected molecular formula candidates the *Spectra view* visualizes
which peak is assigned to a fragment. The corresponding fragmentation
tree is visualized in the *Tree view*. Both views can be displayed in a
separate panel to have a more detailed look. The *CSI:FingerID* panel
displays candidates from structure prediction.

### Overview tab

The *Overview* tab displays the candidate list, spectrum and
fragmentation tree of the selected candidate. Candidates are ordered by
total score, but can be sorted by any other column. Moreover, the list
can be filtered using the corresponding text field. A green row
highlights the molecular formula of the best candidate structure found
by CSI:FingerID.

The length of the bars for the different score columns (isotope pattern,
fragmentation pattern, total) as well as the displayed numbers for
columns *Isotope Score* and *Tree Score*, correspond to *logarithms* of
maximum likelihoods (probability that this hypothesis, i.e. molecular
formula, will generate the observed data). In contrast, the number in
the *Score* column is the posterior probability of the hypothesis
(molecular formula), and these probablities sum to one. A higher
posterior probability of the top hit may indicate that this molecular
formula has a higher chance of being correct; but we stress that **a
posterior probability of 90%, must not be misunderstood as a 90%
probability that this molecular formula identification is correct! **
The displayed probabilities are neither q-values nor Posterior Error
Probabilities.

### Tree view tab

The *Tree view* tab displays the estimated fragmentation tree. Each node
in this tree assigns a molecular formula to a peak in the (merged) MS/MS
spectrum. Each edge is a hypothetical fragmentation reaction. The user
has the choice between different node styles and color schemes.

The displayed fragmentation tree can be exported as JPEG, GIF, and PNG.
Alternatively, the Dot file format contains a text description of the
tree. It can be used to render the tree externally. The command-line
tool Graphviz can transform dot files into image formats (PDF, SVG, PNG
etc). The JSON format yields a machine-readable representation of the
tree.

### Spectrum view tab

In the *Spectrum view* tab, all peaks that are annotated by the
fragmentation tree are colored in orange. Peaks that are annotated as
noise are colored black. Hovering with the mouse over a peak shows its
detailed annotation.

### CSI:FingerID view

This tab shows you the candidate structures for the selected molecular
formula ordered by the CSI:FingerID search score. If you want to filter
the candidate list by a certain database (e.g. only compounds from KEGG
and BioCyc) you can press the filter button. A menu will open displaying
all available databases. Only candidates will be displayed that are
enabled in this filter menu. Note that PubChem is enabled by default
and, therefore, always the complete list is shown. If you want to see
only compounds from KEGG and BioCyc you have to disable PubChem and
enable KEGG and BioCyc.

Another way of filtering is the XLogP slider. If you have information
about retention times and expected logP values of your measured compound
you can use this slider to filter the candidate list by certain XLogP
values. The slider allows you to define min and max values. XLogP is
calculated using the Chemical Development Kit CDK .

The blue and red squares are some visualization of the CSI:FingerID
predictions and scoring. All blue squares represent molecular structures
that are found in the candidate structure and are predicted by
CSI:FingerID to be present in the measured compound. The more intense
the color of the square the higher is the predicted probability for the
presence of this substructure. The larger the square the more reliable
is the predictor. The red squares, however, represent structures that
are predicted to be absent but are, nevertheless, found in the candidate
structure. Again, as more intense the square as higher the predicted
probability that this structure should be absent. Therefore, a lot of
large intense blue squares and as few as possible large intense red
squares are a good indication for a correct prediction.

When hovering with the mouse over these squares the corresponding
description of the molecular structure (usually a SMART expression) is
displayed. When clicking on one of these squares, the corresponding
atoms in the molecule that belong to this substructure are highlighted.
If the substructure matches several times in the molecule, it is once
highlighted in dark blue while all other matches are highlighted in a
translucent blue.

Even if the correct structure is not found by CSI:FingerID — in
particular if the correct structure is not contained in any database —
you can get information about the structure by looking at the predicted
structures: When clicking on the large light green squares you see which
molecular substructures are expected in the measured compound.

You can open a context menu by right click on the compound. It offers
you to open the compound in PubChem or copy the InChI or InChI key in
your clipboard.

If the compound is contained in any biomolecule database, a blue label
with the name of this database is displayed below the compound. You can
click on most of these labels to open the database entry in your browser
window.

You can export a single candidate list by clicking on the *export list*
button.

## Settings

  - *General settings*
    
      - *Allowed solvers:* chose the ILP solver for SIRIUS to use for
        fragmentation tree computation. GLPK is free, Gurobi is
        commericial but offers free academic license.
    
      - *Database cache:* location of cache directory. CSI:FingerID
        download candidate structures from our server and caches them
        for faster retrieval.

  - *Proxy settings*
    
      - Sirius support three different kinds of proxy configuration
        SYSTEM, SIRIUS and NONE. If SYSTEM (default) is select Sirius
        uses the system wide Java proxy settings. If SIRIUS is selected
        it uses the configuration you have specified int the Settings
        -\> Proxy panel. If NONE is selected Sirius ignores all proxy
        settings.
    
      - Edit the information in the Settings -\> Proxy panel if you want
        to address CSI:FingerID via a proxy server. Your specified
        configuration will be tested if you hit the save button (see
        Figure below).

  - *Error report settings*
    
      - Add an email address which will be sent with a bug report. This
        makes it possible for us to contact you, in case we need
        additional information to solve your problem.
    
      - Decide whether specific hardware and operating system
        information is send with your bug report.

## Bug Reports

We do our best so that you will not be confronted with errors while
using SIRIUS. But we cannot test every possible scenario. We encourage
you to send us a bug report in case you encounter an error. It is very
helpful if you specify your email address. Often, errors are very
specific and can only be reproduced and understood with help of the
input file and knowledge of the used parameter settings. Therefore, we
might reach out to you. With your help, we will continue to improve
SIRIUS. You can also contact us at
[sirius@uni-jena.de](sirius@uni-jena.de).

