---
permalink: /gui/
title: "Graphical User Interface"
---

## Overview

SIRIUS 6 ships with a Graphical User Interface. 

{% capture fig_img %}
![Foo]({{ "/assets/images/gui_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS main application window.</figcaption>
</figure>

On top of the screen you find the toolbar (1-5). 
The left most button group (1) is for creating, opening and saving project-spaces.
The second one (2) is for [importing](#data-import) either a single feature or data containing multiple features into
the project-space. The third button group (3) is for exporting data, e.g. for [GNPS FBMN](https://doi.org/10.1038/s41592-020-0933-6) 
or writing [project-space summaries]({{ "/io/#sirius-project-space/" | relative_url }}). 
The fourth button group (4) is for computations, containing "compute button", "job view" and "custom database importer".  
The right most button group (5) contains "log", "settings", "webservice info" and "account info" dialogs. 
"Help" links to this online documentation. "About" gives information on software licence and related publications.

On the left side is the feature list (6) displaying all imported (aligned) features. 
Each *feature* lists MS and MS/MS spectra corresponding to a measured aligned
feature. For each feature, adduct type, precursor mass, retention time and confidence score (if computed) are shown. 
On the right side is the active result view (8). You can choose between different result
views with the tab selector (7).

On the bottom (9), you find your license information for the webservice-based structure elucidation tools, 
the number of computed features and feature limits.  

## Data import

You can import `.ms`, `.mgf`, Agilent's `.cef`, `.mzml` and `.mzxml` files using the *"Import"* button or Drag'n'Drop. 
SIRIUS will read all attributes (MS level, ionization, precursor mass) directly
from the file. You can, however, change these attributes afterward by
selecting the imported feature and clicking on the *Edit* button.
When importing multiple `.mzml` (or `.mzxml`) at once, SIRIUS will ask you if it should align them. 

See [Input Formats]({{ "/io/#input/" | relative_url }}) for descriptions of file
formats and further details.

## Sorting features, filtering features and changing displayed confidence score mode

The feature list can be sorted by right-clicking the feature list to bring up the dialogue below


<img src="{{ "/assets/images/featureList_sort.png" | relative_url }}" height="300" width="300">


Aligned features can be sorted by retention time (RT), mass, name, ID or confidence score (if present). At the bottom of
the dialogue, users also have the option of changing which confidence mode (approximate or exact) should be displayed and 
used for sorting. See [confidence modes]({{ "/advanced-background-information/#confidence-score-modes" | relative_url }}) for more information.


Furthermore, the feature list can be filtered by clicking the appropriate button (see red mark above). This brings up the filter dialogue:

{% capture fig_img %}
![Foo]({{ "/assets/images/filter_empty.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Feature list filtering dialogue.</figcaption>
</figure>

Aligned features can be filtered by mass range, retention time range, confidence score range as well as peak shape quality (only available for
mzMl and mzXML input), minimum number of isotope peaks in the MS1 and detected lipid classes.
Additionally, users can filter for specific element constraints in either the neutral molecular formula or precursor formula,
as well as for specific detected adducts. If structure database results are present, one can filter for hits in specific structure databases, where
"candidates to check" controls how many of the top n candidates should be considered.


## Computing results
As for importing data SIRIUS offers two computation modes: *Single
Computation* and *Batch Computation*. The Single Computation allows you
to setup different parameters for each feature. You can trigger it by
right-clicking on a **single** feature and choosing *Compute* in the context menu or by double clicking a feature.

Right-clicking **multiple** selected features and choosing *Compute* will trigger batch computation for
the selected features.
Clicking the *"Compute All"* button (toolbar) will compute all features in the project-space.

Both dialogs are very similar. In *Single Computations* element prediction can be performed by clicking the respective button. 
In *Batch Computations* check boxes indicate the elements that are automatically predicted for each feature. Also, you
can select if results for features that already have been analyzed should be recomputed and overridden.

The *Show Command* button shows the respective CLI command for the specified parameters. Hence, you can copy/paste this command
and run analysis using the CLI.

In the following, the *Batch Computation* dialogue is shown.

### Compute panel (Basic and advanced)

Starting from SIRIUS 6, the compute dialogue offers two difference modes: "basic" and "advanced". The basic mode offers improved clarity and
contains only those settings which are integral for any kind of analysis. In contrast, the advanced mode only settings only need to be
considered for specific use cases and/or for setting limits to computation times.

{% capture fig_img %}
![Foo]({{ "/assets/images/compute_basic_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Batch compute dialog.</figcaption>
</figure>

The compute panel is split into five subtools: SIRIUS molecular formula annotation (1), ZODIAC (2), CSI:FingerID fingerprint prediction with CANOPUS (3), CSI:FingerID structure
database search (4) and MSNovelist (5). Starting from SIRIUS 6, CANOPUS (3) is automatically performed whenever a fingerprint is predicted and does not need to be enabled 
separately anymore. Subtools can be selected individually or combined, please note that the selection together with potentially existing results needs to constitute a valid SIRIUS workflow.
As an example, one cannot perform structure database search without predicting fingerprints first. Please see [Sub tools and workflows]({{ "/advanced-background-information/#sirius-workflows" | relative_url }}) for a more
detailed explanation on SIRIUS workflows.
(6) If the "Recompute already computed tasks" checkbox is ticked, all previously existing results for selected features in the current project space will be invalidated and overwritten as necessary
for executing the currently selected workflow. Additional parameters for specific subtools can be brought up via the appropriate button (7). 
To easily transition the current workflow selections to a CLI, one can use the "Show Command" (8) button on the bottom right.

### Spectral library matching (background)

If imported spectral libraries are present, SIRIUS will automatically perform spectral matching against those libraries. Currently, this always happens in the background and no parameters
need to be set. See [Spectral library matching via custom databases]({{ "/advanced-background-information/#Spectral-library-matching-via-custom-databases" | relative_url }}) for more information


### Identifying molecular formulas with SIRIUS (1)

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_basic_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Molecular formula annotation compute dialogue.</figcaption>
</figure>

#### General settings (A)

Choose either *Q-TOF*, *Orbitrap* or *FT-ICR* in the instrument field.
The chosen instrument affects only very few parameters of the method
(mainly the allowed mass deviation). If your instrument is not one of
these three then just select the Q-TOF instrument.

You can change the maximal allowed mass deviation in the *ppm* field.
SIRIUS will only consider molecular formulas which mass deviations below
the chosen ppm; for masses below 200 Da, the allowed mass deviation is
$(200 \cdot \frac{ppm_{max}}{10^6})$.

If SIRIUS predicts that the query spectrum might be a lipid, the molecular formula
according to that prediction can be added and enforced (default).

#### Fallback Adducts (B)

If no adducts have been detected in previous steps (either during SIRIUS importing or upstream external annotation), fallback adducts can be 
set. SIRIUS will consider adducts in this list and additionally enforce them if the "enforce" option is chosen.

#### Molecular Formula Generation (C)

The [molecular formula annotation strategy]({{ "/advanced-background-information/#Molecular-formula-annotation-strategies" | relative_url }}) to be employed.
Choosing a suitable strategy here is imperative for a successful SIRIUS analysis and will impact most subsequent steps.

**De novo + bottom up search (recommended)**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_combined_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for bottom up + de novo search.</figcaption>
</figure>

(1) The m/z threshold below of which de novo molecular formula annotation will be performed in addition to bottom up search.
(2) Element filter settings. An element filter can either be applied to only the de novo annotations, or to the bottom up search as well.
"Allowed elements" denotes elements that are part of the element set, upper and lower limits are shown if present. "Autodetect" denotes those elements
for which SIRIUS will detect presence/absence and quantity from the input data (requires MS1 spectra to be present). The element set can
be changed via the (a) button. Before majorly changing the element set, please be aware of the potential impact on running time and quality
(see [here]({{ "/advanced-background-information/#De-novo-annotation" | relative_url }}))

**De novo only**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_denovo_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for de novo only annotation.</figcaption>
</figure>

(1) Element filter settings. An element filter has to be applied for de novo molecular formula annotation.
"Allowed elements" denotes elements that are part of the element set, upper and lower limits are shown if present. "Autodetect" denotes those elements
for which SIRIUS will detect presence/absence and quantity from the input data (requires MS1 spectra to be present). The element set can
be changed via the (a) button. Before majorly changing the element set, please be aware of the potential impact on running time and quality
(see [here]({{ "/advanced-background-information/#De-novo-annotation" | relative_url }}))

**Database search**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_dbsearch_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for formula database search.</figcaption>
</figure>

(1) Selection of databases that should be used for molecular formula annotation. Per default, those databases that constitute the "bio" database are selected.
See [Molecular structures]({{ "/advanced-background-information/#Molecular-structures" | relative_url }}) for the list of databases that come with SIRIUS.
The default selection can be restored by pressing the "bio" button.
(2)  Element filter settings. Applying an element filter is not mandatory for formula database search, but can be optionally applied to filter molecular formula candidates.
"Allowed elements" denotes elements that are part of the element set, upper and lower limits are shown if present. "Autodetect" denotes those elements
for which SIRIUS will detect presence/absence and quantity from the input data (requires MS1 spectra to be present). The element set can
be changed via the (a) button.

**Bottom up search**

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_bottomup_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Settings for bottom up search only.</figcaption>
</figure>

(1) Element filter settings. Applying an element filter is not mandatory for bottom up search, but can be optionally applied to filter molecular formula candidates.
"Allowed elements" denotes elements that are part of the element set, upper and lower limits are shown if present. "Autodetect" denotes those elements
for which SIRIUS will detect presence/absence and quantity from the input data (requires MS1 spectra to be present). The element set can
be changed via the (a) button. Before majorly changing the element set, please be aware of the potential impact on running time and quality
(see [here]({{ "/advanced-background-information/#De-novo-annotation" | relative_url }}))


#### Advanced mode parameters

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_advanced_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Advanced parameters for molecular formula annotation.</figcaption>
</figure>

(1) By default molecular formula candidates which theoretical isotope pattern does deviate too much from the measured isotope pattern are discarded. This setting can be turned off
if you suspect bad quality isotope patterns in the input data.
(2) If isotopic peaks are present in the input MS2 spectrum, they can either be used to **score** or be **ignored**.
(3) Select the number of molecular formula candidates that should be saved
(4) Select the minimum number of molecular formula candidates stored for each ionization, even if it is not part of the top n candidates.
(5) Set a time limit for computing the fragmentation tree for a singular molecular formula candidate (in seconds). Set to 0 to disable the limit.
(6) Set a total time limit for computing the fragmentation trees of all molecular formula candidates of a feature (in seconds). Set to 0 to disable the limit.
(7)+(8) For higher mass compounds, SIRIUS can compute fragmentation trees heuristically instead of exact. The heuristic can be used to pre-rank molecular formula candidates
and then only compute exact trees for the top candidates. (7) controls the m/z value above which this approach will be used. For even higher masses, it might be necessary
to forego exact solutions altogether and use heuristic trees only. (8) controls the m/z value above which trees will exclusively be computed using the heuristic. 

### Improve molecular formula ranking with the ZODIAC tool (2)

ZODIAC performs de novo molecular formula annotation on complete biological datasets (high-resolution, high mass
accuracy LC-MS/MS runs). ZODIAC takes fragmentation trees as input and reranks the molecular formula candidates by
taking similarities of features in the dataset into account.

To run Zodiac, select SIRIUS and ZODIAC in the batch compute panel. Increase the number of reported candidates for SIRIUS, to
increase the chance that the correct molecular formula candidate is contained in the result list. Click “compute”.

Click [here](https://bio.informatik.uni-jena.de/software/zodiac/) to visit the Zodiac release page.

#### Advanced mode parameters

These parameters are very advanced and require a deep understanding on ZODIAC and the underlying Gibbs sampler.

{% capture fig_img %}
![Foo]({{ "/assets/images/zodiac_advanced_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Advanced parameters for ZODIAC molecular formula annotation.</figcaption>
</figure>

(1) Maximum number of candidate molecular formulas considered for features with m/z lower than 300.
(2) Maximum number of candidate molecular formulas considered for features with m/z higher than 800.
(3) Enable/Disable the 2-step approach (running higher quality features first, lower quality features second).
(4) Threshold for the ratio of edges of the complete network to be ignored.
(5) Minimum number of connections per candidate.


### Predicting the molecular fingerprint with CSI:FingerID and predicting compound classes with CANOPUS  (3)

After computing the fragmentation trees you can predict [molecular fingerprints]({{ "/advanced-background-information/#molecular-fingerprints" | relative_url }}) 
and [CANOPUS compound classes]({{ "/advanced-background-information/#Compound-classes" | relative_url }})
These can be used to either search in a structure databases or predict novel structures with [MSNovelist]({{ "/advanced-background-information/#MSNovelist" | relative_url }}).
If **"score threshold"** is activated, fingerprints are only predicted for the top scoring fragmentation trees (molecular formulas). This is recommended and should only be changed if you are interested
in the fingerprint of a molecular formula that has a lower score.

CANOPUS predicts [ClassyFire](http://classyfire.wishartlab.com/) compound classes from the molecular fingerprint. Class
prediction is done without using any structure database. Thus, classes are predicted for all features for which the
fragmentation tree contains at least three fragments, including features that have no structure candidate in the
database. There are no parameters to set. Similar to molecular fingerprints, compound classes are predicted for each
molecular formula separately.

In the [ClassyFire](http://classyfire.wishartlab.com/) ontology, every compound belongs to multiple compound classes. A
compound class describes a structural pattern. For example, a *dipeptide* is also an *amino acid* (because it **
contains** an amino acid substructure), as well as a *carboxylic acid* (for the same reason). A glycosylated amino acid
might belong to both compound classes: *amino acids* and *hexoses*. Different from how compound classes are often
described in chemistry textbooks, ClassyFire compound classes do **not** describe the biosynthetic origin. For example,
a *phytosteroid might* be classified as *bile acids* in Classyfire, because both class of compounds share the same
backbone, although they are involved in different biochemical pathways.

Click [here](https://bio.informatik.uni-jena.de/software/canopus/) to visit the CANOPUS release page.

### Identifying the molecular structure with the CSI:FingerID tool  (4)

Predicted fingerprints can be matched against database structures for structure elucidation. SIRIUS ships with a multitude of databases
(see [Molecular structures]({{ "/advanced-background-information/#Molecular-structures" | relative_url }})). 
Additionally, structures can be added as a "custom database" (see [Import of custom structure and spectra databases]({{ "/gui/#Import-of-custom-structure-and-spectra-databases" | relative_url }})) and then searched matched against
(in addition to existing databases).

{% capture fig_img %}
![Foo]({{ "/assets/images/structure_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Parameters for structure database search.</figcaption>
</figure>

(1) Expansive search fallback settings, please see [Expansive search]({{ "/advanced-background-information/#Expansive-search-(structure-database-search-with-fallback)" | relative_url }}) for a more detailed explanation. 
You can choose to use PubChem as a fallback database in case it contains a hit of higher confidence than
the selected databases (2).
**Confidence mode** controls if approximate or exact confidence mode should be used for the assessment of if a
hit in PubChem is more confident than a hit in the selected databases.

(2) Structure databases to search in. Per default the structure databases making up the "bio" database are selected. If formula database search
was selected earlier in the workflow, selected databases will reflect that selection. You can return to default by clicking "bio". If any custom
databases exist, they can be selected here as well.


### Generating de novo structure candidates with MSNovelist (5)

Sometimes it might be necessary to go beyond the limits of structure database search. Together with the predicted fingerprint, compound classes and custom databases,
SIRIUS 6 offers de novo generation of candidate structures through MSNovelist. See [MSNovelist]({{ "/advanced-background-information/#MSNovelist" | relative_url }}) for more details on the underlying science.

Please note that the likelihood of any de novo generation method performing well for actual novel compounds is very low. Results should be seen as suggestions or starting points
for semi-manual analysis of compounds that cannot be elucidated otherwise. 

**MSNovelist will slow your SIRIUS work flow down significantly, use with caution.**

#### Import of custom structure and spectra databases

Custom structure databases can be added via the "Databases" interface (4) located at the top center of the GUI ribbon.
Starting with version 6.0, SIRIUS additionally supports the import of spectral libraries. Supported import formats for spectral
data are .ms, .mgf, .msp, .mat, .txt (MassBank), .mb, .json (GNPS, MoNA). Spectra need to be annotated with a structure and be centroided.
Imported custom structures can be used in structure database search, imported spectra will be used for spectral library matching, see [Spectral library matching via custom databases]({{ "/advanced-background-information/#Spectral-library-matching-via-custom-databases" | relative_url }}).

{% capture fig_img %}
![Foo]({{ "/assets/images/customDbs_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Custom database dialogue.</figcaption>
</figure>

Custom databases are stored as files with the ".siriusdb" extension. If such a database already exists on the local machine, it can be added to SIRIUS with the
"add existing database" button. Imported databases can be deleted or modified using the respective buttons on the bottom right. To create a new custom database,
use the "create custom database" button on the bottom right.

{% capture fig_img %}
![Foo]({{ "/assets/images/customDbs_import_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Custom database import dialogue.</figcaption>
</figure>

(1) Name of the database that will be shown in the structure database search dialogue with a maximum length of 15 characters.
(2) Desired file name of the database, should end with ".siriusdb"
(3) The database location needs to be any valid, writeable path on your local machine.
(4) The buffer size controls how many structures/spectra should be kept in memory. Can be increased when importing large files on a faster machine.
(5) Input space to drag&drop files or directories containing structure/spectra files.

See [here](https://boecker-lab.github.io/docs.sirius.github.io/cli-standalone/#custom-database-tool) for more details regarding custom database import and supported file formats.

**Please note that you have to be logged in to your SIRIUS account to import custom databases**

#### COSMIC - confidence values for CSI:FingerID searches
Calculating COSMIC confidence scores is parameter free and will be executed automatically every time a CSI:FingerID 
search is performed. COSMIC scores for a feature are shown in the feature list on the left. From SIRIUS 6 onwards, confidence scores
will be computed in "exact" and "approximate" mode. See [COSMIC]({{ "/advanced-background-information/#cosmic---confidence-for-small-molecule-identifications" for details. | relative_url }})


Click [here](https://bio.informatik.uni-jena.de/software/cosmic/) to visit the COSMIC release page


## Visualization of the results
The feature list not only shows information about the input and compute state, it further shows the COSMIC
confidence score for the top CSI:FingerID hit.
TODO: update below
For each feature different tabs can be shown in the result panel.
The *"LC-MS"* tab displays the chromatogram of a feature for it monoisotopic- and further isotope peaks, as well as possibly detected adducts.
It includes a basic quality assessment of the spectrum. This tab is only populated for mzML and mzXML inputs.
The *"Formulas"* panel displays the most important
information of the molecular formula identification. The candidate list contains the best candidate molecular
formulas ordered by score. Molecular formulas are always written
in neutral form. For the selected molecular formula candidates the *Spectra view* visualizes
which peak is assigned to a fragment. The corresponding fragmentation
tree is visualized in the *Tree view*. Both views can be displayed in a
separate panel to have a more detailed look. The *"Predicted fingerprints"* panel shows information about
the molecular properties of the molecular fingerprint predicted by CSI:FingerID. The *"Structures"* panel displays 
results from the CSI:FingerID structure search, while the *"Substructure Annotation"* panel shows possible substructures connected to
the peaks of the MS/MS spectrumfor each candidate. The *"CANOPUS"* panel
shows the [Classyfire](http://classyfire.wishartlab.com/) classes predicted by CANOPUS. 

### LC-MS tab

The LC-MS tab is only visible when LC-MS data (mzML or mzXML) was used for import. When the data came from MGF, ms or similar file formats, the LC-MS information is not available. This is also the case when LC-MS data was processed with OpenMS or MZMine and the results were imported to SIRIUS.

{% capture fig_img %}
![Foo]({{ "/assets/images/lcms.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Overview tab.</figcaption>
</figure>

The LC-MS tab displays the ion chromatogram of a feature (in blue), including its isotope peaks, possible in-source fragments (in brown), as well as detected adducts (in green) for each input file in which the feature was detected. Retention times are always given in minutes. The *extended ion chromatogram* (gray, dashed) is the mass trace that is not part of the detected peak (e.g., a second ion with same mass or just background noise with same mass). In case MS/MS data of the feature was extracted from the selected LC-MS input file, a black arrow marks the retention time at which the MS/MS was shot. A gray dashed line marks the *noise level*; its exact computation may varies from version to version, but it is related to the median intensity of all peaks in the MS scan. Two gray vertical dashed lines mark the median and weighted average retention time of the feature across all input LC-MS data files.

On the right, there is a basic quality assessment panel. It can be used to preemptively get an idea on overall quality of the MS and MS/MS of the feature.


### Formulas tab 

#### Formula annotation overview (1)

The *"Formulas"* tab displays the molecular formula candidate list (1), spectrum (2) and
fragmentation tree (3) of the selected feature. Candidates are ordered by
total score, but can be sorted by any other column. A green row
highlights the molecular formula of the best candidate structure found
by CSI:FingerID.

{% capture fig_img %}
![Foo]({{ "/assets/images/formula_result_marked.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Formulas tab.</figcaption>
</figure>

The length of the bars for the different score columns (Sirius (isotope + tree) and zodiac) as well as the displayed numbers for
columns *Isotope Score* and *Tree Score*, correspond to *logarithms* of
maximum likelihoods (probability that this hypothesis, i.e. molecular
formula, will generate the observed data). In contrast, the number in
the *Sirius Score* column is the posterior probability of the hypothesis
(molecular formula), and these probabilities sum to one. A higher
posterior probability of the top hit may indicate that this molecular
formula has a higher chance of being correct; but we stress that **a
posterior probability of 90%, must not be misunderstood as a 90%
probability that this molecular formula identification is correct! **
The displayed probabilities are neither q-values nor Posterior Error
Probabilities.

#### Spectrum overview (2)

In the *Spectrum view* part of the formulas tab, one can switch between MS1,
MS1 isotope pattern mirror plot and MS2 spectra. Hold right mouse button to area-select,
scroll while hovering an axis to zoom. In the MS2 view, all peaks that are annotated by the
fragmentation tree are colored in green. Peaks that are annotated as
noise are colored black. Hovering with the mouse over a peak shows its
detailed annotation.
Clicking on a green peak will highlight the corresponding node in the fragmentation tree.
Spectra views can be exported using the top right export button.

#### Fragmentation tree overview (3)


The *Tree view* displays the computed fragmentation tree. Each node
in this tree assigns a molecular formula to a peak in the (merged) MS/MS
spectrum. Each edge is a hypothetical fragmentation reaction. The user
has the choice between different node styles and color schemes. Please see 
[Fragmentation trees]({{ "/advanced-background-information/#Fragmentation trees" | relative_url }}) for a detailed explanation on fragmentation trees.


The displayed fragmentation tree can be exported as `svg` or `pdf` vector graphics.
Alternatively, the `dot` file format contains a text description of the
tree. It can be used to render the tree externally. The command-line
tool Graphviz can transform dot files into image formats (`pdf`, `svg`, `png`
etc). The `json` format yields a machine-readable representation of the
tree. See the [`ftree-export`]({{ "/cli-standalone/#fragmentation-tree-export-tool" | relative_url }}) cli tool for how to export fragmentation trees from 
the command line.



### Predicted fingerprints tab
Even if the correct structure is not found by CSI:FingerID — in
particular if the correct structure is not contained in any database —
you can get information about the structure by looking at the predicted
fingerprint. The *"Fingerprint"* tab shows a list of all molecular properties 
the predicted fingerprint consists of. 
For each molecular property its definition and posterior probability is shown
as well as some information about the predictor for this property.
When selecting a molecular property, examples for this property are shown below the list.

{% capture fig_img %}
![Foo]({{ "/assets/images/fingerprint.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Fingerprint view.</figcaption>
</figure>

### Compound classes (CANOPUS) tab

Compound class predictions are visualized as table similar to molecular fingerprints: Each row in the table describes
one class. The *posterior probability* is the probability that the measured spectrum (given the chosen molecular formula)
belongs to this compound class. The other columns contain all related information from the
[ClassyFire](http://classyfire.wishartlab.com/) ontology.

Above the table are two lists: **main classes** and **alternative classes**. The main class of a measurement is the
*most specific* compound class from all compound classes with posterior probability above 50%. The **main classes** list
contains the main class, as well as its ancestors in the Classyfire ontology. The **alternative classes** list contains
all over classes with posterior probability above 50%.

{% capture fig_img %}
![Foo]({{ "/assets/images/canopus.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>CANOPUS view.</figcaption>
</figure>

* (1) The most informative class (light green), and its ancestor classes (light blue).
* (2) Alternative classes. In the ClassyFire chemontology, every compound is assigned to multiple classes. In this example, the compound kaempferol is a flavonoid, but also a benzenoid.
* (3) The table lists all ClassyFire classes, with description parent class and so on. The colored bar denotes the predicted probability for this class. Only classes with probability above 0.5 are listed in (1) and (2).

Starting from SIRIUS 5, this tab also contains the predicted Natural Product classes.

### Structures tab
{% capture fig_img %}
![Foo]({{ "/assets/images/structures_exact.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>structure annotation tab tab.</figcaption>
</figure>

This tab shows you the candidate structures for the selected molecular
formula ordered by the CSI:FingerID search score. The highest scoring candidate is highlighted
in green. If you have approximate confidence mode enabled, all candidates that are within MCES distance 2
will be highlighted (see [Expansive search]({{ "/advanced-background-information/#Expansive-search-(structure-database-search-with-fallback)" | relative_url }})).
If you want to filter the candidate list by a certain database (e.g. only compounds from KEGG
and BioCyc) you can press the filter button (top left). A menu will open displaying
all available databases. Only candidates will be displayed that are
enabled in this filter menu. If you want to see
only compounds from KEGG and BioCyc you have to check only KEGG and BioCyc.

The blue and red squares are a visualization of the CSI:FingerID
predictions and scoring. All blue squares represent molecular structures
that are found in the candidate structure and are predicted by
CSI:FingerID to be present in the measured feature. The more intense
the color of the square the higher is the predicted probability for the
presence of this substructure. The larger the square the more reliable
is the predictor. The red squares, however, represent structures that
are predicted to be absent but are, nevertheless, found in the candidate
structure. Again, as more intense the square as higher the predicted
probability that this structure should be absent. Therefore, a lot of
large intense blue squares and as few as possible large intense red
squares are a good indication for a correct prediction.

When hovering over these squares the corresponding
description of the molecular structure (usually a SMART expression) is
displayed. When clicking on one of these squares, the corresponding
atoms in the molecule that belong to this substructure are highlighted.
If the substructure matches several times in the molecule, it is once
highlighted in dark blue while all other matches are highlighted in a
translucent blue.

You can enable filtering by the selected substructure (button with the structure, top left),
to only show candidates that contain the selected substructure.
Further, you can filter the candidate list using a SMARTS pattern or full-text search (top ribbon).

You can open a context menu by right click on a proposed structure. It offers
you to open the compound in PubChem or copy the InChI or InChI key in
your clipboard.

If the structure is contained in any database, a blue or grey label
with the name of this database is displayed below the structure. You can
click on blue labels to open the database entry in your browser
window. Yellow labels indicate that the candidate is contained in the corresponding
custom database. A red label indicates that this candidate is flagged with an unknown database.
This can for example happen when loading results that have been computed with a custom database that 
is not available on the current system. Black labels are just additional information such as if the candidate 
is part of the CSI:FingerID training data.

This tab also includes visualization for the "El Gordo" lipid class annotation functionality. Lipid structures are often extremely similar to each other,
often only differing in the position of the double bonds. These extremely similar structures are often not even differentiable by mass spectrometry at all, which is why the overarching lipid class is shown above the structure candidates. 

If the PubChem fallback was triggered as part of [Expansive search]({{ "/advanced-background-information/#Expansive-search-(structure-database-search-with-fallback)" | relative_url }}), a notification will be displayed below the top ribbon.

{% capture fig_img %}
![Foo]({{ "/assets/images/structures_specMatch.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>structure annotation with spectral library match.</figcaption>
</figure>

If a structure candidate shown in this tab also has a reference spectrum imported via a custom database, the spectral match will be shown.
Clicking on it will show the spectral matching tab (TODO: link)



### De Novo Structure tab

{% capture fig_img %}
![Foo]({{ "/assets/images/msnovelist.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>De novostructure annotation tab tab.</figcaption>
</figure>

This tab shows de novo structure generation results produced by [MSNovelist]({{ "/advanced-background-information/#MSNovelist" | relative_url }}) and tags them with a "de novo" tag. 
If MSNovelist generated structures that are also contained in regular structure databases, the corresponding tags will be added to that structure. 
As an example, generated structures 1-4 in the image above are also present in structure databases, while generated structure 5 is de novo only.

By default, structure candidates present in structure databases but NOT generated by MSNovelist will also be shown. This can be turned off using the first button
on the top right.


### Substructure Annotation tab

In this tab, a direct connection between the input MS/MS spectrum and the CSI:FingerID structure candidates is visualized.
The table in the top part of the view shows all structure candidates for a given query that were also present in the "Structures" tab. Structure candidates generated
by MSNovelist are also shown here. This can be disabled using the first button on the top right.
By selecting a structure, the bottom part of the view shows the fragmentation spectrum on the left, as well as the given structure candidate on the right. 

{% capture fig_img %}
![Foo]({{ "/assets/images/substructure_annotation.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Substructure annotation view.</figcaption>
</figure>

Peaks in the fragmentation spectrum are color coded as follows:

-Black peaks:  Peaks that are not used to explain the molecular formula of the candidate, and are as such not part of the fragmentation tree (just like in the "Formulas" tab). Usually, these peaks can be considered as noise or not explainable by the precursor ions molecular formula.

-Green peaks: Peaks that are used to explain the molecular formula of the candidate, and as such are part of the fragmentation tree (just like in the "Formulas" tab), but do not have a substructure associated to them (see below)

-Purple peaks: Peaks that are used to explain the molecular formula of the candidate, AND can be associated to a specific substrucure of the candidate's structure. Possible substructures are combinatorially generated and then scored against the peaks in the spectrum, with the highest scoring substructure for each peak being displayed on the right. Blue atoms and bonds make the substructure, while red bonds denote the fragmentation that would have needed to occur for that fragment to be formed.

Peaks can be navigated by left-clicking on them, or using the arrow keys.

### Library matches tab

{% capture fig_img %}
![Foo]({{ "/assets/images/library_matches.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Spectral library matches tab.</figcaption>
</figure>

This tab shows spectral library matches for the measured query spectrum against a spectral library. **Query** denotes which measured spectrum
produced the match, in case multiple MS2 were measured for the same feature (in this example it was MS2 spectrum #26). The mirror plot view can be
zoomed by mouse wheel or by holding right click and drag-selecting an area. Please see [Spectral library matching via custom databases]({{ "/advanced-background-information/#Spectral-library-matching-via-custom-databases" | relative_url }}) for background information on
spectral library search in SIRIUS, as well as [Import of custom structure and spectra databases]({{ "/gui/#Import-of-custom-structure-and-spectra-databases" | relative_url }})

## Data export (Summaries and FBMN export)

Summary files containing analysis results can be exported via the "Summaries" button on the top left.

{% capture fig_img %}
![Foo]({{ "/assets/images/summaries.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Summaries export dialogue.</figcaption>
</figure>

Summaries will generally include three types: formula summaries, canopus summaries and structure summaries. By default, only the top hit is exported,
this can be changed by either selecting "All hits" (can produce very large files) or "top k hits". For formula and canopus summaries, the user can choose
to additionally export adducts belonging to the top hits. See [Summary files]({{ "/io/#summary-files" | relative_url }})) for a breakdown of the generated summary files. TODO: adducts for structures?

### Feature based molecular networking (FBMN) export

TODO


## Settings

The settings dialogue can be opened by pressing the "Settings" button on the top right.

{% capture fig_img %}
![Foo]({{ "/assets/images/settings_general.png" | relative_url }})
{% endcapture %}
<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>General settings Settings.</figcaption>
</figure>

  - *General settings*
      - *UI Theme:* Choose your favourite display mode for less eye strain (requires restart).
    
      - *Scaling factor:* Increases the size of the GUI by the chosen factor (requires restart).
    
      - *Import data without MS/MS:* If checked, features with no MS/MS data will be imported into SIRIUS. Please note that for
        features with no MS/MS data, only isotope pattern analysis can be performed.
      - *Ignore formulas:* If checked, SIRIUS will ignore any molecular formula annotations that may already be present in the input file.
        This can be useful for evaluation, or in case you don't trust formula annotations added externally.
      - *Confidence score display mode:* Sets the confidence score display mode (either approximate or exact). 
    
      - *Allowed solvers:* Choose the ILP solver for SIRIUS to use for
        fragmentation tree computation. GLPK is free, Gurobi is
        commercial but offers free academic license.
    
      - *Database cache:* location of cache directory. CSI:FingerID
        download candidate structures from our server and caches them
        for faster retrieval.



   - *Adduct settings*

      - Add or remove custom adducts for positive and negative ion mods

    

  - *Proxy and Network settings*
    
      - Sirius supports proxy configuration. It can be enabled by changing
        the proxy configuration from NONE to SIRIUS. If SIRIUS is selected
        it uses the configuration you have specified int the Settings
        -\> Network panel. If NONE is selected Sirius ignores all proxy
        settings.
    
      - Edit the information in the Settings -\> Network panel if you want
        to address CSI:FingerID via a proxy server. Your specified
        configuration will be tested if you hit the save button.

 

## Webservice

The connection check dialogue on the top right can help diagnose connection problems.

{% capture fig_img %}
![Foo]({{ "/assets/images/connectionCheck.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>Webservice status dialog.</figcaption>
</figure>

Green checkmarks or red crosses will appear depending on if you have connection to the internet, 
login server, license server and web service. Additionally, information on if the account you are 
currently logged in to has a valid subscription attached to it can be found here. Potential connection
or licensing issues will be given in the description box.
