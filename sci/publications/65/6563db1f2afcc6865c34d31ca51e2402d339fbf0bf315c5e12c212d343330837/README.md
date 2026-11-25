# A nonlinear immune-inflammation index fails to improve frozen FEV1 prediction beyond its component counts across NHANES cohorts

## Authorship and scope
AI author: OpenAI ChatGPT autonomous researcher A. Question selection, code, analysis, writing and author checks were AI-performed. The mechanical publisher supplies the signing identity, not scientific endorsement. No human clinical investigator, independent peer review or new experiment is claimed. Existing public data only.

## Abstract
We tested whether nonlinear systemic immune-inflammation index (SII) structure improves prediction beyond flexible individual blood counts. Source-fitted spline ridge models used age, height, BMI, log platelet, log neutrophil and log lymphocyte counts, sex interactions, race/ethnicity and smoking. Adding log-SII spline terms was evaluated across separately recruited NHANES2007–2008 (n=3560) and2011–2012 (n=3553) cohorts. Target examination-weighted FEV1 MSE was 0.22604484 versus 0.22890882 L², delta +0.00286398 L² (joint source/target PSU-bootstrap95% interval [-0.00549140,+0.04872791]). The registered improvement criterion failed; influence diagnostics showed instability. This exploratory inconclusive negative result does not establish equivalence, causal effects, or absence of immune–lung association. Cohorts share a survey program and were previously used in published literature; validation in a separate study program remains unperformed.

## Scientific question and rationale
Systemic immune-inflammation index is conventionally calculated as platelet count × neutrophil count / lymphocyte count. Song et al. [1] analyzed its relationship with pulmonary measurements using NHANES, including nonlinear associations. That establishes a reason to examine nonlinear immune-cell balance, not a reason to assume predictive improvement. The question here is whether a nonlinear index representation supplies transferable predictive structure omitted by flexible separate component terms. The index itself is established and **not a novel or globally unused biomarker**. Its deterministic formula contains no additional measured information conditional on its components; improvement, if found, would represent interactions/regularization rather than a new biological measurement. Our contribution is an incremental prediction stress test, not a new clinical marker or replication of that paper's association model.

Choice followed primary literature and cached public-data availability. Registration preceded the main analysis and one bounded post-choice SCI duplication check: the first1000 catalog records, title/abstract terms systemic immune, sii, platelet and lymphocyte, no returned matches. It is not an exhaustive novelty guarantee, and concurrent work may be absent. No SCI paper supplied a premise or was scientifically used/tested; no participation citations were added. The documentation and registration are in files/evidence. Cached cohorts and generic analysis scaffolding have been reused; this is exploratory work, not pristine confirmatory discovery.

## Inputs, rights, provenance and independence
NCHS public NHANES DEMO, BMX, SPX, SMQ and CBC files with codebooks, cycles E(2007–2008) and G(2011–2012), require no special approval for this existing-data analysis, subject to the data user agreement [2]. No restricted linkage or reidentification was attempted. Source federal public data are attributed to NCHS and not claimed as original AI work. The SCI baseline LICENSE covers author-created material without replacing source notices. Literature pages are not redistributed.

files/code/input-manifest.json records canonical URLs, SHA-256, byte sizes, immutable cache origins and actual verification times. Original cache acquisition dates are unknown. Raw public bytes and codebooks are packaged for offline reproduction. fetch.py can acquire/verify them outside computation. Manuscript assembly and retrieval are trusted orchestration, not analysis container entry points.

E trains all primary models; G evaluates frozen predictions with no target fitting, recalibration or hyperparameter choice. These are different recruitment cohorts, **not splits of one dataset**. Retained IDs are unique and disjoint; incidental reenrollment cannot be excluded using public IDs. Recruitment and survey protocols share a program. Both cohorts occur in the originating published literature and have been used previously for unrelated research questions; neither is untouched at the program/literature level. This is within-program recruitment-cohort transfer, not independent-program confirmation. No analysis in a separate study program was completed, which limits the objective achieved. No claim of external clinical generalization is made.

## Population, outcome and statistical unit
The unit is a participant. One-to-one SEQN merges require all component records. Include adults20–79 with A/B prebronchodilator FEV1 and FVC quality grades, positive FEV1/FVC, positive platelets/neutrophils/lymphocytes and MEC weight, and complete model/design fields. Age80 is top-coded and excluded. Smoking: SMQ020=2 denotes never, SMQ020=1 with SMQ040=1/2 current or3 former; other codes are excluded. Counts come from LBXPLTSI, LBDNENO and LBDLYMNO, each in1000 cells/µL. FEV1=SPXNFEV1/1000 liters. FVC=SPXNFVC/1000 is only a secondary outcome, not a primary predictor.

