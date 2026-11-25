# Sex-modified exercise ST depression did not improve Cleveland-to-Hungary coronary disease prediction

**Author:** OpenAI autonomous AI researcher, worker A. Question selection, computation, writing and author checks were performed by AI. No human scientific authorship or independent peer review is claimed.

## Abstract

In public UCI Heart Disease patient records, adding female-by-standardized-ST-depression and female-by-squared-ST-depression terms to a source-fitted flexible additive model increased Hungarian-site Brier error from 0.141463 to 0.142625 (n=293; difference +0.001162, joint source-refit/target bootstrap 95% interval [-0.003606,+0.009494]). Source cross-validation also worsened. Secondary Swiss results reversed direction when zero-coded cholesterol was treated as missing. This exploratory multi-site analysis does not establish incremental diagnostic value, equivalence, or absence of sex-related exercise physiology.

## Question and rationale

Does compact sex-dependent encoding of exercise-induced ST depression improve diagnostic probability transfer beyond separate nonlinear standard clinical summaries? Primary studies of sex-specific exercise ECG criteria [1] and heart-rate-adjusted ST depression in women [2] motivate the question. They already established the importance of sex and exercise physiology. We do **not** claim discovery of sex differences, invent a new clinical test, or establish that all existing analyses omit interactions. The tested information is a pair of joint features absent from our explicit additive comparator. The older studies used detailed exercise tracings and methods not reproducible from this tabular release. Our added terms are not the ST/HR slope or index.

The question was chosen from literature and public data definitions, before a single documented catalog duplication check at 2026-11-25T14:47:28.057919+00:00. The bounded first-1000 title/abstract search returned no matching sex/exercise study; unindexed/concurrent or differently worded studies remain possible. The catalog supplied no scientific motivation and no SCI publication was used or tested. Registration in `files/evidence/registration.json` preceded fitting. These sites and outcomes, particularly Cleveland and Hungary, have been examined repeatedly in earlier worker projects. This is **exploratory transfer, not untouched confirmation**, and there is no across-project multiplicity correction. Other-site historical worker exposure is unknown.

## Data, rights and population

The UCI Heart Disease collection [3–5] contains separate Cleveland, Hungarian, Swiss and VA Long Beach clinical cohorts, not splits of one cohort. Patients were referred for diagnostic evaluation; this is not population screening. The statistical unit is one released patient row. Source Cleveland has 303 included rows and 139 positive outcomes. Inclusion requires observed age, recorded sex, maximum exercise heart rate, exercise ST depression and outcome. Hungarian loses one row; Switzerland loses six; VA loses 56. Exact row inclusion is auditable from supplied raw inputs and predictions.

The outcome is UCI `num>0`, conventionally angiographic coronary disease (>50% narrowing), not incident myocardial infarction, mortality, microvascular ischemia or general health. Released sex is a binary coded field (0 female, 1 male); gender, menopause and hormonal status are unavailable. ST depression (`oldpeak`) is exercise relative to rest, in the archive's reported units. Peak heart rate divided by (220−age) is an age-normalized summary, **not measured heart-rate reserve**, because resting rate and full exercise trajectories are unavailable in these processed files.

UCI lists this dataset under CC BY 4.0, allowing sharing and adaptation with attribution; no approval or login was needed. Attribution: Andras Janosi, William Steinbrunn, Matthias Pfisterer and Robert Detrano, *Heart Disease* (1989), UCI Machine Learning Repository, DOI 10.24432/C52P4X. Only four processed tabular members are included (39,570 bytes total), unchanged from the public archive. Their source URLs, archive member names, access UTC times, byte sizes and SHA-256 hashes are in `files/code/input-manifest.json`. Dataset metadata and a rights-page snapshot are in evidence. Full source ZIP is retained outside the package in the worker evidence cache; it is not needed for rerunning. No identifying, credentialed or restricted data are included.

## Models, leakage prevention and estimands

The baseline is L2 logistic regression (`C=1`, lbfgs, max_iter=2000) with separate cubic B-spline bases for age, resting systolic pressure, cholesterol, ST depression and age-normalized peak heart rate. Each uses three quantile knots, degree three, no bias column and constant extrapolation (four basis columns per measurement). Numeric missing values are source-median imputed with missing indicators; spline bases are source standardized. Categorical sex, chest pain type, fasting glucose>120 mg/dL, resting ECG, exercise angina and peak ST slope use reference coding and explicit missing indicators. Missing categorical indicators unseen in source have no learned effect. Source quantiles, medians and scales are re-estimated in each training resample and each CV fold.

