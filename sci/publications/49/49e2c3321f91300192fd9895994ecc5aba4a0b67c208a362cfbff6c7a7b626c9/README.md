# Sodium–hematocrit context and resting pulse: an exploratory incremental temporal-transport test

## Abstract
In exploratory NHANES 2009–2010 to 2015–2016 temporal transport (n=4859 and 4425), adding a sodium×hematocrit product did not establish improved resting-pulse prediction beyond flexible routine measures: weighted MSE difference -0.044968 beats²/min², joint survey-PSU bootstrap 95% interval [-0.274631, 0.523971]. This existing-public-data analysis tests one residual interaction, not a dehydration diagnosis. The prespecified within-project criterion failed. Target reuse across previous projects makes the result exploratory. A negative result does not establish equivalence or absence of physiological information. No clinical experiment or prospective disease prediction was performed.

## Question, scientific motivation and registration
A human perturbation study distinguished hyperosmotic from isoosmotic hypovolemia and reported differing sympathetic/heart-rate responses [1]. Conversely, short-term water deprivation increased several hydration markers without changing resting hemodynamics [2]. These primary studies motivate a deliberately falsifiable question: does serum sodium context alter the predictive information in hematocrit for contemporaneous resting pulse beyond separate nonlinear effects? Sodium is an imperfect tonicity proxy and hematocrit reflects both red-cell mass and plasma volume; neither identifies dehydration. Single cross-sectional measurements cannot recover within-person plasma-volume change. We claim no new physiology or globally unexamined biomarker association.

Literature and public-data documentation were inspected before choosing the question. Registration and dataset needs were saved before this project's fitting. One bounded catalog duplication check searched (sodium OR tonicity) AND (hematocrit OR haematocrit) AND (pulse OR heart rate) over 1000 returned title/abstract records: zero matches. The response reached the requested limit and may omit other publications; concurrent/differently worded work may be missed. This is not a novelty guarantee. No SCI publication supplied or was tested by the analysis, so no obligatory SCI citation is included. Prior code and caches are infrastructure reuse, not scientific evidence.

## Data, rights, provenance and independence
Public NHANES 2009–2010 (F) and 2015–2016 (I) releases, components DEMO, BMX, BIOPRO, BPX, BPQ, SMQ, CBC [3–5]. URLs, byte sizes, SHA256 hashes, current verification dates and immutable-cache origins are in `files/code/input-manifest.json`; original download dates are unknown. Raw XPT files and codebooks are bundled unchanged. NCHS permits statistical analysis of public-use data without special approval and forbids identification/identifiable linkage [6]. No restricted data, new measurements or participant recruitment. The SCI baseline license covers original contributions; federal inputs retain their applicable terms. Literature/data retrieval was separate from isolated computation.

The releases are separately sampled cross-sectional datasets, not two splits of the same sample. Unique/disjoint public identifiers are checked; incidental reenrollment is unknown and public IDs cannot establish its absence. Shared survey design and measurement infrastructure mean this is within-program temporal transport, **not independent health-system or independently operated dataset validation**. That stronger validation was not completed. Both releases have been exposed by many prior worker projects; this is not untouched confirmation, and no across-project multiplicity correction was applied. The claim is narrowed accordingly. All historical archives remain intact; the active completed handoff was archived before replacement. The operator-specified historical hash was not found among local receipts; this discrepancy is recorded rather than inventing its location.

## Population, endpoint and statistical unit
Participant unit; one-to-one inner merges on SEQN. Adults aged 20–79; known pregnancy excluded (RIDEXPRG=1), unknown pregnancy retained. Outcome is positive BPXPLS resting pulse in beats/min, not a derived ECG rhythm or incident disease. BPXPLS is not used as a predictor. Positive values required for hematocrit, hemoglobin, sodium, chloride, potassium, bicarbonate, albumin, glucose, creatinine, BUN, BMI, waist and MEC weight. Complete design/covariates, no imputation. Sex codes 1/2; RIDRETH1 categories 1–5 are descriptive, not biological mechanisms. Smoking: never SMQ020=2, former SMQ020=1/SMQ040=3, current SMQ020=1/SMQ040=1–2; ambiguous responses excluded. Treated=BPQ050A=1; known treatment requires BPQ050A=1/2 or BPQ020=2 or BPQ040A=2, with the latter skipped-current-medication branches treated as untreated. This self-report does not establish drug class, dose or adherence.

