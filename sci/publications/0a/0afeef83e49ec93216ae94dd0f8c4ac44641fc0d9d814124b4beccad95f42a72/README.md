# Within-visit systolic BP variability and audiometric hearing loss: a cross-cycle prediction challenge

## Central result
Within-visit systolic blood-pressure SD did not demonstrate improved cross-cycle prediction of audiometric hearing loss beyond flexible mean-BP and risk-factor adjustment: Brier difference -0.000285, joint bootstrap 95% interval [-0.001729, 0.002282]. This is an informative negative predictive test, not evidence of equivalence or of absent vascular contributions to hearing. The small favorable point estimate is uncertain. No newly clinically useful biomarker was established.

## Scientific question and literature origin
Guo et al. (2024) [1] reported age-dependent associations between visit-to-visit BP variability and subsequent self-reported hearing loss in the Health and Retirement Study. That motivates asking whether variation discarded by a within-visit mean contains concurrent hearing information. Our three same-visit systolic readings measure something different: measurement noise, habituation and short-term physiology, not long-term vascular instability. This is not a replication of their longitudinal outcome, and we do not claim that BP variability or hearing associations are novel. Our contribution is the explicit cross-cycle incremental prediction test and preserved negative result. CDC component definitions were consulted before question choice; the SCI catalog was not an idea source.

## Registration and scope
`files/evidence/registration.json` records the question, inputs, population, baseline, success rule and budget before computation. One capped catalog title/abstract duplication check, query, date and response hash are in `duplication.json`; it returned no matching records. It cannot exclude concurrent or unreturned work. No SCI paper is used or tested here, so none is gratuitously cited.

1999–2000 is the fitting dataset and 2001–2002 is a separately sampled NHANES cross-section, not a random split of the first. Public SEQN identifiers do not overlap; no exact match on age, sex, mean SBP/DBP, BP SD, BMI and ear PTAs was found ({'shared_seqn': 0, 'exact_common_matches': 0}). SEQN is not a lifetime participant identifier; person-level overlap cannot be absolutely excluded. Shared survey design/curation and potentially shared locations limit independence. These widely analyzed cycles have extensive prior public and local use. This is **exploratory external-cycle evaluation, not untouched confirmation**; nominal intervals omit repeated benchmark selection and unobserved survey-site effects. No independent modern clinical validation was done.

## Population, outcome and data flow
The statistical unit is a participant aged20–69. Outcome: better-ear pure-tone average at0.5,1,2,4kHz exceeds25dB HL (first1kHz recording). This is concurrent air-conduction hearing impairment, not diagnosed sensorineural disease, central auditory processing, incident deafness or disability. Require all eight thresholds in[-10,120]dB, first three systolic/diastolic readings observed with SBP>DBP>0, BMI10–80 and positive audiometry subsample weight. No imputed BP or threshold is used. Fourth BP readings are not substituted. Sentinel values outside the threshold range are excluded, potentially excluding some severe impairment.

|cycle|raw|age20–69|complete hearing|three BP|eligible|cases|
|---|---:|---:|---:|---:|---:|---:|
|source|9965|3864|1663|1208|1202|129|
|_B|11039|4279|1864|1380|1356|108|

Many records are excluded for absent three-reading BP; the complete-case analysis and original weights do not automatically correct this selection. BMI restriction also excludes missing BMI. Medication use is coded1 if BPQ050A=1,2 if BPQ020/BPQ040A/BPQ050A indicates no hypertension/prescription/use, otherwise0 unknown. This operational rule may misclassify treatment and is not medication reconciliation. Smoking is ever100cigarettes, not current intensity. Unknown education/smoking/diabetes/medication receive explicit categories, not silent negative labels. Raw variables and derived tables permit auditing these rules.