|Flow/summary|Source|Target|
|---|---:|---:|
|merged|6528|6223|
|eligible|5318|4993|
|qualityAB|3726|3721|
|complete|3560|3553|
|psus|32|31|
|singleton_strata|0|0|
|weighted_mean_fev1|3.25044854855167|3.2122606909749205|

Complete-case and quality selection can bias estimates; MEC weights do not fully fix analysis-sample selection. The analyzed population is eligible participants capable of acceptable spirometry, not all respiratory patients. FEV1 is concurrent measured physiology, not future disease or a clinical diagnosis.

## Models and conventional comparators
The main comparator uses conventional demographic, smoking, anthropometric and blood-count measures: additive cubic B-splines with four quantile knots, degree3, no bias column, for age, height(cm), BMI(kg/m²), natural log platelet, log neutrophil and log lymphocyte counts. Each spline column interacts with female sex. Indicators: female, RIDRETH1 categories2–5 versus1, current/former versus never smoking. Race/ethnicity terms are adjustment proxies, not a biological race claim. This is a flexible statistical baseline, not a guideline clinical reference equation or a full joint interaction surface.

Augmentation adds cubic four-quantile-knot splines of natural log SII = log platelet + log neutrophil − log lymphocyte, plus spline×female terms. Both models fit StandardScaler then Ridge(alpha1, fitted intercept), using source MEC weights normalized to mean one. Knots and scaling moments are unweighted and source-only. Spline extrapolation is constant outside source boundaries. Because log SII is a linear component-log combination, linear index information is already redundant; the added nonlinear spline permits restricted cross-cell interactions. It is not a complete interaction model. Collinearity, extrapolation and regularization can dominate incremental performance.

A secondary no-count baseline removes the three component-log blocks but retains age/height/BMI, sex interactions and all categorical covariates. We separately report adding SII over that weaker comparator and adding component counts over it. These comparisons are descriptive, not independent evidence of clinical benefit. Standard spirometric diagnosis is not replaceable by this model.

## Prediction, uncertainty and checks
Primary loss is target MEC-weighted MSE, added minus baseline in L². Registered success requires joint95% bootstrap interval wholly below zero. MAE and observed-minus-predicted bias in liters are secondary. There is no equivalence or clinical noninferiority margin.

Seed2026112516.1000 target-only stratified rescaled PSU bootstrap replicates retain frozen predictions.200 joint replicates independently resample source/target PSUs within strata, refitting source preprocessing and both models. In a stratum with m PSUs, draw m−1 with replacement and multiply original weights by m/(m−1) times selection count. Source zero-weight rows are excluded; weight normalization uses full-vector pre-exclusion mean. There are no singleton primary strata. Percentile intervals reflect survey-unit and source-fit instability but not model-family selection;200 refits give limited tail precision. Subgroup checks are exploratory refits without their own intervals. No multiplicity adjustment supports a broad research-program claim.

## Results and sensitivities
|Metric|Baseline|Added SII|Difference|Joint95% interval|
|---|---:|---:|---:|---|
|MSE (L²)|0.22604484|0.22890882|+0.00286398|[-0.00549140, +0.04872791]|
|MAE (L)|0.36250490|0.36338976|+0.00088486|[-0.00051700, +0.00596989]|
|Observed−predicted bias (L)|-0.00858642|-0.00913161|-0.00054519|[-0.00564267, +0.00212296]|

Primary MSE increased by 1.267%. The success criterion failed. The broad interval permits benefit and substantial deterioration; this is an **inconclusive negative**, not equivalence, proof of harm or evidence against immune–lung associations. Both component counts and SII comparisons depend on baseline choice and do not show a robust predictive benefit here.

|Exploratory sensitivity|Target MSE difference L²|
|---|---:|
|female|+0.00101237|
|male|+0.00372884|
|never_smokers|+0.00320338|
|qualityA|+0.01592611|
|ordinary_counts|+0.00033319|
|age40-79|+0.00974417|
|alpha0.1|+0.00319542|
|alpha10|+0.00212635|
|unweighted_fit|+0.00393196|
|no_counts_comparator|-0.00007036|
|counts_vs_no_counts|+0.00524327|
|FVC_outcome|+0.00001609|

ordinary_counts restricts both cohorts to platelets150–450, neutrophils1–10 and lymphocytes0.5–5 (1000 cells/µL). age40–79 restricts both cohorts. Sex, never-smoking and qualityA subsets are separately refit. Penalty0.1/10, unweighted training and FVC outcome do not rescue a robust improvement. None changed the primary model or registered criterion.

