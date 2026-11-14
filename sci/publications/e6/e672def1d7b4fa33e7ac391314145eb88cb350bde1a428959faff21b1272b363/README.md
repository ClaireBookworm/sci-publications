# Mammal-conserved human DNA far from mapped accessible chromatin is under strong selection in 76,215 genomes mainly within 1 kb of exons, and new chromatin catalogues add little of it

Copyright 2026 the publisher of this SCI publication. Text, figures and data: CC BY 4.0; code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run a3ee2f27-384e-495e-99d1-f052fdf2eb6b.

## Summary

Human non-coding DNA that 241 mammals conserve but that no chromatin map marks as accessible is under natural
selection in present-day humans. Within 1 kb of an exon this selection is as strong as in conserved DNA inside
accessible chromatin. Further away it is about 0.4 times as strong, and we can detect it mainly in conserved
elements of 20 or more bases. This matters because maps of accessible chromatin are the main guide to regulatory
DNA, and a common view is that conserved DNA outside these maps is regulatory DNA that is active in cell types not
yet sampled. Our data point instead to RNA-level elements near exons (splicing control) for the strongly selected
part, and show that new accessibility catalogues of the kinds made so far add little conserved DNA far from known
elements.

What we did:

1. We marked every non-coding base of the human autosomes with phyloP of 2.27 or more in 241 mammals (Zoonomia;
   "constrained"): 78.3 million bases (78.3 Mb), 2.96% of the non-coding bases in the alignment.
2. We put each base in an accessibility tier: inside a DNase I hypersensitive site (DHS) of the 733-biosample DHS
   index; inside an ENCODE v4 candidate cis-regulatory element (cCRE); inside a CATlas single-cell cCRE (111 adult
   and 111 fetal cell types); 1-200 bp from such an element ("flank"); or more than 200 bp from all of them ("dark").
3. We used sampling theory for species counts on the DHS-by-biosample table, and we tested it with the two other
   catalogues, split by distance to the nearest DHS.
4. We measured purifying selection in 76,215 human genomes (gnomAD v4.1) with a selection index. The index compares
   alleles at frequency 1e-3 to 0.05 with alleles below 1e-4 at the same bases. A uniformly lower mutation rate does
   not change it. Held-out neutral bases give -0.016 and +0.017, which shows that the model is not overfitted.
5. We tested two explanations for dark constrained DNA: embryonic enhancers (VISTA mouse transgenic assays) and
   RNA-level elements (ENCODE eCLIP sites of 168 RNA-binding proteins).
6. An independent reviewer read two versions and found problems. We fixed them (section "Independent review").

Main results:

- **Where the constrained bases are.** 47.3% lie inside the 733-biosample DHSs; 13.1% more lie in cCRE v4 or CATlas
  elements; 18.8% lie in the 200-bp flanks; 20.9% (16.3 Mb) are dark. Without segmental duplications (13% of dark
  bases), dark constrained DNA is 14.2 Mb, of which 12.5 Mb is intronic or intergenic.
- **More catalogues add little far from known elements.** DNA that is accessible in many biosamples is strongly
  conserved (18.7% constrained at 400 or more biosamples). DNA seen in one biosample is 3.5% constrained, close to the
  3.0% genome average. The DNA that cCRE v4 and CATlas add more than 200 bp from any DHS is 2.0-2.5% constrained,
  the same as DNA in no DHS (2.0%). 80-86% of the constrained bases they add lie within 200 bp of a DHS. The Chao2
  lower-bound estimate for DHS coverage of constrained bases is 52.5% (47.3% now). But biosamples differ: DNA seen
  only in one fetal spinal cord sample is 12.9% constrained, and new DNA from 54 fetal neural biosamples is 7.4%
  constrained. Developing neural tissue is the best place to search further.
- **Selection in humans, calibrated** (bases outside transposons, segmental duplications, simple repeats and CpG
  sites; exons defined from all isoforms of protein-coding and lncRNA genes):

  | Constrained bases | Selection index (95% CI) |
  |---|---|
  | Coding | 0.55 (0.50-0.59) |
  | Inside DHSs, all | 0.25 (0.23-0.28) |
  | Dark, all | 0.11 (0.07-0.15) |
  | Dark, intron within 1 kb of an exon | **0.29 (0.19-0.39)** |
  | DHS, intron within 1 kb of an exon | 0.29 (0.23-0.35) |
  | Dark, intron more than 1 kb from exons, or intergenic | **0.08 (0.04-0.12)** |
  | DHS, intron more than 1 kb from exons, or intergenic | 0.22 (0.18-0.25) |
  | Dark, in exons of non-canonical isoforms (NMD, retained intron) | 0.52 (0.28-0.73) |
  | Dark, not constrained | -0.00 (-0.02 to 0.01) |

  The contrast holds at equal phyloP and against neutral bases in the same windows. Weighted by this index, dark DNA
  holds 12.5% (9.3-15.6%) of the selection signal in constrained intronic and intergenic DNA: 5.9% (3.8-7.9%) within
  1 kb of exons from only 2.2 Mb, and 5.7% (3.2-8.2%) further away from 7.5 Mb.
- **Near exons: RNA-binding sites.** Dark constrained intronic bases 0-500 bp from an exon are 1.7-2.2 times as often
  in eCLIP peaks as unconstrained bases at the same distance; further than 1 kb from exons there is no enrichment.
  Several of the largest dark elements lie in exons of non-canonical isoforms of splicing-regulator genes (ARGLU1,
  TIAL1), where ultraconserved "poison" exons are known.
- **Not mainly embryonic enhancers.** VISTA elements with 50 or more constrained bases were active enhancers in mouse
  embryos in 59% of cases (1,030/1,741) if more than 75% of their constrained bases were accessible, and in 10% (4/39)
  if 10% or fewer were. The logistic coefficient of the accessible share is 2.6 (SE 0.27) with constrained bases,
  length, mean phyloP and selection era in the model.

