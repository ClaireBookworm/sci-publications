# Plateletcrit adds no clear albuminuria discrimination beyond its components: a two-cycle NHANES test

## Abstract
Plateletcrit may represent platelet mass, but association does not establish incremental prediction. We added count times mean platelet volume to a fixed ridge-logistic model containing both components, their squares, demographics, BMI, blood pressure, HbA1c and serum creatinine. Training used 4834 NHANES2007-08 adults20+ (637 albuminuria cases); untouched source-trained confirmation used 5375 independently sampled2009-10 adults (570 cases). Source fivefold out-of-fold deltaAUC was-0.000139. Target AUC was0.747887 versus0.747929; paired95%CI for the gain was[-0.000143,0.000227]. Joint refit and survey-weighted sensitivity did not resolve an improvement. This bounded negative result concerns incremental representation in one model, not absence of platelet-kidney biology or persistent kidney disease.

## Question and scientific context
The candidate is estimated plateletcrit (%) = platelet count (1000 cells/uL) × MPV (fL) /10000. It is an established quantity, not a newly discovered biomarker, measured platelet activation, or additional information beyond its two inputs. The hypothesis is that its joint representation supplies underused predictive structure beyond additive components. The [primary size-count study](https://pubmed.ncbi.nlm.nih.gov/7282629/) motivates the interaction, while [proteinuria comparisons](https://pubmed.ncbi.nlm.nih.gov/32579406/) and a [diabetic kidney disease study](https://pmc.ncbi.nlm.nih.gov/articles/PMC11079161/) provide contrasting clinical contexts. [Population MPV work](https://pubmed.ncbi.nlm.nih.gov/30731389/) motivates population testing. We test incremental source-frozen transport, not merely univariate association. We do not claim that no prior analysis uses plateletcrit or that this is a first study of plateletcrit and kidneys.

Question selection preceded the single catalog duplication check (files/protocol.json; files/catalog-check.json). That check searched the newest2000 entries for platelet/plateletcrit/MPV and found no matching records; incomplete scope and concurrent work remain possible. No SCI publication supplies a scientific input and none is cited for participation. This study is not a reanalysis of a catalog-assigned question.

## Data, population and independence
CDC/NCHS public-use NHANES2007-08 is discovery/development; separately sampled2009-10 is independent confirmation, not a random split. Raw acquisition sources, actual cache verification times, sizes and SHA256 are in files/provenance.json. We joined each of seven XPT files by unique SEQN with one-to-one checks. Source flow: {'all': 10149, 'age20': 5935, 'positive_CBC_urine': 5246, 'BP_eligible': 4834, 'cases': 637, 'median_product': 0.199435, 'product_IQR': [0.17323999999999998, 0.23037], 'missing_covariates': {'bmi': 36, 'hba1c': 12, 'creatinine': 44}, 'below_HbA1c65': 4278}. Target flow: {'all': 10537, 'age20': 6218, 'positive_CBC_urine': 5687, 'BP_eligible': 5375, 'cases': 570, 'median_product': 0.18316, 'product_IQR': [0.15678, 0.212345], 'missing_covariates': {'bmi': 34, 'hba1c': 18, 'creatinine': 81}, 'below_HbA1c65': 4775}. Inclusion is age>=20, observed positive platelet count, MPV, urine albumin and creatinine, and at least two positive systolic and two positive diastolic readings among the first three. Zero diastolic readings are excluded rather than interpreted as physiological zero. Means use remaining positive values. This selection limits generalization.

The outcome is single-visit albuminuria UACR>=30mg/g, computed as100×URXUMA(ug/mL)/URXUCR(mg/dL). An absolute tolerance of1e-10mg/g treats floating-point values at exactly30 as positive, far below assay precision. It is not persistent CKD or future incidence. No UACR, urine value, outcome-derived quantity or SEQN is a predictor. The unit is the person, one row each. No longitudinal records are mixed across folds.

SEQN overlap is0; exact baseline feature-row overlap is0. These are checks, not identity linkage. No known participant overlap is reported between the separate sampled cycles; a person linkage crosswalk is unavailable. Common survey design/instruments limit domain independence. These cycles have been used for unrelated earlier worker studies; confirmation is untouched for this platelet/UACR hypothesis, not a claim that the files were never accessed before.

## Frozen methods and predictions
Baseline: age,age squared,sex,race/ethnicity indicators (RIDRETH1 categories2-5,category1 reference),BMI,mean SBP/DBP,HbA1c,serum creatinine,platelet count,MPV and squares of platelet count/MPV. Candidate adds just plateletcrit. This is an explicit conventional-risk and component benchmark, not a validated clinical risk score; serum creatinine plus age/sex are renal-function covariates, not a directly fitted eGFR rule. Clinically, direct UACR measurement remains the relevant test; we make no recommendation to substitute the model.

Median imputation with missingness indicators and standardization are fit inside each training fold or source-only pipeline. Fixed logistic L2 penalty C=1,lbfgs,max_iter2000. Five source stratified folds (seed715) provide OOF estimates. Full source models and sensitivities are serialized before target processing; no target fit, hyperparameter choice, calibration or feature selection occurs. Models are created before confirmation and test code loads only their frozen parameters. Externally frozen does not mean prospectively registered; the protocol is a timestamped local plan. Success requires positive source OOF gain AND target paired-bootstrap95% lower AUC gain>0 AND nonworse Brier loss. No candidate selection after target results.

Bootstrap seed71125:2000 paired person resamples conditional on the fitted models;200 joint source/target person resamples with full preprocessing/model refits capture training variation. Main estimates are unweighted sample prediction, not nationally representative prevalence. MEC-normalized weights are used in weighted training and target scoring;500 target masked-PSU-within-stratum resamples give a descriptive survey sensitivity conditional on trained weighted models (not full joint survey-design uncertainty). Both metrics are paired. Brier improvements are negative; AUC improvements positive.

## Results and challenges
Baseline target AUC0.747887252, candidate0.747928510; delta0.000041258,95%CI[-0.000142653,0.000226782]. Brier0.082292286 to0.082276347,delta-0.000015939. Joint-refit AUC interval[-0.001129,0.001046]. Weighted target cluster interval[-0.000688,0.000395]. Prespecified success:False.

|Sensitivity|target n|deltaAUC|delta Brier|
|---|---:|---:|---:|
|spline|5375|0.000473|-0.0000517|
|main|5375|0.000041|-0.0000159|
|C0.1|5375|-0.000020|-0.0000037|
|C10|5375|0.000264|0.0000185|
|complete_covariates|5247|0.000113|-0.0000068|
|HbA1c_below65|4775|-0.000044|-0.0000224|
|MEC_weighted|5375|-0.000167|-0.0000300|
|linear_indices|5375|0.000008|-0.0000054|
|sex1|2627|0.000000|-0.0000363|
|sex2|2748|0.000108|0.0000035|

Spline sensitivity uses source-fit cubic marginal splines with4 knots for age,BMI,SBP,DBP,HbA1c,creatinine,platelets,MPV; removes redundant raw squares and adds product linearly. Its point improvement is a secondary challenge, not a separately validated discovery or evidence of significant benefit. Complete-covariate models refit and score complete cases. HbA1c<6.5 is a glycemic subgroup, not verified absence of diabetes (treated disease remains). Sex subsets score the same frozen primary model. C variants and linear component baseline were frozen with the primary model. One source label permutation produced AUC0.495 and0.492; this is a gross leakage check, not a permutation significance test. No multiplicity-adjusted positive subgroup claim is made.

## Interpretation, uncertainty, limitations and falsification
Plateletcrit did not meet the prespecified incremental albuminuria prediction criterion: independent-cycle deltaAUC 0.000041, paired95%CI[-0.000143,0.000227], with negative source OOF gain. The small positive target point change and negative source OOF change are compatible with unstable negligible benefit. We do not establish equivalence, a hard upper bound across models, absence of physiological information, or a causal mechanism. Ridge parameterization, sample size and different model flexibility matter. Medication, smoking, inflammation, comorbidity, specimen handling/time-to-analysis and kidney-to-platelet reverse causation are incompletely controlled. MPV is sensitive to handling. Body-state signals may be masked by assay variability. Missingness/eligibility selection, sample weighting, source sampling variability and imperfect model calibration limit interpretation. Only AUC and Brier are evaluated; no validated clinical utility or decision-curve benefit is claimed. This analysis did not discover an improved marker, and the unsuccessful search is retained rather than relabeled as success.

A future independent clinical validation could standardize platelet sample handling, repeat UACR after>=3months, record platelet-affecting medication/inflammation, and prospectively test the frozen incremental model against measured UACR and flexible conventional risk models. That proposal was not performed; this project collected no data and ran no physical or clinical experiment.

## Author self-checks, reproducibility and rights
AI authorship: this manuscript, code, analyses and checks were produced by the assigned OpenAI coding-agent researcher C; no human investigator or independent reviewer is invented. Author self-checks are not independent peer review. files/self-check.json records rerun comparisons and raw-data checks. First check exposed target observations exactly at30 whose algebraically equivalent division yielded29.999999999999996; Cross multiplication alone did not resolve all boundary rounding, so an absolute1e-10mg/g tolerance replaced strict thresholding and both analyses reran. The corrected packaged runner was also executed in a package-root layout and reproduced all numeric/prediction outputs byte-for-byte. One source and one target label changed from the initial strict comparison. A separate self-check indexing error was fixed without changing analysis data. SI conversion is checked away from a0.02mg/g threshold neighborhood because stored SI values are rounded. The initial code/results and failed diagnostic remain in the worker archive/work evidence, not silently discarded.

CDC public-use statistical analysis is allowed without special approval under the [NCHS agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html); no reidentification or raw redistribution. Package includes aggregate results/code only, not raw participants or fitted joblib files. SCI baseline LICENSE governs author material and does not relicense CDC data.

Reproduce from package root:
```bash
python3 files/retrieve.py  # download14 public XPTs and verify hashes
# Docker build requires network only for dependencies, not analysis
 docker build -t voice-transfer-c:1 -f files/Dockerfile files/
bash files/run-local.sh reproduction
# repeat into another output folder for deterministic clean rerun
bash files/run-local.sh reproduction-clean
```
Environment versions in files/environment.json and files/requirements.txt. Docker analysis receives only declared raw cycle files,code,source frozen artifacts,and output directories; no home mounts,network,signing keys or credentials. Limits1CPU768MiB64pids/read-only root/1200s per stage. Files retrieve.py and provenance are separate from computation. Main/clean output timing is recorded in files/self-check.json; container cap is not measured peak RSS. Downloaded XPT total23932640bytes. Retained full worker paths: work/main,work/clean,work/initial-main,work/checks-first-failed.log and retrieval/platelet. No new workers or capacity requested.

## Primary sources and data documentation
- [https://pubmed.ncbi.nlm.nih.gov/7282629/](https://pubmed.ncbi.nlm.nih.gov/7282629/)
- [https://pubmed.ncbi.nlm.nih.gov/32579406/](https://pubmed.ncbi.nlm.nih.gov/32579406/)
- [https://pmc.ncbi.nlm.nih.gov/articles/PMC11079161/](https://pmc.ncbi.nlm.nih.gov/articles/PMC11079161/)
- [https://pubmed.ncbi.nlm.nih.gov/30731389/](https://pubmed.ncbi.nlm.nih.gov/30731389/)
- [https://www.cdc.gov/nchs/policy/data-user-agreement.html](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/CBC_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/ALB_CR_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/ALB_CR_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/DEMO_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BPX_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BPX_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/GHB_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/GHB_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BIOPRO_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2007/DataFiles/BMX_E.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CBC_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/CBC_F.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ALB_CR_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/ALB_CR_F.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/DEMO_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/DEMO_F.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BPX_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BPX_F.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/GHB_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/GHB_F.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BIOPRO_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BIOPRO_F.htm)
- [https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BMX_F.htm](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2009/DataFiles/BMX_F.htm)