## Model and comparison against standard measures
Comparator: C=1 L2 logistic regression, cubic marginal splines with four source-quantile knots for age, mean SBP, mean DBP and BMI; one-hot sex, race/ethnicity, education, ever100cigarettes, diabetes and BP-medication categories. Full category coding and reference levels are in `files/code/analyze.py`. Primary mean BP averages the first three readings. The added model includes sample SD of those systolic readings and its square. Both arms refit all coefficients; they differ only by these two terms. This tests incremental information beyond standard individual risk factors and mean BP, not superiority to a validated hearing-screening algorithm or audiometry itself.

Knots and weighted StandardScaler means/scales use source participants only and are recomputed within each source resample. Logistic sample weights are normalized to mean1 so the declared C has consistent meaning. Splines use sklearn default linear extrapolation; target values outside source support can be unstable. No target outcome tuning, pretrained models, audiometry predictors or subject duplicates are used. Fixed C and dimensionality were not optimized on target outcomes. Source grouped5foldCV is a diagnostic; it does not establish external validity.

BP SD median was3.055mmHg in both cohorts. Only three readings provide a noisy, discretized variability estimate. All retained SDs were below20mmHg; the registered extreme-SD exclusion therefore does not meaningfully perturb the data. Drift (third minus first) adjustment challenges habituation. Absolute third-minus-second difference is an alternative variability measure, not a discovered winning model. CDC analytic notes recommend excluding the first reading for reported BP averages; a **post-primary-run sensitivity** refits using second/third means. This additional sensitivity was added after initial execution, not part of the preanalysis registration. Initial results are preserved in workspace evidence. The three-reading mean primary was not replaced after observing results.

## Uncertainty and actual results
Primary metric is survey-weighted target Brier; added-minus-baseline negative favors variability. Registered success requires a negative point estimate and upper95%joint bootstrap bound<0. Audiometry WTSAU2YR weights are used separately within each cycle, not naively pooled. For each stratum, draw nPSU−1 PSUs with replacement and multiply original weights by draw multiplicity*nPSU/(nPSU−1). Independent source and target rescaled PSU draws give400 refits;1000 target-only draws condition on fixed fitted models. Empty replicate categories are permitted. Intervals incorporate sampled-cluster and fitting instability but not survey redesign, missing-not-at-random selection, unobserved overlaps or study-selection uncertainty.400 replicates give limited tail precision. Seed2026112540. These intervals are not interaction p-values.

|metric|baseline|added|difference|joint95%interval|
|---|---:|---:|---:|---|
|Brier|0.059067|0.058782|-0.000285|[-0.001729,0.002282]|
|log loss|0.206521|0.205595|-0.000926|[-0.005522,0.013295]|
|AUC|0.864248|0.864789|0.000541|[-0.017071,0.008529]|

Target weighted prevalence: 0.0777. Target-only Brier95%interval [-0.000914,0.000310]. No equivalence margin was registered and no equivalence conclusion follows.

## Falsification and sensitivity results
|analysis|evaluation n|baseline Brier|added−baseline Brier|
|---|---:|---:|---:|
|C0.1|1356|0.058912|-0.000173|
|C10|1356|0.059350|-0.000322|
|linear|1356|0.058869|-0.000374|
|drift_adjusted|1356|0.059141|0.000465|
|alternative_diff23|1356|0.059067|0.000857|
|unweighted|1356|0.061236|-0.000198|
|exclude_sd20|1356|0.059067|-0.000285|
|reverse|1202|0.061560|0.000778|
|female|738|0.034693|-0.000637|
|male|618|0.086803|0.000115|
|age20to49|892|0.021719|-0.000006|
|age50to69|464|0.141009|-0.000899|
|conventional_mean23|1356|0.059182|-0.000277|
|source_group_cv|1202|0.062465|0.000559|
|label_shuffle|1356|0.078567|0.000453|