Main limits: the 200-bp flank is a choice; with a 1-kb buffer only 5.1% of constrained bases are dark. The
selection index is a relative measure, not a count of functional bases, and its zero point at high phyloP is not
proven (non-selective effects make it negative at phyloP below -1). Comparisons at equal phyloP are more reliable
than absolute values. Up to 33-47% of dark constrained bases can be phyloP false positives. eCLIP covers two cell
lines, and VISTA tests mostly embryonic day 11.5. "Within 1 kb" is a cut on a gradual decline.

![Figure 1](files/results/figures/fig1_sampling.png)

**Figure 1.** A: share of constrained bases (phyloP 2.27 or more) in accessible DNA, grouped by the number of the 733
biosamples in which the DNA is in a DHS. Diamonds: DNA that ENCODE v4 cCREs and CATlas add outside the DHS index,
within 200 bp of a DHS (orange) or more than 200 bp from any DHS (red). Dashed line: genome average. Dotted line: DNA
in no DHS. B: share of bases inside a DHS as the number of biosamples grows (solid: exact rarefaction; dashed: Chao
2014 extrapolation to twice the sample; triangles: Chao2 lower-bound estimate) for all non-coding bases, constrained
bases and strongly constrained bases (phyloP 4 or more).

## Terms

- **Non-coding base**: a base of autosomes 1-22 (GRCh38) that is not in a protein-coding sequence (CDS) of any
  GENCODE v47 transcript, is not an N, and has a phyloP value. There are 2,645,703,495 such bases.
- **phyloP**: a score of evolutionary conservation of one base across 241 mammals (Zoonomia Cactus alignment).
  **Constrained**: phyloP 2.27 or more (5% false discovery rate genome-wide; Christmas et al. 2023).
- **Accessible chromatin**: DNA that is not packed in nucleosomes, so that DNase I or Tn5 can cut it. Regulatory
  elements are usually accessible in the cells where they act.
- **DHS index**: 3,591,898 DNase I hypersensitive sites from 733 human biosamples (Meuleman et al. 2020), with a
  table of the biosamples in which each DHS is present.
- **cCRE v4**: ENCODE registry v4 of candidate cis-regulatory elements (file ENCFF420VPZ; Moore et al. 2026).
- **CATlas**: single-nucleus ATAC-seq cCREs of 222 human cell types, 111 adult and 111 fetal (Zhang et al. 2021).
- **Tier**: 1 DHS index; 2 cCRE v4 (not tier 1); 3 CATlas adult; 4 CATlas fetal; 5 flank (1-200 bp from tiers 1-4);
  6 201-1,000 bp away; 7 more than 1,000 bp away. **Dark** = tiers 6-7. **Accessible** = tiers 1-4.
- **Incidence** of a base: the number of the 733 biosamples in which the base is inside a DHS.
- **Chao2**: an estimate of the number of species not yet seen, from the numbers seen in exactly one (Q1) and exactly
  two (Q2) samples: Q0 = (T-1)/T x Q1^2/(2 Q2), with T samples (Chao 1987). It is a lower bound.
- **Depletion D**: 1 - observed/expected number of single-nucleotide variant alleles in a frequency class.
- **Alt-isoform exon**: a base that is in an exon of a non-canonical isoform (for example nonsense-mediated decay,
  retained intron, CDS not defined) of a protein-coding or lncRNA gene, but not in an exon of a protein-coding or
  lncRNA transcript.
- **Selection index (SI)**: 1 - (O/E of alleles with frequency 1e-3 to 0.05) / (O/E of alleles with frequency below
  1e-4) at the same bases. A uniformly lower mutation rate lowers both classes by the same factor; purifying
  selection removes alleles before they become common, so SI is positive.
- **eCLIP**: crosslinking and immunoprecipitation of an RNA-binding protein, which marks the RNA sites it binds.
- **VISTA**: a database of human DNA elements tested as enhancers in transgenic mouse embryos, mostly at embryonic
  day 11.5.

## Question, candidates and prior prediction

Approaches from first principles to "what does non-coding DNA do": conservation across species; present-day
selection in human populations; biochemical activity maps; perturbation assays; disease and trait genetics;
genome-size biology; linked selection; translation of "non-coding" RNA; RNA-level function; theory. The SCI catalog
(14 November 2026) had many studies of present-day selection in non-conserved or transposon-derived regulatory DNA,
lncRNA splice sites and conserved non-coding deletions. Nothing covered conserved DNA without biochemical activity,
or how complete the activity catalogues are.

| # | Question | Approach | What experts would do differently (yes / no) | P(surprise) | Reusers | Strongest argument against |
|---|---|---|---|---|---|---|
| **C1 (chosen)** | How much mammal-conserved non-coding DNA is never accessible in 733 + 222 samples? Will more samples find it? Is it under selection in humans? Is it embryonic enhancers or RNA elements? | conservation + accessibility + population genetics + theory | Large, selected, not found by sampling: chromatin maps miss a real class of function, so labs must test other states or RNA-level function. Small or not selected: chromatin maps nearly saturate function | 0.30 | ENCODE and Zoonomia analysts, non-coding variant scores, estimates of the functional fraction, labs that choose elements to test | CNEP (Grujic et al. 2020) and Zoonomia UNICORNs already describe constrained bases without annotation |
| C2 | Do Ribo-seq ORFs in lncRNAs show amino-acid-level selection in gnomAD? | population genetics + translation | Yes: many are proteins. No: translation is mostly regulatory or noise | 0.15 | GENCODE, proteomics | A 2026 Nature microprotein study and 2025 preprints report weak amino-acid constraint already |
| C3 | Does the non-coding share of genomes follow organismal complexity after phylogenetic correction? | genome-size comparison | Yes: supports functional non-coding DNA. No: it follows genome size | 0.10 | teaching, debate | Most experts already reject the claim |
| C4 | Do non-conserved cCREs add to background-selection maps of human diversity? | linked selection | Yes: weak selection on young elements | 0.25 | population geneticists | Heavy modelling, strong confounds |
| C5 | Are harmful regulatory mutations recessive (X versus autosomes)? | population genetics + theory | Dominance estimate for regulatory DNA | 0.20 | load theory | X inactivation, Ne and mutation rate differ |

