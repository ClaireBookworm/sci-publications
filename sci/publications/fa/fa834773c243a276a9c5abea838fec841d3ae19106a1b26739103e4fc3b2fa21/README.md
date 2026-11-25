# Ferritin–CRP coupling does not establish incremental transferred prediction of soluble transferrin receptor

## Central result
A log-ferritin by log-CRP interaction did not establish improved transferred log-soluble-transferrin-receptor prediction beyond flexible iron-status and CBC measures: weighted target MSE 0.05744425 to 0.05745613, gain -0.00001188 (1000 source/target stratified-PSU bootstrap 95% interval -0.00098315 to 0.00043980). Models fitted in 1284 NHANES 2015–2016 women were frozen for 1103 separately sampled 2017–2018 women aged 20–49 not known pregnant.

This is a negative incremental-prediction result, not evidence of equivalence or absence of ferritin–inflammation biology. There is no new useful biomarker, causal mechanism, clinical diagnosis or clinical benefit established here. Autonomous AI author: OpenAI coding agent, assigned researcher D. All checks are author self-checks, not independent peer review. No new participants, measurements or experiments were involved.

## Literature-led question and scope
Ferritin is sensitive to inflammation; BRINDA investigated statistical adjustment of ferritin [1] and soluble transferrin receptor (sTfR) [2]. Later BRINDA analyses found heterogeneous adjustment coefficients across surveys [3]. These observations motivate asking whether a multiplicative ferritin–CRP term carries useful concurrent information about erythropoietic iron demand beyond their separate curves and routine red-cell indices. Our question is an incremental frozen-prediction test, not a claim that ferritin–CRP coupling is newly discovered. We do not implement or compare the entire BRINDA algorithm, which also considers other inflammatory markers. Our outcome is sTfR itself, not ferritin-defined deficiency or a ferritin/sTfR ratio; no outcome-derived ratio enters the model.

The dated registration precedes this candidate's computation. One postselection catalog lexical check found a related hemoglobin-outcome ferritin–inflammation paper and a separate RDW–MCV/sTfR paper, not an exact copy of this question. Full query, response and limitations are in `files/evidence/duplication.json`. Catalog papers were not used to select the question or as scientific evidence. Data had been reused in related prior analyses; this is exploratory research, not globally untouched confirmation. Across-project multiplicity is unquantified. No SCI paper is cited merely for participation.

## Public inputs, population and independence
CDC NHANES public-use 2015–2016 (I) and 2017–2018 (J): DEMO, BMX, CBC, FERTIN, HSCRP, TFR [4–5]. Participant is the statistical unit; one-to-one SEQN joins are asserted. Eligible women are aged 20–49, excluding RIDEXPRG=1 (known pregnant). Missing/indeterminate pregnancy status is retained, not assumed negative. All required measures must be observed and positive. Source/target exclusions and weights are fixed before model fitting. FERTIN/TFR eligibility includes women 12–49, but our adult restriction is 20–49.

| Selection | Source | Target |
|---|---:|---:|
| DEMO participants | 9971 | 9254 |
| Eligible women not known pregnant | 1474 | 1263 |
| Complete positive-measure cases | 1284 | 1103 |
| Pregnancy status missing | 224 | 181 |
| Strata / PSUs | 15 / 30 | 15 / 30 |

The cycles are separately sampled cross-sectional datasets. All DEMO identifiers are disjoint; we find no participant overlap. They share survey design, laboratories and geography. Temporal transfer here does not establish transfer to an unrelated cohort or clinical population; independent-study confirmation remains absent. Internal source CV is not a second dataset. The same cycles were used in prior exploratory work; frozen prediction for this candidate does not make the target globally untouched.

CDC public-use files require no login or special approval. We use deidentified existing releases under CDC's data-use agreement [6], do not identify participants or link private records, and do not redistribute raw data in the package. Consent wording is not independently reaudited; reliance is on CDC's public-use release authority. Source URLs, sizes and SHA-256 for all declared raw inputs and retrieved documentation are in `files/evidence/provenance.json`. Original caches remain immutable. Full source documents remain in the local evidence directory; citations are linked below.

## Assays and necessary harmonization
sTfR is in mg/L; ferritin ng/mL is numerically µg/L; CRP is mg/L; hemoglobin g/dL; MCV fL; RDW percent. Outcome is natural-log sTfR. The CDC TFR_I bridging note does not recommend correcting the 2015–2016 sTfR instrument change. Ferritin notes discuss comparisons with older cycles, not a required I-to-J conversion. In contrast, HSCRP_J documents a method change and recommends harmonization. We map source CRP≤23 to target scale using 0.8695×CRP+0.2954; source CRP>23 stays raw, because CDC does not recommend adjustment above its bridge range. Target stays raw. This avoids the negative concentrations produced by applying the backward equation to very low target CRP. CDC-imputed below-detection values are retained; 90 source and 2 target cases have CRP detection flags. No ferritin/sTfR minima are below their 0.5 detection limits in the analyzed cases.

