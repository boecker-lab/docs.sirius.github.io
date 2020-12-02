---
permalink: /quick-start/
title: "Quick start"
---
<span>**<span style="color: red">\[Extension for CSI:FingerID and CANOPUS needed\!\]</span>**</span>


 - You can download some [sample spectra](https://bio.informatik.uni-jena.de/wp/wp-content/uploads/2015/05/demo.zip)
from the SIRIUS website.
 - To get started quickly you may also want to have a look at our 
   [video tutorials](https://www.youtube.com/playlist?list=PL57Jv_39fTdc_j8eHrH6At81n1AtxeKar).

## Graphical User Interface

### Working in single mode

SIRIUS’s "Single mode" corresponds to analyzing a single compound with
one or more mass spectra.

1.  Click the *Import Compound* Button in the Toolbar to open the importer window.
    
1.  Move each of the three files `demo-data/txt/chelidonine_ms.txt`, `demo-data/txt/chelidonine_msms1.txt`
    and `demo-data/txt/chelidonine_msms2.txt` (one after another) from the demo data via Drag and Drop into
    the importer window.

1.  The following dialog offers you to select the columns for mass and
    intensity values. Just press *Ok* as the default values are already
    correct.

1.  You see the load dialog with three spectra. The first spectra is
    wrongly annotated as *MS/MS* spectrum but should be an *MS1*
    spectrum instead. Just select *MS 1* in the drop down list labeled
    with *ms level*.

1.  All other options are fine. However, you might want to choose a more
    memorizable name in the *compound name* field.

1.  Press the *OK* button. The newly imported compound should now appear
    in your compound list on the left side.

1.  Choose the compound, right-click on it and press *Compute*.

1.  Select SIRIUS. All other options should be fine. Just check that
    the correct parent mass is chosen. You might want to add Chlorine or
    Fluorine to the set of considered elements. Furthermore, you can
    change the instrument type to *Orbitrap*

1.  Just look into the candidate list: The first molecular formula has a
    quite large score. Furthermore, the second molecular formula has a
    much lower score. This is a good indication that the identification
    is correct. However, you can take a look at the fragmentation tree:
    Do the peak annotation look correct? Take a look at the spectrum
    view: Are all high intensive peaks are explained?

1.  To write the results into a summary file press the *Summaries* button.
    You can find the result list `formula_candidates.tsv` at the compound level
    in the [project-space]({{ "/io/#sirius-project-space" | relative_url }}) directory.

### Working in batch mode

SIRIUS’s "Batch mode" corresponds to analyzing many compounds at once,
each having one or more mass spectra.

1.  Move the files `demo-data/ms/Bicuculline.ms` and `demo-data/ms/Kaempferol.ms` and from the demo data via Drag and 
    Drop into the application window.

1.  The two compounds are now displayed in the compound list.

1.  Just check if the ionization and parent mass is correctly annotated.
    You can change this values by right-clicking on the compound and then on
    *Edit*.

1.  Click on the *Compute All* button.

1. Select SIRIUS.

1.  You can now select the allowed elements, the instrument type as well
    as the maximal allowed mass deviation. Be aware that this settings
    will be used for all imported compounds

1.  Choose *Orbitrap* in the instrument field and press *OK*

1.  A *gear* symbol occurs on the lower right corner of each compound.
    This means that the compound is part of a computation job.
    
1.  Sometimes a computation might take a long time (e.g. for compounds
    with a lot of elements or very high masses). You can cancel running
    computations by selecting *Cancel All* int the toolbar.

### Identifying a CASMI challenge

1.  Download the files <http://casmi-contest.org/2014/Challenge2014/Challenge1/1_MS.txt>
    and <http://casmi-contest.org/2014/Challenge2014/Challenge1/1_MSMS.txt>

1.  Click the *Import Compound* Button in the Toolbar to open the importer window.

1.  Move these files via Drag and Drop into the importer window (one after another).

1.  Change the ms level of the first file into *Ms 1*

1.  Click on *OK*

1.  Click on *Compute* in the right-click context menu of the imported
    compound (or *double click*).

1.  Select SIRIUS and choose *Q-TOF* as instrument and press the *OK* button

1.  *C23H46NO7P* should be suggested as number one hit in the candidate
    list

## Command Line Interface

The demo-data contain examples for three different data formats readable
by SIRIUS. The MGF folder contain an example for a MGF file containing a
single compound with several MS/MS spectra measured on an Orbitrap
instrument. SIRIUS recognizes that these MS/MS spectra belong to the
same compound because they have the same parent mass. To analyze this
compound, run:

```
sirius --input demo-data/mgf/laudanosine.mgf --output <outputdir> formula -p orbitrap
```

The `formula_candidates.csv` in `<outputdir>/0_laudanosine_FEATURE1` in should look like this is:

```
rank	molecularFormula	adduct	precursorFormula	SiriusScore	TreeScore	IsotopeScore	numExplainedPeaks	explainedIntensity	medianMassErrorFragmentPeaks(ppm)	medianAbsoluteMassErrorFragmentPeaks(ppm)	massErrorPrecursor(ppm)
1	C21H27NO4	[M + H]+	C21H27NO4	25.082452119514926	18.994076922245473	6.088375197269453	10	0.9945969759520045	0.007637492516338191	0.4982304820065859	0.7071154657093854
2	C17H23N7O2	[M + H]+	C17H23N7O2	22.22488927526431	18.391045563546733	3.833843711717577	10	0.9942823515505598	0.04730810504363186	0.8982080585874639	8.203923029468156
3	C15H28N5O3P	[M + H]+	C15H28N5O3P	20.89183833237749	16.902103208731077	3.9897351236464114	10	0.9945969759520045	0.8982080585874639	1.2778688586195766	3.5880578691669838
4	C19H25N4O3	[M + H]+	C19H25N4O3	19.876051026264605	14.412089485232423	5.463961541032182	9	0.9900893988366646	-0.01690518911597119	0.6982192702970249	4.455519247668116
5	C19H29NO4	[M + Na]+	C19H29NO4	19.715064663870688	15.307483124784351	4.407581539086338	9	0.9888793275025645	0.47275808181554785	1.08803845860352	7.423133528795216
6	C14H27N7O2S	[M + H]+	C14H27N7O2S	18.079922187727448	17.006473166528192	1.073449021199255	10	0.9945969759520045	-0.4982304820065859	1.2778688586195766	-1.2073375083370121
7	C17H30N2O4P	[M + H]+	C17H30N2O4P	17.18707940726463	11.841454023926527	5.345625383338102	9	0.9900893988366646	-0.28967448264108003	0.9201704555380266	-0.1603459126330561
8	C13H33N3O4P2	[M + H]+	C13H33N3O4P2	16.33806795296112	14.02899693346263	2.3090710194984885	10	0.9945969759520047	-0.4982304820065859	1.2778688586195766	-1.0278072909754976
9	C14H32NO7P	[M + H]+	C14H32NO7P	14.23105483607496	12.908155814210904	1.3228990218640573	11	0.9999999999999998	1.08803845860352	1.6223589205801845	7.321687845144798
10	C16H29N4O3S	[M + H]+	C16H29N4O3S	11.327663752064977	10.200105613108695	1.1275581389562817	9	0.9888793275025645	-0.28967448264108003	1.08803845860352	-4.955741290295743
```

This is a ranking list of the top molecular formula candidates. The best
candidate is C<sub>21</sub>H<sub>27</sub>NO<sub>4</sub> with an overall score (*SiriusScore*) of 27.803. This score
is the sum of the *TreeScore* (19.822) and the *IsotopeScore* (7.980).

The last two columns contain the number of explained peaks in MS/MS
spectrum as well as the relative amount of explained intensity. The latter
value should usually be over 80 % or even 90 %. If this value is very
low you either have strange high intensive noise in your spectrum, or
the allowed mass deviation might be too low to explain all the peaks.

If you want to look at the fragmentation trees you can open the output (`<outputdir>`) 
in the GUI and use the included [tree viewer]({{ "/gui/#tree-view-tab" | relative_url }}). 
The output can be imported by dropping the `<outputdir>` into the SIRIUS GUI application window.
Note that the viewer can also export the tree as vector graphics (svg/pdf).

The directory contains two examples of the format. Each file contains a
single compound measured with an Orbitrap instrument. To analyze this
compound run:

```
sirius -o <outputdir> -i demo-data/ms/Bicuculline.ms formula -p orbitrap
```

As the ms file already contains the correct molecular formula, SIRIUS
will directly compute the fragmentation tree without decomposing the
mass (like when specifying exactly one molecular formula via `-f` option).

If you want to enforce a molecular formula analysis and ranking
(although the correct molecular formula is given within the file) use
the `--ignore-formula` option to ignore molecular the formula in the file. 
The number of  formula candidates can be specified via the `-c` option.

```
sirius -o <outputdir> -i demo-data/ms/Bicuculline.ms --ignore-formula formula -p orbitrap -c 5
```

SIRIUS will now ignore the correct molecular formula in the file and
output the 5 best candidates.

The TXT folder contains simple peaklist files. Such file formats can be
easily extracted from Excel spreadsheets. However, they do not contain
meta information like the MS level and the parent mass. So you have to
specify this information via commandline options:

```
sirius -1 demo-data/txt/chelidonine_ms.txt -2 demo-data/txt/chelidonine_msms1.txt demo-data/txt/chelidonine_msms2.txt formula -p orbitrap -z 354.134704589844
```

The demo data contain a clean MS spectrum (e.g. there is only one
isotope pattern contained in the MS spectrum). In such cases, SIRIUS can
infer the correct parent mass from the MS data (by simply using the
monoisotopic mass of the isotope pattern as parent mass). So you can
omit the `-z` option in this cases.