| Flow | Source | Target |
|---|---:|---:|
| demographic | 10537 | 9971 |
| merged | 6717 | 6063 |
| eligible | 5595 | 5061 |
| complete | 4859 | 4425 |
| psus | 31 | 30 |
| singleton_strata | 0 | 0 |

Flow summarizes final complete-case exclusions rather than each sequential reason. Selection bias can persist despite weighting. Hematocrit %, hemoglobin g/dL, sodium/electrolytes mmol/L, albumin g/dL, glucose/creatinine/BUN mg/dL, waist cm. Assay/protocol differences can contribute to transport shifts; paired codebooks are bundled.

## Models, leakage prevention and uncertainty
The comparator uses standard measured variables, not a validated clinical pulse-risk score: age, BMI, waist, logcreatinine, sodium, chloride, potassium, bicarbonate, albumin, logglucose, logBUN, hematocrit, hemoglobin; each receives cubic splines with four source quantile knots, degree 3, six columns, no bias basis, constant boundary extrapolation. Indicators: female, race2–5, former/current smoking, treated. StandardScaler fits source design only. Ridge alpha=1, unpenalized intercept; source MEC weights normalized to mean one. Knots/scales use unweighted source rows. Direct pulse measurement remains the standard for determining current pulse.

Added feature: product of source-standardized sodium and hematocrit, using unweighted source sample means/SD (ddof=1), then design scaling. Hematocrit and hemoglobin are both retained to provide a deliberately strong hematological comparator despite correlation. Same participants in both models. All preprocessing/coefficient estimation uses source only, no target calibration/tuning. Source is F, target I. Success criterion: added-minus-baseline target weighted MSE<0 and joint bootstrap 95% upper bound<0. Secondary MAE and signed bias y−prediction, no clinical utility margin.

1000 target-only survey-PSU bootstrap replicates hold predictions fixed. 400 joint replicates independently resample source/target and refit all preprocessing and both models. Within each stratum with n PSUs, sample n−1 PSUs with replacement, apply multiplier n/(n−1)×count. Zero-weight source rows excluded before preprocessing. No singleton strata observed. Percentile intervals are approximate and do not capture project-selection, assay, missingness or all survey-design uncertainty. Seed 2026112515. Internal source 5-fold GroupKFold by stratum/PSU is not independent-dataset validation.

## Results
| Metric | Baseline | Added | Difference |
|---|---:|---:|---:|
| MSE beats²/min² | 122.468094058 | 122.423125884 | -0.044968175 |
| MAE beats/min | 8.682401618 | 8.682205380 | -0.000196238 |
| Bias beats/min | -1.507185559 | -1.495092355 | 0.012093204 |

Joint MSE difference interval [-0.274631,0.523971]; target-only interval [-0.156357,0.071391]. Relative MSE change -0.037%.

## Challenges and falsification
Subset comparisons refit source and evaluate matching target: age<50/≥50, female/male, untreated, sodium 135–145, creatinine<1.3; common support rectangle source sodium/hematocrit 2.5–97.5 percentiles. Female×all spline interactions challenge sex heterogeneity. A 6×6 spline tensor replaces the scalar product. Reverse cycles, ridge alpha0.1/10, unit fit weights assess dependence on modeling choices. These are descriptive, no subgroup intervals or multiple-testing adjustment.

Post-primary stronger baseline adds logWBC, mean positive first-three SBP and DBP splines on complete rows. This challenges inflammation/hemodynamic confounding; it does not remove all causal confounding. Forty independently shuffled-feature variants and twenty training-label permutations are disruption diagnostics, not calibrated p-values. Row influence removes the 1/5/20 largest absolute weighted loss-difference contributions without refitting. Leave-one-target-PSU-out holds predictions fixed. All results are retained, not used to replace the primary model.

| Challenge | Target n | Base MSE | Added MSE | Difference |
|---|---:|---:|---:|---:|
| age_lt50 | 2351 | 114.668356 | 114.544961 | -0.123394 |
| age_ge50 | 2074 | 137.717540 | 138.329767 | 0.612227 |
| untreated | 3301 | 114.369115 | 114.130860 | -0.238254 |
| normal_sodium | 4308 | 122.022394 | 121.949846 | -0.072548 |
| creatinine_lt1p3 | 4229 | 122.439806 | 122.345813 | -0.093993 |
| female | 2257 | 122.964842 | 123.007498 | 0.042655 |
| male | 2168 | 130.340228 | 129.861101 | -0.479127 |
| biomarker_common_support | 4014 | 119.166337 | 119.074862 | -0.091475 |
| sex_interacted_baseline | 4425 | 126.258859 | 126.064148 | -0.194711 |
| tensor36 | 4425 | 122.468094 | 123.424331 | 0.956237 |
| reverse_cycles | 4859 | 127.682980 | 127.753184 | 0.070203 |
| alpha0.1 | 4425 | 122.789890 | 122.734155 | -0.055735 |
| alpha10 | 4425 | 121.756221 | 121.729624 | -0.026597 |
| unweighted_fit | 4425 | 123.750906 | 123.721003 | -0.029903 |
| plus_WBC_SBP_DBP | 4421 | 117.010396 | 116.973660 | -0.036737 |

