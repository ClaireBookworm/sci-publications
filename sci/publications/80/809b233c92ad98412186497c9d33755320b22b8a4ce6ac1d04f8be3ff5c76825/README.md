# In 76,215 human genomes, mutations that change nucleosome-positioning signals in non-functional DNA show no detectable selection; the apparent selection comes from the mapped cells' own alleles

Copyright 2026 the author. Text, figures and data: CC BY 4.0; code: MIT (see LICENSE).
Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 9b9ae042-e7ae-4a91-adcd-33a9cf8aee99.

## Summary

Point mutations that make non-functional human DNA fit a nucleosome better or worse are not made common or kept rare
by natural selection to a degree we can detect in 76,215 genomes; the patterns that look like selection come from the
way nucleosomes were mapped (and, for nucleosome barriers, probably from how they were defined). This matters because two published studies used such
patterns to claim that selection for nucleosome positioning acts on a large part of the human genome (more than a third
in one claim), which would give a weak, conservation-invisible function to much of the DNA called "junk". It also
matters for any study that tests selection on features measured in present-day cells or sequence.

**Terms.**
- **Nucleosome**: 147 base pairs (bp) of DNA wrapped around histone proteins; the **dyad** is its centre.
- **Rotational rhythm**: inside a nucleosome, A/T is preferred where the DNA minor groove faces the histones and G/C
  about 5 bp away, so the A/T share oscillates with a period of about 10 bp around mapped dyads. **W** = A or T,
  **S** = G or C. **W-preferred** and **S-preferred offsets** are the offsets where A/T or G/C is enriched.
- **W→S / S→W**: a derived (new) allele that replaces A/T by G/C, or G/C by A/T. A W→S allele at a W-preferred offset
  makes the DNA fit the nucleosome less well; at an S-preferred offset it makes the fit better.
- **dAF**: derived allele frequency in gnomAD, with the ancestral base from chimpanzee, gorilla and orangutan.
- **Selection index (SI)**: [ln odds ratio, W-preferred versus S-preferred offsets, of a variant being in a given
  frequency class rather than rare] for W→S alleles minus the same for S→W alleles. If selection keeps the rhythm,
  SI < 0. GC-biased gene conversion and base composition cancel in this contrast; the rare-variant baseline removes
  mutation-rate differences.
- **Map donors**: the seven Yoruba people whose cell lines gave the nucleosome map (Gaffney et al. 2012); six are
  genotyped in 1000 Genomes.
- **Map conditioning**: the donors' own alleles help decide where nucleosomes are called, so alleles that the donors
  carry look "favourable" for positioning without any selection.
- **Reference conditioning**: a feature defined on the present-day reference sequence (here GC-poor barriers) is
  found preferentially where past substitutions created it, so these substitutions look "favoured".

**Main results** (1.87 Gb of non-functional autosomal DNA; 6.36 million nucleosome dyads; 108.5 million polarised
single-nucleotide variants within 62 bp of a dyad; gnomAD v4.1; ± = 95% interval, jackknife over chromosomes):

| Test | Result |
|---|---|
| A/T rhythm around dyads | amplitude 2.2 percentage points, period about 10.2 bp |
| All variants, SI for dAF ≥ 0.5% / ≥ 5% / > 50% | **-0.035 ± 0.008** / -0.067 ± 0.011 / -0.256 ± 0.016 |
| Human-lineage fixed substitutions (sites without a gnomAD variant), same contrast | **-0.386 ± 0.013** |
| Rare variants (mutation), same contrast of density | -0.003 ± 0.001 |
| Pooled map, dAF 0.1-50%: alleles carried / not carried by the donors | **-0.071 ± 0.011 / +0.001 ± 0.006** |
| Map swap (two maps from disjoint donor groups), alleles carried by one group only: own map / other map | **-0.068 ± 0.011 / +0.002 ± 0.014** |
| Own-map effect D_own (selection predicts 0) | **-0.070 ± 0.020** |
| Substitutions scored on the human map with identical definitions (all sites): human lineage / chimpanzee lineage | **-0.326 ± 0.008 / -0.004 ± 0.005** |
| SI expected on the other map if selection had made the substitution contrast | -0.042 (observed +0.002 ± 0.014; z = 6.0) |
| Selection coefficient per rotational base change (heterozygote, at offsets with preference x = 1, a typical peak of the rhythm), other-map test | σ = -0.3e-6, 95% interval -2.4e-6 to 1.8e-6 (4·Ne·σ < 0.1 for Ne = 10,000); the substitution contrast would need σ_div = 6.0e-6 |
| GC-poor barriers (a GC-rule proxy of NIEBs): published statistic ln S, W→S minus S→W, barrier cores | **-0.082 ± 0.013 if defined on hg38; +0.012 ± 0.014 if defined on the ancestral sequence** |
| Same, barrier flanks | +0.029 ± 0.007 (hg38) / -0.012 ± 0.007 (ancestral) |
| Barrier cores, allele frequencies (dAF ≥ 0.5%), W→S minus S→W | +0.003 ± 0.010 (hg38) / +0.017 ± 0.010 (ancestral); low power (selection matching the hg38 statistic predicts about -0.009) |
| Share of the rhythm that the present mutation spectrum makes at equilibrium | 2% (slope 0.022 ± 0.008) |

- **Apparent selection.** Over all variants, common derived alleles look favoured where they improve the nucleosome
  fit; the effect grows with frequency and is largest for fixed substitutions, as weak selection would predict.
