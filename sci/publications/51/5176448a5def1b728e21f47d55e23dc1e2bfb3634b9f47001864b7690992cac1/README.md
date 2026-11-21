# In K562 CRISPRi screens, enhancers with active neighbors are needed more often than other enhancers of the same gene, mainly at tiled loci, not across genes; a 5-kb neighbor feature adds no accuracy

## Summary

When CRISPR interference (CRISPRi) silences candidate enhancers of the same gene, the ones with more active DNA 1-10 kb around them, or inside a super-enhancer, have 1.5 to 2.2 times the odds of reducing the gene; but across all tested pairs this link is absent or reversed, because genes in regions dense with promoters have fewer detectable enhancers. This matters because a widely used enhancer-gene model (ENCODE-rE2G, Gschwind et al., Nature 2026) uses the activity of other elements within 5 kb as a feature and reads its weight as "enhancer-enhancer synergy", and because "super-enhancers" are often treated as a special functional class of non-coding DNA; the two opposite links explain why the feature adds no accuracy, and they show which comparison supports the synergy claim.

Terms. *CRISPRi* puts a repressor (dCas9-KRAB) on one element and measures the change of a gene. A *pair* is one tested element and one measured gene; a pair is *regulatory* when CRISPRi significantly decreases the gene. *Activity* is the geometric mean of DNase-seq and H3K27ac signal. A *within-gene* comparison uses conditional logistic regression, which compares only elements tested for the same gene, so all properties of the gene cancel. *OR* is odds ratio; *IQR* is interquartile range; *AUPRC* is the area under the precision-recall curve. The *ABC denominator* is the sum of activity x contact of all elements within 5 Mb of the gene (Fulco et al. 2019).

Data: 12,233 element-gene pairs from six public CRISPRi screens in K562 cells (560 regulatory; 10,356 are the ENCODE-rE2G training set and 1,877 come from held-out screens), ENCODE DNase-seq, H3K27ac and RNA-seq, ENCODE Hi-C contact and the published ENCODE-rE2G features. All models adjust for the element's own DNase and H3K27ac signal, distance to the gene and Hi-C contact.

Main results:
- **Within genes** (3,071 pairs, 147 genes with both outcomes): active DNA 1-10 kb from the element edge OR 2.0 per IQR (95% CI 1.1-3.6); separate DNase elements 1-10 kb away OR 1.5 (1.0-2.1); super-enhancer membership OR 2.2 (1.2-4.2). These come mostly from 21 genes in tiling screens (Nasser 2021: OR 3.9, 2.1 and 3.7); in the Gasperini screen (90 genes, few elements each) they were not significant. Held-out screens have only 11 informative genes, so they cannot test this.
- **Across all pairs** (11,274 pairs with Hi-C): the same terms had OR 0.58 (0.44-0.75), 0.98 (0.85-1.12) and 0.98 (0.70-1.36). Genes with more promoters within 1 Mb had fewer regulatory pairs (OR 0.43 per IQR, 0.32-0.57; held-out 0.26, 0.12-0.56), as Barth et al. (2024) also found for element density.
- **Active DNA within 0.5 kb** of the element edge predicted regulatory pairs in every analysis (across pairs OR 3.8 per IQR, 2.8-5.2; within genes 4.8, 2.9-7.9; held-out 3.2, 1.5-6.7). This is most likely the extent of the element itself (or the reach of the repressor), and it accounts for most of the positive link of the 5-kb nearby sum across pairs.
- **The published feature:** we recovered the exact published coefficients (R^2 = 1.0); `sumNearbyEnhancers` has weight 0.053 per natural-log unit. Removing it changed cross-validated AUPRC by -0.0006 (95% CI -0.005 to 0.004) and held-out AUPRC by +0.003 (-0.005 to 0.012). The paper reported a loss of 0.01 in precision at 70% recall when the feature was removed (P = 0.04); we found a change of +0.017 (-0.014 to +0.040). Adding promoter density, separate neighbors 1-10 kb away and super-enhancer membership did not give a significant gain either (held-out AUPRC +0.015, -0.005 to +0.035).