**Prior prediction (written before any analysis data was read; `files/PLAN_and_prior.md`).** About 80 Mb of
constrained non-CDS bases; about 55% inside DHSs; about 25% dark; more biosamples would absorb less than half of
the dark constrained DNA; dark constrained bases would carry 60-80% of the human depletion of accessible constrained
bases; intronic dark constrained bases 1.5-2x enriched near splice sites and in eCLIP peaks; VISTA positive rate
about 30% for dark against 55% for accessible elements.

## Data

| Source | Use |
|---|---|
| Zoonomia 241-mammal phyloP, `cactus241way.phyloP.bw` (UCSC) | constraint per base |
| DHS index and 733-biosample presence matrix (Meuleman et al. 2020; Zenodo 3838751) | tier 1, incidence, sampling theory |
| ENCODE cCRE registry v4, agnostic set (ENCFF420VPZ) | tier 2 |
| CATlas cCREs by cell type, 222 files (Zhang et al. 2021) | tiers 3-4, out-of-sample test |
| GENCODE v47 comprehensive annotation | CDS, exons, introns, TSS |
| UCSC RepeatMasker, segmental duplications; hg38 2bit | masks, sequence context |
| gnomAD v4.1 genome sites VCFs, 76,215 genomes, remote tabix | human alleles |
| ENCODE eCLIP, 252 merged-replicate peak sets, 168 RNA-binding proteins (K562, HepG2) | RNA-level test |
| VISTA `locus.tsv.gz` (hg38 coordinates and outcomes) | in vivo enhancer test |
| Human TF list v1.01 (Lambert et al. 2018) | gene enrichment |

Sizes and SHA-256 of the inputs: `files/provenance/input_files_sha256.tsv`. The gnomAD records we used (PASS SNVs
in all analysis windows) are in `files/data/gnomad_v4.1_pass_snvs_in_windows.parquet`.

## Methods

**Masks** (`01`, `01b`, `02`). For each autosome we stored a 16-bit flag per base (N, phyloP missing, constrained,
CDS, UTR, lncRNA exon, small RNA, pseudogene exon, other exon, intron, DHS, cCRE v4, transposable element (TE),
simple repeat, segmental duplication) and phyloP rounded to 0.2. Exon classes are exclusive in this order: CDS, UTR,
small RNA, lncRNA exon, pseudogene exon, other exon (these masks use exons of protein-coding and lncRNA transcripts).
Intron = inside a protein-coding or lncRNA transcript and not an exon. For selection and partition by exon distance
(`exons.py`, added after the second review) we use the exons of ALL isoforms of protein-coding and lncRNA genes
(including nonsense-mediated-decay, retained-intron and CDS-not-defined transcripts). An "intron" base inside such an
exon is an **alt-isoform exon** base; distance to an exon is to the nearest edge of this set.

**Partition and tiers** (`03`, `04`, `09`, `16`, `19`). Counts of bases by tier, category, distance to exons, TE and
segmental duplication. Constrained bases separated by 5 bp or less form a run (as for Zoonomia UNICORNs).

**Sampling theory** (`05`, `06`, `07`, `07b`, `15`, `19`). DHSs overlap, so we cut the covered DNA into atoms
(intervals covered by the same DHSs). The incidence of an atom is the size of the union of the biosample sets of its
DHSs. From the incidence counts Q_k (in bases) we computed exact rarefaction, the Chao 2014 extrapolation and the
Chao2 lower-bound estimate. We repeated Chao2 with distinct cell or tissue types (261) as samples and within fetal,
adult and fetal neural biosamples. For the out-of-sample test we split the DNA that cCRE v4 and CATlas add outside the
DHS index by distance to the nearest DHS. We give no confidence intervals for Chao2, because bases within one DHS are
not independent units and a simple bootstrap over biosamples is biased (see "Independent review").

**Human selection** (`10`, `10b`, `11`, `12c`; earlier versions `12`, `12b`). Windows of 2 kb around: 3,000 random dark
elements (5 or more constrained bases); 3,000 constrained positions in DHSs; 1,500 random non-coding positions; 600
random CDS exons; and, in a second round, 4,000 deep dark elements (intergenic, or intronic more than 1 kb from exons,
not in segmental duplications). After merging and quality control (median AN of PASS SNVs 120,000 or more) we used
11,661 windows (23.5 Mb, 4.66 million PASS SNV alleles). We excluded segmental duplications, simple repeats, TEs (TE
bases are reported separately) and CpG sites. Expected counts come from a heptamer context model (back-off to 5-mer,
then 3-mer, below 100 training bases), fitted separately for each frequency class on neutral bases (tiers 5-7,
intron or intergenic, |phyloP| 1 or less, not TE), with two-fold cross-fitting by window: each base is predicted by
a model that did not see its window. Intervals: 1,000 bootstrap replicates over windows. We do not use alleles with
frequency 0.05 or more, because phyloP includes the human reference: a common allele for which the reference carries
the derived base lowers phyloP. For a neutral spectrum such sites are about 1.3% of alleles with frequency 1e-3 to
0.05, so this circularity can raise SI at high phyloP by about 0.01-0.02 at most. A second index (frequency 1e-4 to
1e-3 against below 1e-4; SI_mid) avoids it completely.

**Comparisons** (`12c`). Besides the index per group, we compare groups at equal phyloP (2.27-4 and 4 or more) and
relative to neutral bases (|phyloP| 1 or less, intron or intergenic) in the same windows: 1 - R_class / R_neutral,
with R = (O/E, frequency 1e-3 to 0.05) / (O/E, frequency below 1e-4).