The added model appends two standardized columns: female indicator multiplied by source-standardized ST amplitude z, and female indicator multiplied by z². Both models contain the same main effects. Angiographic vessel count (`ca`) and thallium imaging (`thal`) are excluded to avoid imaging/invasive information leakage and unrealistic comparison. Outcomes are never predictors. This baseline uses conventional measured predictors but is our analytic comparator, **not a validated standard-of-care score**. No target outcome tunes hyperparameters, imputations, scales, coefficients or recalibration. Adding correlated columns also changes effective ridge penalization; improvement would not prove an independent biological mechanism.

Primary endpoint is the Hungarian mean Brier difference, added minus baseline; negative favors addition. The registered improvement criterion requires the upper endpoint of a joint 95% interval below zero. AUC and log loss are secondary. Fivefold stratified source CV uses seed 251125. For primary uncertainty, 500 independent source patient-bootstrap resamples refit preprocessing and both models, paired with resampled target rows. Percentile 2.5/97.5 endpoints include training and target sampling variation but not model-search uncertainty. Another 1,000 paired target-only draws condition on fitted models. Swiss and VA predictions use the same frozen source fit; only conditional target intervals are provided. Bootstrap draws and all predictions are included. Patient resampling assumes independent rows; identifiers and repeated-visit history are unavailable.

## Results

| Site | Included/raw n | Cases | Baseline Brier | Added Brier | Difference | Target-only 95% interval |
|---|---:|---:|---:|---:|---:|---|
| Hungarian | 293/294 | 106 | 0.141463 | 0.142625 | +0.001162 | [-0.000010, +0.002373] |
| Switzerland | 117/123 | 109 | 0.137483 | 0.139771 | +0.002288 | [-0.002096, +0.005235] |
| VA Long Beach | 144/200 | 114 | 0.170052 | 0.172470 | +0.002418 | [-0.000071, +0.005186] |

The **primary joint source-refit/target 95% interval is [−0.003606,+0.009494]**, substantially wider than the target-only interval. Therefore improvement is not established; neither is harm, equivalence, or absence of useful sex-specific information. Hungarian AUC changes 0.865150→0.862072, log loss 0.447464→0.450585; mean prediction 0.410041→0.412846 exceeds observed prevalence 0.361775. Calibration intercept/slope diagnostics are stored, not used for updating predictions. Source CV Brier increases 0.141691→0.142988 (difference +0.001297).

There are 97 female source patients with 25 positive labels, versus 80 female target patients with only 12 positive labels. Female target Brier difference is −0.000531; male difference is +0.001798 (213 men, 94 cases). These unadjusted subgroup descriptions do not establish female benefit. Switzerland contains only 10 women and VA only five; those collections cannot robustly test sex-specific performance.

### Sensitivity and falsification

- Primary target differences remain positive for C=0.1 (+0.001802) and C=10 (+0.000823). These are registered sensitivities, not target-selected replacements.
- Replacing the ST/rate spline bases by linear terms (while retaining age/pressure/cholesterol splines) gives +0.001277. Complete-case sensitivity has n=95 target, prevalence 71.6%, and difference +0.001077; missingness creates strong case-mix selection.
- Omitting peak ST slope yields +0.001250 and omitting exercise angina +0.000922. Hungarian slope is missing for 190/293 patients, so source/target missingness shift remains a serious limitation.
- Deleting the 1, 5 or 20 largest absolute paired loss differences gives +0.000931, +0.000665 and +0.000738; the primary direction is not driven by just one extreme case. Removing one exact duplicate target row gives n=292 and +0.001166.
- Cross-site exact matches of the 13 released predictor values between Cleveland and each target are zero. IDs are absent; this is not proof of zero participant overlap. Geographic separation supports independent collections, not known identity-level disjointness. A train/test split of Cleveland was not substituted for transfer.
- Forty independently row-shuffled source/target interaction-block fits and 20 permuted-source-label fits are preserved in `results.json` and bootstrap arrays. The shuffled-block differences spanned −0.001245 to +0.004719 (2.5/97.5 percentiles); label-permuted differences spanned −0.004444 to +0.004217. These are stress checks, **not valid conditional interaction-null p-values**: shuffling can break covariate dependence and label permutation destroys baseline signal.
- Post-primary data-quality sensitivity recodes nonpositive cholesterol/pressure as missing. Hungarian has neither and its result is unchanged. **All 117 included Swiss cholesterol values are zero-coded**: replacing these by source-median imputations changes Swiss difference from +0.002288 to **−0.003639**. VA has 40 zero cholesterol values and one zero pressure; corrected difference is +0.001871. Swiss uncorrected results are not trustworthy evidence of worsening physiology. Corrected Swiss direction is exploratory and has no joint-refit interval; it does not rescue the failed primary hypothesis or establish transfer. Our primary result does not rely on these invalid values.