- **Its source.** We built two maps from two disjoint groups of three donors. An allele carried by one group looks
  favoured only in that group's map (SI -0.068) and not in the other map (+0.002). Selection cannot depend on whose
  cells were mapped, so the apparent selection in polymorphism is map conditioning. Alleles carried by both groups look
  favoured in both maps (-0.116). For substitutions, the same contrast is strong on the human lineage
  (-0.326) but absent on the chimpanzee lineage (-0.004), whose derived bases the human donors do not carry; selection
  shared by the two species would make them alike. This agrees with Warnecke et al. (2013), who explained
  nucleosome-linked substitution patterns by nucleosomes moving to fit the present sequence.
- **Barrier claim.** With a GC-rule proxy for the barriers of Drillon et al. (2016), their statistic is reproduced
  when barriers are defined on hg38 and disappears when the same rule is applied to the ancestral sequence. This
  points to reference conditioning, but we did not use their physical model, the statistic is also non-zero for
  null mutation classes, and the allele-frequency test has little power here.
- **Mutation.** The mutation rate changes little with rotational offset, and at equilibrium the present mutation
  spectrum would make only 2% of the rhythm. Because nucleosomes are called where the present sequence fits them,
  the rhythm around called dyads is expected without any maintaining process; this comparison therefore says little
  about maintenance.
- **What is not excluded.** The bound is an average over all sites near called dyads; strong selection on a small
  subset of sites is not excluded. One subset, alleles carried by neither donor group at dAF 1-50% on the group
  maps, shows SI -0.015 ± 0.010 (σ ≈ 3.9e-6); the same alleles on the deeper pooled map
  give -0.003 ± 0.009, and a σ of that size would give -0.028 on the other map,
  against +0.002 ± 0.014 observed. We cannot explain this residual.

Prior prediction (`files/notes/00_question_choice.md`, written before analysis): no rotational selection
(P = 0.8); a barrier signal that follows gBGC (P = 0.3); mutation explains at least half of the rhythm (P = 0.6). The
first held. The barrier signal was real but, with our GC proxy, depended on how barriers were defined, not on gBGC. The mutation prediction was
wrong. The map-conditioning artefact was not predicted (we had missed that Warnecke et al. 2013 proposed it for
substitutions); it was found at an interim look, and the carrier split, the map-swap test and the lineage test were
added afterwards with written predictions (`files/notes/01_run_log.md`, steps 12-17).

![Figure 1](files/figures/fig1_rotational.png)

**Figure 1.** (A) A/T share of ancestral bases around 6.36 million dyads of the pooled map; blue and orange points
are the W-preferred and S-preferred offsets compared; grey bands mark MNase fragment-end artefacts that we exclude.
(B) Contrast between W-preferred and S-preferred offsets for each mutation direction and outcome (all variants).
(C) Pooled map: selection index by frequency for alleles carried (orange) or not carried (blue) by the donors.
(D) Map-swap test with two maps from disjoint donor groups. Black bars in C and D: index expected under the
selection strength that would explain the substitution contrast (two-population simulation). Panel B includes
human- and chimpanzee-lineage substitutions scored with the same definitions.

![Figure 2](files/figures/fig2_barriers.png)

**Figure 2.** GC-poor barrier segments. (A) The divergence-to-polymorphism statistic of Drillon et al. (2016), zone
against GC-matched background, for barriers defined on hg38 (circles) and on the ancestral sequence (squares).
(B) Fraction of variants with dAF ≥ 0.5% in the same zones.

![Figure 3](files/figures/fig3_equilibrium.png)

**Figure 3.** (A) Periodic component of the observed A/T share around dyads and of the A/T share at mutation-only
equilibrium predicted from rare variants and from human-lineage substitutions. (B) Predicted over observed rhythm
amplitude.

## 1. Question and choice of study

### 1.1 Approaches listed before the literature search
(1) comparative conservation; (2) human population genetics (variant density and allele-frequency spectra);
(3) mutation-process modelling; (4) biochemical activity maps, including nucleosome maps; (5) reporter assays;
(6) native-locus perturbation and natural knockouts; (7) trait association; (8) population-genetic theory (load,
drift barrier, gBGC); (9) genome-size comparisons; (10) sequence-to-function models; (11) "bulk" structural functions
(nucleosome positioning, 3D folding, spacing).

About 25 SCI publications already test human regulatory DNA with gnomAD constraint, eQTL and GWAS enrichment,
transposon copies, phyloP calibration, de novo mutations, deletions, RNA structure, small ORFs, lncRNA splice sites
and intron word strand asymmetry. None tests the bulk structural claims of approach 11.

### 1.2 Candidate questions (full text: `files/notes/00_question_choice.md`)

| # | Candidate (approaches) | If yes / if no | P(surprise) | Who reuses it | Strongest argument against |
|---|---|---|---|---|---|
| **A (chosen)** | Do mutations that weaken or strengthen sequence-encoded nucleosome positioning in non-functional DNA have allele-frequency spectra that show selection? Rotational scale (gBGC-free contrast) and translational scale (GC-poor barriers; Drillon et al. 2016 claim 37.5% of the genome) (2+3+4+8+11) | Yes: a large, conservation-invisible fraction of "junk" has a weak structural function. No: the nucleosome-linked patterns are mutational or artefacts, and published selection claims fail | 0.25 | junk-DNA debate, mutation-rate modellers, chromatin biologists, constraint-score builders | selection per base should be far below 1/Ne in humans, so a null is expected |
| B | Is the ~10-bp WW rhythm at equilibrium with the present mutation spectrum? (3+4+8) | mutation explains the rhythm / something else keeps it | 0.30 | as A | can be part of A (it was: Section 5.3) |
| C | Does non-coding DNA scale with organismal complexity after phylogenetic control (Taft, Pheasant & Mattick 2007)? (1+9) | complexity needs non-coding DNA / genome-size noise | 0.10 | textbooks | most experts already reject it; cell-type counts are poor |
| D | Is selection on ultraconserved elements as strong as 100% identity over 300 My needs? (1+2+8) | ultraselection / weak selection suffices | 0.15 | UCE biologists | Dukler et al. 2022 measured strong-selection classes |
| E | Is non-functional human DNA less active than random DNA in one reporter library? (5+4) | genome kept quiet / spurious activity common | 0.25 | regulatory genomics | few libraries have both kinds of sequence |

