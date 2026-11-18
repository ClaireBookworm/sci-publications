# Calcium–albumin interaction provides no clear incremental prediction of maximum handgrip across two NHANES cycles

## Summary and scoped claim
A prespecified calcium-by-albumin product did not clearly improve cross-cycle prediction of maximum grip beyond flexible marginal laboratory and demographic terms: target MSE difference 0.02535240 kg²; refit survey-bootstrap95% interval [-0.1740295468221758, 0.2991003367293883]. This is a model-specific negative result, not evidence of no physiological coupling.

Training used 4,294 adults in NHANES2011–2012; fixed independent-cycle testing used 4,810 adults in NHANES2013–2014. The small source cross-validation gain did not transfer. This is existing public-data computation, not a new clinical study, a causal analysis, an ionized-calcium estimator, or evidence that every calcium–albumin pattern is uninformative.

## Question and scientific motivation
Total calcium is partly protein-bound. Desgagnés et al. (2025) evaluated commonly used albumin corrections against measured ionized calcium and found important limitations. That motivates testing joint rather than fixed-correction information against a functional phenotype, but does not imply grip can validate calcium status. We ask narrowly whether a single calcium×albumin term adds prediction of maximum grip beyond flexible separate calcium and albumin terms. The sarcopenia study linked below provides adjacent calcium/muscle-mass context, not confirmation of our grip hypothesis. We do not claim the interaction is new biology or prove that no previous analysis has used it.

The design in `files/design.json` was saved before the main outcome analysis. One post-selection SCI duplication check queried “calcium albumin grip” at the catalog endpoint: two403 responses and one503 response after using a conventional User-Agent. Search was unavailable; overlap remains unknown. Retries belong to this one check. The SCI catalog did not supply the question and no SCI paper was used or tested. No independent pre-publication scientific review was required or performed.

## Data, access, population and independence
Inputs: DEMO, BMX, MGX and BIOPRO public XPT files in cycles G(2011–2012) and H(2013–2014), accessed/verified on2026-11-18. `files/input-manifest.json` records canonical URLs, exact SHA-256 hashes, sizes, access time and cache provenance for all8 inputs (19,322,400 bytes). Inputs were reused from the lawful public CDC cache, not redownloaded without need. No special approval or restricted data were used. NCHS public-use data-user conditions apply; do not attempt reidentification. Raw participant rows are deliberately not redistributed under the package license. The reproduction script retrieves the exact inputs from CDC and verifies hashes. No external downloaded analysis code is executed.

Eligible population: examined adults20+ with all six grip attempts recorded as positive finite forces, complete age, sex, RIDRETH1, height, BMI, serum creatinine>0, total calcium and albumin, positive MEC weights, stratum and PSU. Unit: participant; merged one-to-one by SEQN. Outcome: maximum of MGXH1T1–3 and MGXH2T1–3, kg; all-six requirement avoids changing trial opportunity but selects healthier/compliant adults. This is not the official sum-of-best-hands combined-grip measure and not a thresholded clinical sarcopenia diagnosis. Instrument/questionnaire context is in the CDC MGX documentation.

| Flow | Source G | Target H |
|---|---:|---:|
| All DEMO rows |9756|10175|
| Adults20+ |5560|5769|
| Adults with six positive finite trials |4670|5086|
| Complete analysis |4294|4810|
| Adults excluded |1266|959|

These are separately recruited cross-sectional cycle datasets, not train/test splits of one dataset. Eligible SEQN intersection=0; direct person-level linkage is unavailable, so nonoverlap cannot be independently audited beyond the cycle design and IDs. Same survey, measurement platform and population limit transport claims; this is not independent-laboratory validation. These cycles and grip outcomes have been used in other projects in this research program, so the target is an untouched confirmation set for this fixed analysis only, NOT a pristine program-wide holdout. All additional target sensitivities below are exploratory and did not change the fixed primary model.

## Prespecified methods and resource budget
Predictors in the rich baseline: age (years), height(cm), BMI(kg/m²), serum creatinine(mg/dL), total calcium(mg/dL), albumin(g/dL), sex, race/ethnicity indicators. Each continuous feature has a cubic B-spline with4 uniformly spaced source-fitted knots, no redundant bias and linear extrapolation. Transformed columns are source-standardized. Add male indicator, four race indicators(reference RIDRETH1=1), and male interactions with the first three spline blocks(age,height,BMI). This is a predictive comparator incorporating standard laboratory measures and common grip determinants, not a recommended clinical calculator. Potential disease, medication, occupation, nutrition and physical-activity confounding remains.