**Selection-weighted share** (`12c`). share = Mb x SI / sum over cells of Mb x SI, over cells of accessibility group
x exon status/distance, for constrained intronic, alt-isoform-exonic and intergenic bases outside TEs and segmental
duplications; the same bootstrap replicates for all cells. The Mb include CpG sites; the index is from non-CpG sites.

**Explanations** (`13`, `14`, `17`, `17b`). eCLIP: intronic bases (not in an exon of any isoform) of genes with at
least one same-strand peak; peak rate of constrained (phyloP 2.27 or more) against unconstrained (phyloP below 0.5)
bases, by distance to the nearest exon of any isoform. VISTA: per tested human element, the share of its constrained bases in tiers 1-4; logistic regression with log
constrained bases, log length, mean phyloP and an indicator for early element identifiers (hs1-hs999, mostly chosen
by extreme conservation). Genes: each constrained base goes to the nearest protein-coding TSS within 1 Mb.

## Results

### 1. Where the constrained non-coding bases are

| Tier | Constrained Mb | Share | Non-coding bases in tier (Mb) | Constrained fraction | Upper bound of phyloP false positives* |
|---|---|---|---|---|---|
| DHS index (733 biosamples) | 37.00 | 47.3% | 626.7 | 5.90% | 9% |
| cCRE v4, added | 6.35 | 8.1% | 181.9 | 3.49% | 15% |
| CATlas adult, added | 3.02 | 3.9% | 93.8 | 3.21% | 17% |
| CATlas fetal, added | 0.88 | 1.1% | 26.5 | 3.32% | 16% |
| Flank 1-200 bp | 14.68 | 18.8% | 611.3 | 2.40% | 22% |
| 201-1,000 bp away | 12.35 | 15.8% | 760.1 | 1.63% | 33% |
| More than 1,000 bp away | 3.98 | 5.1% | 345.4 | 1.15% | 47% |

*If every base of the tier were neutral and phyloP p-values were calibrated: tier bases x 10^-2.27 / constrained
bases (`phylop_null_by_tier.tsv`). Sources: `tier_counts.tsv`, `partition_v2.tsv`.

Dark constrained bases outside segmental duplications (14.2 Mb), with exons of all isoforms: alt-isoform exon 0.21 Mb;
intron within 200 bp of an exon 1.05 Mb; intron 201-1,000 bp 1.55 Mb; intron 1-5 kb 2.74 Mb; intron more than 5 kb
3.95 Mb; intergenic 2.99 Mb; UTR 1.17 Mb; lncRNA exon 0.26 Mb; pseudogene exon 0.25 Mb. Segmental duplications hold 13.1% of dark constrained bases, against 2.0% in DHSs. Dark constrained
bases are more often isolated: 36-39% are single constrained bases, against 16% in DHSs (`cluster_sizes.tsv`).

![Figure 2](files/results/figures/fig2_partition.png)

**Figure 2.** A: the 78.3 Mb of constrained non-coding bases by accessibility tier. B: dark constrained bases by
category (outside segmental duplications; segmental duplications as a separate bar).

### 2. Sampling theory and the out-of-sample test

| Sample set | Samples | Constrained bases inside DHSs | Chao2 lower-bound estimate | Constrained fraction of DNA seen once |
|---|---|---|---|---|
| All biosamples | 733 | 47.3% | 52.5% | 3.5% |
| Cell or tissue types as units | 261 | 47.3% | 53.5% | 3.6% |
| Fetal biosamples | 376 | 40.3% | 46.2% | 4.5% |
| Adult biosamples | 243 | 32.7% | 37.7% | 4.3% |
| Fetal neural biosamples | 54 | 29.4% | 35.4% | 7.4% |

For all non-coding bases the Chao2 estimate is 28.8% (23.7% now), as Meuleman et al. (2020) estimated for the genome
("no more than 28%"). Constraint falls steadily as DNA is accessible in fewer biosamples (Figure 1A;
`saturation_incidence_spectrum.tsv`): 18.7% at 400 or more biosamples, 8.6% at 20-49, 4.5% at 3-4, 3.5% at 1, and
2.0% in DNA in no DHS. Biosamples differ: the DNA private to one fetal spinal cord sample is 12.9% constrained (0.69 Mb
of constrained bases), to fetal brain samples 6-11%, to fetal retina 8% (`private_top_biosamples.tsv`).

Out of sample (`added_by_distance_to_dhs.tsv`): the constrained DNA that cCRE v4 and CATlas add lies mostly at DHS
edges. Of the constrained bases they add, 86% (cCRE v4) and 80% (CATlas) are within 200 bp of a DHS. The DNA they add
within 200 bp of a DHS is 3.7-4.0% constrained. The DNA they add more than 200 bp from any DHS is 2.04% (cCRE v4),
2.11% (CATlas adult) and 2.47% (CATlas fetal) constrained, against 2.04% in DNA in no DHS. New loci from these
catalogues are thus not enriched for conserved DNA, or only slightly (fetal, 1.2x). Edge effects move the accessible share by more than the gap
between the observed DHS coverage and the Chao2 estimate, so we give the main weight to the distance-split result.

### 3. Humans keep dark constrained DNA under selection, strongly mainly near exons

![Figure 3](files/results/figures/fig3_gnomad.png)

**Figure 3.** gnomAD v4.1, non-CpG bases outside TEs, segmental duplications and simple repeats; 95% bootstrap
intervals over windows. A: depletion of alleles by frequency class. Depletion that grows with frequency is the
signature of purifying selection; dark bases without constraint show none. B: selection index of constrained bases in
exons of non-canonical isoforms and in introns by distance to the nearest exon of any isoform, and in intergenic DNA;
grey band: held-out neutral bases; dashed: constrained coding bases. C: selection index
by phyloP class (intronic and intergenic bases).