We chose A (with B as its mutation arm) by expected value: it tests the two published claims that would give weak
function to the largest share of "junk", and either answer changes how nucleosome-linked sequence patterns are used
(selection or not).

## 2. Related work and what is new

- **Selection claims.** Prendergast & Semple (2011, Genome Res 21:1777) compared human-lineage divergence with
  polymorphism (10 genomes; SNPs with minor allele frequency > 15%) around CD4+ T-cell dyads and attributed about
  10.5% of A→C changes to selection; gBGC was dismissed by argument. Drillon et al. (2016, BMC Genomics 17:526)
  predicted 1.58 million nucleosome-inhibiting energy barriers (NIEBs) from sequence (on hg18) and read a ratio of
  divergence to 1000 Genomes polymorphism (MAF > 1%) as positive selection for S→W and negative selection for W→S in
  barriers, and the reverse in flanks, over 37.5% of the genome. Langley, Karpen & Langley (2014, PLoS Genet) found
  preferred alleles at higher frequency at Drosophila nucleosomes (Ne about 1e6).
- **Repositioning and ascertainment (closest prior work).** Warnecke et al. (2013, PLoS Comput Biol 9:e1003373)
  found the same nucleosome-linked substitution patterns in humans and an archaeon and argued that nucleosomes
  reposition in response to substitutions, so that patterns in divergence need not reflect selection; this is map
  conditioning for substitutions, and it already questioned Prendergast & Semple (2011). Langley et al. (2014)
  raised the donor problem in flies and controlled for it by using fragments where the mapped genome carries the
  ancestral allele. Defining sites on one present-day sequence biases tests of selection on regulatory sequence
  (He et al. 2011, PLoS Genet; Jiang & Zhang 2024, Mol Biol Evol). Gaffney et al. (2012) showed that donor
  genotypes affect positioning.
- **Mutation explanations.** Chen et al. (2012, Science) and Tolstorukov et al. (2011) on nucleosomes and mutation;
  Pich et al. (2018, Cell) showed ~10-bp periodic somatic and germline mutation rates that follow minor-groove
  orientation; Li & Luscombe (2020, Nat Commun) related de novo mutation rates to positioning stability.
- **Nucleosome map.** Gaffney et al. (2012, PLoS Genet): paired-end MNase-seq of seven Yoruba lymphoblastoid lines;
  sequence explains part of positioning (Segal et al. 2006; Kaplan et al. 2009; Struhl & Segal 2013).
- **SCI work.** sci:sha256:eac6ec26… (strand asymmetry of intron words) showed that strand-specific mutation and
  recurrence mimic selection in singleton frequencies; we reuse its gnomAD streaming code and its simulation design.
  sci:sha256:74358334… found no selection on new transcription-factor motif matches in non-functional DNA.
  sci:sha256:5f2b4de8… showed that phyloP is miscalibrated in neutral DNA.
- **New here.** The concepts are not new; what is new is (i) an allele-frequency test of selection on rotational
  positioning in humans at scale (108.5 million polarised variants, with a contrast that removes gBGC, composition
  and mutation-rate differences); (ii) a donor-level map swap that separates map conditioning from selection in
  population data without relying on a demographic model; (iii) a replication of the lineage asymmetry with
  three-ape parsimony calls; (iv) a calibrated bound on selection per rotational base change; (v) applying
  the ancestral-definition fix to the barrier claim of Drillon et al. (2016), for which we found no published
  critique.

## 3. Data (all public)

| Data | Source | Use |
|---|---|---|
| gnomAD v4.1 genome sites VCFs, chr1-22 (76,215 genomes) | `https://storage.googleapis.com/gcp-public-data--gnomad/release/4.1/vcf/genomes/` (streamed, not stored; 539.5 GB; byte counts equal object sizes) | AC, AN, AC/AN in African-ancestry samples, FILTER, lcr/segdup |
| 147-bp MNase-seq fragment centres, seven LCLs pooled (Gaffney et al. 2012) | EPD MGA, `https://epd.expasy.org/mga/hg19/gaffney12/allSamples147.sga.gz` | dyads |
| 1000 Genomes phase 3 genotypes (GRCh37) | AWS mirror `https://1000genomes.s3.amazonaws.com/release/20130502/` (streamed twice, 17.4 GB each: summed and per-sample counts) | genotypes of six of the seven map donors (NA18507, NA18508, NA18516, NA18522, NA19238, NA19239; NA19193 is not in phase 3) |
| Per-donor 147-bp MNase fragment centres (GSM907783, -84, -86, -88, -90, -91) | EPD MGA, same directory | two donor-group maps for the swap test (downloaded with curl because the shared fetch store was paused by its disk quota; sizes equal the server content-lengths; SHA-256 in provenance) |
| hg38 2bit; RepeatMasker; GENCODE v46 basic (UCSC table); phastConsElements100way; genomicSuperDups; hg19ToHg38 chain | UCSC | sequence and masks |
| hg38 net.axt alignments to panTro6, gorGor6, ponAbe3 | UCSC | ancestral states by parsimony |
| ENCODE SCREEN cCREs V3 (GRCh38) | `https://downloads.wenglab.org/V3/GRCh38-cCREs.bed` | mask |
| gnomAD LCR intervals | gnomAD resources bucket | mask |

