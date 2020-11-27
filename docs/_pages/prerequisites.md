---
permalink: /prerequisites/
title: "Prerequisites"
---
## Spectral quality

SIRIUS and CSI:FingerID have been trained on a wide variety of data,
including data from different instrument types. Nevertheless, certain
aspects of the mass spectra are important so that our software can
successfully process your data:

  - Be reminded that SIRIUS requires **high mass accuracy** data: The
    mass deviation should be within 20 ppm. We are confident that SIRIUS
    can also give useful information for worse mass accuracy (say,
    50 ppm), but you should know what you are doing if you are
    processing such data.

  - It is understood that some molecules generate more fragments,
    whereas others have sparse fragmentation spectra. But it is also
    important to understand that without sufficient information, it is
    impossible to deduce the structure or even the molecular formula
    from a tandem mass spectrum that contains almost no peaks. For
    example, three peaks in a fragmentation spectrum measured with 1 ppm
    mass accuracy contain about 60 bit of information, ignoring
    dependencies between fragments and distibution of molecular masses.
    With this information, it is simply not possible to find the correct
    structure in a database such as PubChem, containing 100 million
    structures. In comparison, ten peaks measured with 20 ppm mass
    accuracy contain about 156 bit of information, again ignoring
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
    observing a fragment corresponding to an loss is in fact very
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
example : Here, the isotopologue of a molecule where each atom is the
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

There are different ways of computing the mass of an ionized molecule
such as or that will result in slightly different results: in
particular, adding the mass of a proton vs. subtracting the mass of an
electron. Following suggestions by , SIRIUS computes this mass by
*subtracting the rest mass of an electron*. To this end, the
monoisotopic mass of is the monoisotopic mass of the molecule
(95.049690 Da) minus the rest mass of an electron (0.000549 Da), which
totals as 95.049141 Da. Similarly, the monoisotopic mass of equals
117.031634 Da - 0.000549 Da = 117.031085 Da.

| element (symbol) | AN | isotope | abundance (%)                          | mass (Da)       |
| ---------------: | -: | :-----: | -------------------------------------: | --------------: |
|      hydrogen () |  1 |         |                        (99.988%) |         (1.007825) |
|                  |    |         |                         (0.012%) |         (2.014102) |
|         boron () |  5 |         |                        (19.9^*%) |        (10.012937) |
|                  |    |         |                        (80.1^*%) |        (11.009305) |
|        carbon () |  6 |         |                         (98.93%) |             (12.0) |
|                  |    |         |                          (1.07%) |        (13.003355) |
|      nitrogen () |  7 |         |                        (99.636%) |        (14.003074) |
|                  |    |         |                         (0.364%) |        (15.001090) |
|        oxygen () |  8 |         |                        (99.757%) |        (15.994915) |
|                  |    |         |                         (0.038%) |        (16.999131) |
|                  |    |         |                         (0.205%) |        (17.999160) |
|      fluorine () |  9 |         |                           (100%) |        (18.000938) |
|       silicon () | 14 |         |                        (92.223%) |        (27.976927) |
|                  |    |         |                         (4.685%) |        (28.976495) |
|                  |    |         |                         (3.092%) |        (29.973770) |
|      phosphor () | 15 |         |                           (100%) |        (30.973762) |
|        sulfur () | 16 |         |                         (94.99%) |        (31.972071) |
|                  |    |         |                          (0.75%) |        (32.971459) |
|                  |    |         |                          (4.25%) |        (33.967867) |
|                  |    |         |                          (0.01%) |        (35.967081) |
|      chlorine () | 17 |         |                         (75.76%) |        (34.968853) |
|                  |    |         |                         (24.24%) |        (36.965903) |
|      selenium () | 34 |         |                          (0.89%) |        (73.922476) |
|                  |    |         |                          (9.37%) |        (75.919214) |
|                  |    |         |                          (7.63%) |        (76.919914) |
|                  |    |         |                         (23.77%) |        (77.917309) |
|                  |    |         |                         (49.61%) |        (79.916521) |
|                  |    |         |                          (8.73%) |        (81.916699) |
|       bromine () | 35 |         |                         (50.69%) |        (78.918337) |
|                  |    |         |                         (49.31%) |        (80.916291) |
|        iodine () | 53 |         |                           (100%) |       (126.904473) |
Isotopes with masses and abundances as used by SIRIUS. In this table,
*masses have been rounded to six decimals* for the purpose of
presentation; internally, SIRIUS uses masses with higher precision. ‘AN’
is atomic number. *Isotope abundances of boron can vary strongly, so
isotope pattern analysis is of little use for identifying the correct
molecular formula in case boron is
present.

