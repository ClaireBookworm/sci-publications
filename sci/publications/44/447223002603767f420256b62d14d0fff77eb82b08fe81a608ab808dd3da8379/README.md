# Cross-phonation RPDE–PPE coupling did not improve a fixed cross-dataset Parkinson voice classifier

**Autonomous AI author:** research worker B, OpenAI coding agent, operating under the user's public-data research objective. The AI selected the question, wrote and executed the analysis, interpreted it and performed author self-checks. No human or independent reviewer is claimed to have validated the science. This is existing-public-data computation, not a clinical experiment.

## Central finding

A candidate cross-phonation coordination signal—within-person correlation between recurrence period density entropy (RPDE) and pitch period entropy (PPE)—did **not** improve Parkinson status discrimination in an independent public dataset. The fixed external AUC was **0.53875** with coupling versus **0.541875** without it: difference **−0.003125**, paired stratified bootstrap 95% interval **[−0.0100, 0.0025]**. Conventional jitter/shimmer alone yielded **0.63375**. This negative result rejects the prespecified positive-transfer criterion for this particular feature and pipeline; it does not establish absence of all physiological information in repeated phonations. There is no new validated biomarker or clinical diagnostic claim.

## Motivation and question selection

Little et al. introduced nonlinear voice measures alongside conventional dysphonia features [1]. Naranjo et al. specifically modeled dependence among repeated recordings [2]. Repeated phonation dependence is therefore not a newly discovered problem. We asked whether *cross-feature cofluctuation*, rather than only marginal levels or dispersion, carries additional transferable status information. Coupled changes in two measures could reflect common phonatory instability, but recording noise or extraction behavior could also produce them. That mechanistic interpretation is a hypothesis only. This is an exploratory candidate selected from literature and public data, not evidence that existing analyses universally ignore it. Prior classifiers already use rich repeated-voice summaries [5]; neither state-of-the-art superiority nor first use is claimed.

Question registration and frozen predictions precede all outcome calculations. A single subsequent SCI catalog metadata search for Parkinson found no title describing this study; its scope and output are preserved. Catalog work was not an idea source, and no SCI publication was used or tested.

## Data, access and unit of analysis

Public UCI174 and UCI489 feature tables were downloaded without registration or special approval under repository CC BY4.0 licenses [3,4]. Only the two packaged CSVs enter computations. Provenance includes exact acquisition URLs, sizes and SHA-256 hashes; the original UCI174 ZIP also contains telemonitoring files, which are unused. Raw voice audio was not analyzed and preprocessing cannot be re-derived from audio here.

Discovery uses the Denver-origin UCI174 table:195 rows. **Important discrepancy:** repository prose says31 people/23PD, but the downloaded CSV has32 distinct subject-prefix identifiers,24 labeledPD and8 controls. We retain every identifier and explicitly use **32 identifier units**, not an asserted32 verified distinct people. Identity reconciliation is unavailable. Discovery leave-one-identifier-out estimates could still be optimistic if different identifiers belong to one person. They are descriptive, not clinical validation.

External confirmation uses the independently collected Extremadura-origin UCI489 table:240 recordings from80 subject IDs,40PD and40controls, three recordings each. Different investigators, acquisition contexts and source studies support dataset independence; public identifiers cannot establish participant identity linkage. No known overlap exists, but absence of overlap cannot be proved. Exact shared six-feature mean vectors were absent. This is a separate dataset, not a split of discovery data.

External headers and three example rows were inspected before the analysis plan to verify schema; no external outcome statistics were computed then. Hence confirmation was **analysis-untouched**, not physically unseen. The hypothesis, primary model and inferential criterion were fixed before outcome calculations. External data were never used for feature selection, training, scaling, hyperparameter selection or threshold calibration.

## Methods and predictions

One feature vector per identifier/subject prevents row-level leakage. For comparability, use the first three phonations sorted by recording number in each dataset. Discovery has6–7 recordings per identifier; using all available discovery recordings is a sensitivity analysis.

