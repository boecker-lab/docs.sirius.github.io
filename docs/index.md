## Welcome

SIRIUS is a *Java* software for analyzing metabolites from tandem mass
spectrometry data. It combines the analysis of isotope patterns in MS
spectra with the analysis of fragmentation patterns in MS/MS spectra,
and uses CSI:FingerID as a web service to search in molecular structure
databases.

SIRIUS requires **high mass accuracy** data. The mass deviation of your
MS and MS/MS spectra should be within 20 ppm. Mass Spectrometry
instruments such as TOF, Orbitrap and FT-ICR usually provide high mass
accuracy data, as well as coupled instruments like Q-TOF, IT-TOF or
IT-Orbitrap. Spectra measured with a quadrupole or linear trap do not
provide the high mass accuracy that is required for our method. See
Sec. [3.4](#sec:mass-deviations) on what “mass accuracy” means in
detail for SIRIUS.

SIRIUS expects **MS and MS/MS** spectra as input. It is possible to omit
the MS data, but it will make the analysis more time consuming and might
give you worse results. In this case, you should consider limiting the
candidate molecular formulas to those found in PubChem.

SIRIUS expects **processed peak lists** (centroided spectra). It does
not contain routines for peak picking from profiled spectra, nor
routines for merging spectra in an LC/MS run. This is a deliberate
design decision: We want you to use the best peak picking software out
there — or alternatively, your favorite software. There are several
tools specialized for this task, such as [OpenMS](https://www.openms.de/), 
[MZmine](http://mzmine.github.io/) or [XCMS](https://github.com/sneumann/xcms). 
See our video tutorials on how to preprocess tour data for SIRIUS
with [OpenMS](https://www.youtube.com/watch?v=ZTEY8_fnuZE) or 
[MZmine](https://www.youtube.com/watch?v=Q0D6q9xQLSE).
However, since version 4.4.0 SIRIUS contains a zero parameter
preprocessing tool to directly import LCMS-Runs from format 
to help you getting started quickly. See how to use 
[MSconvert/ProteoWizard](http://proteowizard.sourceforge.net/index.html)
to convert your vendor formats to mzml for SIRIUS in this 
[video tutorial](https://www.youtube.com/watch?v=xnjvZlSlp40). 

SIRIUS will identify the molecular formula of the measured precursor
ion, and will also annotates the spectrum by providing a molecular
formula for each fragment peak. Peaks that receive no annotation are
assumed to be noise peaks. Furthermore, a **fragmentation tree** is
predicted; this tree contains the predicted fragmentation reaction
leading to the fragment peaks.

SIRIUS uses CSI:FingerID to identify the structure of a compound by
searching in a molecular structure database. Here and in the following,
“structure” refers to the identity and connectivity (with bond
multiplicities) of the atoms, but no stereochemistry information.
Elucidation of stereochemistry is currently beyond the power of
automated search engines.

SIRIUS can be used within an analysis pipeline. For example, you can
identify the molecular formula of the ion and the fragment peaks, and
use this information as input for other tools such as FingerID or MAGMa
to identify the structure of the measured compound. For this purpose,
you can also use the SIRIUS library directly, instead of the command
line interface. See .

Since version 3.1, our software ships with a **Graphical User
Interface** (GUI). The GUI version also includes the commanline tool. A
slim version without GUI is available as separate download. Since
version 4.4.0 the GUI and CLI share the same persistence layer, so
**all** results and intermediate steps can be exported/imported between
GUI and CLI

## Literature

The *scientific development* behind SIRIUS and CSI:FingerID required
numerous man-years of PhD students, postdocs and principal
investigators; an educated guess would be roughly 35 man-years. This
estimate does not include building the shiny Graphical User Interface
that was introduced in version 3.1. But it is not the user interface or
software development that does the work here; it is our scientific
research that made SIRIUS and CSI:FingerID possible. It is understood
that the work of 15 years cannot be described in a single paper.

Please cite all papers that you feel relevant for your work. Please do
not cite this manual or the SIRIUS or CSI:FingerID website, but rather
our scientific papers.

### SIRIUS 4

  - Kai Dührkop, Markus Fleischauer, Marcus Ludwig, Alexander A.
    Aksenov, Alexey V. Melnik, Marvin Meusel, Pieter C. Dorrestein, Juho
    Rousu, and Sebastian Böcker. **SIRIUS 4: a rapid tool for turning
    tandem mass spectra into metabolite structure information.** *Nat
    methods*, 16, 2019. doi: <https://doi.org/10.1038/s41592-019-0344-8>

### ZODIAC – molecular formula annotation

  - Marcus Ludwig, Louis-Félix Nothias, Kai Dührkop, Irina Koester,
    Markus Fleischauer, Martin A. Hoffmann, Daniel Petras, Fernando
    Vargas, Mustafa Morsy, Lihini Aluwihare, Pieter C. Dorrestein,
    Sebastian Böcker **ZODIAC: database-independent molecular formula
    annotation using Gibbs sampling reveals unknown small molecules.**
    *bioRxiv*, 842740, 2019. doi: <https://doi.org/10.1101/842740>

### Searching in molecular structure databases

  - Kai Dührkop, Huibin Shen, Marvin Meusel, Juho Rousu and Sebastian
    Böcker. **Searching molecular structure databases with tandem mass
    spectra using CSI:FingerID.** *Proc Natl Acad Sci U S A*,
    112(41):12580–12585, 2015.

  - Huibin Shen, Kai Dührkop, Sebastian Böcker and Juho Rousu.
    **Metabolite Identification through Multiple Kernel Learning on
    Fragmentation Trees.** *Bioinformatics*, 30(12):i157–i164, 2014.
    Proc. of *Intelligent Systems for Molecular Biology* (ISMB 2014).

### Fragmentation Tree Computation

  - Sebastian Böcker and Kai Dührkop. **Fragmentation trees reloaded.**
    *J Cheminform*, 8:5, 2016.

  - W. Timothy J. White, Stephan Beyer, Kai Dührkop, Markus Chimani and
    Sebastian Böcker. **Speedy Colorful Subtrees.** In *Proc. of
    Computing and Combinatorics Conference* (COCOON 2015), volume 9198
    of *Lect Notes Comput Sci*, pages 310–322. Springer, Berlin, 2015.

  - Imran Rauf, Florian Rasche, François Nicolas and Sebastian Böcker.
    **Finding Maximum Colorful Subtrees in practice.** *J Comput Biol*,
    20(4):1–11, 2013.

  - Florian Rasche, Aleš Svatoš, Ravi Kumar Maddula, Christoph Böttcher
    and Sebastian Böcker. **Computing fragmentation trees from tandem
    mass spectrometry data.** *Anal Chem* 83(4):1243–1251, 2011.

  - Sebastian Böcker and Florian Rasche. **Towards de novo
    identification of metabolites by analyzing tandem mass spectra.**
    *Bioinformatics* 24(16):i49–i55, 2008.

### Isotope pattern analysis

  - Sebastian Böcker, Matthias C. Letzel, Zsuzsanna Lipták and Anton
    Pervukhin. **SIRIUS: decomposing isotope patterns for metabolite
    identification.** *Bioinformatics* 25(2): 218–224, 2009.

  - Sebastian Böcker, Matthias Letzel, Zsuzsanna Lipták and Anton
    Pervukhin. **Decomposing metabolomic isotope patterns.** In *Proc.
    of Workshop on Algorithms in Bioinformatics* (WABI 2006), volume
    4175 of *Lect Notes Comput Sci*, pages 12–23. Springer, Berlin,
    2006.

### Passatutto – Fragmentation tree based decoy spectra

  - Kerstin Scheubert, Franziska Hufsky, Daniel Petras, Mingxun Wang,
    Louis-Felix Nothias, Kai Dührkop, Nuno Bandeira, Pieter C.
    Dorrestein, Sebastian Böcker. **Significance estimation for large
    scale metabolomics annotations by spectral matching.** *Nat Commun*
    8, 1494 (2017) <https://doi.org/10.1038/s41467-017-01318-5>

### Auto-detection of elements

  - Marvin Meusel, Franziska Hufsky, Fabian Panter, Daniel Krug, Rolf
    Müller and Sebastian Böcker. **Predicting the presence of uncommon
    elements in unknown biomolecules from isotope patterns.** *Anal
    Chem*, 88(15):7556–7566, 2016.

### Mass decomposition

  - Kai Dührkop, Marcus Ludwig, Marvin Meusel and Sebastian Böcker.
    **Faster mass decomposition.** In *Proc. of Workshop on Algorithms
    in Bioinformatics* (WABI 2013), volume 8126 of *Lect Notes Comput
    Sci*, pages 45–58. Springer, Berlin, 2013.

  - Sebastian Böcker and Zsuzsanna Lipták. **A fast and simple algorithm
    for the Money Changing Problem.** *Algorithmica* 48(4):413–432,
    2007.

  - Sebastian Böcker and Zsuzsanna Lipták. **Efficient Mass
    Decomposition.** In *Proc. of ACM Symposium on Applied Computing*
    (ACM SAC 2005), pages 151-157. ACM press, New York, 2005.