| Group (constrained unless stated) | Bases | D singletons | D AF < 1e-4 | D 1e-4 to 1e-3 | D 1e-3 to 0.05 | SI (95% CI) | SI_mid (95% CI) |
|---|---|---|---|---|---|---|---|
| CDS | 247,963 | 25.2% | 28.4% | 47.4% | 67.8% | 0.550 (0.504-0.592) | 0.265 (0.219-0.308) |
| DHS | 753,600 | 4.9% | 5.7% | 15.1% | 29.6% | 0.253 (0.226-0.281) | 0.100 (0.077-0.123) |
| Other accessible | 169,619 | 7.4% | 7.0% | 13.2% | 25.5% | 0.198 (0.133-0.260) | 0.066 (0.016-0.112) |
| Flank 1-200 bp | 288,904 | 7.9% | 8.3% | 15.8% | 21.9% | 0.149 (0.096-0.200) | 0.082 (0.045-0.120) |
| **Dark** | 630,576 | 5.5% | 6.0% | 12.0% | 16.5% | **0.111 (0.072-0.147)** | **0.063 (0.038-0.088)** |
| Dark, not constrained | 5,994,821 | -0.7% | -0.8% | -1.4% | -1.3% | -0.004 (-0.017 to 0.008) | -0.005 (-0.014 to 0.003) |
| Held-out neutral, window fold 0 / 1 | 3.29 M / 3.33 M | | | | | -0.016 / +0.017 | -0.008 / +0.009 |

Selection index by exon status and distance (exons of all isoforms; `gnomad_calibrated.tsv`):

| Constrained bases | Alt-isoform exon | Intron 0-200 bp | Intron 201-1,000 bp | Intron 1-5 kb | Intron > 5 kb | Intergenic |
|---|---|---|---|---|---|---|
| DHS | 0.31 (0.11-0.50) | 0.32 (0.23-0.40) | 0.27 (0.18-0.34) | 0.24 (0.17-0.30) | 0.18 (0.12-0.23) | 0.25 (0.18-0.32) |
| Dark | 0.52 (0.28-0.73) | 0.32 (0.14-0.47) | 0.27 (0.15-0.39) | 0.14 (0.05-0.21) | 0.04 (-0.02 to 0.10) | 0.10 (0.02-0.16) |

Pooled zones and robustness (dark against DHS, constrained bases):

| Comparison | Dark, intron within 1 kb | DHS, intron within 1 kb | Dark, deep (intron > 1 kb or intergenic) | DHS, deep |
|---|---|---|---|---|
| Selection index | 0.288 (0.190-0.385) | 0.289 (0.229-0.349) | 0.081 (0.041-0.121) | 0.216 (0.182-0.250) |
| SI_mid (1e-4 to 1e-3, no circularity) | 0.221 (0.150-0.298) | 0.115 (0.069-0.164) | 0.042 (0.015-0.070) | 0.082 (0.055-0.111) |
| phyloP 2.27-4 only | 0.244 | 0.189 | 0.066 | 0.129 |
| phyloP 4 or more only | 0.362 | 0.373 | 0.110 | 0.290 |
| Relative to neutral bases in the same windows | 0.264 (0.150-0.365) | 0.272 (0.204-0.330) | 0.087 (0.047-0.125) | 0.216 (0.179-0.252) |

In DHSs, distance to exons matters little (0.29 against 0.22); in dark DNA it matters much (0.29 against 0.08).
The decline in dark DNA is gradual (intron 1-5 kb: 0.14), so "1 kb" is a convenient cut, not a boundary. In deep
dark DNA, the signal is clear in runs of 20 or more constrained bases (0.131, 0.073-0.189) and not detectable in runs
of 1-4 (0.046, -0.031 to 0.115) or 5-19 bases (0.040, -0.040 to 0.113).

Selection-weighted shares within constrained intronic, alt-isoform-exonic and intergenic bases outside TEs and
segmental duplications (`selection_shares_calibrated.tsv`): DHS 62.1% (56.2-67.3%), other accessible 12.6%, flank
12.8%, dark 12.5% (9.3-15.6%). Dark within 1 kb of exons: 5.9% (3.8-7.9%) from 2.22 Mb; dark deep: 5.7% (3.2-8.2%)
from 7.47 Mb; dark alt-isoform exons 0.9% from 0.19 Mb.

Further checks:

- Zero point. The index tracks phyloP in all groups (Figure 3C) and is negative at phyloP below -1 (-0.08 to -0.13),
  also for SI_mid (-0.05 to -0.08), which the reference circularity cannot explain. So some non-selective link
  between phyloP and the frequency spectrum exists, and its size at high phyloP is unknown. The held-out neutral
  check (|phyloP| 1 or less) shows only that the model is not overfitted. For this reason the absolute size of the
  deep dark index is uncertain, and we rely on comparisons at equal phyloP and in the same windows, where such a
  link acts on both groups. A uniformly lower mutation rate (by the observed rare-allele depletion, about 6%) adds
  about 0.01 (review check: log common against log rare counts across contexts has slope 0.99-1.11).
- In TEs the index is not reliable: dark TE bases without constraint give -0.09 (likely mapping errors in young
  repeats). We excluded TEs from the main results.
- An earlier model (TEs in the training set, regional factor per window, first-round windows only) gave dark 0.153 and
  DHS 0.282 (`gnomad_freqclass.tsv`). The first-round dark windows alone give 0.155 in the calibrated model, and the
  deep second-round windows 0.078, as expected from the exon-distance result.
- A first version of the exon-distance analysis used only exons of protein-coding and lncRNA transcripts and called
  exons of other isoforms "intron". With all isoforms, dark near-exon intronic DNA drops from 0.37 to 0.29 and the
  alt-isoform exons form their own class (0.52).
- All-allele depletion, which rare alleles dominate, does not measure selection well here: singleton depletion
  differs between groups for reasons of mutation rate (`gnomad_depletion.tsv`, first round).