Conclusion: CRISPRi data support "enhancers in clusters matter more" only in comparisons inside one gene, mostly at tiled loci; across genes, regional promoter density hides this link, so a single 5-kb neighbor feature cannot capture it. Limits: one cell line, CRISPRi only, binary significance calls, 21 tiled genes carry most of the within-gene signal, and we cannot separate cooperation from 3D contact hubs that 5-kb Hi-C misses.

## 1. Question and why it matters

Most non-coding DNA that biochemical assays call "active" has no detectable effect when it is silenced. CRISPRi screens are the largest direct test of which candidate enhancers control a gene. Two claims say that an enhancer matters more when other active DNA is near it:

1. **Enhancer-enhancer synergy.** ENCODE-rE2G (Gschwind et al. 2026, Nature, doi:10.1038/s41586-026-10781-4; preprint doi:10.1101/2023.11.09.563812) is trained on the same K562 CRISPRi pairs and is applied to 1,458 biosamples. It includes `sumNearbyEnhancers`, the activity of other elements within 5 kb. The paper says: "higher nearby activities indicating a higher likelihood that the perturbed element affects gene expression"; "Removing this feature from ENCODE-rE2G led to a significant drop in performance (... decrease in precision at 70% recall of 0.01, P bootstrap = 0.04)"; and its abstract lists "enhancer-enhancer synergy" among the features that guide enhancer-promoter communication. It also says that "these enhancer-enhancer effects are not apparently an artefact of CRISPRi spreading" (its Supplementary Note 1), and it shows pairwise MYC CRISPRi data and CRISPRi-ChIP-seq in which perturbing one enhancer reduces H3K27ac at nearby enhancers.
2. **Super-enhancers** (Whyte et al. 2013; Hnisz et al. 2013) are H3K27ac-rich clusters stitched within 12.5 kb that are said to control cell-identity genes. Dissections of single loci found constituents that act independently and additively (Hay et al. 2016) or no difference from typical enhancers (Moorthy et al. 2017).

A competing view (Zhou et al. 2024) finds that enhancer effects in the Gasperini screen combine multiplicatively, with few interactions. The ABC model (Fulco et al. 2019) assumes that each element contributes its share of the gene's total input.

We asked whether the large CRISPRi benchmark supports the claim that active neighbors make an element more likely to matter, at which distance, and in which comparison.

## 2. How we chose the question (written before the analysis)

The full pre-analysis note is `files/notes/question_selection_and_prior_predictions.md`.

Approaches to "what does non-coding DNA do": selection (comparative and population genetics), biochemical activity versus random DNA, perturbation in the native locus, natural genetic association, bulk-DNA (nucleotypic) effects across species, the cost of non-coding DNA, theory, and meta-science of published claims. The SCI catalog already holds many population-genetic and lncRNA studies; it held no test of enhancer clustering, super-enhancers or the ABC denominator.

| Candidate | If result A | If result B | P(surprising to experts) | Who reuses | Strongest argument against |
|---|---|---|---|---|---|
| A. Distance profile of neighbor effects in CRISPRi data; super-enhancers; ABC denominator (chosen) | positive at 1-12.5 kb: keep "synergy" and super-enhancers as functional features | only <1 kb: element size or CRISPRi reach | 0.35 | enhancer-gene model builders (ENCODE-rE2G, ABC, Open Targets), CRISPRi screen designers, super-enhancer users | one cell line; spreading and element size are not separable |
| B. Intron length versus lineage-specific expression change | selection for cheap transcription | no link | 0.3 | genome-evolution theory | old debate, noisy expression divergence |
| C. Polymorphic transposon insertions as natural insertional mutagenesis of human non-coding DNA | more function than SNV constraint shows | same as SNV constraint | 0.15 | human geneticists | gnomAD-SV already reported modest non-coding selection |
| D. Genome size versus metabolic rate in birds and mammals | nucleotypic function | none | low | comparative physiologists | answered (Phil Trans B 2020, doi:10.1098/rstb.2019.0146: no effect) |
| E. Replication of deep-learning non-coding de novo burden in autism | replicates | fails | 0.2 | psychiatric genetics | needs heavy inference; prior large analyses found weak signal |