Sizes and SHA-256: `files/provenance/input_files_sha256.tsv`; stream records: `files/provenance/gnomad_stream_log.tsv`
and `files/provenance/mapgt_stream_log.tsv`. The Ensembl EPO ancestral sequence was the first choice but its server
delivered < 0.1 MB/s; we used parsimony instead (Section 4.2).

## 4. Methods

**4.1 Non-functional DNA** (`build_mask.py`): autosomes; removed GENCODE v46 basic exons ± 100 bp, cCREs ± 50 bp,
phastCons100way elements ± 10 bp, simple/low-complexity/satellite/RNA repeats, gnomAD LCRs and segmental
duplications; 1.875 of 2.875 Gb kept (0.81 Gb no repeat, 0.24 Alu, 0.41 L1, 0.42 other).

**4.2 Ancestral states** (`axt_outgroup.py`, `ancestral_parsimony.py`): human-chimpanzee ancestor by parsimony with
gorilla and orangutan (rules in the script); high-confidence calls only (2.61 Gb; 0.57% differ from hg38).

**4.3 Dyads** (`lift_mnase147.py`, `call_dyads.py`): 129.5 million fragment centres lifted to hg38; s5 = fragments
centred within ± 2 bp; candidates with s5 ≥ 3, greedy selection by s5 with 147-bp exclusion: 6,362,421 dyads
(3.17 M with s5 ≥ 5, 1.03 M with s5 ≥ 10). A K562 single-end dyad set (Zenodo 3820875) showed no 10-bp rhythm and
was not used.

**4.4 Variants** (`vcfn.c`, `stream_chrom.sh`): PASS biallelic SNVs, AN ≥ 130,000, no lcr/segdup flag, in the mask:
377.1 million. Polarised: the high-confidence ancestral base must be REF or ALT; dAF from AC/AN. Mutation types:
96 strand-collapsed trinucleotide classes; CpG types excluded from the tests.

**4.5 Rotational test** (`rot_aggregate.py`, `analyze_rot.py`): offset of each site from its nearest dyad
(-90..+90). Rotational preference x(o) = A/T share of ancestral no-repeat bases minus an 11-bp moving average, scaled
to amplitude 1; W-preferred offsets x ≥ 0.4 (39 offsets), S-preferred x ≤ -0.4 (42 offsets), only |o| ≤ 62 (MNase
fragment-end biases distort offsets -71..-68 and +72..+75; Figure 1A). **This window was chosen after the first full
results**, which used |o| ≤ 70 and so included the fragment-end artefacts; with that window the substitution
contrast was -1.20 and the carried-allele index -0.21 (run log step 14). The pooled dyads lie about 2 bp from the
fragment midpoint (x(o) is symmetric about +2), which explains the asymmetric artefact windows; it does not
affect the tests because the offset classes are taken from the data. Mantel-Haenszel (MH) odds ratios for each
outcome and MH rate ratios for densities, stratified by type × repeat class × positioning class (s5 3-4, 5-9,
10-19, ≥ 20) × 10-bp |offset| bin. SE by delete-one-chromosome jackknife (22 blocks).

**4.6 Carrier split (added after an interim look; `vcfgt.c`, `stream_gt.sh`, `lift_gt.py`, `rot_carrier.py`,
`analyze_carrier.py`).** For each gnomAD variant, the number of derived alleles among the 12 haplotypes of the six
genotyped donors (1000 Genomes phase 3, lifted to hg38, alleles checked; NA19193 is not genotyped). Within each dAF
class (0.1-0.5%, 0.5-1%, 1-5%, 5-50%, > 50%), MH odds of being in (class, carrier status) rather than rare (≤ 10
copies) at W-preferred versus S-preferred offsets. NA19193 contributes reads to the pooled map but has no genotypes,
so some "not carried" alleles may be carried by it (this can only shrink the carried/not-carried difference).
The "rare in African samples" control was defined after the interim look (the first version, absence from all
African-ancestry samples, was empty because of admixture).

**4.7 Map-swap test (added after the carrier split; `stream_gt2.sh`, `vcfgt2.c`, `lift_gt2.py`, `call_dyads.py`,
`map_compare.py`, `rot_swap.py`, `analyze_swap.py`).** Per-donor 147-bp fragment files (EPD) were combined into two
maps from disjoint donor groups of equal depth: A = NA18507, NA18508, NA18522 (59.2 M fragments) and B = NA18516,
NA19238, NA19239 (58.5 M); NA19193 was left out. Dyads: s5 ≥ 2 (6.23 M and 6.29 M). Both maps show the same rhythm
(amplitude 2.1 points each; profile r = 0.997); 16.5% of A dyads coincide with a B dyad and 24% lie within 10 bp,
mostly because these maps are shallow. The per-donor EPD files place fragment centres 1 bp from the pooled file,
so each group map uses the rotational preference x(o) of its own A/T profile (Δx 1.65 and 1.64); a first version
used the pooled x(o), which attenuated all swap statistics by about 20% (found by the independent review). Each
group map's x(o) is scaled to its own A/T amplitude (2.10 and 2.12 points, hg38 bases), so σ from the group maps is
in units about 4% different from the pooled-map σ_div; this is negligible next to the other uncertainties. Per-sample genotypes gave, for each variant, the carrier status: one
group only, both, or neither. SI as in 4.5 (strata type × repeat class × 10-bp |offset| bin). Own-map effect D_own = mean over the two groups of [SI on the carrying group's map
minus SI on the other map]. Selection predicts D_own = 0; map conditioning predicts D_own < 0.