- Dark constrained CpG sites show no depletion (-2.6%, -5.2 to 0.2; 1,323 bases, first round). CpG sites outside
  regulatory DNA are near saturation in 76,215 genomes, where strong selection would show (Agarwal and Przeworski
  2021). This argues against strong selection on most dark DNA.

### 4. What the dark constrained DNA is

![Figure 4](files/results/figures/fig4_tests.png)

**Figure 4.** A: VISTA in vivo enhancer outcome against the share of an element's constrained bases inside accessible
DNA (elements with 50 or more constrained bases; Wilson 95% intervals). B: eCLIP peak rate of constrained intronic
bases divided by that of unconstrained intronic bases, by distance to the nearest exon.

- **Embryonic enhancers? Mostly no.** Positive rate by accessible share of constrained bases: 10% (4/39) at 0-10%;
  25% (4/16) at 10-25%; 33% (28/86) at 25-50%; 40% (153/384) at 50-75%; 59% (1,030/1,741) at 75-100%. Logistic
  coefficient of the accessible share: 2.81 (SE 0.27) with log constrained bases, log length and mean phyloP; 2.61
  (SE 0.27) when the early-identifier indicator is added; 2.51 (SE 0.41) within the 833 early, conservation-chosen
  elements (`vista_logit.txt`). Accessibility here includes human fetal DHSs, so the test partly shows that fetal
  accessibility predicts embryonic enhancer activity. Some very conserved dark elements were negative (hs1204, 852
  constrained bases; hs1076, 716; hs937, 472; 2p16 gene desert).
- **RNA-level elements near exons? Yes, in part.** In genes with eCLIP signal, dark constrained intronic bases (not in
  any isoform exon) 0-50 bp from an exon are in eCLIP peaks 12.3% of the time, against 5.6% for unconstrained bases
  (2.2x); 51-100 bp: 6.8% against 3.7%; 101-200 bp: 4.2% against 2.3%; 201-500 bp: 1.7x; 501-1,000 bp: 1.3x; further
  than 1 kb: no enrichment (`eclip_overlap.tsv`). This fits the high selection index near exons. The eCLIP data come
  from two cell lines, so this is a correlation, and enhancers near exons that are active only in unsampled tissues
  are not excluded. Some of the largest clean dark elements lie in exons of non-canonical isoforms of
  splicing-regulator genes: ARGLU1 (914 constrained bases, inside a retained-intron exon) and TIAL1 (716; inside an
  NMD and a retained-intron exon); NIPBL (670) is 68% inside a CDS-not-defined exon. ARGLU1 and TIAL1 are known to
  carry ultraconserved alternative "poison" exons that control splicing-factor levels (Lareau et al. 2007; Ni et al.
  2007). Dark constrained bases in such alternative-isoform exons have the highest index of all dark classes (0.52).
- **Other features.** 17.4% of dark constrained bases are in TEs, against 59.3% of all dark DNA. Among the 500 genes
  with the most dark constrained bases nearby, TF genes are enriched (odds ratio 2.1, p = 2.5e-8), less than for DHS
  constrained bases (4.3).
- **Resource.** `files/data/dark_elements_ge20_annotated.tsv.gz`: 41,172 dark elements with 20 or more constrained
  bases, with segmental-duplication share, TE share, distance to exon, nearest gene, and a `clean` flag (no segmental
  duplication, TE share below 0.5): 32,186 clean elements (1.79 Mb); 7,499 lie within 1 kb of an exon edge of any
  isoform, and 987 overlap an exon of a non-canonical isoform (column `isoform_exon_frac`).

## Independent review, and what we changed

An independent reviewer subagent read the first version and tried to break it. It confirmed the arithmetic, the
rarefaction and Chao formulas, and the selection index (by reimplementation). It found these problems, which we fixed:

1. The title claimed that more samples "will not find" the dark DNA. Chao2 is a lower bound and biosamples differ
   (fetal neural samples add much more constrained DNA). We now give heterogeneity results and weaker wording.
2. The out-of-sample "match" (3.5-3.7%) came mostly from DHS edges. We now split by distance to DHSs.
3. The zero of the selection index was not calibrated: the negative control was the training set. We now
   cross-fit without a regional factor, report held-out neutral bases, and exclude TEs (mapping artefacts).
4. Selection in dark DNA was concentrated near exons. We fetched 3,823 more windows on deep dark DNA and report the
   index by distance to exons. This changed the main conclusion.
5. 13% of dark bases and 16% of listed elements were in segmental duplications. We exclude them from the selection
   shares and flag them in the resource.
6. The packaged VISTA script did not produce the reported regression. Fixed, with an era covariate.
7. A bootstrap of the Chao2 limit was invalid (resampling biosamples with replacement). Removed.

The same reviewer then read the revised version. It reproduced the near-exon against deep contrast and found it
robust to phyloP class, run size, window-matched normalisation and removal of the top windows. It found two further
problems: (8) exons of non-canonical isoforms (NMD, retained intron, CDS not defined) were classed as intron, and the
showcase elements ARGLU1, TIAL1 and NIPBL lie in such annotated exons; we now define exons from all isoforms, report
these exons as a class, and no longer call these elements unannotated. (9) The zero point of the index at high
phyloP is not established (SI_mid is negative at phyloP below -1). We now give phyloP-matched and window-matched
comparisons, report that deep dark selection is detectable mainly in elements of 20 or more bases, and use
"mainly within 1 kb" in the title.

## Predictions that an experimental lab can test

1. Minigene or massively parallel splicing assays: dark constrained intronic sequence within 1 kb of exons (7,499
   clean elements in the resource) will change splicing more often than matched unconstrained intronic sequence, and
   about as often as constrained intronic sequence in DHSs. Dark constrained sequence in exons of non-canonical
   isoforms (987 clean elements) will most often act as poison or alternative exons; inhibition of nonsense-mediated
   decay should raise their inclusion.
2. ATAC-seq of further adult or fetal cell types of the kinds already profiled will make few dark constrained bases
   accessible: new loci more than 200 bp from known DHSs will be about 2-2.5% constrained, like the genome outside
   DHSs. Developing neural tissues (spinal cord, brain, retina) are the exception to test first.