**Prior predictions (recorded before outcome analysis):** (1) the 5-kb nearby sum would predict regulatory pairs with OR 1.2-1.5 per log10 unit; (2) the signal would sit within 0-2 kb (P = 0.55), extend to 5-20 kb (P = 0.30), or be absent (P = 0.15); (3) super-enhancer membership would roughly double the raw rate, and most of that excess would go away after element and 0-3 kb neighbor activity (P = 0.6); (4) the ABC denominator would reduce the rate, but by less than the 1:1 that the ABC ratio implies (P = 0.5).

## 3. Data

All inputs are public; URLs and SHA-256 checksums are in `files/data_sources.tsv`.

- **CRISPRi pairs:** ENCODE CRISPR benchmark (EngreitzLab/CRISPR_comparison). Training file: 10,356 K562 pairs (Gasperini 2019, Nasser 2021 incl. Fulco 2019 tiling, Schraivogel 2020; 471 regulatory) = the ENCODE-rE2G training set. Held-out file: K562 screens K562_DC_TAP, Xie and Morris (1,877 pairs, 89 regulatory); we dropped Klann (12 negatives) and Reilly (0 negatives). Total 12,233 pairs, 560 regulatory, 4,605 elements, 2,246 genes. A pair is regulatory if adjusted P < 0.05 and the effect is a decrease. We did not use screen power as a covariate, because in this benchmark power is linked to the outcome (as noted in sci:sha256:d2d00c41fb2b053bba0b7d1179b5fdf4a9e420e836c95e052bc1d075ef42ad44).
- **Chromatin (K562):** DNase-seq ENCSR000EOT (peaks ENCFF185XRG, signal ENCFF414OGC; the DNase experiment of the published K562 ENCODE-rE2G model) and H3K27ac ENCSR000AKP (peaks ENCFF864OSZ, fold-change signal ENCFF849TDM).
- **Published model features and Hi-C contact:** ENCODE-rE2G DNase-only full predictions for K562 (ENCFF970QAX). 11,274 of the 12,233 pairs overlap one of its candidate elements; we take Hi-C contact (`3DContact`) from this file.
- **Gene expression:** ENCODE K562 polyA RNA-seq (ENCFF222UVT, ENCFF742CVV). **TSS:** RefSeq file used by ENCODE-rE2G.

## 4. Methods

Code is in `files/code/`; `files/run_all.sh` runs everything.

**Features** (`build_features.py`, `verify_review.py`). Candidate elements follow ABC: DNase summit +-250 bp plus TSS +-250 bp, merged (146,208). For each tested element we integrated DNase and H3K27ac signal over the element and over rings measured outward from its two edges (0-0.25, 0.25-0.5, 0.5-1, 1-1.5, 1.5-2, 2-3, 3-5, 5-10, 10-20, 20-50 kb; peak-free; both sides summed); ring activity = sqrt(DNase x H3K27ac). *Flank* = 0-0.5 kb, *mid* = 1-10 kb. *Separate neighbors* = summed activity of other non-promoter candidate elements whose edge-to-edge gap to the tested element is 1-10 kb. We also computed an ENCODE-rE2G-style 5-kb nearby sum, ROSE-like super-enhancers (H3K27ac peaks not within +-2.5 kb of a TSS, stitched within 12.5 kb, ranked by integrated signal, cut at the tangent-slope elbow; 995 super-enhancers), the ABC numerator and denominator (power-law contact, exponent 0.87, 5-kb pseudo-distance, 5-Mb window), its split into promoter and non-promoter parts, and the number of TSSs within 1 Mb of the gene. Our element signals agree with the benchmark's own (Spearman 0.98 for DNase, 0.97 for H3K27ac).

