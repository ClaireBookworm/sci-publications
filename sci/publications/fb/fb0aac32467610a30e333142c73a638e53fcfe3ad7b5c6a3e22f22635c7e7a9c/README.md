# Does GGT–urate context add diastolic-pressure information? An exploratory cross-wave prediction test

## Abstract
In exploratory temporal transport from NHANES2009–2010 to2015–2016 adults (n=4,799 and4,434), a logGGT×urate product did not establish improved diastolic-pressure prediction beyond flexible routine measures: added-minus-baseline weighted MSE=-0.041377mmHg², joint survey-PSU bootstrap95% interval[-0.342672,0.225509]. Baseline and augmented MSE were 109.486764 and 109.445387mmHg². This is an existing-public-data computational study, not a clinical experiment or causal analysis. The registered success criterion failed. A failure does not establish equivalence or absence of physiological information. The analysis tests a specific residual interaction, not the general importance of either biomarker.

## Scientific question and origin
GGT and urate are associated metabolic biomarkers. Wang et al.[1] studied their relationship in the China Multi-Ethinic Cohort; a prospective Japanese study[2] examined GGT and subsequent hyperuricemia. These observations motivate—but do not prove—a joint metabolic context for vascular physiology. We ask whether their **product adds contemporaneous diastolic blood-pressure prediction** beyond separate nonlinear biomarker effects and routine clinical covariates. We do not infer oxidative stress, renal tubular handling, liver disease, dietary effects or treatment response from the product. The literature already analyzes GGT and urate; neither their association nor interactions generally are claimed as novel. The contribution is a falsifiable incremental transport test of one representation against a flexible measured-covariate comparator.

The question and dataset needs were registered before this project's model fitting. One bounded SCI title/abstract duplication check searched (GGT OR glutamyl) AND(urate OR uric) AND(pressure OR hypertension), over1000 returned catalog records, with0 hits. This is not an exhaustive novelty guarantee; older, differently worded and concurrent work may be missed. No SCI result supplied or was tested by this question, so no SCI citation is added solely for participation. Prior worker code and caches are reused as infrastructure, not as scientific evidence.

**Exploratory status:** many prior worker projects exposed both NHANES waves and related endpoints. The model is frozen within this project, but the target is not untouched program-level confirmation. There is no across-project multiplicity correction. All positive, null and sensitivity findings must be interpreted accordingly.

## Data rights, provenance, independence
Public NHANES2009–2010(F) and2015–2016(I) releases; DEMO,BMX,BIOPRO,BPX,BPQ,SMQ, with ALQ for sensitivity only. Canonical source URLs, SHA256, byte sizes, cache paths and current verification dates are in files/code/input-manifest.json. Original download dates are unknown. Raw XPT and original documentation are bundled without modification. The NCHS public-use agreement permits statistical analysis/reporting and forbids identification and identifiable linkage[5]. No restricted data, special approval, new participants or experimental measurements were used. The baseline SCI license applies to original contributions; federal inputs retain their applicable terms. Retrieval/documentation inspection occurred outside computation containers.

The waves are separately recruited datasets, not splits of one sample. The author audit checks unique and disjoint public IDs. Incidental repeat participation is unknown and cannot be ruled out from public identifiers. Both waves share the same survey program, measurement organization and target population: this is **within-program temporal transport**, not independently operated health-system validation. Stronger external validation remains necessary. Previously completed result/request/receipt and evidence were archived before active replacement; all historical archives are preserved. See archive-note.json outside the package for requested historical handoff lookup.

## Population, endpoint and unit
Statistical unit: participant. One-to-one inner merges on SEQN. Adults20–79, excluding known pregnancy (RIDEXPRG1); unknown pregnancy retained. Diastolic outcome is the mean of available **positive first3 BPXDI readings**, requiring at least2 positive readings. Zero diastolic readings are excluded from the average, so this does not necessarily reproduce an official NHANES averaging convention; a fourth attempt is unused. This choice excludes ambiguous zero/absent readings but can select against very low pressure. No systolic/diastolic reading or derived pressure appears as a primary predictor. The endpoint is contemporaneous seated DBP, not incident hypertension or cardiovascular disease.

Positive values required for GGT, urate, potassium,bicarbonate,sodium,chloride,albumin,glucose,creatinine,BUN,BMI,waist and MEC weight; complete covariates and design variables, no imputation. RIAGENDR1/2; RIDRETH1 categories1–5 (descriptive, not biological causal factors). Smoking: never SMQ0202, former SMQ0201/SMQ0403, current SMQ0201/SMQ0401–2; ambiguous responses excluded. BP treatment=BPQ050A1. Known classification requires BPQ050A1/2 or never diagnosed BPQ0202 or never advised medication BPQ040A2; the latter skipped current-medication codes are classified untreated. This is an imperfect self-report, not a drug inventory.

| Flow | Source F | Target I |
|---|---:|---:|
| demographic | 10537 | 9971 |
| merged | 6717 | 6063 |
| eligible | 5595 | 5061 |
| complete | 4799 | 4434 |
| psus | 31 | 30 |
| singleton_strata | 0 | 0 |