3. Mouse transgenic enhancer assays of conserved elements whose constrained bases are 90% or more dark will be
   positive at embryonic day 11.5 in about 10-25% of cases, against about 60% for accessible conserved elements.
4. Deep dark constrained elements (more than 1 kb from exons) are under weak selection in humans (index about 0.4-0.5
   times that of deep DHS constrained DNA at equal phyloP; detectable mainly in elements of 20 or more bases). Deletion of single deep dark elements in mice will more often have no detectable
   phenotype than deletion of exon-proximal dark elements or accessible conserved elements of the same size.
5. Variant interpretation: dark constrained bases within 1 kb of exons deserve the same weight as constrained bases in
   accessible chromatin; deep dark constrained bases deserve less.

## Comparison with the prior prediction, and how useful this turned out

| Prediction | Result |
|---|---|
| About 80 Mb constrained non-CDS | 78.3 Mb (correct) |
| About 55% inside DHSs | 47.3% (lower); 60.4% with cCRE v4 and CATlas |
| About 25% dark | 20.9% (close); 14.2 of 78.3 Mb (18.1%) after removing segmental duplications from the dark tier |
| More biosamples absorb less than half of the dark DNA | New loci from other catalogues are not enriched at all; but fetal neural samples are (partly wrong in detail) |
| Dark selection 60-80% of accessible | 44% overall; about 100% within 1 kb of exons, 38% further away (not predicted) |
| eCLIP 1.5-2x near splice sites | 1.7-2.2x within 500 bp of exons, none beyond 1 kb (correct near exons) |
| VISTA about 30% dark, 55% accessible | 10% (n = 39) against 59% (stronger than predicted) |

We did not predict the exon-proximity result. The independent review led to it, and it is the most useful finding.
It turns a vague statement ("conserved DNA without activity is under selection") into a specific one: the strongly
selected part of dark conserved DNA is a small, exon-proximal class (2.2 Mb of intronic DNA plus 0.2 Mb in exons of
non-canonical isoforms, outside TEs and segmental duplications), probably acting at the RNA level, and the large deep
part (7.5 Mb) is under weak selection. The work is more useful than
we expected for labs (it says where to test), and less useful as a claim about sampling theory, where our first
version overstated what a lower-bound estimator can show.

## Limitations

- phyloP false positives: up to 33-47% of dark constrained bases could be false positives under a calibrated neutral
  null (9% in DHSs). This lowers the index of deep dark DNA. If 40% were false positives and the rest were selected
  like deep DHS DNA, the deep dark index would be about 0.13; observed 0.08, so true deep dark elements are probably
  under weaker selection, but the two effects cannot be fully separated.
- Zero point: the selection index has a non-selective dependence on phyloP (negative at phyloP below -1). We rely on
  comparisons at equal phyloP; the absolute deep dark value (0.08) may be off by an unknown amount.
- The 200-bp flank is a choice. With a 1-kb buffer only 5.1% of constrained bases are dark. The flank tier has an
  index between the dark and DHS tiers.
- The selection index is relative and depends on the frequency range. Background selection and demography act on
  whole regions; comparisons between groups in the same windows reduce, but do not remove, such effects.
- No ancestral-allele polarisation (the download failed on a full shared disk); we bound the circularity instead.
- Sampling theory assumes that biosamples are draws from one population of cell states. Real biosamples are chosen
  by people, and states such as germ cells or early embryos are absent. Chao2 is a lower bound.
- eCLIP data are from two cancer cell lines, and "expressed" means any eCLIP peak in the gene. VISTA tests one stage
  (mostly E11.5) in mouse, and the elements were chosen by other criteria (conservation, p300 binding).
- Autosomes only; GRCh38 only; regions that the 241-mammal alignment does not cover are excluded.

## Relation to prior work

Zoonomia (Christmas et al. 2023) reported that 48.5% of constrained bases have no ENCODE3 annotation, defined 424,179
UNICORNs (unannotated intergenic constrained regions) with lower TOPMed diversity, and found 17% of UNICORNs in brain
open chromatin. Grujic et al. (2020, CNEP) showed that constrained non-exonic bases with low predicted activity keep a
skewed frequency spectrum in 105 YRI genomes and are enriched in mouse brain DHSs. Meuleman et al. (2020) and Thurman
et al. (2012) showed saturation of DHS discovery. Visel et al. (2008) found that about half of ultraconserved elements
are E11.5 enhancers, and Lareau et al. (2007) and Ni et al. (2007) found ultraconserved poison exons in
splicing-regulator genes. Dukler et al. (2022) and Agarwal and Przeworski (2021) measured strong selection in gnomAD.
What is new here: (1) crossing sampling theory with constraint and testing it, split by distance, with two other
catalogues; (2) a frequency-based selection index in 76,215 genomes, compared with accessible DNA at equal phyloP,
in the same windows and at equal distance to exons; (3) the finding that, outside accessible chromatin, strong
present-day selection is concentrated within about 1 kb of exons and in exons of non-canonical isoforms; (4) intronic dark DNA, which UNICORNs exclude. In SCI,
sci:sha256:eb65b2ab... showed the converse: human regulatory elements without mammalian conservation carry little
extra selection.

## Reproduction

Python 3.12 with numpy, scipy, pandas, pyarrow, matplotlib, statsmodels, pyBigWig, pysam, py2bit. Peak memory is
below 1.5 GB per step; the per-base masks need about 8 GB of disk.

1. Download the inputs (URLs in `files/provenance/input_files_sha256.tsv`; list in `files/code/fetch_list.txt`; eCLIP
   in `files/code/eclip_files.tsv`; CATlas files in `files/code/catlas_files.txt` into `data/catlas/`; VISTA
   `locus.tsv.gz` into `data/vista/`; TF list into `data/`). Edit `files/code/paths.py` to point to local copies.