**4.7b Lineage test (added after the review; `rot_lineage.py`, `analyze_lineage.py`).** Following Warnecke et al.
(2013): human-lineage substitutions (chimpanzee = gorilla ≠ human, orangutan ≠ human) and chimpanzee-lineage
substitutions (human = gorilla ≠ chimpanzee, orangutan ≠ chimpanzee) per ancestral site, W-preferred versus
S-preferred offsets of the pooled map, MH rate ratios as in 4.5. The human donors carry every human-lineage
substitution but none of the chimpanzee-lineage ones, so map conditioning predicts a contrast only on the human
lineage; selection shared by the two species predicts it on both.

**4.8 Calibration** (`sim_sfs.py`, `calibrate.py`, `sim_two_pop.py`, `calibrate2.py`, `summarize.py`,
`final_bounds.py`): forward Wright-Fisher simulations with additive selection (Poisson random field). A
one-population model (Tennessen et al. 2012 European branch; 12 seeds) for all variants; a two-population model
(Tennessen et al. 2012 African and European branches with migration; a sample of 41,000 African and 111,000
non-African chromosomes; 12 donor haplotypes from the African population, split into two groups of 6) for the carrier
classes (12 + 12 seeds), because carrier status is tied to allele age: alleles carried by Yoruba donors are older and
feel weak selection more than non-carried alleles of the same frequency. The model reproduces the observed share of
carried alleles per frequency class (simulated 6.6%, 19.6%, 43.5% and 69.4% at dAF 0.1-0.5%, 0.5-1%, 1-5% and 5-50%;
observed 6.5%, 20.3%, 42.5% and 72.3%) and the share of carried alleles that only one donor group carries (0.61
simulated, 0.575 observed). Model of
selection: a mutation at offset x has s_het = σx (W→S) or -σx (S→W); SI = 2kσΔx, with k the simulated slope of
ln[n(class)/n(rare)] on s and Δx = 1.61 the difference of mean x between the compared offsets. σ_div is the σ that would
make the observed substitution contrast (fixation probabilities, Ne = 10,000): 6.0e-6 (4.1e-6 with Ne = 14,474,
the African-branch size of the same model). Minor approximations: gnomAD AN is stored as AN/4, so dAF for alleles
whose derived base is the reference base can be off by up to 2 copies; the simulated rare class is dAF < 0.1% while
the observed baseline is ≤ 10 copies; the jackknife uses chromosomes of unequal size (MH and jackknife SEs agree).

**4.9 Barrier test** (`barrier_aggregate.py`, `analyze_barrier.py`): barrier centre = 147-bp window with GC ≤ 0.30
and GC ≥ 0.40 in the two flanking 300-bp segments; core = within 36 bp of a centre; flank = 80-250 bp from a centre.
Defined twice: on hg38, and on the ancestral sequence (hg38 with high-confidence ancestral bases put back). The
Drillon-style statistic ln S = ln RR(substitutions, zone vs GC-matched background) − ln RR(polymorphisms with
dAF ≥ 1%, same); allele-frequency test as in 4.5. Strata: type × GC bin × repeat class.

**4.10 Equilibrium** (`analyze_equilibrium.py`): context-standardised rates of W→S and S→W rare variants (and
substitutions) by offset; π(o) = u_SW/(u_SW + u_WS); periodic components (minus 11-bp moving average) regressed on the
observed one over |o| ≤ 62.

## 5. Results

### 5.1 Rotational positioning: an apparent selection signal that belongs to the map donors

All variants together (Figure 1B) show the pattern expected from weak selection: at W-preferred offsets, W→S derived
alleles are less often common and S→W alleles more often common (dAF ≥ 0.5%: ln OR -0.016 ± 0.005 and +0.019 ± 0.006;
SI -0.035 ± 0.008). The contrast grows with frequency (dAF ≥ 0.1%: -0.021; ≥ 5%: -0.067; > 50%: -0.256) and is
largest for human-lineage substitutions (-0.386 ± 0.013). The null classes A↔T and C↔G show no contrast. Rare-variant density differs by only
-0.003 ± 0.001, so mutation and recurrence do not make the signal (the singleton index, +0.004 ± 0.002, is the
complement of the common-variant index, not an independent control). The SI is similar in every repeat class (no repeat -0.036, Alu -0.029, L1 -0.035, other -0.036) and
positioning class (`files/results/rot_selection_index.tsv`).

**Pooled map, split by the donors' genotypes** (Figure 1C, `files/results/carrier_selection_index.tsv`):

| dAF class | SI, carried by donors | SI, not carried | n W↔S variants not carried |
|---|---|---|---|
| 0.1-0.5% | -0.049 ± 0.037 | +0.001 ± 0.007 | 1.79 M |
| 0.5-1% | -0.046 ± 0.031 | +0.008 ± 0.017 | 0.50 M |
| 1-5% | -0.046 ± 0.019 | -0.007 ± 0.012 | 0.55 M |
| 5-50% | -0.094 ± 0.014 | +0.005 ± 0.020 | 0.27 M |
| 0.1-50% pooled | -0.071 ± 0.011 | +0.001 ± 0.006 | 3.11 M |

The signal is in carried alleles. Variants whose derived allele is rare in African-ancestry samples (< 0.1%, so almost
never in the Yoruba donors) also show none (non-African dAF ≥ 0.1%: +0.009 ± 0.012). But this split alone does not
separate the two explanations well: in the two-population simulation, carried alleles are older and so several times more
sensitive to weak selection than non-carried alleles of the same frequency (pooled slopes -3,486 and -485 per unit s).
Under σ_div the expected SI is -0.067 for carried alleles (observed -0.071) and -0.009 for non-carried alleles
(observed +0.001 ± 0.006; z = 3.7).

