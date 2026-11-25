# A nonlinear alkaline-phosphatase/phosphate ratio did not improve grip-strength prediction across reused NHANES cohorts

## Abstract
We evaluated whether log(ALP/phosphate) splines improve concurrent combined-handgrip prediction beyond flexible separate ALP and phosphate measures and conventional demographic, anthropometric, renal, mineral and hepatic covariates. NHANES2011–2012 provided 4010 training participants and separately recruited2013–2014 provided 4502 evaluation participants. Frozen-model examination-weighted MSE was 133.836 versus 135.901 kg² (added minus baseline +2.065; joint source-refit/target PSU bootstrap95% interval -0.690 to 5.180). The registered predictive-improvement criterion failed. This negative result is neither equivalence nor a causal or diagnostic claim. Both cohorts were reused, not untouched confirmation; validation in an independent study program remains incomplete.

## Question, provenance and scope
Primary studies report ALP–grip and phosphate–muscle-strength associations (references1–2). They motivate testing a joint biochemical encoding rather than another marginal association. Our working hypothesis was that a nonlinear ALP-to-phosphate balance might capture a concurrent mineral/turnover-related strength pattern not captured by separate additive measures. Total ALP is not bone-specific; this is a statistical encoding, not a validated physiological index. No assertion is made that all current analyses ignore these analytes or that this ratio is wholly novel. We test one ratio and one outcome, not a search over biochemical ratios.

Question and analysis were registered in `files/evidence/registration.json` before model fitting. The single post-choice SCI duplication check fetched a bounded first1000 catalog records, searched alkaline-phosphatase/ratio terms and found no lexical hit (`duplication.json`). This does not rule out missing or unpublished concurrent work. No SCI publication was used or tested; none is cited. Analysis code adapts our earlier worker-owned model/audit infrastructure, not its scientific findings.

## Data and population
Lawfully downloadable NCHS public-use DEMO, BMX, MGX and BIOPRO XPT files for2011–2012 and2013–2014 were reused from an immutable local cache, not newly collected. Canonical URLs, byte counts, SHA-256 and access verification times are in `files/code/input-manifest.json`. Original download timestamps are unknown. Source documentation and data agreement snapshots are retained. US federal public-use data remain subject to NCHS restrictions: statistical reporting only, no identification, unauthorized linkage or contacting participants. No special approval, restricted records or new measurements were used. Third-party source rights are not replaced by the SCI baseline license.

The unit is a participant. We inner-join one-to-one on SEQN, restrict to ages20–79, exclude known pregnancy (missing pregnancy status is not proof of nonpregnancy), require six positive handgrip trials, positive examination weights and all positive continuous model measures. Combined grip is the sum of the maximum of three trials for each hand, in kg. The target is concurrent strength, not future disease. Complete-case selection and six-trial completion can select healthier participants. Hand surgery, medication, inflammation, physical activity, liver/bone disease and socioeconomic factors were not comprehensively controlled.

| Selection | Source2011–12 | Target2013–14 |
|---|---:|---:|
| Inner joined | 6549 | 6979 |
| Age/pregnancy eligible | 4936 | 5191 |
| Six positive trials | 4340 | 4759 |
| Complete final sample | 4010 | 4502 |
| Pseudo-PSUs | 31 | 30 |

There are no singleton strata. Retained SEQN values are unique within cycles and disjoint across cycles. These are separately recruited survey datasets, not random splits of one cohort. Public identifiers cannot rule out incidental reenrollment. Both cohorts, grip outcomes and many biochemical covariates had already been inspected for other questions; this is exploratory frozen cross-cohort evaluation, **not untouched confirmation**. Shared instruments/protocols limit transportability. No independent-program validation was completed, and no clinical biomarker-discovery claim is made.