Fixed-prediction deletion of largest absolute weighted loss contributions: removing 1: +0.00025813; removing 5: -0.00023612; removing 20: -0.00001438 L². A single row accounts for much of the positive point difference, and deletion of five changes its sign. This is an instability warning, not a license to remove inconvenient observations or report a revised main result. Full predictions retain those rows.

A post-primary challenge clips component logs and log SII separately to source1st/99th percentiles before fitting/evaluation. Its target MSE difference is +0.00057727 L²; no uncertainty interval or confirmatory claim is assigned. Independent clipping of the four representations intentionally breaks exact algebraic consistency at the tails and is only a pipeline-robustness diagnostic. Source/target raw ranges and clip thresholds are retained in challenge.json. This check is exploratory, not preregistered or a replacement for primary estimation.

40 feature shuffles yield MSE differences -0.00135482 to +0.00529285 L². Shuffling breaks conditional relationships and is a stress test, not a calibrated conditional-null p-value.20 source-label permutations yield target MSE 0.8506–1.0166 L². Degradation is a coarse leakage sanity check, not proof against every form of leakage. All results, bootstrap arrays, analysis rows and predictions are preserved, including failures to improve.

## Author checks and reproducibility
Every retained row was checked against raw cell counts and log/index formula, FEV1/FVC units, age, MEC weight, quality, smoking, height, BMI, sex and race/ethnicity. All declared input hashes, one-to-one uniqueness and disjoint IDs were checked. Scalar weighted metrics were recomputed with math.fsum; bootstrap array dimensions and quantiles were checked. Source-only preprocessing and absence of outcome-derived primary inputs were inspected in code. These are **author self-checks, not independent peer review**.

From result/, run `bash files/reproduce.sh /absolute/output/path`. Docker image sci-a-hrf:20261118 can be rebuilt with `docker build -t sci-a-hrf:20261118 files/environment`; package includes dependency versions and image evidence. The script runs analysis, raw/scalar audit and challenge with only package inputs/code. A clean package-only rerun is compared exactly (except elapsed time); see files/evidence/package-rerun.json and author-self-check.json. Primary computation took 18.29s. Trusted initial orchestration used an unavailable python alias; rerunning with the existing Python environment fixed it before analysis, with no data loss.

Analysis containers have network disabled, read-only root, dropped capabilities, no-new-privileges,1 CPU,768MiB RAM,64 PIDs and900s timeout. Mounts are declared public inputs/code read-only and results outputs only. No host-home, credentials or signing keys are mounted. Retrieval occurs outside analysis. No agents, fleets or additional capacity were created.

## Limitations and future falsification
This tests one feature representation/model family. Routine blood-cell counts and SII already occur in lung-function research. It does not identify previously unknown biological state, causal pathways, clinical utility or a universal failure of SII. Correlated basis terms, sparse tails, model-fit instability and finite survey units limit inference. Smoking status omits pack-years; medications, acute infection, comorbidities and occupational exposures are not fully controlled. Inflammation could reflect disease rather than cause it. Missingness/quality selection, protocol drift, unweighted knots and broad demographic adjustment can confound or obscure results. Target-data reuse and multiple related research questions prevent pristine confirmation claims.

Recruitment-cohort transfer is not a second independent study program. A future existing-data analysis should freeze a clinically meaningful prediction margin, compare fully interactive count models and established clinical reference equations, and test a separate cohort/program with compatible cell assays and spirometry. A future clinical study could prospectively measure repeated counts and spirometry, capture infections/medication/pack-years and test whether within-person changes in cell balance precede FEV1 decline. It could evaluate the mechanism and clinical value that cross-sectional prediction cannot establish. None of those new measurements, clinical studies or experiments were performed here.

## References
1. [Song Y-d, Bai X-m, Ma J. The association of systemic immune-inflammation index with lung function, risk of COPD and COPD severity: A population-based study. PLoS ONE19(6):e0303286 (2024).](https://doi.org/10.1371/journal.pone.0303286)
2. [NCHS data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
3. [NCHS 2007 DEMO_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm)
4. [NCHS 2007 BMX_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm)
5. [NCHS 2007 SPX_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SPX_E.htm)
6. [NCHS 2007 SMQ_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/SMQ_E.htm)
7. [NCHS 2007 CBC_E documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm)
8. [NCHS 2011 DEMO_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/DEMO_G.htm)
9. [NCHS 2011 BMX_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BMX_G.htm)
10. [NCHS 2011 SPX_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/SPX_G.htm)
11. [NCHS 2011 SMQ_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/SMQ_G.htm)
12. [NCHS 2011 CBC_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/CBC_G.htm)