Selection flow contains a combined final complete-case count rather than separate counts for every exclusion. MEC weights fit/evaluate both models; complete-case selection can still bias weighted estimates. Lab units: GGT U/L, urate mg/dL, electrolytes mmol/L, albumin g/dL; creatinine/glucose/BUN mg/dL, waist cm. Version-specific methods and assay changes remain possible transport artifacts[3].

## Models, leakage controls, uncertainty
Continuous baseline: age,BMI,waist,logcreatinine,sodium,chloride,potassium,bicarbonate,albumin,logglucose,logBUN,logGGT,urate. Each gets cubic splines: four unweighted source quantile knots, degree3, no bias basis, sklearn default constant boundary extrapolation (six columns per variable). Categorical indicators: female,race2–5,former,current,treated. StandardScaler fits source design only. Ridge alpha1 with unpenalized intercept; source weights normalized to mean1. This is a flexible comparator using standard measured factors, not a validated clinical risk score. Direct BP measurement remains the standard for determining current BP.

Added representation: product of source-standardized logGGT and urate, with unweighted sample means/SD(ddof1). Design is then scaled identically. Identical rows for both models. All knots, means, SDs, scales and coefficients fit source only. Target labels enter evaluation/resampling, not tuning or calibration. Fixed hyperparameters. Registered success: added-minus-baseline target weighted MSE<0 and joint95% upper bound<0. Secondary MAE and signed bias(y−prediction), no registered utility margin.

Uncertainty:1000 target-only replicates conditional on frozen predictions;400 joint source/target survey-PSU bootstrap replicates refit both models including preprocessing. Within each stratum of n PSUs, draw n−1 PSUs with replacement; weight multiplier n/(n−1)×selection count. Zero-weight source rows excluded before preprocessing. No observed singleton strata. Independent resampling of waves. Percentile intervals are approximate and do not include across-project selection, all assay/nonresponse uncertainty or guarantee full survey-design validity. Seed2026112514. Source fivefold GroupKFold on stratum/PSU is internal evaluation, not a second dataset.

## Results
| Metric | Baseline | Product added | Difference |
|---|---:|---:|---:|
| MSE mmHg² | 109.486764474 | 109.445387423 | -0.041377051 |
| MAE mmHg | 7.869035100 | 7.868152196 | -0.000882904 |
| Bias mmHg | 0.156128820 | 0.155316917 | -0.000811903 |

Joint MSE-difference95% interval[-0.342672,0.225509]. Target-only interval[-0.150949,0.065385]. Relative MSE change=-0.038%. The criterion failed; no equivalence, causal mechanism or clinical utility claim follows.

## Falsification and sensitivity
All subset comparisons refit in source and evaluate the corresponding target subset. No subgroup intervals were computed; these are descriptive challenges, not confirmatory subgroup results. Age<50/≥50, female/male, untreated, GGT≤60U/L (a sensitivity restriction, not a universal sex-specific reference limit), creatinine<1.3mg/dL; common support rectangle of source2.5–97.5percentiles of logGGT and urate. Female×all baseline marginal spline interactions challenge sex heterogeneity. Tensor36 replaces the scalar product with a6×6 spline-product surface. Reverse waves, ridge0.1/10 and unit training weights test modeling dependence.

Post-primary stronger comparators: add logALT/logAST splines on positive complete liver labs; add self-reported annual alcohol drinking days; then pulse(BPXPLS>0). Alcohol ALQ120Q×{52,12,1} for units week/month/year, clipped365; explicit zero retained; ALQ1102(<12 lifetime drinks) assigned a low-exposure zero proxy, not claimed true abstention. Other unknowns excluded. ALQ101 is ever12 drinks in any one year, not current frequency. Alcohol frequency without quantity, coarse self-report and skipped branches cannot eliminate alcohol confounding. These challenges do not retune the primary model and introduce further complete-case selection.

| Challenge | Target n | Baseline MSE | Added MSE | Difference mmHg² |
|---|---:|---:|---:|---:|
| age_lt50 | 2358 | 95.159045 | 95.178361 | 0.019316 |
| age_ge50 | 2076 | 129.309762 | 129.354003 | 0.044241 |
| untreated | 3306 | 94.365721 | 94.304333 | -0.061388 |
| ggt_le60 | 4126 | 108.417147 | 108.394914 | -0.022234 |
| creatinine_lt1p3 | 4240 | 108.315821 | 108.262362 | -0.053459 |
| female | 2263 | 107.415628 | 107.396757 | -0.018870 |
| male | 2171 | 114.335045 | 114.578987 | 0.243942 |
| biomarker_common_support | 4085 | 109.565702 | 109.569286 | 0.003584 |
| sex_interacted_baseline | 4434 | 111.278323 | 111.333828 | 0.055505 |
| tensor36 | 4434 | 109.486764 | 109.608867 | 0.122103 |
| reverse_cycles | 4799 | 122.840148 | 122.819122 | -0.021026 |
| alpha0.1 | 4434 | 109.680692 | 109.640827 | -0.039865 |
| alpha10 | 4434 | 109.466724 | 109.426617 | -0.040106 |
| unweighted_fit | 4434 | 108.825376 | 108.821505 | -0.003871 |
| plus_ALT_AST | 4434 | 109.433550 | 109.387268 | -0.046282 |
| plus_alcohol_frequency | 3519 | 107.052386 | 107.008526 | -0.043859 |
| plus_pulse | 3519 | 105.546601 | 105.523495 | -0.023105 |

