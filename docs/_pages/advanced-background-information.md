---
permalink: /advanced-background-information/
title: "Advanced background information"
---

## Spectral quality

SIRIUS and CSI:FingerID have been trained on a wide variety of data,
including data from different instrument types. Nevertheless, certain
aspects of the mass spectra are important so that our software can
successfully process your data:

  - Be reminded that SIRIUS requires **high mass accuracy** data: The
    mass deviation should be within 20 ppm. We are confident that SIRIUS
    can also give useful information for worse mass accuracy (say,
    50 ppm), but you should know what you are doing if you are
    processing such data.

  - It is understood that some molecules generate more fragments,
    whereas others have sparse fragmentation spectra. But it is also
    important to understand that without sufficient information, it is
    impossible to deduce the structure or even the molecular formula
    from a tandem mass spectrum that contains almost no peaks. For
    example, three peaks in a fragmentation spectrum measured with 1 ppm
    mass accuracy contain about 60 bit of information, ignoring
    dependencies between fragments and distibution of molecular masses.
    With this information, it is simply not possible to find the correct
    structure in a database such as PubChem, containing 100 million
    structures. In comparison, ten peaks measured with 20 ppm mass
    accuracy contain about 156 bit of information, again ignoring
    dependencies and distributions. To this end, we ask you to provide
    **rich fragmentation spectra** to SIRIUS, meaning that you **must
    not noise-filter** these spectra, or let the peak
    picking/centroiding software do that for you. At present, SIRIUS
    considers up to 60 peaks in the fragmentation spectrum, and decides
    for itself which of these peaks are considered noise.

  - You will find that CSI:FingerID can sometimes identify the correct
    structure although the fragmentation spectrum is (almost) empty — do
    not get fooled, this is often nothing but lucky guessing. If you
    know how to structurally elucidate a compound based on an empty
    spectrum, please contact us and tell us how.

  - You may have heard that peaks in a MS/MS spectrum with high mass
    carry more information than peaks with low mass: This is a
    misunderstanding. For example, if CSI:FingerID has to differentiate
    between 10000 candidates with identical molecular formula, then
    observing a fragment corresponding to an H<sub>2</sub>O loss is in fact very
    uninformative. To this end, **do not set up your instrument to favor
    peaks of large masses**, sacrificing those with smaller masses.

  - Some instrument types (e.g., time-of-flight) suffer from detectors
    that can run into saturation; saturated peaks can have mass
    differences much larger than those expected for other peaks.
    Unfortunately, most peak picking software do not mark such peaks as
    "misshaped". To this end, it is possible that the most intense
    peak in a spectrum is not explained, as its mass deviation is
    extremely high.

## Monoisotopic masses

The monoisotopic mass of a molecule (or ion) is formally defined as "the
sum of masses of the atoms in a molecule (or ion) using the unbound,
ground-state, rest mass of the most abundant isotope for each element."
Using this definition, the monoisotopic mass is usually not the most
abundant isotopologue of the molecule (e.g., peptides and proteins), it
is often not resolved from other isotopologue peaks, and it may be
undetectable in an MS experiment as it has intensity below noise level.
In particular, given the isotope pattern of an unknown molecule, it is
generally impossible to determine which of the peaks correspond to the
monoisotopic peak. In total, this definition is not very practical.