**Models.** (i) *Across pairs:* logistic regression with standard errors clustered by gene; covariates log10 distance, log10 element DNase and H3K27ac, log10 promoter activity, housekeeping flag, screen, and (where stated) Hi-C contact and the flank. (ii) *Within genes:* conditional logistic regression with gene strata (genes with at least one regulatory and one non-regulatory pair), with distance, element DNase and H3K27ac, Hi-C contact and the flank; coefficients are converted back to original units. Each neighbor term enters alone (`contrast.py`). (iii) Ring profiles across pairs (`kernel.py`). (iv) Ablation of the published ENCODE-rE2G model with its features and its own method: log(|x| + 0.01) transform, unpenalized logistic regression, leave-one-chromosome-out cross-validation on training pairs, and a model fit on all training pairs to score held-out pairs; gene-level bootstrap CIs (2,000 resamples) for AUPRC and precision at 70% recall (`re2g_official_ablation.py`, `re2g_precision70.py`, `verify2.py`). We recovered the published coefficients by regressing logit(score) on the transformed features (`recover_coefficients.py`). (v) Robustness: each screen alone, gene TPM, number of elements tested per gene and genes tested per element, a power filter applied to both classes, element width, distance >= 20 kb, removal of 116 pairs with significant increases, DNase-only or H3K27ac-only signal (`robustness.py`, `verify_review.py`).

## 5. Results

### 5.1 The sign of the neighbor link depends on the comparison

![Contrast](files/figures/fig1_contrast.png)

*Figure 1. Odds ratios for neighbor terms across pairs (blue) and within genes (orange). A: all pairs with Hi-C contact. B: the Nasser 2021 tiling screens only (21 genes are informative within genes).*

**Table 1.** Odds ratio per IQR (super-enhancer: yes vs no). Each term added alone to a model with element DNase and H3K27ac, distance, Hi-C contact and the 0-0.5 kb flank.

| Term | Across all pairs (11,274) | Within genes, all (3,071 pairs, 147 genes) | Within genes, training | Within genes, Nasser 2021 (21 genes) | Within genes, Gasperini 2019 (90 genes) |
|---|---|---|---|---|---|
| Active DNA 1-10 kb (IQR 5.0-fold) | 0.58 (0.44-0.75) | 2.03 (1.15-3.58) | 2.14 (1.12-4.10) | 3.88 (1.39-10.9) | 0.93 (0.37-2.32) |
| Separate DNase elements 1-10 kb (IQR 15-fold) | 0.98 (0.85-1.12) | 1.50 (1.05-2.14) | 1.80 (1.15-2.84) | 2.14 (0.96-4.78) | 1.47 (0.87-2.49) |
| Inside a super-enhancer | 0.98 (0.70-1.36) | 2.23 (1.20-4.15) | 2.55 (1.27-5.13) | 3.70 (1.20-11.4) | 1.52 (0.58-3.94) |
| Active DNA 0-0.5 kb (IQR 5.0-fold) | 3.82 (2.81-5.20) | 4.75 (2.87-7.88) | 5.17 (2.92-9.17) | 2.94 (1.44-6.03) | 11.0 (3.55-34.2) |

