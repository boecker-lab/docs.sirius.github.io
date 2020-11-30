---
permalink: /quick-start/
title: "Quick start"
---

## Graphical User Interface

<span>**<span style="color: red">\[Update needed\!\]</span>**</span>

### Working in single mode

SIRIUS’s “Single mode” corresponds to analyzing a single compound with
one or more mass spectra.

1.  Move the three files , and from the demo data via Drag and Drop into
    the application window

2.  The following dialog offers you to select the columns for mass and
    intensity values. Just press *Ok* as the default values are already
    correct.

3.  You see the load dialog with three spectra. The first spectra is
    wrongly annotated as *MS/MS* spectrum but should be an *MS1*
    spectrum instead. Just select *MS 1* in the drop down list labeled
    with *ms level*.

4.  All other options are fine. However, you might want to choose a more
    memorizable name in the *compound name* field.

5.  Press the *OK* button. The newly imported compound should now appear
    in your compound list on the left side.

6.  Choose the compound, right-click on it and press *Compute*.

7.  In the compute dialog all options should be fine. Just check that
    the correct parent mass is chosen. You might want to add Chlorine or
    Fluorine to the set of considered elements. Furthermore, you can
    change the instrument type to *Orbitrap*

8.  Just look into the candidate list: The first molecular formula has a
    quite large score. Furthermore, the second molecular formula has a
    much lower score. This is a good indication that the identification
    is correct. However, you can take a look at the fragmentation tree:
    Do the peak annotation look correct? Take a look at the spectrum
    view: Are all high intensive peaks are explained?

9.  You can now save the result list as CSV file (by pressing the
    *Export Results* button). Maybe you want save your workspace, too.
    Just press the *Save Workspace* button.

### Working in batch mode

SIRIUS’s “Batch mode” corresponds to analyzing many compounds at once,
each having one or more mass spectra.

1.  Move the files and from the demo data via Drag and Drop into the
    application window

2.  The two compounds are now displayed in the compound list

3.  Just check if the ionization and parent mass is correctly annotated.
    You can change this values by clicking on the compound and then on
    *Edit*.

4.  Click on the *Compute All* button.

5.  You can now select the allowed elements, the instrument type as well
    as the maximal allowed mass deviation. Be aware that this settings
    will be used for all imported compounds

6.  Choose *Orbitrap* in the instrument field and press *OK*

7.  A *...* symbol occurs on the lower right corner of each compound.
    This means that the compound will be computed soon. A gear symbol
    tells you that this compound is currently computed in background. A
    check mark appears in all compounds that were successfully computed,
    a red cross marks compounds which computation fails.

8.  Probably you will not see anything than a check mark, as the
    computation is very fast. However, if you see a compound with a red
    cross you might want to compute it again in Single Mode. Check if
    the parent mass and ionization is correct.

9.  Sometimes a computation might take a long time (e.g. for compounds
    with a lot of elements or very high masses). You can cancel the
    computation of a single compound by selecting *Cancel Computation*
    in the right-click context menu. You can cancel the computation of
    all compounds by clicking on *Cancel Computation* in the toolbar.

### Identifying a CASMI challenge

1.  Download the files
    <http://casmi-contest.org/2014/Challenge2014/Challenge1/1_MS.txt>
    and
    <http://casmi-contest.org/2014/Challenge2014/Challenge1/1_MSMS.txt>

2.  Move these files via Drag and Drop into the application window

3.  Change the ms level of the first file into *Ms 1*

4.  Click on *OK*

5.  Click on *Compute* in the right-click context menu of the imported
    compound

6.  Choose *Q-TOF* as instrument and press the *OK* button

7.  *C23H46NO7P* should be suggested as number one hit in the candidate
    list

## Command Line Interface

You can download some sample spectra from the SIRIUS website at
<https://bio.informatik.uni-jena.de/wp/wp-content/uploads/2015/05/demo.zip>

The demo-data contain examples for three different data formats readable
by SIRIUS. The MGF folder contain an example for a MGF file containing a
single compound with several MS/MS spectra measured on an Orbitrap
instrument. SIRIUS recognizes that these MS/MS spectra belong to the
same compound because they have the same parent mass. To analyze this
compound, run:

sirius –input demo-data/mgf/laudanosine.mgf –output \<outputdir\> formula
-p orbitrap

The in should look like this is:

ank formula adduct TrIsScore TreeScore IsotopeScore explPeaks
explIntensity 1 C21H27NO4 \[M + H\]+ 27.803 19.822 7.980 10 0.994 2
C17H23N7O2 \[M + H\]+ 23.451 18.391 5.060 10 0.994 3 C15H28N5O3P \[M +
H\]+ 21.900 16.902 4.997 10 0.994 4 C19H29NO4 \[M + Na\]+ 21.645 15.827
5.817 9 0.988 5 C19H25N4O3 \[M + H\]+ 21.543 14.412 7.131 9 0.990 6
C17H30N2O4P \[M + H\]+ 18.438 11.841 6.597 9 0.990 7 C13H33N3O4P2 \[M +
H\]+ 17.091 14.028 3.062 10 0.994 8 C14H32NO7P \[M + H\]+ 15.046 12.908
2.138 11 1 9 C15H32N2O4P \[M + Na\]+ 9.401 5.956 3.445 10 0.994 10
C16H35NO2P2 \[M + Na\]+ 8.940 5.532 3.407 9 0.988

This is a ranking list of the top molecular formula candidates. The best
candidate is with an overall score (*TrIsScore*) of 27.803. This score
is the sum of the *TreeScore* (19.822) and the *IsotopeScore* (7.980).

The last two columns contain the number of explained peaks in MS/MS
spectrum as well as the relative amount of explained intensity. The last
value should usually be over 80 % or even 90 %. If this value is very
low you either have strange high intensive noise in your spectrum, or
the allowed mass deviation might be too low to explain all the peaks.

If you want to look at the fragmentation trees you can open the output
in the GUI and use the included tree viewer (see,
[6.5.2](#gui:tree-view)). Note that the viewer can also export the tree
as vector graphics .

The directory contains two examples of the format. Each file contains a
single compound measured with an Orbitrap instrument. To analyze this
compound run:

sirius -o \<outputdir\> -i demo-data/ms/Bicuculline.ms formula -p
orbitrap

As the ms file already contains the correct molecular formula, SIRIUS
will directly compute the fragmentation tree without decomposing the
mass (like when specifying exactly one molecular formula via option).

If you want to enforce a molecular formula analysis and ranking
(although the correct molecular formula is given within the file) use
option to ignore molecular the formula in the file. The number of
formula candidates can be specified via the option:

sirius -o \<outputdir\> -i demo-data/ms/Bicuculline.ms –ignore-formula
formula -p orbitrap -c 5

SIRIUS will now ignore the correct molecular formula in the file and
output the 5 best candidates.

The TXT folder contains simple peaklist files. Such file formats can be
easily extracted from Excel spreadsheets. However, they do not contain
meta information like the MS level and the parent mass. So you have to
specify this information via commandline options:

The demo data contain a clean MS spectrum (e.g. there is only one
isotope pattern contained in the MS spectrum). In such cases, SIRIUS can
infer the correct parent mass from the MS data (by simply using the
monoisotopic mass of the isotope pattern as parent mass). So you can
omit the option in this cases.

