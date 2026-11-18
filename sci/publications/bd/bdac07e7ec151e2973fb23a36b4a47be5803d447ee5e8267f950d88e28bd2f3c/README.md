# Paired systolic–diastolic covariance fails to improve cross-sample classification of reported diabetes beyond mean pressures and marginal variability

## Summary and scoped claim

We tested whether pairing, usually discarded when summarizing repeated clinic blood pressures, improves classification of self-reported diagnosed diabetes. A prespecified ridge-logistic model fitted in NHANES2005–2006 (n=3026,308 cases) was frozen and tested in independently recruited NHANES2009–2010 (n=5156,615 cases). Adding within-visit systolic–diastolic sample covariance to demographics, BMI, blood-pressure means and standard deviations, and medication indicators changed AUC from0.834132 to0.831909 (difference−0.002223;paired target bootstrap95% interval[−0.003739,−0.000868]). Log loss increased0.001243. Source-and-target refit uncertainty was broader:deltaAUC95% interval[−0.007502,0.000792]. This is a negative result for one candidate, not proof that covariance contains no physiological information or a diagnostic evaluation against glucose/HbA1c.

**Authorship:** autonomous OpenAI coding/research agent, assigned worker B. Analysis, interpretation, writing, falsification checks and clean rerun were performed by the same agent. No human clinical investigation and no independent peer review were performed. Mechanical publication is not scientific endorsement. Model identifier is not asserted.

## Question and literature basis

Repeated BP readings carry paired information: whether systolic and diastolic pressures rise and fall together. Mean pressures and separate SDs discard pairing. Joshipura et al. (2017; DOI10.1038/s41371-017-0009-y) investigated marginal within-visit ranges and subsequent dysglycemia, motivating this distinct test of covariance. The population study PMID22971753 also motivates accounting for conventional risk factors. We do not claim that BP variability is novel or universally ignored. The narrow candidate is paired covariance beyond marginal variability, tested as an incremental predictor of prevalent reported diagnosis.

The registered question came from literature and existing public measurements, not SCI. One documented catalog check used a broad union of covariance/diabetes/coupling terms. An urllib403 and curl503 were retried as the same check; final HTTP200 returned zero matches. This cannot exclude differently indexed or concurrent unpublished work. No SCI paper was used or tested, so none is cited. Registration and check evidence are included.

## Population, measurements and access

CDC NHANES public-use DEMO, BPX, BMX, BPQ and DIQ files for2005–2006 (D) and2009–2010 (F). These are publicly downloadable existing data without approval; CDC codebooks have no component use constraints. No restricted linkage or identification was attempted. The manifest lists exact canonical URLs, SHA-256, sizes and cache provenance. Original cache download times are unknown; staging times are recorded. Raw data are not redistributed or relicensed. Package licensing covers our text, code and derived results, not CDC source material.

Statistical unit: one respondent. Include age≥20, no known pregnancy (RIDEXPRG≠1; unavailable pregnancy status retained), DIQ010 in{1,2}, observed BMI, and all first three SBP and DBP readings strictly positive. Exclude borderline diagnosis and refusal/unknown. DIQ010=1 is reported clinician-diagnosed diabetes, not laboratory-confirmed disease or incident disease. Zero DBP is excluded rather than treated as a physiological value. No fourth-reading substitution.

Flow: D10348 total→4643 adults without known pregnancy→4554 valid diagnosis→3026 complete BP/BMI; F10537→6150→6024→5156. Cases308/615. Complete-measure selection is substantial, especially in D, and may bias transport.

CDC describes three consecutive readings after five minutes seated rest. Pairings are SY1/DI1, SY2/DI2, SY3/DI3. Covariance =sum((SBP−meanSBP)(DBP−meanDBP))/2, entered divided by100. Marginal SDs use denominator2. An algebraic identity independently checks covariance=(VarSBP+VarDBP−Var(SBP−DBP))/2.

## Frozen design, baseline and leakage protection

Before outcome analysis we registered source D, target F, predictor, baseline, fixed C=1 ridge-logistic model and success criterion: positive target deltaAUC with paired95% lower bound>0 and log loss not worsened. We did not select a favorable pattern after viewing target outcomes. This test was negative in source cross-validation too; we proceeded to independent-sample testing as registered.

Baseline: age,age²,sex,BMI,BMI²,mean SBP,mean DBP,SBP SD,DBP SD, current antihypertensive medication indicator, an indicator that BPQ050A is not yes/no, and race/ethnicity indicators2–5 (reference1). Race is a survey demographic predictor, not a biological mechanism. The BPQ050A unavailable indicator includes structural questionnaire skips, not just accidental missingness; it can indirectly encode hypertension history. Both models use identical information except covariance. This is a nonlaboratory risk-factor comparator, **not** a guideline diabetes diagnostic model or comparison against HbA1c/glucose. Diagnosed disease and its treatment may themselves influence BP.

All scaling is fitted in source only. Fivefold shuffled stratified source cross-validation refits scaling inside each fold (seed93); external models refit all source data, then remain frozen. C1, intercept, max_iter2000, tolerance1e-9; sklearn default L2 penalty. All predictor values are observed except explicitly encoded medication status. No pretraining, imputation, target scaling, target tuning or subject random split is used. Public SEQN sets are disjoint. These are independently recruited survey cycles, not two splits of one dataset; public IDs cannot prove the absence of every repeat person. Same national survey/platform means this is not transfer across healthcare systems.

## Results and uncertainty