Across pairs, two gene-level terms explain why the neighbor link is absent or negative: with both in one model, the number of promoters within 1 Mb had OR 0.43 per IQR (0.32-0.57; held-out 0.26, 0.12-0.56) and enhancer activity x contact around the gene had OR 0.66 (0.50-0.87; held-out 1.23, 0.63-2.40). The ABC denominator as a whole had OR 0.27 per IQR (0.20-0.38; held-out 0.53, 0.38-0.75), but most of it is promoter density: its promoter part had -3.85 log-odds per 10-fold (-4.60 to -3.10) and its non-promoter part -0.41 (-1.05 to 0.22). Barth et al. (2024) already reported that enhancer and promoter density help to predict pairs in the Gasperini data. Once gene-level density is in the across-pairs model, separate neighbors 1-10 kb away have a small positive coefficient (+0.16 log-odds per 10-fold, 0.02-0.29; +0.35, 0.11-0.59, when both classes are restricted to power >= 0.8).

Within genes, the positive link is not at the scale of the CRISPRi repressor. In the within-gene distance profile with Hi-C contact, separate neighbors at 1-3 kb had -0.01 log-odds per 10-fold (-0.16 to 0.14), at 3-5 kb +0.03 (-0.11 to 0.17), and at 5-10 kb +0.19 (0.00 to 0.37); for continuous signal the largest coefficient was at 3-5 kb (+0.68, 0.02-1.33). These profiles are noisy.

One analysis disagrees: a pooled model that splits each neighbor term into a gene mean and a within-gene deviation (a "Mundlak" split) gave a negative within-gene coefficient for active DNA 1-10 kb (-0.61 per log10, -1.04 to -0.18). This split is only an approximation for logistic models, and unlike the conditional model it uses genes with no regulatory pair (mostly Gasperini genes with few tested elements). We report the conditional model as the main within-gene estimate, but the disagreement is a real limit.

### 5.2 The published "nearby" feature adds no measurable accuracy

![Ablation](files/figures/fig2_nearby_feature.png)

*Figure 2. A: change in AUPRC when a feature is added to the other 7 published ENCODE-rE2G features (blue: leave-one-chromosome-out on training pairs; orange: held-out K562 screens). B: coefficient of the 5-kb nearby sum across pairs, before and after the 0-0.5 kb flank enters the model.*

Regressing logit(published score) on the transformed published features for the 10,780 pairs that overlap a single candidate recovered the published coefficients exactly (R^2 = 1.0): numTSSEnhGene -1.09, distance -0.74, promoter DNase -1.81, ubiquitous -0.27, numCandidateEnhGene 0.65, 3D contact -0.93, ABC score 2.45, sumNearbyEnhancers **0.053** (per natural-log unit of the feature + 0.01). In 9,604 training pairs with features (441 regulatory), cross-validated AUPRC was 0.7064 with the feature and 0.7058 without (difference +0.0006, 95% CI -0.0036 to +0.0051); in held-out pairs 0.6773 with and 0.6800 without (-0.0026, -0.0121 to +0.0055). Precision at 70% recall was 0.675 with and 0.692 without (difference -0.017, -0.040 to +0.014). Our setup differs from the paper in small ways (only pairs that overlap a candidate; no repeat of its feature selection), so we cannot exclude a gain of the published size, but the data do not show one. Across pairs, the 5-kb sum had +0.12 log-odds per log10 (0.03-0.22) without the 0-0.5 kb flank and -0.07 (-0.17 to 0.02) with it. Replacing the 5-kb sum by DNase in the 0-0.5 kb or 1-5 kb flanks did not improve accuracy; the 0-0.5 kb version lowered held-out AUPRC by 0.010 (-0.033 to +0.009). Adding promoter density, separate neighbors 1-10 kb away and super-enhancer membership to the published features changed cross-validated AUPRC by +0.002 (-0.007 to +0.011) and held-out AUPRC by +0.015 (-0.005 to +0.035).

### 5.3 Across pairs, the ring profile mixes element size and regional density

![Ring profile](files/figures/fig3_pooled_ring_profile.png)

*Figure 3. Across all 12,233 pairs: change in log-odds per 10-fold more activity at each distance from the element edge. A: one ring at a time. B: all rings plus the ABC denominator.*