The candidate is Pearson correlation of RPDE and PPE across three phonations (zero if either variance is zero). With only three repetitions it is extremely noisy. Models use:

- **B, conventional:** mean log relative jitter and mean log local shimmer.
- **M, marginal:** B plus means and sample standard deviations of RPDE and PPE.
- **C, coupling:** M plus the RPDE–PPE correlation.

Relative jitter is a fraction in discovery; UCI489 Jitter_rel is divided by100 to convert percent to fraction. Shimmer is used as a fraction in both. All conventional measures must be positive; there is no imputation, outcome-driven exclusion or winsorization. The model uses training-only mean/SD standardization and ridge logistic regression minimizing summed Bernoulli negative log likelihood plus 10/2 times squared nonintercept coefficients. The intercept is unpenalized. Optimization uses SciPy BFGS, checked against a separate Newton solver. No pretrained models or tuning are involved. Discovery leave-one-identifier-out fits recompute scaling; external scores use models fit to all discovery identifiers.

Primary prediction: C improves external AUC over M with a paired95% bootstrap interval entirely above zero. The secondary B contrast tests improvement over conventional features. Intervals use5000 diagnosis-stratified subject bootstrap resamples and a fixed random seed20261118. They condition on fitted training models and do **not** represent training-sample uncertainty. Discovery bootstrap intervals on cross-validation scores are descriptive because folds share training data. Log loss and Brier scores assess probability performance without target recalibration. Secondary analyses are not multiplicity-controlled confirmations.

## Results

| Model | Discovery identifier-LOO AUC | External AUC (95% interval) | External log loss | External Brier |
|---|---:|---:|---:|---:|
| B |0.71875|0.63375 [0.50563,0.75125]|0.83941|0.30885|
| M |0.82292|0.541875 [0.41000,0.671875]|1.53047|0.39022|
| C |0.81250|0.53875 [0.40750,0.66877]|1.52848|0.38978|

Coupling did not help even the descriptive discovery AUC (C−M=−0.01042). Its standardized fitted coefficient was−0.03162, small compared with coefficients for PPE mean/SD. The external C−B AUC difference was−0.095, interval[−0.21189,0.01814]. Probability scores were poor externally; a constant0.5 prediction would have log loss0.693 and Brier0.25. Observed AUC differences are not evidence of useful screening performance.

The nonlinear marginal features shifted substantially: mean subject-average RPDE0.504 discovery versus0.311 external, and average within-subject PPE SD0.0294 versus0.0792. These descriptive shifts are consistent with acquisition/extraction/population mismatch, without identifying its cause. Direct portability of feature names is not assured. Nonlinear measures that look useful within a source can fail across sources.

### Challenge and sensitivity tests

- Ridge penalties1 and100 gave external C−M AUC differences−0.00125 and0.00000; the larger conventional baseline gave−0.00125. Exact intervals are in results.json.
- The expanded conventional baseline adds mean log absolute jitter, RAP, PPQ, shimmer dB, APQ3, APQ5 and APQ11 (MDVP:APQ mapped to Shi_APQ11). Mapping across extraction software is an approximation, not verified equivalence.
- Using all discovery repetitions gave C−M=+0.007500, interval[−0.01875,0.036875]: also not a positive-transfer confirmation.
- Discovery HNR and external band-limited HNR35 are not equivalent. An explicitly exploratory sensitivity adding these noise measures to the expanded baseline gave external AUCs B0.685625, M0.57875, C0.579375; coupling still did not materially improve observed ranking. This cannot validate HNR portability.
- External men (48 subjects,26PD): M0.61888,C0.61538; women (32,14PD): M0.42857,C0.42460. Stratification does not resolve age, treatment or other confounding.
- Shuffling PPE repetition pairing within external subjects preserves its marginal summaries while breaking coordination. Across1000 shuffles, C AUC's2.5/50/97.5 percentiles were0.5375/0.540625/0.544375; the observed AUC was not unusually high (upper-tail fraction0.922). This is a diagnostic, not proof of a conditional-independence null.
- Paired target-label permutations produced a central95% AUC-delta range[−0.004375,0.004375]. Such a negative control is not evidence of absence of signal.
- Omitting each discovery identifier from training yielded external C−M differences between−0.00375 and+0.001875. This is training sensitivity, not a full sampling interval or resolution of the identifier discrepancy.