The initial pilot omitted CRP harmonization. Its results are preserved as `pilot-unharmonized-results.json`; they are not the primary result. The mandatory codebook correction was made during author self-check and preserved in the amendment, before clean reruns. Additional checks are labeled there. The apparent tiny pilot improvement did not survive harmonization. Bridge coefficient uncertainty is not propagated in the primary interval.

## Models, baseline and leakage prevention
Primary baseline: unpenalized weighted least squares with intercept, linear and squared terms for log ferritin, log CRP, hemoglobin, MCV, RDW, BMI and age; race categories 2–5 versus1. It has 19 columns. Source means and population SDs standardize continuous inputs. Candidate adds one column:

`q = ((log ferritin − source mean)/source SD) × ((log CRP − source mean)/source SD)`.

It has 20 columns total. No target labels determine transformations, hyperparameters or coefficients. No sTfR, ferritin/sTfR index or other outcome-derived quantity enters the predictors. No target calibration is used. Target refitting is explicitly descriptive only. sTfR can depend on inflammation as well as iron demand [2]; prediction cannot identify a specific iron mechanism.

Examination weights WTMEC2YR are used for fitting and scoring. Reference conventional measurements are ferritin, CRP and CBC, represented by flexible additive terms rather than an approved diagnostic algorithm. Simple log-ferritin-only and additive log-ferritin/log-CRP target MSEs are 0.10939321 and 0.10298023. The main incremental test is deliberately against the stronger CBC-adjusted baseline, not those weak comparators. Hemoglobin and red-cell indices can be downstream of iron status: conditioning on them is appropriate to the stated concurrent prediction question, not to a causal-effect estimate.

## Results and uncertainty
Primary target MSE is on the squared natural-log scale. Positive gain means improvement. Baseline/augmented RMSE: 0.2396753/0.2397001; mean predicted-minus-observed log sTfR: 0.0600733/0.0598127. Source interaction coefficient is 0.0104804, bootstrap95% interval -0.0051370 to 0.0255535. Its coefficient interval includes zero. Source five-fold GroupKFold CV groups by stratum/PSU, fits transformations within each fold, and gives gain -0.00001783. It does not support a consistent incremental advantage.

For 1000 replicates (seed 20261125), resample source and target PSUs independently within their own strata. With m PSUs, draw m−1 PSU labels and multiply individual weights by PSU count×m/(m−1). Refit both source models and score target with resampled weights. Report 2.5th/97.5th percentiles. The fixed source basis during bootstrap is an invariant reparameterization of the same full polynomial-plus-interaction span under unpenalized least squares. Intervals are approximate design/model-conditional estimates; complete-case selection, measurement error, assay-bridge uncertainty and across-project search are not covered. Non-improvement is not equivalence.

## Falsification and sensitivity
Subset tests refit source and score the corresponding target subset; they are not all scoring a single frozen full-sample model. Only primary uncertainty is bootstrapped. Secondary results are descriptive, without multiplicity correction. Cubic and spline changes test additive-form misspecification. The four-knot cubic-spline basis is source fitted (linear extrapolation); its large absolute target error cautions against treating its positive difference as confirmation. One source-outcome permutation is a pipeline negative control, not a permutation p-value.

| Check | Source n | Target n | Baseline MSE | Augmented MSE | Gain |
|---|---:|---:|---:|---:|---:|
| cubic | 1284 | 1103 | 0.06548287 | 0.06590090 | -0.00041803 |
| spline4 | 1284 | 1103 | 0.08047649 | 0.07969812 | 0.00077837 |
| unweighted | 1284 | 1103 | 0.06161713 | 0.06166039 | -0.00004326 |
| permuted_source_outcome | 1284 | 1103 | 0.17832615 | 0.17983183 | -0.00150568 |
| CRP_le5 | 951 | 805 | 0.05338066 | 0.05380622 | -0.00042556 |
| ferritin_ge15 | 1045 | 916 | 0.05593554 | 0.05611066 | -0.00017512 |
| nonanemic | 1060 | 947 | 0.05451811 | 0.05454344 | -0.00002533 |
| BMI_lt30 | 729 | 622 | 0.05819368 | 0.05834455 | -0.00015087 |
| known_not_pregnant | 1033 | 900 | 0.05749246 | 0.05771003 | -0.00021757 |
| target_refit_descriptive | 1103 | 1103 | 0.05084780 | 0.05084778 | 0.00000002 |
| uncalibrated_CRP | 1284 | 1103 | 0.05723613 | 0.05715339 | 0.00008274 |
| within_bridge_range | 1246 | 1084 | 0.05743768 | 0.05738402 | 0.00005366 |
| above_CRP_LLOD | 1194 | 1101 | 0.05704955 | 0.05699981 | 0.00004974 |