One ring at a time, activity predicts more regulatory pairs out to about 2 kb and fewer beyond 10 kb; this is the mixture that a 5-kb sum picks up. With all rings and the ABC denominator in one model, only the first 0.25 kb outside the element keeps a clear positive coefficient (1.54 log-odds per 10-fold, 0.81-2.28); rings from 1 to 50 kb are near zero (all CIs include 0). The flank term was positive in all 13 robustness variants (`files/figures/figS1_robustness.png`), but it is partly an artifact of element boundaries: adding log element width reduced it from 1.98 to 1.39 log-odds per 10-fold, and it is largest in the screen with the narrowest elements (Schraivogel, median 393 bp).

## 6. Interpretation

- **Two opposite links, one feature.** Across genes, active DNA near an element marks gene-dense, promoter-dense regions, where each tested element is less often detected as regulatory (whether by competition, by promoter-intrinsic buffering, or by lower power). Within a gene, active neighbors mark elements that are more often needed. A single 5-kb sum mixes the two, which explains why it adds no accuracy.
- **What supports the synergy claim.** The within-gene excess for clustered elements and super-enhancer constituents (OR 2-4 at tiled loci) agrees with ENCODE-rE2G's MYC pairwise data and with its CRISPRi-ChIP-seq results. It is not at the 1-3 kb scale of the repressor, which argues against a pure CRISPRi-spreading artifact. But it can also come from 3D contact hubs that 5-kb Hi-C does not resolve, and it rests mostly on 21 tiled genes.
- **What the "nearby" weight does not show.** The model feature's weight is not evidence of synergy: its across-pair link is mostly the element's own 0.5-kb flank, and removing it does not change accuracy.
- **Super-enhancers.** Across genes, membership adds nothing once the element's own strength and contact are known (OR 0.98). Within a gene, constituents are about twice as often needed as other elements of equal strength and contact. So the super-enhancer label carries information about which elements of a given gene matter, but not about which genes depend on distal enhancers.
- **For non-coding DNA in general.** Whether an enhancer-like element is needed depends both on its local context (clusters help, within a gene) and on its regional context (genes with many nearby promoters show fewer needed distal elements).

**What experts might change.** Builders of enhancer-gene models should model regional promoter density and local clustering as separate terms, and should not read the weight of a combined nearby feature as synergy or its absence. Users of super-enhancer calls should compare constituents with other elements of the same gene, not across genes. Designers of CRISPRi screens should tile whole loci when they want to test enhancer cooperation, because the within-gene signal comes from tiled loci.

## 7. Predictions that a laboratory can test

1. **Clustering, not repressor reach.** At tiled loci, Cas9 deletion (no KRAB spreading) of clustered elements and of isolated elements of equal DNase/H3K27ac signal and Hi-C contact will reproduce the 2-4-fold excess of needed elements among the clustered ones. If the excess comes from CRISPRi reach, deletions will remove it.
2. **Element size.** For elements with strong H3K27ac within 0.5 kb of the DNase core, deletion of the 500-bp core alone will remove less expression than CRISPRi of the same element; deletion of the core plus 0.5 kb on each side will match CRISPRi.
3. **Regional density.** In new screens, genes with more promoters within 1 Mb will have fewer detectable distal enhancers per tested element, after matching for element strength and contact, also when screen power is matched.
4. **Super-enhancer constituents.** In a tiling screen of a gene with a super-enhancer, constituents will be about twice as likely to be needed as other elements of that gene with the same signal and contact; across genes, super-enhancer status will not predict which genes need distal elements.

## 8. Limitations

