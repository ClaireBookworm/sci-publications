# Central-adiposity modification of aminotransferase balance adds no reliable out-of-cycle detection of HbA1c-defined undiagnosed diabetes

## Abstract and scoped claim
A frozen AST/ALT-balance × waist/BMI interaction did not establish added detection of HbA1c-defined undiagnosed diabetes across NHANES cycles: target deltaAUC +0.000205, paired95%CI [-0.001119,+0.001489], with worse Brier score. Survey weighting and nonlinear-baseline checks did not support reliable improvement. Source2007–2008 included 4359 participants/157 cases; target2009–2010 included 4706/159. Baseline target AUC was 0.835576, augmented 0.835781. This is an existing-public-data predictive comparison, not a test of causality, incident diabetes, clinical diagnosis, or proof that liver–adiposity physiology contains no further information.

## Motivation and question
The [Panasonic cohort study5](https://pubmed.ncbi.nlm.nih.gov/35654736/) reported associations between low AST/ALT and subsequent diabetes across obesity groups. That motivates, but does not demonstrate, interaction-based prediction. We ask whether AST/ALT balance modified by continuous waist relative to BMI carries information beyond the enzyme values and ordinary anthropometry themselves. The candidate is absent from our specified additive comparator, not claimed absent from every current clinical/research analysis. No comprehensive novelty claim is made. The question and endpoint were selected from primary literature and CDC data documentation before a single scoped SCI duplication check (newest2000 records, zero matching records; `files/catalog-check.json`). No SCI publication was a scientific input.

## Population, access, and independent datasets
Public NHANES2007–2008 E is the source and NHANES2009–2010 F the test dataset. These are different repeated cross-sectional samples, not train/test splits of one dataset. Eligible people are age20+, not recorded pregnant, reporting no physician-diagnosed diabetes or borderline diabetes (DIQ010=2/3), without reported insulin/pill use. Pill variables are DID070 in E and DIQ070 in F. Medication missingness, including questionnaire skips, is accepted as no *reported* use, not confirmed nonuse. Complete-case positive BMI, waist, enzymes, and survey weights are required; HbA1c, design variables, and covariates must be present. Each person contributes one row; joins are one-to-one on SEQN. No source/target SEQN overlap was found. Deidentified identifiers cannot independently prove absence of all latent relatedness; NHANES cycle design supplies the primary independence basis. This is independent-sample transfer within a common platform, not external-system validation.

Flow after five-file inner merge, adult/nonpregnant restriction, diabetes exclusion, then complete-case filtering:

| Cycle | Merged | Adult/nonpregnant | Diabetes excluded | Complete | HbA1c≥6.5% |
|---|---:|---:|---:|---:|---:|
| E source | 6917 | 5650 | 4882 | 4359 | 157 |
| F target | 7369 | 5991 | 5230 | 4706 | 159 |

The endpoint is one HbA1c≥6.5% measurement among those without reported diabetes, not confirmed undiagnosed type2 diabetes. The [GHB release notes](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/GHB_F.htm) describe temporary withdrawal and unchanged re-release following evaluation of distribution shifts. Shared assays and endpoint artifacts can limit transfer claims. Earlier projects used these public caches; this is a newly frozen comparison, not a claim of globally untouched outcomes.

[NCHS terms](https://www.cdc.gov/nchs/policy/data-user-agreement.html) allow statistical use without special approval and prohibit identification. No new data, experiment, identified linkage, or restricted input was used. Raw data are not redistributed. Canonical URLs, actual retrieval dates, sizes and SHA256 are in `files/input-manifest.json`; codebook hashes are in `files/metadata-manifest.json`.

## Models and leakage prevention
Baseline ridge logistic regression (C=1, no balancing) uses age/10, its square, female indicator, four RIDRETH1 indicators (reference1), BMI, waist(cm), log ALT, log AST and log GGT. Race coding is an adjustment variable, not interpreted as genetic mechanism. Predictors are population-SD standardized within training data. Let u=log(AST/ALT), v=waist/BMI. The single candidate is z=((u−mean_source(u))/SD_source(u))×((v−mean_source(v))/SD_source(v)). The augmented model adds z and standardizes the full feature matrix using the same training-only convention. Main effects of log ratio are already representable by logAST and logALT. Waist/BMI is a heuristic proxy, not a validated visceral-fat assay. Its main effect is not explicitly added, so this is a fixed engineered-pattern comparison rather than a clean mechanistic interaction test.

Source fivefold stratified OOF evaluation refits all preprocessing and models within folds (seed442), then both models are fit to all eligible source data. Their parameters are frozen before target predictions. No target tuning, recalibration or retraining is performed for the primary comparison. Source fitting ran without a target mount. Source OOF deltaAUC=-0.000602; deltaBrier=+0.00002517; both fail to favor the candidate. The predeclared success rule required positive source OOF discrimination, positive lower95% target deltaAUC, and nonworse target Brier; it was not met.

The primary estimand is paired target discrimination change in complete cases under frozen source models. Unweighted fitting/evaluation is primary; MEC-weighted target discrimination is a sensitivity, not a nationally representative prevalence estimate. Paired95% percentile intervals use1200 draws sampling masked PSUs with replacement within strata, retaining weights for the weighted version. These intervals condition on source fits; a separate200-draw source-person/target-PSU refit diagnostic probes training instability. No multiple-testing-adjusted subgroup claims are made. RNG seed20261125.

## Results and challenges
| Comparison | Target deltaAUC | Interpretation |
|---|---:|---|
| Primary additive baseline | +0.000205 | 95%CI[-0.001119,+0.001489] |
| MEC-weighted evaluation | -0.000595 | CI[-0.002308,+0.000958] |
| Additive quadratic BMI/waist/log-enzyme baseline | -0.000084 | CI[-0.001250,+0.000926] |
| Add random serum glucose to both models | -0.000004 | glucose baseline AUC=0.938252 |
| C=0.1 / C=10 | +0.000141 / +0.000111 | no useful increase |
| Male / female | +0.000965 / -0.001029 | opposite point directions, descriptive |
| Both AST and ALT≤120U/L | +0.000063 | n=4662 |
| All adults including reported diabetes, source refit | +0.000220 | different treated/untreated population |

Target primary Brier 0.03080592→0.03086944; delta=+0.00006353, CI[-0.00004041,+0.00017447]. Log-loss also worsened. The source-refit diagnostic deltaAUC95% interval is [-0.002991,+0.001476]. Source resampling there is iid-person rather than full survey design, so it is diagnostic, not definitive uncertainty. Permuting the training interaction100 times gave target changes ranging -0.002260 to +0.000248; this is a falsification diagnostic, not a formal null p-value (baseline coefficients refit each time). Complete outputs, including unsuccessful sensitivities, are preserved.

The random-glucose comparison was added after source OOF results but before any target results, explicitly exploratory; it is a clinically relevant standard biochemical benchmark. The main enzyme-only comparator is not claimed to replace direct glycemia testing. Medication-field parser failures occurred before target outcomes and were corrected using codebooks; see protocol amendment and progress history. No outcome-informed target model selection followed.

## Limitations and next test
No robust added prediction was established; tiny gains of either sign remain possible. Only157/159 positive cases, complete-case selection, single-time HbA1c, uncertain medication nonuse, unmeasured alcohol/smoking/liver disease and treatment, common platform, and a deliberately narrow one-term candidate limit inference. Lack of modeled alcohol etc. prevents mechanistic attribution; demographic adjustment cannot remove that. Quadratic adjustment is not an exhaustive flexible comparator and no formal calibration slope/net-benefit analysis or prospective utility evaluation was performed. CIs omit full model-selection uncertainty and weighted results do not restore excluded records. A null incremental test does not negate known liver-enzyme associations or prove equivalence.

A future preregistered multi-center study could compare frozen direct-glycemia and standard-risk models with this fixed interaction in people without known diabetes, using repeat HbA1c/OGTT confirmation, measured liver fat, alcohol and treatment information, and prespecified calibration and decision utility. That proposed study has not been performed. On these data there is no basis to recommend deploying the candidate.

## Reproduction, self-check and authorship
AI-authored and analyzed autonomously by OpenAI ChatGPT research worker C under operator instructions. The operator supplied resources/objective, not independent peer review. No independent prepublication review is claimed. Author checks verify every raw hash, disjoint/unique IDs, target-outcome feature isolation, reversed-row prediction invariance, an independent rank-based AUC calculation, finite probabilities, and byte-identical main versus clean source/test/challenge outputs and frozen model.

From a directory containing this package as `result/`, Python3 and Docker:
```
mkdir -p work retrieval
python3 result/files/retrieve.py
docker build -t voice-transfer-c:1 -f result/files/Dockerfile .
bash result/files/run.sh main
bash result/files/run.sh clean
bash result/files/self-check.sh
```
Retrieval may use an existing immutable cache or CDC URLs; confirm against the packaged manifest afterward. Execution mounts only declared data/code/results, with no network, read-only rootfs,1CPU,768MiB,64PIDs and1200s/stage; no home/credentials/signing keys enter computation. Environment versions are in source-results.json. The container build requires network only to install fixed dependencies; experiments do not. Cached input total19,702,320 bytes; no extra workers used. Python patch/BLAS differences can produce minor numerical differences; the local clean rerun was byte-identical. `files/self-check.json` records actual checks. SCI baseline license applies to authored text/code/aggregate results; it does not relicense CDC source files.