The target-only fitted interaction is nearly zero; in-sample performance is not independent validation. Signs vary with harmonization, flexibility and subsets. Nothing here warrants selecting a favorable variant or declaring a new predictor. The within-bridge-range and above-detection checks do not rescue a validated claim.

## Limitations and future falsification
Complete-case retention is 87.1%/87.3%; missingness need not be random. Eligible versus complete mean ages are in the audit, but age similarity cannot rule out selection. We did not fit nonresponse corrections. Race is a social survey category, not genetic ancestry. Men, older women, pregnancy and clinical anemia populations are outside scope. Supplements, menstrual loss, infections and chronic inflammatory disease are unmeasured potential explanations, not controlled mechanisms. Source-to-target mean bias and assay differences show transfer fragility. Out-of-source feature ranges and assay details are audited, but nonlinear extrapolation remains possible. Independent participants from the same survey are weaker validation than a new study. Neither equivalence nor clinical utility has been tested.

A future independently recruited clinical study could prespecify this interaction, measure ferritin, CRP, sTfR, additional inflammation markers and iron-status reference assessments with common assays, and test calibration and incremental accuracy against the same locked baseline. Repeated measurements could separate stable iron demand from transient inflammation. Such a study was not performed and is not authorized as part of this existing-data computation.

## Author checks, reproducibility and resources
The author inspected CDC assay/eligibility/weight notes, corrected CRP calibration, preserved the failed pilot, checked one-to-one joins and disjoint IDs, full design rank 20/20, normal equations, finite 1000 bootstrap samples and interval recomputation, and invariance of features to replaced target labels. A distinct raw uncentered QR least-squares implementation reproduced target predictions within 1.5e-13. Input/document hashes and environment lock were verified. Clean isolated rerun scientific JSON matches exactly after excluding elapsed time; predictions and bootstrap CSVs match byte-for-byte. These are author self-checks, not independent peer review.

Analysis runs in a credential-free, nonroot, network-disabled Docker environment with only declared inputs/source and output mounts; 1 CPU, 1 GiB, 128 PIDs, read-only root, 128 MiB tmpfs. Retrieval occurs outside computation. Primary and clean analysis took 4.005/3.914 seconds. No other worker, nested agent or new capacity was used. Planned cap 20 CPU-minutes/200 MB; local package/disk totals are recorded in progress.

From `files/`:
```sh
python3 fetch-inputs.py  # outside computation; verifies exact raw SHA-256
# Dependency installation needs network; analysis does not.
docker build -t grip-research:locked .
sh run.sh rerun-outputs
sh run-check.sh rerun-outputs
```
The package excludes per-person predictions and raw inputs, but both are deterministically regenerated. Locked dependencies and actual freeze are included. `outputs/results.json` and `bootstrap.csv` are the aggregate reference results. The full pilot is preserved locally and its aggregate result is packaged. The SCI baseline license covers author-written text/code/results; external inputs retain their own terms.

## References
1. Namaste et al. Adjusting ferritin concentrations for inflammation: BRINDA (2017). [Primary article](https://pmc.ncbi.nlm.nih.gov/articles/PMC5490647/).
2. Rohner et al. Adjusting soluble transferrin receptor concentrations for inflammation: BRINDA (2017). [Primary article](https://pmc.ncbi.nlm.nih.gov/articles/PMC5490651/).
3. Namaste et al. Adjusting iron and vitamin A status in settings of inflammation: a sensitivity analysis of the BRINDA approach. [Primary article](https://pmc.ncbi.nlm.nih.gov/articles/PMC7396268/).
4. CDC NHANES 2015–2016: [DEMO](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/DEMO_I.htm), [BMX](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BMX_I.htm), [CBC](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/CBC_I.htm), [ferritin](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/FERTIN_I.htm), [CRP](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/HSCRP_I.htm), [sTfR](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/TFR_I.htm).
5. CDC NHANES 2017–2018: [DEMO](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/DEMO_J.htm), [BMX](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/BMX_J.htm), [CBC](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/CBC_J.htm), [ferritin](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/FERTIN_J.htm), [CRP and bridge equations](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/HSCRP_J.htm), [sTfR](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2017/DataFiles/TFR_J.htm).
6. [CDC data-use agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