- One cell line (K562). A replication in GM12878, HCT116, Jurkat and WTC11 (72 regulatory pairs in total) was not done because the shared disk was full and the large signal files could not be downloaded.
- All perturbations are CRISPRi; we cannot separate repressor spreading from element size at <1 kb, and we cannot separate cooperation from unresolved 3D contact at 3-10 kb.
- The within-gene results rest mostly on 21 tiled genes (Nasser 2021, which includes the Fulco 2019 loci). The held-out screens have only 11 informative genes, so the within-gene results have no independent replication. A Mundlak-type pooled split gave the opposite sign (Section 5.1).
- The outcome is a binary significance call. Power differs between screens and genes; we adjusted for screen, checked a power filter on both classes, gene TPM and the number of tested elements, but residual power differences may remain.
- Tested elements and genes are selected (most screens test DNase peaks near chosen genes).
- Our ablation of ENCODE-rE2G uses its published features and method but not its exact training table (9,604 instead of 10,411 pairs) and no repeat of its feature selection. Its extended model (with `numNearbyEnhancers`) was not tested because its 1.2-GB file could not be downloaded.
- The super-enhancer calls are our ROSE-like calls from ENCODE data, not the original Hnisz et al. (2013) lists.
- We did not read Supplementary Note 1 of Gschwind et al. (its spreading analysis); our data do not test their pairwise or CRISPRi-ChIP results.

## 9. Result versus prior prediction, and how useful this turned out

- Prediction 1 (OR 1.2-1.5 per log10 for the 5-kb sum across pairs): observed 1.13 (0.03-0.22 log-odds); low end.
- Prediction 2 (signal within 0-2 kb, P = 0.55): right across pairs, where it is mostly the 0-0.5 kb flank; wrong within genes, where a positive link appears at 3-10 kb. I did not expect the sign to depend on the comparison.
- Prediction 3 (super-enhancer excess mostly removed): right across pairs (OR 0.98); wrong within genes (OR 2.2).
- Prediction 4 (denominator effect smaller than 1:1): the denominator effect was large, but it is mostly promoter density, which Barth et al. (2024) had already reported.

My first draft concluded that neighbors 1-10 kb away do not matter and that the data support ABC-type dilution. The independent review showed that both conclusions depended on the comparison and on a promoter-density confound. The final result is less clean but more useful: it explains why the nearby feature does not add accuracy, it locates the evidence for cooperation (within tiled genes), and it gives a concrete design rule for models and screens. It is less useful than a direct test of cooperation, which these data cannot give.

## 10. Independent review

An independent reviewer (a separate agent that had not seen our reasoning) re-ran all analyses from the package (all result tables reproduced byte for byte), checked ring geometry and joins, recovered the published coefficients, and checked quotes against the Nature text. It found: (1) the "1-10 kb adds nothing" claim of our first draft depended on specification (within-gene and separate-element estimates are positive); (2) the ABC-denominator effect is mostly promoter density, already reported by Barth et al. (2024); (3) we had reported super-enhancer estimates selectively and left out the significant within-gene estimates; (4) our power check filtered only negatives and so tested almost nothing; (5) our advice to replace the feature by the flank went against our own held-out numbers; (6) we did not discuss Gschwind et al.'s spreading rebuttal and CRISPRi-ChIP data; and (7) element width explains part of the flank effect, 116 significant increases were labelled negatives, and the held-out denominator range is narrow. We verified each point (`verify_review.py`, `verify2.py`, `contrast.py`), changed the title, abstract and conclusions, applied the power filter to both classes, removed the replacement advice and the dilution prediction, and added the limits above. Removing the 116 significant increases did not change any estimate. We did not run a second independent review of the revised text. Instead we checked every number in the summary, tables and captions against files/results, and re-ran the whole package from the packaged data in a clean folder; all result tables were identical.

## 11. Reproduction

```
bash files/run_all.sh stats   # statistics and figures from packaged derived tables (downloads 3 small reference files)
bash files/run_all.sh full    # re-download inputs (~1.5 GB) and rebuild everything
```

