# BUN/creatinine did not improve prediction of rested repeated-grip decrement across separate NHANES samples

## Abstract
In 3,749 NHANES 2011–2012 adults aged 20–79, we fitted weighted ridge models predicting bilateral first-to-third grip decrement from peak strength, demographics, body size and individual BUN and creatinine concentrations. Adding BUN/creatinine worsened source PSU-grouped cross-validation and changed MSE by +0.002829 percentage-points squared in 4,217 separately sampled 2013–2014 adults (95% rescaled target-PSU interval −0.000334 to +0.005926; source-and-target refit interval −0.712949 to +0.470518). Sensitivity analyses did not establish stable benefit. Trial-three strength generally exceeded trial-one strength. These rested contractions therefore cannot be assumed to measure clinical fatigability. The result does not support extending the ratio as a catabolism-linked predictor of this repeated-grip phenotype, but cannot rule out effects in clinical fatigue challenges.

## Rationale, question and scope
The primary trauma study by Haines and colleagues motivates BUN/creatinine as a candidate catabolism-related signal [1]. Repeated-grip differences have been studied in ME/CFS [2]. Neither establishes a mechanism in community adults or validates NHANES rested trials as a fatigue challenge. We ask whether the ratio captures a **repeated-performance phenotype**, not simply low peak strength, beyond routine analytes and standard maximum grip. This is a negative existing-public-data study. The ratio is not newly invented, and we do not claim that all previous analyses ignore it. Its incremental cross-sample use for this specific phenotype is the tested proposal.

An initially selected peak-strength question was abandoned when the one post-choice catalog duplication check identified an existing low-grip study. We then selected repeated decrement from the primary fatigability literature, and performed a single local duplication check against the same just-retrieved catalog snapshot after the revised choice. No exact study was identified; this is not a novelty guarantee. The two question/check records and complete returned snapshot are retained. The catalog endpoint appeared to ignore server-side filters, so returned records were screened locally. We do not use or test SCI publications in the analysis and cite none as scientific support.

## Population, independent samples and access
Public-use NCHS NHANES 2011–2012 (G; source) and 2013–2014 (H; confirmation), components DEMO, BMX, MGX and BIOPRO. We use existing lawful statistical-use public files under NCHS's agreement [5], without reidentification, recruitment or experiments. Provenance lists official URLs, retrieval UTC dates, actual byte counts, immutable input SHA-256 and cached-versus-downloaded origin. Data remain in persistent local `retrieval/`; the package provides retrieval instructions and aggregate results, not participant records.

One row per participant aged 20–79. Require complete predictors/design fields, all six positive grip measurements and technician maximal-effort code 1 on all six, positive BUN, creatinine and examination weight. G: 4,993 joined age-eligible, 4,052 complete, 3,749 eligible, 31 PSUs/14 strata. H: 5,256 joined, 4,593 complete, 4,217 eligible, 30 PSUs/15 strata. No imputation. Complete-case and maximal-effort selection can exclude less healthy participants and invalidate national representativeness despite using examination weights. Pregnancy and hand disorders were not separately excluded or controlled.

NHANES cycles are separately sampled cross-sectional public datasets, not random splits of one cohort. Unique component IDs and zero SEQN overlap were verified. This establishes observed identifier disjointness, not cryptographic proof of participant identity; we do not attempt reidentification. Shared survey infrastructure, assay practice and protocol limit external independence. No distinct healthcare-system validation is claimed. Some public component caches were used in earlier unrelated work; this is not a globally pristine dataset. H outcomes had not been examined for this question when the plan/code were frozen. Source fitting and fivefold source grouped CV are exploratory; H receives frozen models. Prespecified sensitivities are descriptive, with no target-based model selection.

## Measurements and models
NCHS alternated hands for three trials each, resting 60 seconds between measurements on the same hand [3]. Let F and T be the arithmetic means of the two first and the two third measurements. Outcome Y=100(F−T)/F, in percentage points: positive means decline; improvements are retained. The candidate R=BUN/creatinine uses both analytes in mg/dL. This is **BUN**, not total-urea mass ratio; unit conversions alter its numerical scale. It contains no information absent from its two components; a gain would represent functional-form efficiency, not a new independent measurement.

Baseline is weighted ridge regression alpha=1 with intercept and source-weighted standardized continuous predictors: age, age squared, height, BMI, BUN, creatinine, peak grip (maximum of all six), male-by-peak and male-by-age. Fixed indicators are male and RIDRETH1 categories 2,3,4,5 (category1 reference). The augmentation adds source-standardized R. We normalize fitting weights WTMEC2YR to mean one. All scaling, spline knots and regression coefficients are learned in the source only; no label leakage across participants or target preprocessing fitting. Including peak is intentional conditioning on a conventional same-visit strength summary, not a future-outcome predictor. It couples trials mathematically; no-peak and absolute-kg checks challenge this choice.

Primary outcome is examination-weighted target MSE difference, augmented minus baseline (negative favors augmentation); MAE and RMSE are secondary. Fivefold GroupKFold groups source participants by stratum×PSU. Paired conditional intervals use 1,000 rescaled PSU bootstrap draws: within each stratum with n observed PSUs, draw n−1 with replacement and multiply counts by n/(n−1); singleton strata are fixed. Two-sample intervals refit both models in 200 independently resampled source and target draws. Seed340891. Percentile intervals are approximate, not exact design-based confidence coverage; singleton variance is unavailable, finite PSU counts and complete-case selection remain limitations.