Above, masses have been rounded to six decimals; internally, SIRIUS uses
double precision for representing masses. Masses of isotopes are taken
from the AME2016 atomic mass evaluation . See
[Table]() for the isotope
masses and abundances used by SIRIUS, again rounded to six decimals for
presentation.

**We suggest to calibrate your instrument with ion masses as calculated
above.** In any case, you should *be aware of this tiny mass
difference*, as this can result in unexpected behavior when decomposing
masses; see for example .

## Mass deviations

SIRIUS assumes that mass deviations (the difference between the measured
mass and the theoretical mass of the ion) are normally distributed . The
user-defined parameter "mass accuracy" is given in parts-per-million
(ppm). SIRIUS interpretes this parameter as a "guarantee" and, hence,
assumes that **this is the maximum allowed mass deviation**; it will
**discard** all explanations that require a larger mass deviation. This
implies that **if in doubt, you should use a larger mass accuracy** to
ensure that SIRIUS can successfully annotate peaks in the spectrum. For
masses below 200 Da, we use the absolute mass deviation at 200 Da, as we
found that small masses vary according to an absolute rather than a
relative error.

## Molecular formulas

Unless instructed otherwise, SIRIUS will consider all molecular formulas
that are chemically feasible and explain the precursor mass of the
molecule/ion: For example, if your query compound is pinensin A (,
monoisotopic mass (2213.962) Da) then SIRIUS will consider all
19 746 670 candidate molecular formulas that explain this
monoisotopic mass (assuming set of elements , see below, and 10 ppm mass
accuracy). SIRIUS penalizes candidate molecular formulas that deviate
too strongly of what we assume a molecular formula of a biomolecule to
look like (for example, will receive a penalty), but this penalty is
used cautiously: Only 2.6 % of the molecular formulas of all PubChem
compounds — and, hence, only a tiny fraction of molecular formulas from
compounds not marked as biomolecules — are penalized. Molecular formulas
are never rewarded by SIRIUS.

SIRIUS uses a short list of outlier molecular formulas which would be
penalized by the above method, as they are not "biomolecule-like"; these
molecular formulas are not penalized, as they have been observed in
metabolomics experiments (for example, as solvents), but are also not
rewarded. These outlier molecular formulas will be considered as
candidates by SIRIUS even if they violate elemental constrains such as
"at most 2 fluorine".

Considering all molecular formulas implies that a set of elements has to
be provided from which these molecular formulas are generated. SIRIUS
includes methods for the auto-detection of elements from the isotope and
fragmentation pattern of the query compound .

In case you compound is large (above 600 Da) or you have incomplete
information (no isotope pattern), you can restrict SIRIUS to only
consider molecular formulas found in PubChem. Doing so, it is not
possible to ever detect molecules with a novel molecular formula,
though.

Recent evaluations (for example, as part of the CASMI contest )
indicated that one can determine molecular formulas by searching in a
structure database using tool such as MAGMa, CFM-ID or CSI:FingerID.
(Somewhat consequently, the CASMI 2016 contest did no longer have a
category for molecular formula identification.) **We strongly advice
against doing so, as this is apparently a wrong prior problem.**
Molecular formulas in a structure database are far from being uniformly
distributed: For a certain precursor mass plus mass inaccuracy, you may
find 90% molecular structures with only one molecular formulas. Assuming
that the molecular structures in our evaluation set are uniformly chosen
at random, even a method that uniformly draws a molecular structure,
then reports its molecular formula will get 81% correct identifications
for the molecular formula identification task. A method that still
ignores the mass spectrometry data beyond the monoisotopic mass, but
reports the majority vote in the structure database would even reach 90%
correct identifications. (This is comparable to an app for bird
identification that allways outputs "It is a house sparrow!" for
Britain, because the house sparrow is the most common bird in Britain.
This app will get a lot of correct identifications.) Clearly,
computational methods such as MAGMa, CFM-ID or CSI:FingerID are not
random, but they will fall for this pit, too. To this end, we advice for
the "classical chemical identification pipeline" where **molecular
formulas are identified first, then molecular structure.**