Many researchers that work on the simulation and interpretation of
isotope patterns have therefor introduced a slightly different and more
practical definition of the monoisotopic mass of a molecule, see for
example [*Dittwald et al.*](https://doi.org/10.1007/s13361-015-1180-4)
and [*Meusel et al.*](https://doi.org/10.1021/acs.analchem.6b01015): 
Here, the isotopologue of a molecule where each atom is the
isotope with the lowest nominal mass (according to the natural isotope
distribution of elements) is referred to as *monoisotopic*. This
definition has the advantages that the monoisotopic mass of a molecule
is always the sum of monoisotopic masses of the atoms, which can be
defined analogously; the monoisotopic peak is in all cases the first
peak of the ideal isotope pattern; and, the monoisotopic (isotopologue)
peak is always resolved from all other isotopologue peaks, even at unit
mass accuracy. Clearly, the monoisotopic peak of a molecule may again be
undetectable in an MS experiments.

SIRIUS uses the second, more practical definition of "monoisotopic".
This results in notable differences only for molecules that contain
"uncommon elements" such as boron or selenium.

## Theoretical masses of ions

There are different ways of computing the mass of an ionized molecule such as C<sub>6</sub>H<sub>7</sub>O + 
or C<sub>6</sub>H<sub>6</sub>ONa + that will result in slightly different results: in particular, 
adding the mass of a proton vs. subtracting the mass of an electron. Following
suggestions by [*Ferrer & Thurman*](https://doi.org/10.1002/rcm.3102), 
SIRIUS computes this mass by subtracting the rest mass of an electron. To this
end, the monoisotopic mass of C<sub>6</sub>H<sub>7</sub>O + is the monoisotopic mass of the molecule 
C<sub>6</sub>H<sub>7</sub>O (95.049690 Da) minus the
rest mass of an electron (0.000549 Da), which totals as 95.049141 Da. Similarly, the monoisotopic mass of 
C<sub>6</sub>H<sub>6</sub>ONa + equals 117.031634 Da - 0.000549 Da = 117.031085 Da.

Above, masses have been rounded to six decimals; internally, SIRIUS uses
double precision for representing masses. Masses of isotopes are taken
from the [AME2016 atomic mass evaluation](http://nuclearmasses.org/resources_folder/Wang_2017_Chinese_Phys_C_41_030003.pdf). 
See the [Table](#isotopes-with-masses-and-abundances-as-used-by-sirius) below for the isotope
masses and abundances used by SIRIUS, again rounded to six decimals for
presentation.

### Isotopes with masses and abundances as used by SIRIUS
In this table, *masses have been rounded to six decimals* for the purpose of
presentation; internally, SIRIUS uses masses with higher precision. ‘AN’
is atomic number. *Isotope abundances of boron can vary strongly, so
isotope pattern analysis is of little use for identifying the correct
molecular formula in case boron is present.

| element (symbol) | AN | isotope       | abundance (%)                    | mass (Da)          |
|-----------------:|---:|:-------------:|---------------------------------:|-------------------:|
|     hydrogen (H) |  1 |<sup>1</sup>H  |                        99.988% |         1.007825 |
|                  |    |<sup>2</sup>H  |                         0.012% |         2.014102 |
|        boron (B) |  5 |<sup>10</sup>B |                        19.9<sup>*</sup>% |        10.012937 |
|                  |    |<sup>11</sup>B |                        80.1<sup>*</sup>% |        11.009305 |
|       carbon (C) |  6 |<sup>12</sup>C |                         98.93% |             12.0 |
|                  |    |<sup>13</sup>C |                          1.07% |        13.003355 |
|     nitrogen (N) |  7 |<sup>14</sup>N |                        99.636% |        14.003074 |
|                  |    |<sup>15</sup>N |                         0.364% |        15.001090 |
|       oxygen (O) |  8 |<sup>16</sup>O |                        99.757% |        15.994915 |
|                  |    |<sup>17</sup>O |                         0.038% |        16.999131 |
|                  |    |<sup>18</sup>O |                         0.205% |        17.999160 |
|     fluorine (F) |  9 |<sup>18</sup>F |                           100% |        18.000938 |
|     silicon (Si) | 14 |<sup>28</sup>Si|                        92.223% |        27.976927 |
|                  |    |<sup>29</sup>Si|                         4.685% |        28.976495 |
|                  |    |<sup>30</sup>Si|                         3.092% |        29.973770 |
|     phosphor (P) | 15 |<sup>32</sup>P |                           100% |        30.973762 |
|       sulfur (S) | 16 |<sup>33</sup>S |                         94.99% |        31.972071 |
|                  |    |<sup>34</sup>S |                          0.75% |        32.971459 |
|                  |    |<sup>35</sup>S |                          4.25% |        33.967867 |
|                  |    |<sup>36</sup>S |                          0.01% |        35.967081 |
|    chlorine (Cl) | 17 |<sup>35</sup>Cl|                         75.76% |        34.968853 |
|                  |    |<sup>37</sup>Cl|                         24.24% |        36.965903 |
|    selenium (Se) | 34 |<sup>74</sup>Se|                          0.89% |        73.922476 |
|                  |    |<sup>76</sup>Se|                          9.37% |        75.919214 |
|                  |    |<sup>77</sup>Se|                          7.63% |        76.919914 |
|                  |    |<sup>78</sup>Se|                         23.77% |        77.917309 |
|                  |    |<sup>80</sup>Se|                         49.61% |        79.916521 |
|                  |    |<sup>82</sup>Se|                          8.73% |        81.916699 |
|     bromine (Br) | 35 |<sup>79</sup>Br|                         50.69% |        78.918337 |
|                  |    |<sup>81</sup>Br|                         49.31% |        80.916291 |
|       iodine (I) | 53 |<sup>127</sup>I|                           100% |       126.904473 |

**We suggest to calibrate your instrument with ion masses as calculated
above.** In any case, you should *be aware of this tiny mass
difference*, as this can result in unexpected behavior when decomposing
masses; see for example [*Pluskal
et al.*](https://doi.org/10.1021/ac3000418).

## Mass deviations

SIRIUS assumes that mass deviations (the difference between the measured
mass and the theoretical mass of the ion) are normally distributed(
[*Jaitly et al.*](https://doi.org/10.1021/ac052197p); 
[*Zubarev & Mann*](https://doi.org/10.1074/mcp.M600380-MCP200); 
[*Böcker & Dührkop*](https://dx.doi.org/10.1186%2Fs13321-016-0116-8)). 
The user-defined parameter "mass accuracy" is given in parts-per-million
(ppm). SIRIUS interpretes this parameter as a "guarantee" and, hence,
assumes that **this is the maximum allowed mass deviation**; it will
**discard** all explanations that require a larger mass deviation. This
implies that **if in doubt, you should use a larger mass accuracy** to
ensure that SIRIUS can successfully annotate peaks in the spectrum. For
masses below 200 Da, we use the absolute mass deviation at 200 Da, as we
found that small masses vary according to an absolute rather than a
relative error.

## SIRIUS workflows

SIRIUS is segmented into sub tools: Formula annotation (SIRIUS + ZODIAC), Fingerprint prediction (CSI:FingerID + CANOPUS), Structure database search (CSI:FingerID) and de novo structure
generation (MSNovelist). These sub tools follow a certain hierarchy, and can not be combined freely. For example, to predict CANOPUS compound classes the molecular formula annotation sub tool
has to be run first (or results have to be present from a previous run). See below figure for how the different sub tools depend on each other.

{% capture fig_img %}
![Foo]({{ "/assets/images/workflow_dependencies.png" | relative_url }})
{% endcapture %}

<figure>
  {{ fig_img | markdownify | remove: "<p>" | remove: "</p>" }}
  <figcaption>SIRIUS sub tool dependencies.</figcaption>
</figure>


## Spectral library matching via custom databases

SIRIUS 6 offers to import local libraries containing spectral reference data. Supported import formats for spectral
data are .ms, .mgf, .msp, .mat, .txt (MassBank), .mb, .json (GNPS, MoNA). Spectra need to be annotated with a structure and be centroided.
SIRIUS will automatically perform spectral library search against all available libraries every time the molecular formula annotation subtool
is used. Spectral library matching is performed using the cosine score with squared peak intensities and ignored precursor peak.

### Spectral matching influence on SIRIUS and CSI:FingerID results

In SIRIUS 6, spectral library matches are treated as annotations to CSI:FingerID results. A spectral library match will not influence the rank of
a structure candidate, but annotated to CSI:FingerID results instead. In case where a high quality spectral library hit is found where the corresponding molecular
formula would not have been considered by SIRIUS, that molecular formula will be forcibly added to the list of molecular formula candidates.
This done to ensure that no spectral library matches are lost when using CSI:FingerID.


## Molecular formula annotation concepts

SIRIUS supports three different approaches to generate the set of molecular formula candidates considered for annotation of a feature: de novo, database search and bottom up. 
Understanding them is vital to being able to apply the annotation [strategy](#molecular-formula-annotation-strategies) that best fits your task or research question.
It is also important to understand the implications of the molecular formula annotation step for structure annotation and compound class prediction:
Only those molecular formula candidates that are considered by the molecular formula annotation strategy are used to annotate structures via database search
and compound classes later on. 

**IMPORTANT: If a molecular formula is not part of the candidate set in this step, it will not be considered for all subsequent steps!**

### De novo annotation

SIRIUS will consider all molecular formulas
that are chemically feasible (considering valencies) and explain the precursor mass of the
molecule / ion: For example, if your query compound is pinensin A 
(C<sub>96</sub>H<sub>139</sub>N<sub>27</sub>O<sub>30</sub>S<sub>2</sub>,
monoisotopic mass of 2213.962 Da) then SIRIUS will consider all
19,746,670 candidate molecular formulas that explain this
monoisotopic mass (assuming a set of elements, see below, and 10 ppm mass
accuracy). SIRIUS penalizes candidate molecular formulas that deviate
too strongly of what we assume a molecular formula of a biomolecule to look like 
(for example, C<sub>2</sub>H<sub>2</sub>N<sub>12</sub>O<sub>12</sub> will receive a penalty), 
but this penalty is used cautiously: Only 2.6% of the molecular formulas of all PubChem
compounds — and, hence, only a tiny fraction of molecular formulas from
compounds not marked as biomolecules — are penalized. Molecular formulas
are never rewarded by SIRIUS. These penalties apply to the other approaches as well. 

SIRIUS uses a short list of outlier molecular formulas which would be
penalized by the above method, as they are not "biomolecule-like"; these
molecular formulas are not penalized, as they have been observed in
metabolomics experiments (for example, as solvents), but are also not
rewarded. However, fragment annotations in the MS/MS, and hence subformulas of these outlier
molecular formulas, may be penalized during fragmentation tree computation.

Considering all molecular formulas implies that a set of elements has to
be provided from which these molecular formulas are generated. SIRIUS
includes methods for the [auto-detection of elements](https://doi.org/10.1021/acs.analchem.6b01015) 
from the isotope and fragmentation pattern of the query compound.
The element set should only be manually altered, if the user has a good reason to do so (e.g. prior knowledge about the 
feature of interest). Expanding the element set too much will result in extreme computation times and increased bogus annotations. 
The standard element set considered is C,H,N,O,P, while presence and abundance of Cl,B,Se,S,Br will
be autodetected from the input isotope pattern in the MS1 spectrum.

### Formula database search

Instead of considering the complete space of molecular formulas possible for a given mass and element set, one can also restrict that space
to a database. In that case, SIRIUS will only consider molecular formulas that are part of the selected database(s) and it is possible to further apply
element set restrictions to that. Naturally, this approach is unable to annotate novel molecular formulas ("novel" meaning not part of the selected database) and will harshly restrict the
space of molecular formulas candidates. Since the space of possible formula candidates is so much smaller then with de novo, this approach does not require a 
predefined element set. In contrast to de novo, this approach may annotate formulas with uncommon elements that cannot be detected from the MS1 (since considering a large set of uncommon element for de novo is usually no good practice, see above).

### Bottom up search

The "bottom up" approach is somewhat of a middle ground between the vast molecular formula space of de novo annotation and the very limited space of formula database search.
It is inspired by [*Xing et al.*](https://doi.org/10.1038/s41592-023-01850-x).
For each fragment observed in the MS/MS spectrum, its mass and corresponding root loss mass are used to query a database of potential subformulas.
The resulting subformula candidates for fragment and root loss are added pairwise to create formula candidates for the precursor.
Thus, this resulting space of precursor formula candidates depends on the fragments present in the spectrum. The space is not limited to exactly those
precursor formulas already present in databases, but can also contain novel formulas that are combinations of two known molecular formulas. 
However, due to the dependence on a database, the approach produces a much smaller number of formulas compared to de novo annotation, which leads to a substantial speed up in computation time.
Since the space of possible formula candidates is limited, it is not strictly necessary to apply restrictions on the considered element set.
The formula database used for bottom up search contains the "bio" database formulas as well as a list of commonly appearing losses.
In contrast to de novo, this approach may annotate formulas with uncommon elements that cannot be detected from the MS1 (since considering a large set of uncommon element for de novo is usually no good practice, see above).



## Molecular formula annotation strategies

The molecular formula annotations shown above can either be used individually or combined. Choosing the correct molecular formula annotation
strategy is integral for a successful analysis.  Below are some standard strategies that cover most applications and can serve as examples:

### De novo + bottom up (recommended for generic applications)

In the recommended combined approach, features are divided into "low" (m/z<400) and "high"(m/z>=400) mass features. Bottom up search is performed in both cases, but for low mass features SIRIUS
additionally performs de novo molecular formula annotation as a means to ensure no formula is missed. Due to de novo only being performed for lower masses, computation times are only minorly impacted compared to solely
performing bottom up search.
The m/z threshold can be adjusted based on running time constraints and capabilities of your local machine.
Element set constraints have to be set for de novo annotation and can additionally be set to apply to bottom up search as well. This approach can produce molecular
formula annotations with no corresponding structure database hit.

### De novo only

The "de novo only" strategy should be employed when specifically expecting molecular formulas that cannot be generated by bottom up search (meaning that the precursor
formula in question is not a combination of database subformulas). This may especially be the case when looking for "unknown unknowns".
Additionally, the expected element set needs to be well-defined and should not contain many "uncommon" elements due to the combinatorial explosion of possible candidates for large masses (see example in [de novo](#de-novo-annotation)).
The local machine running the SIRIUS client should be powerful enough to handle de novo annotation of higher mass compounds. This approach can produce molecular
formula annotations with no corresponding structure database hit.


### Database search only

"Database search only" should be employed when the user is only interested in features with a structure database hit and additionally requires extremely fast
computation times. This approach cannot produce formula annotations with no structure database hit and will only consider molecular formulas that are part
of the selected databases.

### Bottom up only

"Bottom up only" can be employed for a minor speed up over the recommended combined approach. In general, it does not hold any significant advantages over the recommended
strategy, since disadvantages of de novo annotation only are mostly relevant for high-mass compounds.



## Fragmentation trees

Fragmentation trees annotate the fragmentation spectrum with molecular
formulas, and identify likely losses between the ions in the
fragmentation spectrum. Fragmentation trees can be used both to identify
the molecular formula of a query compound, and to derive information
about its fragmentation: For example, this is used in CSI:FingerID to
predict the molecular fingerprint of the query compound. Fragmentation
trees are computed directly from the fragmentation spectrum, and do not
use or require any spectral libraries or molecular structure databases
(for the subtle "exemptions" from this rule, 
see [*Böcker & Dührkop*](https://dx.doi.org/10.1186%2Fs13321-016-0116-8)). 
Fragmentation trees are computed by combinatorial optimization; the underlying optimization
problem constitutes a Maximum Aposterior Estimator. The optimization
problem (finding a maximum colorful subtree) is NP-hard but nevertheless
solved optimally, explaining why computations sometimes require
significant running time for large molecules with rich fragmentation
spectra.

With SIRIUS 4.0, fragmentation tree computation has again been speeded
up significantly (around 36-fold to the previous version), through
intricated [algorithm engineering](https://drops.dagstuhl.de/opus/volltexte/2018/9325/pdf/LIPIcs-WABI-2018-23.pdf). 
If you think that computations should be speeded up even further, 
we ask you to cite our papers on swiftly computing fragmentation trees (
[*Dührkop et al.*](https://drops.dagstuhl.de/opus/volltexte/2018/9325/pdf/LIPIcs-WABI-2018-23.pdf);
[*White et al.*](https://doi.org/10.1007/978-3-319-21398-9_25); 
[*Rauf et al.*](https://doi.org/10.1089/cmb.2012.0083); 
[*Böcker & Rasche*](https://doi.org/10.1093/bioinformatics/btn270)), 
as this would give us an incentive to
continue our work on this topic: We stress that the current version of
SIRIUS is **many million times faster** than the initial version . In
fact, this initial version could not process more than 15 peaks in the
fragmentation spectrum, due to exploding running times *and* memory
requirements.

Modeling the fragmentation process as a tree comes with two
restrictions: Namely, "pull-ups" and "parallelograms". A **pull-up** is
a fragment which is inserted too deep into the trees. Due to our
combinatorial optimization, SIRIUS will try to generate deep trees, assuming that there are many small fragmentation 
steps instead of few larger ones. SIRIUS will, for example, prefer three consecutive 
C<sub>2</sub>H<sub>2</sub> losses to a single C<sub>6</sub>H<sub>6</sub> loss. This does not affect the quality of the
molecular formula identification; but when interpreting fragmentation trees, you should keep in mind this side effect
of the combinatorial optimization. **Parallelograms** are consecutive fragmentation processes that happen in more than
one order: For example, the precursor ion looses H<sub>2</sub>O then CO<sub>2</sub>, **but also** CO<sub>2</sub> 
then H<sub>2</sub>O. SIRIUS will always decide
for one order of such fragmentation reactions, as this is the only valid way to model the fragmentation as a tree.

We have incorporated support for experimental setups (MS<sup>E</sup>,
MS<sup>all</sup>, All Ion Fragmentation) where isotope peaks and
fragment peaks are measured together in the same spectrum. For such
experiments, SIRIUS 4 offers a combined isotope and fragmentation
pattern analysis. For DDA (Data-Dependent Acquisition) fragmentation
spectra, isotope patterns are disturbed through the mass filter,
resulting in non-trivial modifications of masses and intensities. At
present, SIRIUS does not make use of these isotope patterns, but simply
flags these peaks and ignore them in the optimization process. Support
for DDA isotope patterns will be added in an upcomming version of
SIRIUS.

## Molecular fingerprints

*Molecular fingerprints* can be used to encode the structure of a
molecule: Most commonly, these are binary vector of fixed length where
each bit describes the presence or absence of a particular, fixed
*molecular property*, usually the existence of a certain substructure.
As an example, consider PubChem CACTVS fingerprints with length 881
bits: Molecular property 121 encodes the presence of at least one
"unsaturated non-aromatic heteroatom-containing ring size 3". Most
bits are just explained via their SMARTS (SMiles ARbitrary Target
Specification) string : For example, molecular property 357 of PubChem
CACTVS encodes SMARTS string     
<span>\[#6\](&#126;\[#6\])(:c)(:n)</span>,    
corresponding to a central carbon atom connected to a second carbon atom 
via any bond, to a third aromatic carbon atom via an aromatic bond, and 
to an aromatic nitrogen atom via an aromatic bond. See
<ftp://ftp.ncbi.nlm.nih.gov/pubchem/specifications/pubchem_fingerprints.pdf>
for the full description of the CACTVS fingerprint. We ignore all
molecular properties that can be derived from the molecular formula of
the query compound (for example, bits 0 to 114 of PubChem CACTVS).

Given the molecular structure of a compound, we can deterministically
compute its molecular fingerprint: We use the [Chemistry Development Kit
(CDK)](https://cdk.github.io/) for this purpose. 
[*Heinonen et al.*](https://doi.org/10.1093/bioinformatics/bts437) 
pioneered the idea of predicting a complete molecular 
fingerprint from the fragmentation spectrum of a query
compound: Before this, only few, usually hand-selected properties
(presence or absence of certain substructures) were predicted from
fragmentation spectra, in particular for GC-MS with Electron Ionization;
see [*Curry & Rumelhart*](https://doi.org/10.1016/0898-5529(90)90053-B)  for an excellent example.

Given the fragmentation spectrum and fragmentation tree of a query
compound, CSI:FingerID predicts its molecular fingerprint using Machine
Learning (linear Support Vector Machines), see 
[*Shen et al.*](https://dx.doi.org/10.1093%2Fbioinformatics%2Fbtu275) and 
[*Dührkop et al.*](https://doi.org/10.1073/pnas.1509788112) for the technical
details. CSI:FingerID does not predict a single fingerprint type but
instead, five of them: Namely, CDK Substructure fingerprints, PubChem
CACTVS fingerprints, [Klekota-Roth fingerprints](https://doi.org/10.1093/bioinformatics/btn479), 
FP3 fingerprints, and MACCS fingerprints. In addition, CSI:FingerID predicts [ECFP2 and ECFP4](https://doi.org/10.1021/ci100050t)
fingerprints  that appear sufficiently often in the training data.
Different from other fingerprints, ECFP are not encoded via SMARTS
matching; instead, a hash function encodes the neighborhood of each atom
in the molecule. In principle, these fingerprints can encode
$2^{32} \approx 4.2 \cdot 10^9$ different substructures (molecular properties); in
practice, it is possible but very unlikely that two substructures share
the same value, due to a hash collision.

CSI:FingerID predicts only those molecular properties that showed
reasonable prediction quality in cross validation (F<sub>1</sub> at least
(0.25), see below). In total, (3,215) molecular properties are
predicted by CSI:FingerID 1.1.

[//]: # (Update the exact number of molecular properties and optionally add some explantion on the new selection strategy)

CSI:FingerID does not only predict if some molecular property is zero
(absent) or one (present); it also provides an **estimate how sure it is
about this prediction**. Mathematically speaking, we estimate the
posterior probability that the molecular property is present: Estimates
close to one indicate that CSI:FingerID is rather sure that the
molecular property is present; similarly, estimates close to zero for an
absent molecular property; whereas estimates between (0.1) and (0.9)
hint towards an unsure situation. Posterior probabilities are estimated using a method by 
[*Platt*](https://www.researchgate.net/profile/John_Platt/publication/2594015_Probabilistic_Outputs_for_Support_Vector_Machines_and_Comparisons_to_Regularized_Likelihood_Methods/links/004635154cff5262d6000000.pdf), 
so we also refer to these estimates as "Platt probabilities". **But even if CSI:FingerID is 99% sure that a molecular
property is present, this does not mean that it is indeed present!**
CSI:FingerID predicts thousands of molecular properties, and 10 out of
1000 predictions should be incorrect at this level of accuracy.
Furthermore, estimation parameters were derived from the training data,
and if your query molecule structures are very different from those in
the training data, it is rather likely that some estimates are
imprecise. In addition to Platt probabilities, we also report the
performance of each molecular property classifier in cross validation:
The F<sub>1</sub> score is the harmonic mean of precision (fraction of
retrieved instances that are relevant) and recall (fraction of relevant
instances that are retrieved). Molecular properties that have a
classifier with F<sub>1</sub> score close to one, are more trustworthy than
those with F<sub>1</sub> score close to zero; again, this has to be treated
with some care, as these measures were estimated from the training data
using cross validation.

It is important to understand that the predicted molecular fingerprint
which is returned by the CSI:FingerID web service, has *per se* no
connections to any structures in any molecular structure database. That
means that **even if the correct molecular structure is not contained in
any structure database, the predicted fingerprint is still valid**
within the prediction power of the method. For example, you can use it
to hypothesize about the structure of an "unknown unknown" not present
in any structure database. We have added a tab in the Graphical User
Interface that allows you to examine the predicted molecular
fingerprint.

## Molecular structures

By default, SIRIUS searches in a biomolecule structure
database; it can also search in the (extremely large) PubChem database. In addition, SIRIUS now offers to search in your own custom "suspect
database".

  - When searching *[PubChem](https://pubchem.ncbi.nlm.nih.gov/)*, we use a local copy of the database where
    we have precomputed all molecular fingerprints, as computing the
    fingerprints of the candidates "on the fly" is too time-consuming.
    We are sporadically updating our local copy of PubChem. You can
    lookup the date of the latest database update in the database
    dialog.

  - The *biomolecule structure database* (bioDB) is an amalgamation of several
    structure databases that contain small molecules of biological interest (metabolites and other
    compounds of biological relevance; molecules that are products of
    nature, or synthetic products with potential bioactivity; contaminants observed in experiments).
    This biomolecule structure database consists of roughly the following datbases:
    [HMDB](https://hmdb.ca/),
    [KNApSAcK](http://www.knapsackfamily.com/KNApSAcK/),
    [CHEBI](https://www.ebi.ac.uk/chebi/),
    [KEGG](https://www.genome.jp/kegg/kegg1.html),
    [HSDB](https://pubchem.ncbi.nlm.nih.gov/source/11933),
    [MaConDa](https://www.maconda.bham.ac.uk/),
    [Biocyc](https://biocyc.org/),
    [GNPS](https://gnps.ucsd.edu/ProteoSAFe/libraries.jsp),
    [YMDB](http://www.ymdb.ca/),
    [Plantcyc](https://plantcyc.org/),
    [NORMAN](https://www.norman-network.com/nds/),
    [SuperNatural](http://bioinf-applied.charite.de/supernatural_new/index.php),
    [COCONUT](https://coconut.naturalproducts.net/),
    [BloodExposome](https://bloodexposome.org/),
    [TeroMol](http://terokit.qmclab.com/),
    [LOTUS](https://lotus.naturalproducts.net/),
    [FooDB](https://foodb.ca/),
    [MiMeDB](https://mimedb.org/),
    [LIPIDMAPS](https://www.lipidmaps.org/)
    and structures from [PubChem](https://pubchem.ncbi.nlm.nih.gov/) annotated with [MeSH](https://www.nlm.nih.gov/mesh/meshhome.html) 
    terms or with one of the classes "bio and metabolites", "drug", "safety and toxic" or "food".
    The exact compositon may vary depending on the SIRIUS (backend) version.
    
## COSMIC - Confidence for Small Molecule IdentifiCations

The COSMIC confidence score ([*Hoffmann et al.*](https://doi.org/10.1038/s41587-021-01045-9)) assigns a confidence to CSI:FingerID structure annotations.
The CSI:FingerID score was developed to rank different structure candidates for a single feature. 
However, it is not well suited to rank the top-hits of different features based on their likelihood of being correct.
Thus, the COSMIC confidence score was developed for this task.
The idea is similar to False Discovery Rates and q-values: the higher the confidence, the higher the chance of the hit being correct. 
This allows high-throughput experiments: All features in a large dataset are analysed
using CSI:FingerID, the top-ranked hit for each feature will be evaluated by COSMIC and the
most trustworthy structure annotations can be selected for further analysis. COSMIC does not re-rank
structure candidates of a particular feature nor does it discard any identifications.

The confidence score is predicted using Support Vector Machines with enforced feature directionality (different SVMs are used for different lengths of the structure candidate list). The resulting score is a Platt-probability estimate and thus, is between 0 and 1.0.
However, it should not be interpreted as a probability of being correct. In evaluation, we found that a score of 0.64 corresponded to roughly a 10% FDR. However, this value can highly depend on your own data.

### Confidence score modes

There are two modes of the confidence score: _exact_ and _approximate_.
The exact mode answers the question "Is this exact molecular structure hit the true structure of my unknown compound?".
The approximate mode tells you "Is this structure hit correct or highly similar to the true structure?".
Here, we define _highly similar_ as being one simple chemical reaction away from the true structure. More theoretical, the hit and the true structure shall have a Maximum Common Edge Subgraph ([MCES](https://en.wikipedia.org/wiki/Maximum_common_edge_subgraph)) distance of 2.
Thus, for example, a bogus hit is interpreted as being "correct" if only a side group is moved compared to the true structure.

The confidence in exact mode will usually be very low if the top and 2nd best structure candidate are highly similar. This happens for many well studied molecules for which you often find multiple derivatives in the structure database.
If you consider almost-correct hits to be useful, you should opt for the approximate mode.


## Expansive search (structure database search with fallback)

SIRIUS 6 offers the possibility to perform structure database search with a confidence score based fallback (expansive search). Structure database search will be performed for the set of databases
the user selected ("requested databases"), and then additionally for "PubChem". SIRIUS will then check if the top hit in PubChem has a confidence score that is at least twice as high as the confidence 
score of the top hit from the requested databases. If that is the case, the search will be "expanded" and the results for database search in PubChem will be shown. 

## Compound classes

CANOPUS ([*Dührkop et al.*](https://doi.org/10.1038/s41587-020-0740-8)) predicts the presense/absense of more than 2500 _compound classes_. 
This covers a wide range from very general classes such as  "Lipids and lipid-like molecules" to very specific classes such as "Phosphatidylethanolamines", "Thiazolidines", or "7-alpha-hydroxysteroids".

Most of the compound classes are based on the [ClassyFire ontology](https://doi.org/10.1186/s13321-016-0174-y). 
In contrast to ClassyFire however, CANOPUS predicts these classes solely based on the MS/MS spectrum. 
It can even predict the class if no molecular structure of this class is present in the molecular structure database searched by CSI:FingerID.
It is important to note, that these compound classes do not follow the concept of attributing a compound to its biosynthetic precursor or pathway.
It categorizes similar compounds based on functional groups and common substructures. Only based on the MS/MS spectrum and without additional knowledge of the measured organism, it is not possible to assign this biochemical concept of a class - the same compound may be derived from different biosynthetic precursors.  

Additionally, CANOPUS predicts compound classes based on the categories from [NPClassifier](https://doi.org/10.1021/acs.jnatprod.1c00399). This classification system is more general, but may align better with the concept of biosynthetic pathway mapping. Note, that this is still not using taxonomic information and suggestions are solely based on the MS/MS data.  

## MSNovelist

MSNovelist ([*Stravs et al.*](https://doi.org/10.1038/s41592-022-01486-3)) generates molecular structures de novo from the MS/MS spectrum - without the need of a database.
It is ideally suited to complement structure database search in the case of poorly represented analyte classes and novel compounds. It is not meant to replace database search in general.
Structural elucidation of small molecules from MS/MS data remains a challenging task - and identifying a structure without database candidates is even more challenging. 
MSNovelist proposes structures which can serve as a great starting point for elucidation of specific unknowns. This information may be complemented with [CANOPUS](#compound-classes) compound class predictions.

Candidate structures are generated from the predicted fingerprint. Multiple candidates structures (their SMILES representation) are sampled based on an autoregressive model - generating each SMILES token by token.
After candidate generation, all candidates are ranked using the CSI:FingerID scoring. 


## Training data

The fragmentation tree computation of SIRIUS **is not trained on any
data**, since no machine learning is used for this step. The parameters
for fragmentation tree computation were estimated from two MS/MS spectra
datasets, with 2005 compounds from GNPS  and 2046 compounds from Agilent
("MassHunter Forensics/Toxicology PCDL" version B.04.01 from Agilent
Technologies Inc., Santa Clara, CA, USA). Parameters of this step were
not optimized to maximize, say, the molecular formula identification
rate, and estimates should be very robust. All spectra were recorded in
positive ion mode. Fragmentation tree computation and molecular formula
estimation appear to work very well for negative ion mode data, too; but
there is no guarantee for that.

The Machine Learning part of CSI:FingerID, namely the essemble of linear
Support Vector Machines, is trained on spectra from NIST, Massbank and GNPS
An up-to-date list of all structures that are part of the training
data of CSI:FingerID can be downloaded from the webservice:

##### Training structures for positive ion mode:
<https://www.csi-fingerid.uni-jena.de/v1.4.8/api/fingerid/trainingstructures?predictor=1>
##### Training structures for negative ion mode:
<https://www.csi-fingerid.uni-jena.de/v1.4.8/api/fingerid/trainingstructures?predictor=2>

**We would like to explicitly and emphatically thank everyone who made
their spectra publically available.** With that, you have done a huge
favor not only to us, but to everyone in the metabolomics community;
which, unfortunately, is not recognized by the community at the moment.
We sincerely hope that the metabolomics community will become aware of
the urgent need for open data and data sharing in the near future (just
like the genomics community did 25 years ago, or the proteomics
community 10 year ago); and that you will receive your well-deserved
accolades then.

We are constantly adding new training data that becomes publically
available. If you have data from reference compounds, we ask you to
upload these to a public database such as [GNPS](https://gnps.ucsd.edu/ProteoSAFe/static/gnps-splash.jsp) 
or [MassBank](https://massbank.eu/MassBank/); if this is
not possible for some reason, **you can contact us so that we can add
your data to the CSI:FingerID training data without making it publically
available**. Please help us improve the performance of CSI:FingerID by
providing additional training data!