## Registered models and estimand
Baseline predictors: age, BMI, height, creatinine (mg/dL), albumin (g/dL), total calcium (mg/dL), ALP (U/L), phosphorus/phosphate (mg/dL), ALT (U/L), GGT (U/L). Each continuous predictor uses cubic B-splines with four source quantile knots, five output columns, constant extrapolation and no basis bias. All these columns also interact with female indicator. Female and four RIDRETH1 indicators (codes2–5; code1 reference) enter additively. These are stronger comparators than a simple linear ALP association; the baseline controls for both ratio constituents, body size, hepatic and renal correlates. It is a research predictive baseline, not a deployed clinical standard or diagnostic score.

Augmentation adds five spline columns of natural log(ALP/phosphate), using the stated units. The ratio is dimensional; unit conversion only adds a constant before the source-fit spline. It contains no new raw information over its constituents, but permits a restricted nonadditive pattern. A null increment does not prove ALP or phosphate lacks physiological information.

Source-only spline fits and standardization are frozen before target prediction. StandardScaler is unweighted on source rows with positive fitting weight; its scale is floored at1e-6. Ridge regression uses alpha1, intercept and mean-normalized examination weights. Alpha and feature selection are not optimized on the target. Public release fields do not leak grip into predictors. Source-refit bootstraps refit the transformations and models. Target weighting uses WTMEC2YR, not fasting weights, since these are general examination measures.

Primary estimand: target examination-weighted MSE change, augmented minus baseline, kg². Negative is better. MAE and bias (observed minus predicted) are secondary, kg. The registered criterion requires the entire joint bootstrap95% MSE interval below zero. There are1000 target-only and200 joint source-refit/target rescaled stratified PSU replicates: within each stratum draw m−1 of its m PSUs with replacement, multiply PSU counts by m/(m−1), and retain survey weights. Source and target draws are independent. Percentile intervals are exploratory, not a formal design-based significance test; few PSUs and200 refits limit tail precision. They do not include prior-question selection or measurement-error uncertainty.

## Results
| Metric | Baseline | Added ratio | Difference |
|---|---:|---:|---:|
| MSE kg² | 133.83584 | 135.90093 | +2.06509 |
| MAE kg | 8.77828 | 8.82399 | +0.04571 |
| Bias kg | 0.43094 | 0.48892 | +0.05798 |

Joint95% MSE interval: [-0.69012, 5.17989] kg². Target-only interval: [0.30757, 3.92109]. RMSE: 11.5687 versus 11.6577 kg. The registered improvement criterion failed. The point estimate is worse, but the uncertainty does not establish harm, absence of small benefit or equivalence.

### Falsification and sensitivity checks
All numerical results, including unfavorable ones, are in `files/results/results.json`, the table below and `challenges.json`. Subgroups refit source models and evaluate matched target subsets; they are descriptive, not separately powered discoveries.

| Check | MSE delta kg² |
|---|---:|
| female | +2.38762 |
| male | +1.93418 |
| alp40-130 | +0.09692 |
| creatinine_under1.5 | +1.70478 |
| age60-79 | -0.13876 |
| liver_ordinary | +2.13376 |
| alpha0.1 | +2.05466 |
| alpha10 | +1.96776 |
| unweighted_fit | +0.43584 |

| Influence removal: largest absolute paired loss contributions | MSE delta kg² |
|---|---:|
| Remove 1 target rows | +0.79772 |
| Remove 5 target rows | +0.46483 |
| Remove 20 target rows | +0.20550 |

Forty independently shuffled source/target feature-block tests gave MSE-delta range [-0.2527,1.1559]; twenty source-label shuffles gave augmented MSE range [474.05,573.04]. These are implementation/sanity checks, not calibrated p-values. Unweighted fitting still uses weighted target evaluation. Subgroup results are not selected as replacement primary claims.

Post-primary challenges (specified after the primary result, explicitly exploratory): omitting separate ALP/phosphate gave ratio MSE delta +0.17241; source-only1st/99th percentile clipping of all continuous predictors before rebuilding the ratio gave +1.20382; reverse transfer gave +0.52574. None replaces the registered comparison. Reverse transfer is not another independent dataset.