**Map-swap test** (Figure 1D, `files/results/swap_selection_index.tsv`; dAF 0.1-50%):

| Alleles | SI on map A | SI on map B |
|---|---|---|
| carried by group A only (0.27 M) | **-0.072 ± 0.016** | -0.004 ± 0.017 |
| carried by group B only (0.27 M) | +0.008 ± 0.020 | **-0.064 ± 0.015** |
| carried by both groups (0.40 M) | -0.115 ± 0.016 | -0.117 ± 0.017 |
| carried by neither group (2.15 M) | +0.001 ± 0.007 | -0.005 ± 0.009 |

An allele looks favoured only on the map made from cells that carry it: own-map mean -0.068 ± 0.011,
other-map mean +0.002 ± 0.014, D_own = -0.070 ± 0.020 (z = -6.9). The same holds for dAF
0.1-1% (D_own -0.082 ± 0.035) and 1-50% (-0.065 ± 0.023). Selection cannot produce this. On the
other map these alleles cannot have influenced the nucleosome calls, so the other-map SI measures selection on
old, donor-carried alleles: +0.002 ± 0.014 (dAF 0.1-50%) and -0.000 ± 0.018 (dAF 1-50%), where
the selection that would make the substitution contrast (σ_div = 6.0e-6) predicts -0.042 and
-0.060 (z = 6.0 and 6.5). This gives σ = -0.3e-6 (95% interval -2.4e-6 to
1.8e-6). With Ne = 14,474 instead of 10,000, σ_div is 4.1e-6 and the expected other-map index is about
-0.029, still rejected (z about 4.2).

**Lineage test** (`files/results/lineage_contrast.tsv`). With identical definitions, human-lineage substitutions show
SI -0.326 ± 0.008 (W→S -0.151, S→W +0.176; 2.26 M substitutions) and
chimpanzee-lineage substitutions -0.004 ± 0.005 (W→S +0.000, S→W +0.004; 2.33 M), the same
as the mutation-only contrast of rare variants (-0.003). Selection shared by both species would give similar values
on both lineages; the asymmetry is what map conditioning predicts, and it replicates Warnecke et al. (2013) with
three-ape parsimony calls. The substitution contrast is therefore not evidence of selection. Notes: this human-lineage
set (all sites, including about 1.9 M that carry a gnomAD variant) differs from the fixed-difference set used for
σ_div (-0.386, variant sites excluded); the chimpanzee null classes are not zero (A↔T -0.042 ± 0.011, C↔G +0.020 ± 0.011),
which points to small offset-dependent mutation or alignment effects on that lineage, tiny next to -0.33; and the
chimpanzee calls come from one panTro6 genome without a polymorphism filter, which dilutes them slightly.

All tests side by side (`files/results/selection_bounds_all.tsv`):

| Test | Alleles | SI observed (95%) | SI expected under σ_div | z | σ, 95% interval |
|---|---|---|---|---|---|
| Pooled map | not carried by donors, dAF 0.1-50% | +0.001 ± 0.006 | -0.009 | +3.7 | -0.9e-6 (-4.5e-6 to 2.7e-6) |
| Pooled map | not carried by donors, dAF 1-50% | -0.003 ± 0.009 | -0.022 | +4.4 | 0.8e-6 (-1.5e-6 to 3.1e-6) |
| Pooled map | carried by donors, dAF 0.1-50% (map-conditioned) | -0.071 ± 0.011 | -0.067 | -0.7 | 6.3e-6 (5.3e-6 to 7.3e-6) |
| Group maps, own map | one group only, dAF 0.1-50% (map-conditioned) | -0.068 ± 0.011 | -0.042 | -4.6 | 9.6e-6 (8.0e-6 to 11.3e-6) |
| **Group maps, other map** | **one group only, dAF 0.1-50%** | +0.002 ± 0.014 | -0.042 | +6.0 | -0.3e-6 (-2.4e-6 to 1.8e-6) |
| Group maps, other map | one group only, dAF 1-50% | -0.000 ± 0.018 | -0.060 | +6.5 | 0.0e-6 (-1.7e-6 to 1.8e-6) |
| Group maps | neither group, dAF 0.1-50% | -0.002 ± 0.005 | -0.010 | +2.9 | 1.2e-6 (-2.0e-6 to 4.4e-6) |
| Group maps | neither group, dAF 1-50% | -0.015 ± 0.010 | -0.023 | +1.6 | 3.9e-6 (1.4e-6 to 6.5e-6) |

### 5.2 Nucleosome barriers: the published statistic comes from defining barriers on the reference

The GC-poor barrier proxy covers 58.3 million centre positions on hg38 and 59.5 million on the ancestral sequence.
With barriers defined on hg38 we reproduce the published pattern (Figure 2A): in cores, ln S = +0.052 ± 0.012 for
S→W and -0.029 ± 0.008 for W→S (difference -0.082 ± 0.013); in flanks, +0.008 and +0.037 (difference +0.029 ± 0.007).
With the same definition applied to the ancestral sequence, the core difference is +0.012 ± 0.014 and the flank
difference -0.012 ± 0.007: the pattern disappears or reverses. The reason is that a GC-poor window on hg38 is more
likely to be one in which the human lineage replaced G/C by A/T; the substitutions that made the feature are then
counted as evidence that the feature was selected. Allele frequencies (Figure 2B) show no selection in the claimed
direction for either definition (core W→S minus S→W, dAF ≥ 0.5%: +0.003 ± 0.010 and +0.017 ± 0.010). This test has little
power: with the one-population calibration, selection strong enough to make the hg38 divergence difference (-0.100)
would change this allele-frequency difference by only about -0.009. Two further cautions: the divergence statistic
is also non-zero for the null classes (ln S for A↔T and C↔G in flanks +0.036 and +0.039; A↔T in cores -0.045), so it
carries biases unrelated to W/S selection; and the ancestral definition keeps hg38 bases at low-confidence sites, so
it may not be fully free of reference conditioning (its flanks show a small reversal, -0.012 ± 0.007). Variant
density is lower in cores than in GC-matched background for all mutation classes (-0.03 to -0.06), which points to a
lower mutation rate in these segments. We did not use the physical model of Drillon et al., so we show that their
statistic is sensitive to the definition, not that their exact barrier set is explained.