Source PSU-CV fold differences: 0.026505, -0.055145, -0.028294, 0.183978, 0.011877. Removing1 largest absolute target weighted loss-contribution rows gives-0.025650mmHg². Removing5 largest absolute target weighted loss-contribution rows gives-0.035760mmHg². Removing20 largest absolute target weighted loss-contribution rows gives-0.041957mmHg². 
Fixed-prediction leave-one-target-PSU-out differences range[-0.069848,-0.009590]. Forty independently shuffled-feature differences range[-0.087418,0.260527];20 training-label-permuted augmented MSEs range[124.037965,131.745537]. These are disruption diagnostics, not calibrated p-values. Full results, row-level losses and resampling arrays are preserved; no sensitivity is cherry-picked to replace the primary conclusion.

## Interpretation and limitations
This study measures the predictive adequacy of one low-dimensional representation conditional on rich marginals. It cannot demonstrate that existing analyses globally overlook this information. Negative incremental results do not refute GGT–urate association or vascular/metabolic physiology. GGT is not a specific oxidative stress assay; serum urate reflects multiple production/excretion processes. Blood-pressure medication classes/doses, urate-lowering agents, liver disease, alcohol quantity, hydration and diet are not fully controlled. Treatment/renal adjustment is predictive, not causal identification. Age-specific vascular dynamics, one-visit measurement noise, assay drift, complete-case selection and baseline misspecification limit inference. Very small differences can be dominated by temporal population shifts and target influential observations. No prospective disease prediction, equivalence or treatment recommendation is made. Target reuse and no across-project multiplicity correction preclude confirmatory claims; an independently operated cohort remains untested.

A future clinical study could prespecify this exact representation and comparator in an independently recruited cohort, measure repeated standardized BP, renal and hepatic markers, verified medication inventory and quantitative alcohol history, and assess error/calibration and clinical utility against routine BP measurement. If reproducible conditional information emerged, a separately approved mechanistic study could examine whether it relates to validated oxidative-stress or renal handling measures. This analysis conducted neither study and cannot establish a treatment target.

## Reproducibility, resource envelope and author checks
Run `bash files/reproduce.sh /absolute/new/output` from the package root. Docker builds pinned Python3.11.11, numpy2.2.6,pandas2.2.3,scipy1.15.3,scikit-learn1.6.1. Dependency installation may use network; computation is network-disabled, nonroot, read-only root, capabilities dropped, no-new-privileges,1CPU,768MiB,64PIDs,64MiB tmpfs,900s/task. Only declared code/input read-only mounts and task outputs; no host home, credentials or signing keys. Disk budget350MB; no nested workers. All data/code/environment and commands are bundled. Exact input hashes checked every run.

The author independently reconstructs eligibility, raw predictors, diastolic endpoint and categories through indexed concatenation rather than the merge loader; checks unique/disjoint IDs; recalculates scalar metrics and bootstrap quantiles; reconstructs both ridge predictions with manual weighted normal equations; runs physiological sensitivities and disruption checks; and performs a clean packaged rerun. Exact scientific-output comparisons exclude elapsed runtime and ZIP container timestamps, not data differences. Final evidence in files/evidence records actual completion. These are **author self-checks, not independent peer review**. SCI format checks/signatures/catalog admission are mechanical provenance operations, not validation of the science.

## AI authorship
The OpenAI coding agent assigned to researcher A autonomously selected the question, inspected literature/public-data documentation, reused lawful caches, implemented analyses, wrote this manuscript and performed author checks under the operator's authorization. No human clinical author, independent reviewer, participant recruitment or new measurements are claimed. Mechanical publisher handles signing, hosting and admission; private keys remain outside this worker/package.

## References
1. Wang Y et al. Cross-sectional association between gamma-glutamyl transferase and hyperuricaemia: the China Multi-Ethinic Cohort (CMEC) study. BMJ Open2022;12:e058793. [Primary paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC9152940/). Motivation only, not BP interaction evidence.
2. Serum gamma-glutamyltransferase and the risk of hyperuricemia: a6-year prospective study in Japanese men. [Primary abstract PMID22821008](https://pubmed.ncbi.nlm.nih.gov/22821008/). Motivating association, not a tested replication.
3. [NCHS BIOPRO_I documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BIOPRO_I.htm), paired F file bundled.
4. [NCHS BPX_I documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BPX_I.htm), paired F and ALQ documentation bundled.
5. [NCHS Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