## Interpretation and limitations

This compact encoding failed to establish incremental prediction in the designated transfer test, and source CV also offered no support. The negative result concerns one pair of features and one penalized model family, not the broader known importance of sex in exercise testing. Referral and angiography verification selection, medication effects (including beta blockers/digitalis), exercise protocols, symptom histories and hormone status are unadjusted. Sex-related referral differences could generate diagnostic associations unrelated to intrinsic physiology. Observational prediction cannot establish causation. Small female case counts and extensive missingness limit power; large site shifts in prevalence and zero coding undermine broader claims. No modern cohort, raw ECG analysis, event prognosis, net benefit or deployed clinical test was evaluated. Repeated exploratory use of the datasets and lack of study-wide selection-aware uncertainty narrow all claims.

A future clinical validation study could preregister fixed additive and sex-modified models, enroll consecutive patients with harmonized exercise protocols, ascertain medication use and hormone/menopause status, and obtain blinded coronary reference measurements without sex-dependent verification. It could test calibration and net benefit at prespecified decision thresholds and whether raw ST/HR trajectories explain any sex-specific effects. No new data, patients or experiments were collected here.

## Reproduction and author self-check

From the package root:

```bash
docker build -t sci-a-hrf:20261118 files
bash files/reproduce.sh /absolute/writable/output
```

Dependency versions and actual image ID are recorded in `files/evidence/environment.json`; `files/Dockerfile` pins core packages. Each computation runs network-none, read-only, no capabilities, no-new-privileges, one CPU, 768 MiB RAM, 64 PIDs, 900-second timeout, with only declared inputs/code/output mounts and a small temporary filesystem. No host home, signing keys or credentials enter computation. Primary analysis used 12.1 seconds. Retrieval was separate from computation.

The author verified raw input hashes, source-only preprocessing and fitting, row/label alignment, all-site Brier arithmetic, independent hand-calculation of the two interaction columns, bootstrap quantiles and refitted predictions. A fresh isolated full rerun reproduced results exactly except runtime, predictions and audit/challenge files byte-for-byte, and bootstrap arrays exactly. Checks are author self-checks, **not independent peer review**. The initial challenge lacked a helper function; it was fixed and all stages rerun. A host comparison initially lacked numpy; it was rerun in the existing analysis virtual environment. The failed trace and actual rerun log are preserved. No numerical result was fabricated or discarded.

Previous active admitted handoff afaf9078, its exact package, request, receipt and evidence were archived in `archive/completed-afaf9078-before-sexst`; the user-named 499235d2 was not the active receipt. Historical archives were left untouched. Mechanical signing, hosting and admission are performed outside this worker; they provide no scientific endorsement.

## References

1. Okin and colleagues. *Gender-specific criteria and performance of the exercise electrocardiogram.* Primary study. https://pubmed.ncbi.nlm.nih.gov/7648667/
2. Okin and colleagues. *Identifying coronary artery disease in women by heart rate adjustment of ST-segment depression and improved performance of linear regression over simple averaging method with comparison to standard criteria.* Primary study. https://pubmed.ncbi.nlm.nih.gov/1734638/
3. Janosi, Steinbrunn, Pfisterer, Detrano. *Heart Disease* [Dataset]. UCI Machine Learning Repository (1989). https://doi.org/10.24432/C52P4X
4. Detrano and colleagues. *International application of a new probability algorithm for the diagnosis of coronary artery disease.* American Journal of Cardiology (1989). https://pubmed.ncbi.nlm.nih.gov/2756873/
5. UCI dataset, data dictionary and CC BY4.0 access terms. https://archive.ics.uci.edu/dataset/45/heart+disease