### 5.3 The rhythm is not explained by the present mutation spectrum

Rare variants give W→S and S→W mutation rates that change by 1.6% and 1.3% across offsets, but these changes are only
weakly in phase with the A/T rhythm (r = 0.03 and 0.20). At mutation-only equilibrium they would make a rhythm of
0.2 percentage points, 2% (± 0.8%) of the observed 2.2 points (Figure 3). Human-lineage substitutions would make a
rhythm 2.4 times the observed one, in phase (r = 0.97), but substitutions are carried by every donor and so are
subject to map conditioning (Section 5.1). Because nucleosomes are called where the present sequence fits them, the
rhythm around called dyads is expected even without any process that maintains it, so this comparison says little
about maintenance. Pich et al. (2018) found ~10-bp periodic mutation in germline variation; we see a modulation of
1.3-1.6% but only weakly in phase with the composition rhythm at these dyads, so it cannot by itself make the rhythm.

## 6. Interpretation and limits

- **What changes.** We find no measurable selection on the sequence signals that position nucleosomes in
  non-functional human DNA. The cleanest test bounds the heterozygote selection coefficient of a base change at a typical peak of the rotational preference (x = 1) at σ < 1.8e-6 (95%), so 4·Ne·σ < 0.1: drift, not selection, governs these sites in humans (on average; strong selection on a small subset is not excluded). The published claim based on mapped dyads (Prendergast & Semple 2011) is explained by
  map conditioning (in agreement with Warnecke et al. 2013); the barrier claim (Drillon et al. 2016) is sensitive to
  whether barriers are defined on the present-day reference, as far as our GC proxy can show. Any test of selection on a feature measured in present-day cells or defined on present-day sequence must
  condition on who carries the alleles, or use maps and definitions that the tested alleles could not influence.
- **Size of the map effect.** Common alleles that the donors carry are enriched at offsets they fit by about 2-5% in
  odds per mutation direction (dAF 1-50%), and fixed human-lineage substitutions by about 19% each way. With three
  donors per map the effect for an allele carried by one group is SI -0.057. A single base thus changes where a map
  puts a nucleosome often enough to bias genome-wide sequence statistics; sequence models of positioning trained on
  such maps inherit this. Whether the effect is allele-specific positioning in the cells or partly technical (MNase
  cutting and fragment-size selection are strongly sequence-dependent, Figure 1A) is not settled by our data.
- **Residual.** Alleles carried by neither donor group at dAF 1-50% show SI -0.015 ± 0.010 on the group maps
  (σ ≈ 3.9e-6, 95% 1.4e-6 to 6.5e-6), but -0.003 ± 0.009 on the pooled map, which has more reads and
  better dyads. A σ of 3.9e-6 would give -0.028 on the other map for one-group alleles, against
  +0.002 ± 0.014 observed. No single selection coefficient fits all subsets; the residual is more likely a
  property of the shallower group maps or of imputed genotypes than selection, but we did not find its cause.
  Higher-quality donor genotypes (NYGC 30x) could test this.
- **Limits.** (i) One map source (lymphoblastoid lines of seven donors); germline chromatin was not mapped, so
  selection acting only through germline nucleosomes is tested only as far as rotational settings are shared between
  cell types. (ii) NIEBs were approximated by a GC rule, not by the published physical model (their coordinates are
  available only on request); the reference-conditioning argument applies to any sequence-defined feature, but its
  size for the exact NIEB set is not measured. (iii) Genotypes are from 1000 Genomes phase 3 (partly imputed from low
  coverage; misclassification pushes D_own towards zero); NA19193 is not genotyped and was left out of the group maps. (iv) The calibration rests on one
  demographic model (Tennessen et al. 2012); it reproduces the carrier shares well (Section 4.8), but the σ bounds
  depend on it and on Ne = 10,000 for σ_div; they are approximate within a factor of about 2.
  (v) Selection weaker than the bounds, or selection on translational positioning without a rotational component, is
  not excluded. (vi) The equilibrium analysis ignores context beyond trinucleotides. (vii) The ancestral states come
  from parsimony with three apes, not from the Ensembl EPO alignment.

## 7. Predictions that an experimental lab can test

1. **Allele-specific positioning.** In haplotype-resolved MNase-seq or chemical mapping of individuals heterozygous for
   common W/S variants inside well-positioned nucleosomes, the dyad distribution on each haplotype will shift so that
   the W allele sits more often at minor-groove-in offsets (our estimate: a few percent change in the odds of the
   rotational setting per allele), if map conditioning is biological; if it is technical, the shift will appear in
   MNase maps but not in chemical (hydroxyl-radical) maps of the same cells.
2. **Map dependence.** Any nucleosome or chromatin map made from donors with known genotypes will show a
   selection-like contrast only for the donors' alleles; maps from donors who lack an allele will not.