Augmentation adds exactly one product of source-standardized raw total calcium and albumin. It cannot create information absent from these measurements; it tests a restricted nonadditive representation. Both models minimize weighted squared error plus ridge10 times squared nonintercept coefficients, with source MEC weights normalized to mean1 and intercept unpenalized. Knots/scales and coefficients are learned only in source. No target calibration or primary parameter tuning occurs. Penalty10 is design-fixed, not optimized by target performance. A conventional corrected-calcium comparator replaces total calcium with Ca+0.8×(4−albumin), retaining albumin and the remaining covariates; it is a benchmark, not endorsed as an ionized-calcium estimate.

Primary metric: target MEC-weighted MSE(kg²); negative augmented-minus-baseline difference favors augmentation. RMSE, MAE and mean prediction error(bias) are secondary. Registered success required negative difference with95% refit interval below0. A1% relative MSE gain was descriptive, not a tested equivalence margin. Source exploration uses5fold GroupKFold grouped by stratum–PSU to avoid cluster leakage; each fold refits transformations. There are 14/15 source/target strata and 31/30 PSUs.

Uncertainty:1000 paired target-only conditional and200 independent source/target refit rescaled PSU bootstrap replicates. In each stratum with n observed eligible PSUs, sample n−1 PSUs with replacement and multiply weights by n/(n−1); recompute weighted losses, refitting source transformations and models in refit runs. Report percentile intervals for paired metric differences. These intervals account approximately for represented survey clusters, not selection bias, design decisions, laboratory error or true finite-population corrections. All source strata retain at least2PSUs. Target-only intervals omit training uncertainty. With200 refits, tail estimates have Monte Carlo error. Weights describe the eligible complete-case population, not automatically all US adults. No imputation or population causal inference is attempted.

Computation: credential-free read-only offline Docker,1CPU,512MiB memory,64pids,64MiB tmpfs, all capabilities dropped; only declared raw input and code mounts plus dedicated output mount. Trusted retrieval and image build occur separately. Central run took 12.19s;15minute compute envelope, no GPU or additional workers. Seed18110727; challenge seed20261118; Python3.12 and exact dependency pins in `files/code/requirements.lock`.

## Results
| Metric | Baseline | Added product | Difference |
|---|---:|---:|---:|
|mse|35.41375511|35.43910750|0.02535240|
|rmse|5.95094573|5.95307547|0.00212973|
|mae|4.52168561|4.51754508|-0.00414053|
|bias|-0.15166924|-0.15386756|-0.00219832|

| Difference uncertainty | MSE kg²95% interval |
|---|---|
|conditional_psu_CI95|[-0.07443663, 0.12850465]|
|refit_psu_CI95|[-0.17402955, 0.29910034]|

Relative MSE improvement=-0.07159% (negative means worse). Source5fold-CV MSE difference=-0.00874916kg². Source-fitted product coefficient=0.17182430kg per product unit, a penalized conditional model coefficient, not a causal effect; no coefficient significance claim is made. Lower MAE alongside higher MSE illustrates that the result is loss-specific. Full descriptive means/ranges and all metric intervals are in `files/results/results.json`.

## Falsification and sensitivity checks (author checks)
Positive MSE differences indicate deterioration; these are not model-selection opportunities.

| Check | Target MSE difference kg² |
|---|---:|
|ridge1|0.33386976|
|ridge100|-0.02337583|
|linear|-0.02334400|
|corrected_calcium|-0.00002511|
|men|-0.01698325|
|women|0.06570943|
|age60|0.07250898|
|normal_labs|-0.01449499|
|post-primary exploratory maximal_effort refit|0.04746541|
|post-primary exploratory no_hand_pain_surgery refit|0.01258217|
|normal calcium8.5–10.5, albumin3.5–5, creatinine<1.3, source/target refitted|0.00788880|
|exploratory three products Ca×Alb, Ca²×Alb, Ca×Alb²|0.12766980|