## Fragmentation trees

Fragmentation trees annotate the fragmentation spectrum with molecular
formulas, and identify likely losses between the ions in the
fragmentation spectrum. Fragmentation trees can be used both to identify
the molecular formula of a query compound, and to derive information
about its fragmentation: For example, this is used in CSI:FingerID to
predict the molecular fingerprint of the query compound. Fragmentation
trees are computed directly from the fragmentation spectrum, and do not
use or require any spectral libraries or molecular structure databases
(for the subtle "exemptions" from this rule, see ). Fragmentation trees
are computed by combinatorial optimization; the underlying optimization
problem constitutes a Maximum Aposterior Estimator. The optimization
problem (finding a maximum colorful subtree) is NP-hard but nevertheless
solved optimally, explaining why computations sometimes require
significant running time for large molecules with rich fragmentation
spectra.

With SIRIUS 4.0, fragmentation tree computation has again been speeded
up significantly (around 36-fold to the previous version), through
intricated algorithm engineering . If you think that computations should
be speeded up even further, we ask you to cite our papers on swiftly
computing fragmentation trees , as this would give us an incentive to
continue our work on this topic: We stress that the current version of
SIRIUS is **many million times faster** than the initial version . In
fact, this initial version could not process more than 15 peaks in the
fragmentation spectrum, due to exploding running times *and* memory
requirements.

Modeling the fragmentation process as a tree comes with two
restrictions: Namely, "pull-ups" and "parallelograms". A **pull-up** is
a fragment which is inserted too deep into the trees. Due to our
combinatorial optimization, SIRIUS will try to generate deep trees,
assuming that there are many small fragmentation steps instead of few
larger ones. SIRIUS will, for example, prefer three consecutive losses
to a single loss. This does not affect the quality of the molecular
formula identification; but when interpreting fragmentation trees, you
should keep in mind this side effect of the combinatorial optimization.
**Parallelograms** are consecutive fragmentation processes that happen
in more than one order: For example, the precursor ion looses then ,
*but also* then . SIRIUS will always decide for one order of such
fragmentation reactions, as this is the only valid way to model the
fragmentation as a tree.