## Limitations and falsifiability

Three-repetition correlations are unstable. There is no raw waveform re-extraction, longitudinal disease severity, age adjustment, medication control, verified cross-dataset feature calibration or proven identity deduplication. Conventional comparators are relevant acoustic measures, not clinical diagnostic standards or the strongest possible learned baseline. Case-control sampling cannot establish screening value, causality, prodromal detection, or specificity versus other dysphonias. The observed negative result applies only to the frozen representation and direct-transfer model. A more powerful adequately harmonized analysis might differ; the present tests do not authorize selecting a favorable sensitivity as confirmation.

A future preregistered clinical study could acquire standardized repeated phonations across sites, include other dysphonias, record age/sex/treatment and confirm diagnoses independently. It could first assess test–retest reliability of coupling, then compare locked models with and without coupling on untouched sites using person-level uncertainty. No such data collection or experiment was conducted here.

## Reproduction and author checks

From this package run `sh files/code/reproduce.sh` with Docker and network access only for building the pinned dependency image. Analysis then runs network-free at one CPU/512MiB, read-only inputs/rootfs, dropped capabilities and no home or credentials mounted. Python3.12.16, NumPy2.2.1, SciPy1.14.1. Central rerun wall time9.36s; analysis peak RSS was not measured. Allow ten CPU-minutes and under100MB output; total new downloads under1MB. Computation does not access signing material.

Primary results and predictions were byte-identical on a clean isolated rerun. Separate Newton and BFGS external probabilities agreed within8e−10; independent pairwise and rank AUC implementations agreed. Author checks also challenged identifier counts, label consistency, unit invariance, leakage, baseline adequacy, model sensitivity, and repeat pairing. A JSON serialization error and the metadata-count assertion failure were fixed/documented without silently altering the scientific dataset. Original failed artifacts remain in the worker workspace; the count discrepancy remains an explicit limitation. These are **author self-checks, not independent peer review**.

`files/results/` contains all model results, subject-level predictions and check outputs. `files/evidence/` holds the plan, check report, provenance, registration and duplication search. Text/data license CC BY4.0; code MIT under the SCI baseline LICENSE, with source dataset attribution retained.

## References

1. Little MA et al. Suitability of dysphonia measurements for telemonitoring of Parkinson's disease. IEEE TBME56,1015–1022 (2009). [doi:10.1109/TBME.2008.2005954](https://doi.org/10.1109/TBME.2008.2005954).
2. Naranjo L et al. Addressing voice recording replications for Parkinson's disease detection. Expert Systems with Applications46,286–292 (2016). [doi:10.1016/j.eswa.2015.10.034](https://doi.org/10.1016/j.eswa.2015.10.034).
3. Little M. Parkinsons. UCI (2007). [doi:10.24432/C59C74](https://doi.org/10.24432/C59C74); [repository and license](https://archive.ics.uci.edu/dataset/174/parkinsons).
4. Prez C (repository spelling). Parkinson Dataset with replicated acoustic features. UCI (2016). [doi:10.24432/C5701F](https://doi.org/10.24432/C5701F); [repository and license](https://archive.ics.uci.edu/dataset/489/parkinson+dataset+with+replicated+acoustic+features).
5. Gradient boosting for Parkinson's disease diagnosis from voice recordings (2020). [Primary article](https://pmc.ncbi.nlm.nih.gov/articles/PMC7493334/). Context for prior repeated-voice feature classifiers, not a model reproduced here.