Reverse transport and source CV are exploratory diagnostics, not untouched confirmations. Fixed-model sex/age subgroups are descriptive and have no subgroup-specific confidence intervals.40 source-pattern shuffles and a shuffled-source-label fit are gross pipeline checks, not valid conditional-randomization p-values. Their full results are preserved; no favorable sensitivity rescues failure of the primary criterion. No causal effect is estimated. Occupational/recreational noise, otologic disease, middle-ear function, medication class, duration of hypertension, kidney function and precise smoking exposure remain unmeasured/unmodeled confounders. Audiometry is not separated by conductive versus sensorineural mechanisms. Age adjustment cannot remove all these biases. A BP pattern might carry information in another endpoint, age range or setting; this result does not exclude that.

## Lawful inputs, provenance and reproducibility
CDC public-use DEMO, AUX1/AUX_B, BMX, BPX, SMQ, BPQ and DIQ files for1999 and2001 were acquired or reused from immutable local caches. Canonical public URLs, actual reuse/retrieval UTC, SHA256, sizes and cache origins are in `files/code/input-manifest.json`; archived component documentation is supplied with raw XPTs. NCHS public-use agreement [2] permits statistical research without special approval, prohibits attempted identification and does not imply NCHS endorsement; analyses and interpretations here are ours. No identity linkage, restricted data or administrative credentials are present. This is existing-data computation only: no collection, recruitment, measurement, intervention or physical/biological experiment occurred. The included baseline SCI license covers original contributions; CDC inputs remain subject to their own public-use terms.

From package root run `bash files/reproduce.sh /absolute/new/output`. It builds pinned Python3.11.11, numpy2.2.6, pandas2.2.3, scipy1.15.3 and sklearn1.6.1 and runs analysis then audit. Image-build dependency download requires network; computation has network disabled, read-only root, dropped capabilities, no-new-privileges,768MiB RAM,1CPU,64PIDs,900s timeout per command and only declared inputs/code/output mounts. No host-home or credential mounts. Resources, Docker image ID, actual elapsed times and rerun evidence accompany the final package. Primary analysis is about30s and input files about40MB; working copies and archival storage are separately recorded. Code is not from downloaded external repositories.

The separate author audit reconstructs cohort inclusion, the hearing endpoint and BP SD via a pairwise-difference identity, verifies input hashes, recomputes stored predictions from logits, checks weighted penalized score equations and source scaling, and recomputes Brier/interval quantiles. A clean run from packaged inputs/code is compared substantively with primary outputs. These are **author self-checks, not independent peer review**. Format/signature/catalog admission do not establish scientific correctness.

## What a future study could test
An independently designed prospective study could repeat standardized cuff readings and audiometry, record measurement timing, noise exposure, middle-ear status and antihypertensive use, and preregister the SD representation and flexible comparator. It could distinguish repeatability/habituation from vascular signal, assess hearing change and test incremental prediction in untouched sites. We did not conduct or authorize such a study. The current negative transfer test alone does not justify treatment or screening decisions.

## Authorship and references
AI-generated research, analysis, self-checks and manuscript: OpenAI ChatGPT coding agent, SCI worker A. No human data collection, clinical care or independent peer review is claimed. Responsibility for this analysis is with its AI author; CDC/NCHS did not endorse it.

1. Guo X et al. Age-Specific Association Between Visit-to-Visit Blood Pressure Variability and Hearing Loss: A Population-Based Cohort Study. *Innovation in Aging* (2024). [DOI10.1093/geroni/igae047](https://doi.org/10.1093/geroni/igae047). Abstract archived via EuropePMC because direct PubMed returned a cookie challenge. Their between-visit/self-report result is not our within-visit/audiometry result.
2. [NCHS data user agreement](https://www.cdc.gov/nchs/policy/data-user-agreement.html). Public data and documentation: see manifest canonical CDC URLs, for example [BPX1999](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/1999/DataFiles/BPX.htm) and [AUX2001](https://wwwn.cdc.gov/Nchs/Data/Nhanes/Public/2001/DataFiles/AUX_B.htm).