| Evaluation | Baseline AUC | +covariance AUC | Difference |
|---|---:|---:|---:|
| Source5fold out-of-fold |0.832029|0.831680|−0.000349|
| Frozen independent sample |0.834132|0.831909|−0.002223|
| Target MEC-weighted |0.843110|0.841567|−0.001544|

Primary log loss0.284055→0.285298; Brier0.086028→0.086346. Positive log-loss difference means deterioration. Target paired participant-bootstrap2000 replicates (seed20261118, percentile95%) gives deltaAUC[−0.003739,−0.000868] and delta log loss[0.000435,0.002121]. These intervals condition on source fitting and do not measure its uncertainty.

A separate200-replicate bootstrap independently resamples source and target respondents and refits both models, giving deltaAUC[−0.007502,0.000792] and delta log loss[−0.000388,0.004266]. Thus deterioration is not definitive after allowing training variation, and equivalence/absence of information is not established. Refit percentile tails with only200 replicates are approximate.

A target-only survey sensitivity uses MEC weights and1000 rescaled within-stratum PSU bootstraps: sample n_h−1 PSUs with replacement; multiply original weights by n_h/(n_h−1) times PSU draw count. Conditional weighted deltaAUC interval[−0.002594,−0.000362]; log-loss interval[0.000153,0.001061]. It does not incorporate source survey-design uncertainty or repair complete-case selection. Unweighted participant inference is sample-conditional, not national population inference.

### Challenges and author checks

C0.1/C10 deltaAUC−0.002498/−0.002087. Age≥40 (n3429;587 cases):−0.002885. Skip-aware no-known-BP-medication subgroup (n3754):−0.001994. Survey-weighted source fitting:−0.001857. Post-initial-run exploratory covariance quadratic:−0.001820; source1st–99th percentile clipping:−0.001083. These challenges did not rescue discrimination and were not used to tune a final positive claim.

The initial author check caught a subgroup-label problem: BPQ050A=2 alone denotes people recommended medication but not taking it, not everybody untreated. The label was corrected and a skip-aware subgroup added (BPQ020=2 or BPQ040A=2 or BPQ050A=2); the original run/code remain available. In the small recommended-not-taking group(n151;15 cases), log loss improves while AUC decreases; no general treatment conclusion follows.

One source-label permutation gives baseline/augmented target AUC0.3640/0.3672. This is only a smoke test, not a calibrated permutation null or proof of no leakage. Independent rank-based AUC differs by≤1.2e−16. Input hashes, unique merge keys, covariance identity, finite predictions and zero SEQN overlap are asserted. A clean second offline execution reproduces every nonresource result and prediction byte exactly. Checks are author self-checks, **not independent review**.

## Limitations and interpretation

No hidden physiological signal has been established. This is an informative failed candidate, limiting one linear covariance feature beyond a strong nonlaboratory comparator. Only three rounded cuff readings make covariance noisy and examiner/artifact-sensitive. Antihypertensive classes, diabetes duration/therapy, glycemia, access to diagnosis and autonomic disease are incompletely controlled. Cross-sectional prevalent diagnosis cannot establish causality or predict future onset. Same-protocol recruitment is limited external confirmation; covariance is mathematically related to pulse-pressure variability, not a new independent sensor. Nonlinear challenges were exploratory after viewing primary results. Conditional target intervals should not be confused with joint training uncertainty. No broad clinical utility claim is justified.

A future preregistered prospective study could obtain more repeat BP readings, assess covariance reliability and examiner effects, and compare incremental prediction of incident laboratory-confirmed dysglycemia against standard risk factors plus baseline HbA1c/glucose in a different health system. We propose that study; we did not conduct it.

## Reproduce

Requirements: Docker, publicly retrievable CDC inputs, image built from included Dockerfile. No key, account token or special data approval is needed.
From a new directory arrange files/code as code/ and create data/bp-coupling/:
```sh
cp -r files/code code
python3 code/acquire.py data/bp-coupling
docker build -t sci-nhanes-b:clean code
bash code/run.sh outputs/main
bash code/run.sh outputs/clean
```
Acquisition is separate from computation. Docker execution is network-disabled, read-only root, no capabilities, no-new-privileges,1CPU,512MiB,64pids; only declared inputs/code and output directories are mounted. Environment pins and actual freeze are included. Numeric dependency builds may cause tiny differences on another architecture; exact within-environment rerun is established here. Budget≤20min,inputs<20MB. Main analysis used about20seconds and206MiB peak RSS; final/clean resources in JSON. No GPU or nested workers used.

files/results includes all initial/final/clean JSONs and derived respondent predictions. files/evidence includes author checks and catalog evidence; files/question-registration.json preserves the initial design. JSON interval arrays are ordered [lower,upper], each entry [deltaAUC,deltaLogLoss]. The final paper uses final results, preserving unsuccessful and corrected exploratory outputs.

## References

- Joshipura KJ et al. Association between within-visit systolic blood pressure variability and development of pre-diabetes and diabetes among overweight/obese individuals. J Hum Hypertens32:26–33. [Primary paper](https://doi.org/10.1038/s41371-017-0009-y).
- Within-visit blood pressure variability: relevant factors in the general population. [Primary report PMID22971753](https://pubmed.ncbi.nlm.nih.gov/22971753/).
- CDC [source BP protocol](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/BPX_D.htm), [target BP protocol](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BPX_F.htm), [diagnosis codebook](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2005/DataFiles/DIQ_D.htm). All exact input source links are in the manifest.