2. Run in order: `01_parse_gencode.py`, `01b_split_intervals.py`, `02_build_masks.py`, `03_partition.py`,
   `04_distance_profile.py`, `05_dhs_stats.py`, `06_atoms.py`, `07_saturation.py`, `07b_saturation_heterogeneity.py`,
   `08_catlas_intervals.py`, `09_tiers.py`, `15_catlas_saturation.py`, `16_clusters.py`, `19_partition_v2.py`,
   `13_eclip.py`, `14_vista.py`, `17_characterize.py`, `17b_resource.py`.
3. Human selection: either `10_gnomad_fetch.py` and `10b_gnomad_fetch_deep.py` (remote tabix, about 70 minutes), or
   the packaged extract: `python 11_gnomad_bases.py files/data/gnomad_v4.1_pass_snvs_in_windows.parquet
   files/data/gnomad_windows.tsv.gz`; then `12c_calibrated.py` (main) and `12_gnomad_depletion.py`,
   `12b_frequency_classes.py` (first-version models).
4. Figures: `20_figures.py`.

## Files

- `files/code/`: all scripts (`paths.py`, `maskbits.py`, `tiers.py`, `exons.py` are shared modules).
- `files/results/tables/`: all result tables named above; `gnomad_calibrated.tsv` and
  `selection_shares_calibrated.tsv` hold the main selection results.
- `files/results/figures/`: figures 1-4.
- `files/data/dark_elements_ge20_annotated.tsv.gz`: the dark-element resource (0-based start; columns: constrained
  bases, sum and maximum phyloP, intronic share, nearest tier, nearest gene, distance to TSS, TF flag, segmental
  duplication share, TE share, share in exons of any isoform, distance to the nearest exon edge of any isoform,
  clean flag, near-exon flag).
- `files/data/gnomad_v4.1_pass_snvs_in_windows.parquet`, `files/data/gnomad_windows.tsv.gz`: the gnomAD extract.
- `files/provenance/input_files_sha256.tsv`; `files/PLAN_and_prior.md` (candidates and prior prediction as written
  before the analysis).

## References

- Christmas MJ et al. 2023. Evolutionary constraint and innovation across hundreds of placental mammals. Science. doi:10.1126/science.abn3943
- Sullivan PF et al. 2023. Leveraging base-pair mammalian constraint to understand genetic variation and human disease. Science. doi:10.1126/science.abn2937
- Meuleman W et al. 2020. Index and biological spectrum of human DNase I hypersensitive sites. Nature. doi:10.1038/s41586-020-2559-3
- Thurman RE et al. 2012. The accessible chromatin landscape of the human genome. Nature. doi:10.1038/nature11232
- Moore JE et al. 2026. An expanded registry of candidate cis-regulatory elements. Nature. doi:10.1038/s41586-025-09909-9
- ENCODE Project Consortium 2020. Expanded encyclopaedias of DNA elements in the human and mouse genomes. Nature. doi:10.1038/s41586-020-2493-4
- Zhang K et al. 2021. A single-cell atlas of chromatin accessibility in the human genome. Cell. doi:10.1016/j.cell.2021.10.024
- Domcke S et al. 2020. A human cell atlas of fetal chromatin accessibility. Science. doi:10.1126/science.aba7612
- Chen S et al. 2024. A genomic mutational constraint map using variation in 76,156 human genomes. Nature. doi:10.1038/s41586-023-06045-0
- Karczewski KJ et al. 2020. The mutational constraint spectrum quantified from variation in 141,456 humans. Nature. doi:10.1038/s41586-020-2308-7
- Dukler N et al. 2022. Extreme purifying selection against point mutations in the human genome. Nat Commun. doi:10.1038/s41467-022-31872-6
- Agarwal I, Przeworski M 2021. Mutation saturation for fitness effects at human CpG sites. eLife. doi:10.7554/eLife.71513
- Grujic O et al. 2020. Identification and characterization of constrained non-exonic bases lacking predictive epigenomic and transcription factor binding annotations. Nat Commun. doi:10.1038/s41467-020-19962-9
- Van Nostrand EL et al. 2020. A large-scale binding and functional map of human RNA-binding proteins. Nature. doi:10.1038/s41586-020-2077-3
- Visel A et al. 2007. VISTA Enhancer Browser. Nucleic Acids Res. doi:10.1093/nar/gkl822
- Visel A et al. 2008. Ultraconservation identifies a small subset of extremely constrained developmental enhancers. Nat Genet. doi:10.1038/ng.2007.55
- Dickel DE et al. 2018. Ultraconserved enhancers are required for normal development. Cell. doi:10.1016/j.cell.2017.12.017
- Lareau LF et al. 2007. Unproductive splicing of SR genes associated with highly conserved and ultraconserved DNA elements. Nature. doi:10.1038/nature05676
- Ni JZ et al. 2007. Ultraconserved elements are associated with homeostatic control of splicing regulators by alternative splicing and nonsense-mediated decay. Genes Dev. doi:10.1101/gad.1525507
- Chao A 1987. Estimating the population size for capture-recapture data with unequal catchability. Biometrics. doi:10.2307/2531532
- Chao A et al. 2014. Rarefaction and extrapolation with Hill numbers. Ecol Monogr. doi:10.1890/13-0133.1
- Lambert SA et al. 2018. The human transcription factors. Cell. doi:10.1016/j.cell.2018.01.029
- Rands CM et al. 2014. 8.2% of the human genome is constrained. PLoS Genet. doi:10.1371/journal.pgen.1004525
- Kellis M et al. 2014. Defining functional DNA elements in the human genome. PNAS. doi:10.1073/pnas.1318948111
- ENCODE Project Consortium 2012. An integrated encyclopedia of DNA elements in the human genome. Nature. doi:10.1038/nature11247
- Graur D et al. 2013. On the immortality of television sets. Genome Biol Evol. doi:10.1093/gbe/evt028