We have incorporated support for experimental setups (MS<sup>E</sup>,
MS<sup>all</sup>, All Ion Fragmentation) where isotope peaks and
fragment peaks are measured together in the same spectrum. For such
experiments, SIRIUS 4 offers a combined isotope and fragmentation
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
"unsaturated non-aromatic heteroatom-containing ring size 3". Most
bits are just explained via their SMARTS (SMiles ARbitrary Target
Specification) string : For example, molecular property 357 of PubChem
CACTVS encodes SMARTS string `[#6](\~[#6])(:c)(:n)`, corresponding to a
central carbon atom connected to a second carbon atom via any bond, to a
third aromatic carbon atom via an aromatic bond, and to an aromatic
nitrogen atom via an aromatic bond. See
[here](ftp://ftp.ncbi.nlm.nih.gov/pubchem/specifications/pubchem_fingerprints.pdf)
for the full description of the CACTVS fingerprint. We ignore all
molecular properties that can be derived from the molecular formula of
the query compound (for example, bits 0 to 114 of PubChem CACTVS).

Given the molecular structure of a compound, we can deterministically
compute its molecular fingerprint: We use the Chemical Development Kit
CDK  for this purpose.  pioneered the idea of predicting a complete
molecular fingerprint from the fragmentation spectrum of a query
compound: Before this, only few, usually hand-selected properties
(presence or absence of certain substructures) were predicted from
fragmentation spectra, in particular for GC-MS with Electron Ionization;
see  for an excellent example.

Given the fragmentation spectrum and fragmentation tree of a query
compound, CSI:FingerID predicts its molecular fingerprint using Machine
Learning (linear Support Vector Machines), see  and  for the technical
details. CSI:FingerID does not predict a single fingerprint type but
instead, five of them: Namely, CDK Substructure fingerprints, PubChem
CACTVS fingerprints, Klekota-Roth fingerprints , FP3 fingerprints, and
MACCS fingerprints. In addition, CSI:FingerID predicts ECFP2 and ECFP4
fingerprints  that appear sufficiently often in the training data.
Different from other fingerprints, ECFP are not encoded via SMARTS
matching; instead, a hash function encodes the neighborhood of each atom
in the molecule. In principle, these fingerprints can encode
(2^{32} approx
4.2 cdot 10^9) different substructures (molecular properties); in
practice, it is possible but very unlikely that two substructures share
the same value, due to a hash collision.

CSI:FingerID predicts only those molecular properties that showed
reasonable prediction quality in cross validation (F(_1) at least
(0.25), see below). In total, (3,215) molecular properties are
predicted by CSI:FingerID 1.1.

CSI:FingerID does not only predict if some molecular property is zero
(absent) or one (present); it also provides an **estimate how sure it is
about this prediction**. Mathematically speaking, we estimate the
posterior probability that the molecular property is present: Estimates
close to one indicate that CSI:FingerID is rather sure that the
molecular property is present; similarly, estimates close to zero for an
absent molecular property; whereas estimates between (0.1) and (0.9)
hint towards an unsure situation. Posterior probabilities are estimated
using a method by , so we also refer to these estimates as "Platt
probabilities". **But even if CSI:FingerID is 99 % sure that a molecular
property is present, this does not mean that it is indeed present!**
CSI:FingerID predicts thousands of molecular properties, and 10 out of
1000 predictions should be incorrect at this level of accuracy.
Furthermore, estimation parameters were derived from the training data,
and if your query molecule structures are very different from those in
the training data, it is rather likely that some estimates are
imprecise. In addition to Platt probabilities, we also report the
performance of each molecular property classifier in cross validation:
The F(_1) score is the harmonic mean of precision (fraction of
retrieved instances that are relevant) and recall (fraction of relevant
instances that are retrieved). Molecular properties that have a
classifier with F(_1) score close to one, are more trustworthy than
those with F(_1) score close to zero; again, this has to be treated
with some care, as these measures were estimated from the training data
using cross validation.

It is important to understand that the predicted molecular fingerprint
which is returned by the CSI:FingerID web service, has *per se* no
connections to any structures in any molecular structure database. That
means that **even if the correct molecular structure is not contained in
any structure database, the predicted fingerprint is still valid**
within the prediction power of the method. For example, you can use it
to hypothesize about the structure of an "unknown unknown" not present
in any structure database. We have added a tab in the Graphical User
Interface that allows you to examine the predicted molecular
fingerprint.

## Molecular structures

By default, SIRIUS searches in either PubChem or a biomolecule structure
database; in addition, SIRIUS now offers to search in your own "suspect
database".

  - When searching *PubChem*, we use a local copy of the database where
    we have precomputed all molecular fingerprints, as computing the
    fingerprints of the candidates "on the fly" is too time-consuming.
    We are sporadically updating our local copy of PubChem. You can
    lookup the date of the latest database update in the database
    dialog.

  - The *biomolecule structure database* is an amalgamation of several
    structure databases that contain biomolecules (metabolites and other
    compounds of biological relevance; molecules that are products of
    nature, or synthetic products with potential bioactivity).
    Currently, this biomolecule structure database consists of KNApSAcK
    , HMDB , ChEBI , KEGG , HSDB , MaConDa , BioCyc , UNPD , a subset of
    biomolecules from ZINC , all structures from GNPS  and MassBank ,
    and MeSH-annotated compounds from PubChem .

## COSMIC - COnfidence for Small Molecule IdentifiCations

TODO:Coming soon... Describe the new
confidence Score!

## Compound classes

TODO:Coming soon... Infos about Compound
Class prediction

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
Support Vector Machines, is currently (CSI:FingerID version 1.1) trained
on (12,108) compounds from  and (2,073) compounds from MassBank .
In total, CSI:FingerID is trained on (14,181) compounds with
(8,210) unique structures. Again, all spectra were recorded in
positive ion mode. Different from fragmentation tree computation, we
assume that the **prediction of molecular fingerprints is challenging
for negative ion mode data**, given that we have no training data.
Hopefully, enough negative ion mode data will become available in the
near future, so that we can retrain CSI:FingerID on this data and solve
this issue for good.

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
upload these to a public database such as GNPS or MassBank; if this is
not possible for some reason, **you can contact us so that we can add
your data to the CSI:FingerID training data without making it publically
available**. Please help us improve the performance of CSI:FingerID by
providing additional training data!