3. **Population-specific maps.** A map from European-ancestry cells will show the selection-like contrast for alleles
   common in Europeans but rare in Yoruba, and the Yoruba map will not; the reverse holds for Yoruba-specific alleles.
   (A fitness assay cannot test our bound: effects below 1e-6 per base are far below cell-culture resolution.)
4. **Barriers.** In vitro reconstitution on human and chimpanzee orthologues of GC-poor barriers will show occupancy
   differences that follow their GC differences, with no excess in the human direction when barriers are chosen on the
   ancestral or chimpanzee sequence.

## 8. Comparison with the prior prediction and usefulness

- Predicted: no rotational selection (P = 0.8); held, within the bounds above. Predicted a barrier signal that follows
  gBGC; the signal was real but, with our GC proxy, depended on the definition, not on gBGC. Predicted that mutation explains at least
  half of the rhythm; wrong (2%). Not predicted: the map-conditioning artefact, which produced a strong,
  selection-like, frequency-dependent signal in 108 million variants.
- **About as useful as expected, and less novel than we first thought.** The null result was expected by most
  population geneticists, and the idea that nucleosomes reposition on the present sequence was published for
  substitutions (Warnecke et al. 2013). The independent review found that we had missed this work. What the study
  adds is a direct, model-free separation of map conditioning from selection in population data (map swap), a
  calibrated bound, a lineage replication, and the first test of the barrier claim. The methodological lesson —
  test selection only on alleles that could not have shaped the map or the feature — applies to chromatin states,
  transcription-factor binding and DNA-shape annotations. Limits to reuse: one map source, a GC proxy for NIEBs,
  and an unexplained small residual in one subset.

## 9. Reproduction

`files/code/run_all.sh` lists every step with its inputs (paths of the fetched inputs are given as environment
variables). Python 3.12 with numpy, scipy, pandas, matplotlib, pyliftover; a C compiler with zlib. Peak memory (resident set)
up to about 2 GB for chromosome 1 in `rot_aggregate.py`, below 1.5 GB elsewhere; derived data about 6 GB (not included). The package includes the merged count arrays
(`files/data/`), from which `analyze_rot.py`-type statistics can be recomputed without the streams; the per-chromosome
arrays used for the jackknife are regenerated by the pipeline. Run log: `files/notes/01_run_log.md`.

## 10. Files

- `files/code/` all code (C streamers, Python aggregation and analyses, simulations, figures, `run_all.sh`).
- `files/results/` tables: `rot_mh.tsv`, `rot_selection_index.tsv`, `rot_x.tsv`, `rot_per_type.tsv`,
  `lineage_contrast.tsv`, `swap_dx.tsv`,
  `carrier_mh.tsv`, `carrier_selection_index.tsv`, `swap_selection_index.tsv`, `mapAB_profile.tsv`,
  `mapAB_overlap.tsv`, `selection_bounds_all.tsv` (all tests with calibration), `calibration*.tsv`, `sim_pooled.tsv`,
  `sim2_pooled.tsv`, `expected_si_divergence.tsv`, `barrier_stats.tsv`, `barrier_ws_minus_sw.tsv`,
  `barrier_centres.tsv`, `equilibrium.tsv`, `equilibrium_profiles.tsv`, `phase_profile.tsv`, and the raw
  simulation outputs (`sim/`, `sim2/`, `sim3/`). `selection_bounds.tsv` (one-population calibration) and
  `selection_bounds_two_pop.tsv` are earlier calibrations kept for transparency; `selection_bounds_all.tsv` is final.
  `phase_profile.tsv` is hg38 composition by positioning class (offsets ±90, including the fragment-end artefacts).
- Unused code kept for the record: `lift_dyads.py` (K562 dyads, no rotational signal) and `ancestral.py` (Ensembl EPO
  reader; that download was too slow).
- `files/data/` merged count arrays: `rot_merged.npz` [96 types × 181 offsets × 4 positioning classes × 4 repeat
  classes], `barrier_merged.npz`, `carrier_merged.npz`, `swap_merged.npz`, `lineage_merged.npz`, with
  per-chromosome totals.
- `files/provenance/` input sizes and checksums, stream logs, mask, dyad, ancestral and liftover summaries.
- `files/notes/` question choice with prior predictions; run log (with the interim look and the added analyses);
  `02_review.md`, the independent review and how each point was handled.
- `files/figures/` the three figures.

## References

Prendergast JGD, Semple CAM (2011) Genome Res 21:1777. Drillon G et al. (2016) BMC Genomics 17:526. Langley SA,
Karpen GH, Langley CH (2014) PLoS Genet 10:e1004457. Pich O et al. (2018) Cell 175:1074. Li C, Luscombe NM (2020)
Nat Commun 11:1363. Gaffney DJ et al. (2012) PLoS Genet 8:e1003036. Chen X et al. (2012) Science 335:1235.
Tennessen JA et al. (2012) Science 337:64. 1000 Genomes Project Consortium (2015) Nature 526:68. Chen S et al.
(2024) Nature 625:92 (gnomAD genomes constraint). Segal E et al. (2006) Nature 442:772. Kaplan N et al. (2009) Nature
458:362. Struhl K, Segal E (2013) Nat Struct Mol Biol 20:267. Graur D et al. (2013) Genome Biol Evol 5:578.
Taft RJ, Pheasant M, Mattick JS (2007) BioEssays 29:288. Dukler N et al. (2022) Nat Commun 13:4312. Warnecke T et al.
(2013) PLoS Comput Biol 9:e1003373. He BZ et al. (2011) PLoS Genet 7:e1002053. Jiang D, Zhang J (2024) Mol Biol Evol
41:msad284.