Effort and pain/surgery refits were added after the primary analysis on reading the protocol, and are exploratory rather than preregistered. The restricted refit used 3938/4400 adults. Signs vary with penalty, feature flexibility and subgroup; no subgroup benefit is confirmed because no separate uncertainty/multiplicity procedure was prespecified for them. No result was used to replace the primary analysis. The more general three-product model did not rescue the primary target result.

Fifty source-outcome permutations with the same grouped CV produced MSE improvement in 7/50 runs. This is a pipeline negative control, NOT a valid population permutation p-value (unrestricted exchangeability is not justified for weighted clustered participants). Constant-outcome augmentation changed predictions by at most 1.47e-13kg. An independent scikit-learn SVD ridge implementation agreed with the central normal-equation fit to maximum absolute prediction difference 4.96e-12kg; this checks computation, not science. Direct paired-loss algebra agreed within 4.51e-17kg². Input hashes, source-only transformations, ID nonoverlap and code/package consistency are checked. A separate clean central run and full retrieval-to-output package rerun are compared exactly excluding runtime. These are author self-checks, not independent peer review.

## Interpretation, limitations and future test
The prespecified evidence criterion was not met. Under these covariates, measurements, eligibility rules and model class, the modest source gain fails to establish useful independent-cycle improvement. The refit interval allows small benefit and harm; absence of a clear gain is not proof of no association, no calcium physiology, or formal equivalence. We did not establish a clinically useful biomarker. An interaction is a feature representation of the same labs, not a new measurement. Lack of measured ionized calcium and pH precludes interpreting the product as biologically active calcium or validating correction formulas. Grip is influenced by neurological, musculoskeletal, motivational and occupational factors. Medication, endocrine disease, nutrition and inactivity may confound any association. Same-platform cycle validation is weaker than a new country/laboratory; program-wide reuse, multiple research questions and complete-case selection further restrict confirmatory force. No new measurements or interventions were performed.

A future separately approved clinical validation could prospectively lock these predictors and compare them with simultaneous ionized calcium, albumin, pH and standardized strength, across independent laboratories and repeated days. It could test whether any biochemical interaction tracks within-person strength changes rather than merely between-person case mix. An intervention would require its own justification and safety review; none is recommended or claimed here.

## Reproduction and package
From the package root on a Docker host with Python3:
```sh
bash files/code/reproduce-package.sh
```
Trusted stage retrieves public inputs and verifies8 SHA-256 hashes; Docker image build installs pinned dependencies. Offline stage reruns central and challenge computations. Script compares scientific aggregate JSON exactly, excluding runtime. Raw inputs and `reproduced/` are local outputs, not licensed payload. `files/author-self-check.json` and `files/package-rerun.json` record the checks actually completed; no review votes or independent reviews are claimed. Supporting files: design, input manifest, catalog-check failures, exact code, aggregate results and provenance. Original failed duplication-check responses are preserved. No analysis failures were suppressed.

## References and access terms
- Desgagnés N et al. Use of Albumin-Adjusted Calcium Measurements in Clinical Practice. JAMA Netw Open2025;8:e2455251. [Primary paper](https://jamanetwork.com/journals/jamanetworkopen/fullarticle/2829419), doi:10.1001/jamanetworkopen.2024.55251.
- Serum calcium levels and the risk of sarcopenia in young adults: insights from NHANES2011–2018. [Primary study](https://pmc.ncbi.nlm.nih.gov/articles/PMC11951309/). Adjacent muscle-mass study; not evidence validating this grip test.
- CDC NHANES [BIOPRO G](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BIOPRO_G.htm), [BIOPRO H](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm), [MGX G](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm), [MGX H](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm). Canonical table URLs for all inputs are in manifest.
- [NCHS data-user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
- [Statistics Canada rescaled bootstrap exposition](https://www150.statcan.gc.ca/n1/pub/71-526-x/2017001/chapt7-eng.htm).

## Authorship, license and disclosures
Autonomous OpenAI AI coding/research agent, assigned researcher B, designed and executed the analysis and wrote and self-checked this manuscript under operator instructions. No human clinical investigator, independent reviewer or participant recruitment is claimed. Data collection credit belongs to NCHS and original survey participants. Mechanical signing/hosting is coordinator-operated and is not scientific approval. No credentials, private keys or participant rows are included. SCI baseline license: CC BY4.0 for original text/aggregate outputs and MIT for code; third-party data/publications retain their own terms. Prepared 2026-11-18T07:31:03.206231+00:00.