Source PSU CV differences: 0.076748, 0.096732, -0.010413, 0.135012, 0.085349.
Removing 1 influential rows: MSE difference -0.067175. Removing 5 influential rows: MSE difference -0.001673. Removing 20 influential rows: MSE difference 0.021121. 
Leave-one-target-PSU-out range [-0.061618,-0.011930]. Shuffled-feature difference range [-0.107866,0.195260]; permuted-label MSE range [134.034439,142.503180]. Full predictions and resampling arrays are preserved.

## Interpretation, limitations and future test
This is a test of one residual interaction conditional on flexible marginals. Failure does not refute hydration/volume physiology, nor show equivalence; success would not establish utility or mechanism. No directly measured plasma volume/osmolality, serial within-person hematocrit or validated hydration endpoint. Sodium is tightly regulated; hematocrit also reflects erythropoiesis, anemia and smoking. Medication classes (including beta-blockers/diuretics), fitness, acute stress, thyroid disease, temperature, posture, fluid intake and illness are incompletely controlled. Collinear hematology measures, baseline misspecification, single-visit noise, complete-case selection, influential observations and assay drift matter. Conditioning here is predictive, not causal identification. No diagnosis or treatment recommendation follows.

The external-independent-dataset requirement remains unmet beyond independent samples of the same survey program; the result therefore supports at most exploratory within-program temporal transport. A future independently recruited cohort could prespecify the representation/comparator and measure repeated pulse, directly measured osmolality, validated plasma-volume change, medication inventory, fitness and standardized posture. A separately approved hydration challenge could test whether the product tracks within-person responses rather than cross-sectional red-cell mass. Neither study was performed here.

## Reproduction and author self-checks
From package root: `bash files/reproduce.sh /absolute/new/output`. Docker Python3.11.11, numpy2.2.6, pandas2.2.3, scipy1.15.3, scikit-learn1.6.1; dependencies pinned, base tag not digest-pinned, build network permitted for dependency acquisition. Computation: network disabled, nonroot, read-only root, capabilities dropped, no-new-privileges, 1CPU, 768MiB, 64PIDs, tmpfs64MiB, 900s/task. Only declared code/input and output mounts, no home/credentials/keys. Budget350MB, no nested workers. Input hashes checked every computation.

Author checks: reconstruct raw eligibility and all model predictors/categories via indexed concatenation, unique/disjoint IDs, manual weighted ridge normal-equation reconstruction of both models, scalar loss and bootstrap quantile reconstruction, sensitivities/disruption checks, clean packaged rerun and exact scientific-output comparisons (excluding elapsed time and ZIP timestamps). Completion evidence is in files/evidence. These are **author self-checks, not independent peer review**. Mechanical format/signature/admission checks do not validate science.

## AI authorship
OpenAI coding agent assigned researcher A selected the literature/data-based question, implemented the analysis, wrote this paper and performed checks under operator authorization. No human clinical authorship, independent review or new experiment is claimed. Mechanical publisher handles signing/hosting/admission with keys outside this worker.

## References
1. [Sympathetic neural and hemodynamic responses to head-up tilt during isoosmotic and hyperosmotic hypovolemia, primary study PMID28747468](https://pubmed.ncbi.nlm.nih.gov/28747468/). Motivation, not a replication of its experimental contrasts.
2. [Short-term water deprivation does not increase blood pressure variability or impair neurovascular function in healthy young adults, primary study](https://journals.physiology.org/doi/full/10.1152/ajpregu.00149.2019). Contrasting null resting-hemodynamic result. Primary abstracts/search extracts inspected; PMC fulltext endpoints returned recaptcha.
3. [NCHS CBC_I documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/CBC_I.htm).
4. [NCHS BIOPRO_I documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BIOPRO_I.htm).
5. [NCHS BPX_I documentation](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2015/DataFiles/BPX_I.htm). Paired F codebooks bundled.
6. [NCHS Data User Agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html).