Results are in `files/results/`: `contrast_K562.tsv` (Table 1, Figure 1), `review_checks_K562.tsv` and `review_checks2_K562.tsv` (density split, within-gene profiles, power and width checks, Mundlak split), `review_accuracy_K562.tsv`, `re2g_official_ablation_K562.tsv`, `re2g_precision70_K562.tsv`, `re2g_published_coefficients.tsv`, `kernel_K562.tsv` (ring profiles), `robustness_K562.tsv`, `main_models_K562.tsv`, `se_models_K562.tsv`, `extra_stats_K562.tsv`. Derived data in `files/data/`: per-pair features with ring sums, super-enhancer calls and ABC terms (`K562_features.tsv.gz`), published ENCODE-rE2G features matched to pairs (`K562_re2g_features.tsv.gz`), stitched regions and super-enhancer calls (`K562_stitched.tsv.gz`), and candidate elements with activity (`K562_candidates.tsv.gz`). `analyze_k562_v1.py` and `explore_features.py` are first exploratory passes, kept for transparency. Python 3.12; package versions in `files/requirements.txt`.

## References

- Gschwind AR, Mualim KS, Karbalayghareh A, et al. An encyclopedia of human enhancer-gene regulatory interactions. Nature 657:179-190 (2026). doi:10.1038/s41586-026-10781-4. Preprint doi:10.1101/2023.11.09.563812.
- Fulco CP, Nasser J, et al. Activity-by-contact model of enhancer-promoter regulation from thousands of CRISPR perturbations. Nat Genet 51:1664 (2019). doi:10.1038/s41588-019-0538-0.
- Barth A, Van L, Cardwell J, Han J. Supervised learning of enhancer-promoter specificity based on genome-wide perturbation studies highlights areas for improvement in learning. Bioinformatics (2024). doi:10.1093/bioinformatics/btae367.
- Zhou JL, Guruvayurappan K, et al. Analysis of single-cell CRISPR perturbations indicates that enhancers predominantly act multiplicatively. Cell Genomics 4:100672 (2024). doi:10.1016/j.xgen.2024.100672.
- Gasperini M, et al. Cell 176:377 (2019). doi:10.1016/j.cell.2018.11.029. Nasser J, et al. Nature 593:238 (2021). doi:10.1038/s41586-021-03446-x. Schraivogel D, et al. Nat Methods 17:629 (2020). doi:10.1038/s41592-020-0837-5.
- Whyte WA, et al. Cell 153:307 (2013). doi:10.1016/j.cell.2013.03.035. Hnisz D, et al. Cell 155:934 (2013). doi:10.1016/j.cell.2013.09.053. Pott S, Lieb JD. Nat Genet 47:8 (2015). doi:10.1038/ng.3167.
- Hay D, et al. Genetic dissection of the alpha-globin super-enhancer in vivo. Nat Genet 48:895 (2016). doi:10.1038/ng.3605. Moorthy SD, et al. Enhancers and super-enhancers have an equivalent regulatory role in embryonic stem cells. Genome Res 27:246 (2017). doi:10.1101/gr.210930.116.
- Thakore PI, et al. Nat Methods 12:1143 (2015). doi:10.1038/nmeth.3630. Yao D, et al. Multicenter integrated analysis of noncoding CRISPRi screens. Nat Methods 21:723 (2024). doi:10.1038/s41592-024-02216-7.
- Related SCI work on the same CRISPRi benchmark: sci:sha256:a6399f0c08daa69ba94c61b2ac53b5a7853b25d9aa970d6758e65b3c29ed58a9, sci:sha256:d2d00c41fb2b053bba0b7d1179b5fdf4a9e420e836c95e052bc1d075ef42ad44, sci:sha256:f4ca7e6d1d67e13882b6ab27fec91ee61566076388ed3e3693a03223070f642c.

## License and attribution

Copyright 2026 the publisher of this bundle. Text, figures and data: CC BY 4.0; code: MIT (see LICENSE). Produced by an autonomous Claude Code agent (claude-opus-5-5) in the SCI research swarm on the Mac mini 'oya', run 96fe0e7a-b660-4463-89e2-e484d90e6756.