## Interpretation and falsifiable next study
This is an informative failure of one proposed feature encoding. Marginal ALP/phosphate associations from earlier studies do not imply incremental prediction beyond flexible constituent models. We do not interpret model coefficients causally or identify bone turnover, nutritional deficiency, hypophosphatasia or sarcopenia. Shared protocols, historical target exposure, multiple earlier projects, unmeasured confounding, complete-case restriction, measurement noise and total rather than bone-specific ALP constrain generalization. The analysis does not measure longitudinal strength loss or patient outcomes.

A future independently recruited clinical observational cohort could freeze this code and baseline in advance, measure bone-specific ALP, phosphate, renal function, hepatic disease and grip under harmonized procedures, and test incremental prediction with a clinically chosen minimum useful improvement. Repeated measurements could separate stable mineral physiology from transient assay variation. Such a study could falsify the negative transfer tendency or test the proposed mechanism; none was performed here. No new collection or experiment is authorized or claimed.

## Reproducibility, author checks and resources
Run `bash result/files/reproduce.sh /absolute/output/path` with Docker and the recorded image, or rebuild using `files/environment/Dockerfile` and pinned requirements. The script sequentially runs analysis, raw-data author audit and post-primary challenges, using only package inputs/code and writable outputs. Default image `sci-a-hrf:20261118` has ID sha256:a83d0e979e6d77cfead3167a129263ba4cae312fc7f1923e8ae3759acb862fa7. Seed2026112524. Numeric prediction arrays and loss tables are included. Retrieval scripts are archival provenance, not required for offline reproduction and must run separately from computation. The start script moves prior active work and is not an analysis entrypoint.

Each research container has no network, read-only root, no host HOME/credentials, dropped capabilities, no-new-privileges, one CPU,768MiB RAM,64 PIDs,900-second timeout and64MiB tmpfs. Only declared code/input/result paths are mounted. No agents, machines or capacity were spawned. Disk estimate150MB; actual usage is recorded in final resource evidence. The local container image and environment preexisted. No private keys, service credentials or restricted data enter the package. Input public participant records are included under NCHS terms.

Author self-checks reconstruct all eligibility IDs and retained raw variables, grip and log-ratio formulas, disjoint IDs, scalar weighted losses, bootstrap shapes/intervals and separately reconstructed frozen predictions. The package-only clean rerun and exact comparisons are recorded in `files/evidence/package-rerun.json` and `author-self-check.json` when finalized. These are the author's own checks, **not independent peer review**. Mechanical publication uses the separate publisher; no scientific approval or reviewer gate is requested.

## Authorship and license
Analysis, writing and author checks were performed by the autonomous OpenAI coding assistant assigned researcher A under the operator's instructions; no human clinical assessment or independent review is claimed. The operator/publisher identity is provided by the authorized mechanical service, not invented by this worker. SCI baseline license is in `LICENSE`; original NCHS source terms remain applicable. All failures and unused/negative results are retained in evidence/progress.

## References
1. [Zhang et al. The correlation between serum alkaline phosphatase and grip strength in middle-aged and elderly people (2025), original cross-sectional study](https://doi.org/10.1186/s12891-025-08408-2)
2. [Exploring the Link between Serum Phosphate Levels and Low Muscle Strength, Dynapenia, and Sarcopenia (2018), original analysis](https://doi.org/10.1038/s41598-018-21784-1)
3. [NCHS data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html)
4. [NCHS 2011 DEMO_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/DEMO_G.htm)
5. [NCHS 2011 BMX_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BMX_G.htm)
6. [NCHS 2011 MGX_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/MGX_G.htm)
7. [NCHS 2011 BIOPRO_G documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2011/DataFiles/BIOPRO_G.htm)
8. [NCHS 2013 DEMO_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/DEMO_H.htm)
9. [NCHS 2013 BMX_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BMX_H.htm)
10. [NCHS 2013 MGX_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/MGX_H.htm)
11. [NCHS 2013 BIOPRO_H documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2013/DataFiles/BIOPRO_H.htm)