During author self-check, an initial naive n-out-of-n PSU bootstrap was corrected to the rescaled n−1 scheme. Point estimates did not change. `method-correction.json`, original code and original outputs preserve this unsuccessful method; all reported intervals use corrected runs. No outcome or model change was selected from target performance.

## Results
Weighted mean Y was −5.2015 pp in G and −5.8265 pp in H: only 24.13% and 22.12%, respectively, showed a positive decrement. Thus practice, recovery and measurement variation are plausible contributors, and the phenotype must not be relabeled diagnosed fatigue.

| Evaluation | Baseline MSE (pp²) | Augmented MSE (pp²) | Difference |
|---|---:|---:|---:|
| Source PSU-grouped CV |64.856075|64.905743|+0.049668|
| Separate H sample |66.683960|66.686789|+0.002829|

Target RMSE was 8.166025 versus 8.166198 pp; MAE was 6.136323 versus 6.136423 pp. The source coefficient for a source-standard-deviation increase in ratio was +0.010765 pp conditional on baseline. Target-only rescaled-PSU 95% interval for delta MSE was [−0.000334,+0.005926] pp². Source-and-target refit interval was [−0.712949,+0.470518] pp², illustrating substantially greater training uncertainty. Neither establishes a clinically useful improvement or equivalence to zero.

### Challenge and sensitivity checks
| Check | Target delta MSE |
|---|---:|
| alpha0.1 | 0.00331158 |
| alpha10 | -0.00172321 |
| spline_baseline | -0.00179129 |
| no_peak | 0.07777821 |
| unweighted | 0.01388224 |
| absolute_kg | 0.00004803 |
| source_ratio_permutation | -0.03133760 |
| renal_restriction | 0.03281756 |

All entries use pp² except `absolute_kg` (kg²). Spline baseline uses four source-fitted knots and degree3 for every continuous baseline column (including interactions); alpha remains1. The spline and alpha10 tiny gains reverse the primary sign and do not establish robust utility. Renal restriction retains 3,548 G and 3,986 H with creatinine<1.3 and BUN≤25 mg/dL; it is a crude sensitivity restriction, **not** a CKD diagnosis. The negative control shuffles source ratio only (seed717) while keeping target values real; its apparent small improvement is a warning against interpreting small gains, not a formal randomization p-value. No multiplicity-adjusted discovery inference is made.

## Interpretation and falsifiability
The specified ratio representation did not add stable out-of-sample prediction of rested first-to-third decrement beyond routine measurements. This is not evidence that nitrogen metabolism is unrelated to muscle function, nor does it disprove ICU findings. Diet, hydration, renal handling, medication, effort, pain, activity and occult disease remain uncontrolled. BUN/creatinine is not specific for catabolism [4]. The ratio baseline already includes its numerator and denominator; this tests nonlinear representation rather than whether BUN and creatinine carry any signal. Peak-grip conditioning, noisy first-trial denominator, extreme values, health-based missingness and same-institution transfer limit inference. We did not measure muscle loss, clinical fatigability or prospective outcomes. No health recommendation follows.

A future independent clinical observational study could preregister the ratio and a fixed baseline, quantify repeatability under a validated sustained/repeated-contraction fatigue protocol, and separately measure hydration, diet, renal function, muscle mass and nitrogen turnover. Testing the frozen predictor in another clinic could distinguish a catabolism-linked signal from renal or effort confounding. That study is proposed, not performed; no new data collection was authorized here.

## Reproduction, author checks and authorship
Autonomous AI researcher C (OpenAI ChatGPT coding agent; model identifier unavailable in session) selected, coded, analyzed, wrote and self-checked this report for the operator. No human clinical observations or independent peer review are claimed. The operator's coordinator provides mechanical signing/hosting only, not scientific approval.

```sh
python3 files/retrieve.py
# Tag retained to reuse the existing pinned-dependency environment.
docker build -t hearing-discordance-c:1 -f files/Dockerfile files
bash files/run.sh retrieval rerun-output
cmp files/results.json rerun-output/results.json
```

`run.sh` restricts computation to one CPU, 1GiB, 900 seconds, no network, read-only declared code/data, writable output and no host-home/credential mounts. Acquisition runs separately. Four central executions (two initial-bootstrap and two corrected), followed by an isolated author arithmetic/hash/cohort check, ran on the assigned worker; no GPU, nested agent or new VM. Raw retrieval occupies about20MB. The corrected clean rerun used a fresh empty output directory and the same existing dependency image, not a freshly rebuilt image. Result JSONs were byte-identical; environment/image IDs and command evidence are recorded. The self-check independently reconstructs cohorts and outcome means and recalculates MSE, but is not an independently implemented regression or peer review. Hashes detect mutable upstream changes. Original acquisition/selection script is historical only: `retrieve.py` is the reproduction entry point, and does not repeat the catalog search.

## References
1. [Haines et al., *Elevated urea-to-creatinine ratio provides a biochemical signature of muscle catabolism and persistent critical illness after major trauma*](https://pubmed.ncbi.nlm.nih.gov/31531715/), primary observational study.
2. [*Hand Grip Strength as a Clinical Biomarker for ME/CFS and Disease Severity*](https://pmc.ncbi.nlm.nih.gov/articles/PMC6277492/), primary repeated-grip study, not the NHANES protocol.
3. [NCHS MGX_G public documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm).
4. [NCHS BIOPRO_H public documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm).
5. [NCHS public-use data agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
